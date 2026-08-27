# Journey Field Story — Migration Review (Phase 8.2)

**Status: 0015–0018 ALL APPLIED.**
Canonical Supabase migration history:

| migration | version | status |
| --- | --- | --- |
| `0015_journey_field_story_foundation.sql` | `20260825052429` | applied |
| `0016_journey_media_evidence.sql` | `20260825052443` | applied |
| `0017_journey_field_note_relations.sql` | `20260825052451` | applied |
| `0018_journey_media_authenticated_visibility_hardening.sql` | `20260825062405` | applied |

Applied migrations are immutable — applied SQL is never edited. `0018` is a
forward security hardening migration, applied after authenticated non-staff
policy drift was detected in `journey media staff read`: it aligns that
fallback with anon visibility, requiring the linked `journey_updates` row to be
`published`.

- Target backend: **external Supabase ref `iwiqprhoohkxvjyxojto`** (canonical). Not Lovable Cloud.
- Canonical migration location: `/db/migrations` only. `/supabase/migrations` and `src/integrations/supabase/*` are unused residue and are not touched.
- **No UI was built** (no admin screen, no public component, no route).
- **No change to the Journey Email Notification Layer** — it stays paused.
- **No deployment, no publish, no seed data.**

## Post-apply verification

- Live `journey media staff read` policy now requires the linked `journey_update` to be `published` for the authenticated non-staff fallback.
- All new Phase 8.2 tables have Row Level Security enabled.
- No new foreign key from any new Phase 8.2 table points to `journey_applications` or `journey_participants`.
- Security advisor: no new Phase 8.2 security issue. The pre-existing project warning **Leaked Password Protection Disabled** remains.
- Performance advisor (INFO): composite FK `journey_media_update_same_journey` has no covering index in the exact FK column order. Recorded as future performance cleanup — **not** part of this migration gate.


## Design principles honoured

- Additive only. No existing table is dropped, no column removed, no existing policy or grant weakened, no existing content rewritten.
- Replay-safe: `if not exists`, `drop policy if exists` before `create policy`, enum creation guarded, revoke-then-grant so a replay cannot leave a widened privilege.
- Reuses `public.content_status`, `public.set_updated_at()`, and the hardened helpers `private.can_edit()` / `private.has_role(app_role)` from 0012. No new function, therefore no new RPC surface.
- Bilingual VI/EN by paired `*_en` columns with Vietnamese as canonical fallback — identical to `journeys`, `posts`, `ecosystem_projects`, `media_assets`.
- Operational Journey remains a separate domain from the editorial Field Journal (`posts.kind='journey'`). Nothing converts, copies or reclassifies posts.
- `journey_applications` and `journey_participants` receive **no new grant, policy, relation or exposure**.

## Data model

### 0015 — Field Updates + Impact Snapshot

`public.journeys` is **not altered**. The Impact Snapshot narrative lives in a dedicated one-to-one table `public.journey_impact_snapshots`.

**Why a separate table is required (security, not style):** a journey row is publicly readable while its status is `registration_open` or `preparing`. RLS is row-level, so any impact column placed on `journeys` would be readable through a direct Data API query (`/journeys?select=impact_summary`) long before the journey is completed — draft/future impact prose would leak. Omitting the field in the UI is not a security boundary. With its own table, the anon predicate is strictly `parent journey.status = 'completed'`.

`public.journey_impact_snapshots`: `journey_id` uuid **primary key** FK → `journeys` `on delete cascade` (enforces one-to-one), `summary`/`summary_en`, `key_observations`/`key_observations_en`, `created_at`/`updated_at` with the `set_updated_at` trigger. All editor-written prose; nothing computed or aggregated; **no personal data, no seed**.


`public.journey_updates`
| column | type | notes |
| --- | --- | --- |
| `id` | uuid PK | `gen_random_uuid()` |
| `journey_id` | uuid | FK → `journeys` `on delete cascade` |
| `kind` | `journey_update_kind` | new enum: `field_note | activity | milestone` |
| `happened_at` | timestamptz | nullable |
| `title` / `title_en` | text | `title` not null |
| `body` / `body_en` | text | |
| `location` / `location_en` | text | |
| `cover_media_id` | uuid | FK → `media_assets` `on delete set null` |
| `status` | `content_status` | default `draft` |
| `sort_order` | int | default 0 |
| `created_by` | uuid | FK → `auth.users` `on delete set null` |
| `published_at` | timestamptz | nullable |
| `created_at` / `updated_at` | timestamptz | `set_updated_at` trigger |

Also carries the constraint `journey_updates_id_journey_unique UNIQUE (id, journey_id)` — the composite target used by 0016 for same-journey integrity. It is added inside a `DO` block that first checks `pg_constraint`, and there is **no standalone pre-created index under another name**: the constraint creates and owns its own unique index, so a replay can never leave a redundant second index behind.

`public.journey_impact_items`: `id`, `journey_id` (cascade), `label`/`label_en` (label not null), `value`/`value_en` (value not null), `unit`/`unit_en`, `note`/`note_en`, `sort_order`, timestamps. Editorial rows only; **no personal data**. Like the snapshot narrative, impact items are public **only when the parent journey is completed**.

Indexes: `(journey_id, status, happened_at desc nulls last, created_at desc)`, `(journey_id, sort_order)` on both tables, plus FK-supporting indexes on `cover_media_id` and `created_by`.

### 0016 — Journey media evidence

- `media_assets.captured_at timestamptz` (nullable), commented as editor-entered capture time, never inferred from upload time, filename or EXIF.
- `public.journey_media` is a **relationship table only**: `id`, `journey_id` (cascade), `media_asset_id` (cascade), nullable `journey_update_id`, nullable `evidence_category` constrained to `activity | people | place | output | context`, `sort_order`, `created_at`. It stores no url/path/alt/caption/credit — `media_assets` stays the single source of truth.
- **Same-journey integrity is declarative**: `foreign key (journey_update_id, journey_id) references journey_updates (id, journey_id) on delete cascade`. An update from a different journey cannot be referenced. No trigger and no `SECURITY DEFINER` function is introduced, so no elevated RPC is exposed.
- **Delete behaviour of a journey update**: a row carrying `journey_update_id` is an update-specific relationship, so deleting the `journey_updates` row cascade-deletes **only that relationship row**. The `media_assets` record is never deleted, and an independent journey-level relationship for the same asset (`journey_update_id is null`) survives. Relationship semantics stay simple and predictable, with no conversion of update-level rows into journey-level rows and therefore no uniqueness edge case.

- **Duplicate protection with correct NULL semantics**: two partial unique indexes — `(journey_id, media_asset_id) where journey_update_id is null` for journey-level attachments, `(journey_update_id, media_asset_id) where journey_update_id is not null` for update-level attachments. A plain unique constraint would not work, because NULLs never collide.

### 0017 — Journey ↔ Field Journal relations

`public.journey_field_notes`: `journey_id` (cascade), `post_id` (cascade), `sort_order`, `created_at`, primary key `(journey_id, post_id)`. Pointer only; `public.posts` is not altered and `posts.kind` semantics are unchanged.

## Access model (summary; full cases in the RLS test matrix)

| table | anon | editor (`private.can_edit()`) | admin |
| --- | --- | --- | --- |
| `journey_updates` | SELECT only when update `published` **and** parent journey public | read all, insert, update | + delete |
| `journey_impact_snapshots` | SELECT only when parent journey `status='completed'` | read all, insert, update, delete | same |
| `journey_impact_items` | SELECT only when parent journey `status='completed'` | read all, insert, update, delete | same |
| `journey_media` | SELECT only when journey public **and** asset `is_public` **and** `evidence_status='documentation'` (**and** parent update published when linked) | read all, insert, update, delete (unlink) | same; deleting the file itself stays admin-only via existing `media_assets` policy |
| `journey_field_notes` | SELECT only when journey public **and** post `published` and `kind='journey'` | read all, insert, update, delete | same |

Every new public table gets explicit `GRANT`s in the same migration (`select` to `anon` where a public policy exists, full DML to `authenticated`, `all` to `service_role`) after a `revoke all`, then `ENABLE ROW LEVEL SECURITY`, then policies.

## Migration risks

1. Enum `journey_update_kind` is new; adding values later is additive but not transaction-reversible. Keep the set at three.
2. Composite FK requires `journey_updates` to exist first — apply strictly 0015 → 0016 → 0017.
3. Policy subselects on `journeys`, `media_assets` and `posts` run with the caller's privileges, so those tables' own RLS applies on top. This is deliberate defence-in-depth, but it means a future tightening of `media_assets` anon SELECT can only ever narrow journey media, never widen it.
4. `journey_media` public reads join three tables; if a journey ever accumulates many assets, verify the plan uses `journey_media_journey_sort_idx`.
5. `revoke all ... from anon, authenticated` before granting is safe on a fresh table; it must not be copied onto pre-existing tables.
6. Application code has not been updated for these tables yet — the schema can be applied without any client change, and no existing query selects the new columns.

## Explicitly out of scope for Phase 8.2

Comments, reactions, followers, chat, gamification, public profiles, social feed, "My Journey", Contributor/Host/Partner dashboards, public accounts or public authentication, realtime, `memories`, contributions/finance, BI or computed metrics, notification changes, any conversion between `posts.kind='journey'` and operational journeys, Lovable Cloud DB, and any deploy or publish.

## Next gate

Owner review of 0015–0017 → apply on `iwiqprhoohkxvjyxojto` in order → run the RLS test matrix → only then build admin and public UI (Phase 8.2.2+).
