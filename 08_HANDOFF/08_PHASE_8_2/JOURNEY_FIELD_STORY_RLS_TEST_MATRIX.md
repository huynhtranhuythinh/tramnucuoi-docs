# Journey Field Story — RLS Test Matrix (Phase 8.2)

Scope: `journey_updates`, `journey_impact_snapshots`, `journey_impact_items`, `journey_media`, `journey_field_notes` from migrations 0015–0018, which are **APPLIED** on external Supabase `iwiqprhoohkxvjyxojto` (see `JOURNEY_FIELD_STORY_MIGRATION_REVIEW.md`).

This document is the RLS verification/test matrix for that applied schema: it defines the expected access outcome for each fixture and role. It does not by itself record which individual cases were executed — treat an unmarked row as a defined expectation, not as a proven test result.

Roles under test:
- **anon** — publishable key, no session (the public site).
- **editor** — signed in, `user_roles.role='editor'`, so `private.can_edit()` is true and `private.has_role('admin')` is false.
- **admin** — signed in, `user_roles.role='admin'`.
- `service_role` is never used in testing.

"Public journey status" means `registration_open | preparing | completed`. `draft` and `archived` are never public.

## A. journey_updates

| # | Fixture | anon | editor | admin |
| --- | --- | --- | --- | --- |
| A1 | update `draft`, journey `registration_open` | DENY (0 rows) | ALLOW read | ALLOW read |
| A2 | update `published`, journey `draft` | DENY | ALLOW read | ALLOW read |
| A3 | update `published`, journey `archived` | DENY | ALLOW read | ALLOW read |
| A4 | update `published`, journey `completed` | ALLOW read | ALLOW | ALLOW |
| A5 | update `published`, journey `preparing` | ALLOW read | ALLOW | ALLOW |
| A6 | INSERT update | DENY (no grant, no policy) | ALLOW | ALLOW |
| A7 | UPDATE update row | DENY | ALLOW | ALLOW |
| A8 | DELETE update row | DENY | **DENY** (admin-only) | ALLOW |
| A9 | INSERT with `journey_id` of a draft journey | DENY | ALLOW (staff authoring is status-independent) | ALLOW |

Explicit leak checks: A1, A2 and A3 must each return `[]` for anon on both a filtered and an unfiltered `select=*`.

## B. journey_impact_snapshots + journey_impact_items

Impact Snapshot narrative and impact items are **post-journey**: public only when the parent journey is `completed`.

| # | Fixture | anon | editor | admin |
| --- | --- | --- | --- | --- |
| B1 | items under journey `draft` | DENY | ALLOW read | ALLOW read |
| B2 | items under journey `archived` | DENY | ALLOW read | ALLOW read |
| B3 | items under journey `completed` | ALLOW read | ALLOW | ALLOW |
| B4 | INSERT / UPDATE / DELETE item | DENY | ALLOW | ALLOW |
| B5 | items under journey `registration_open` | DENY | ALLOW read | ALLOW read |
| B6 | items under journey `preparing` | DENY | ALLOW read | ALLOW read |
| B7 | snapshot row under journey `registration_open` | DENY | ALLOW read | ALLOW read |
| B8 | snapshot row under journey `preparing` | DENY | ALLOW read | ALLOW read |
| B9 | snapshot row under journey `draft` / `archived` | DENY | ALLOW read | ALLOW read |
| B10 | snapshot row under journey `completed` | ALLOW read | ALLOW | ALLOW |
| B11 | INSERT / UPDATE / DELETE snapshot row | DENY | ALLOW | ALLOW |
| B12 | second snapshot row for the same `journey_id` | — | primary-key violation (one-to-one) | same |

Explicit leak checks: for B5–B9 anon must receive `[]` on a direct Data API query such as `/journey_impact_snapshots?select=*` and `?select=summary,key_observations` — proving that hiding the section in the UI is not what protects it. This is why the narrative is a separate table: on `public.journeys`, whose rows are anon-readable while `registration_open`/`preparing`, row-level access would expose the impact columns to any direct `select`.


## C. journey_media

| # | Fixture (journey / asset / update) | anon | editor | admin |
| --- | --- | --- | --- | --- |
| C1 | public / `is_public=true`, `documentation` / none | ALLOW read | ALLOW | ALLOW |
| C2 | public / `is_public=true`, `evidence_status='sample'` / none | DENY | ALLOW | ALLOW |
| C3 | public / `is_public=true`, `unclassified` / none | DENY | ALLOW | ALLOW |
| C4 | public / `is_public=false`, `documentation` / none | DENY | ALLOW | ALLOW |
| C5 | journey `draft` / public documentation asset | DENY | ALLOW | ALLOW |
| C6 | public / public documentation asset / linked update `draft` | DENY | ALLOW | ALLOW |
| C7 | public / public documentation asset / linked update `published` | ALLOW read | ALLOW | ALLOW |
| C8 | INSERT relation row | DENY | ALLOW | ALLOW |
| C9 | DELETE relation row (unlink) | DENY | ALLOW | ALLOW |
| C10 | DELETE the underlying `media_assets` row | DENY | DENY (existing media policy) | ALLOW |
| C10a | Authenticated **non-staff** (no `admin`/`editor` role): public journey / public documentation asset / linked update `draft` | n/a | n/a | must be DENY for that account |
| C10b | Authenticated **non-staff**: public journey / public documentation asset / linked update `published` | n/a | n/a | must be ALLOW read for that account |

C6/C7 must each be run **twice on the authenticated path**: once with a session
whose `private.can_edit()` is true (editor/admin — ALLOW in both fixtures, the
staff bypass) and once with a signed-in account whose `private.can_edit()` is
false. In the `can_edit() = false` case the expected result is identical to the
anon column: C6 → DENY (`[]`), C7 → ALLOW read. C10a/C10b are those two
non-staff runs stated explicitly.

C10a/C10b prove the staff-read fallback matches anon visibility exactly: without
`private.can_edit()`, an authenticated non-staff session must not observe a
relation row that points at an unpublished field update, because the relation id
alone would reveal that a draft update exists. The predicate is present in both
the hardened 0016 policy body (fresh replay) and the corrective 0018 migration
(already-applied databases), so the check must pass on either path.



Integrity checks (any role with write access):
- C11 — INSERT with `journey_update_id` belonging to a **different** journey → FK violation (`journey_media_update_same_journey`). Must fail at the database, not in app code.
- C12 — INSERT the same `(journey_id, media_asset_id)` twice with `journey_update_id is null` → unique violation.
- C13 — INSERT the same `(journey_update_id, media_asset_id)` twice → unique violation.
- C14 — the same asset attached once at journey level and once at update level → allowed (two distinct relations).
- C15 — `evidence_category` outside `activity | people | place | output | context` → check violation; `null` allowed.
- C16 — DELETE a `journey_updates` row that has media linked to it → its **update-specific** `journey_media` rows (`journey_update_id = that update`) cascade-delete. The `media_assets` records survive untouched, and any independent journey-level relation for the same asset (`journey_update_id is null`) survives.
- C17 — DELETE the parent `journeys` row → all its `journey_media`, `journey_updates`, `journey_impact_snapshots`, `journey_impact_items` and `journey_field_notes` rows cascade away; `media_assets` and `posts` rows survive untouched.



## D. journey_field_notes

| # | Fixture | anon | editor | admin |
| --- | --- | --- | --- | --- |
| D1 | journey public, post `published` + `kind='journey'` | ALLOW read | ALLOW | ALLOW |
| D2 | journey public, post `draft` | DENY | ALLOW | ALLOW |
| D3 | journey public, post `published` but `kind='news'` | DENY | ALLOW | ALLOW |
| D4 | journey `draft`, post published journey-kind | DENY | ALLOW | ALLOW |
| D5 | INSERT / UPDATE / DELETE relation | DENY | ALLOW | ALLOW |
| D6 | duplicate `(journey_id, post_id)` | — | PK violation | PK violation |

## E. PII containment (must not regress)

| # | Check | Expected |
| --- | --- | --- |
| E1 | editor SELECT on `journey_applications` | DENY (admin-only, unchanged from 0014) |
| E2 | editor SELECT on `journey_participants` | DENY |
| E3 | anon SELECT on `journey_applications` / `journey_participants` | DENY |
| E4 | anon INSERT into `journey_applications` while journey `registration_open` | ALLOW (unchanged) |
| E5 | anon INSERT while journey not `registration_open` | DENY (unchanged) |
| E6 | `\d+` / catalog diff on both PII tables after 0015–0017 | zero new grants, zero new policies, zero new columns, zero new FK pointing at them |
| E7 | no new table references `journey_applications` or `journey_participants` | confirmed by grep of 0015–0017 |
| E8 | no new `SECURITY DEFINER` function or client-callable RPC introduced | confirmed by grep of 0015–0017 |

Editor gains authoring rights over field story content and relations, and gains **no** path to applicant or participant data as a side effect.

## F. Regression checks on existing systems

| # | Check | Expected |
| --- | --- | --- |
| F1 | anon reads `media_assets` | unchanged: only `is_public=true` metadata |
| F2 | anon reads `posts` | unchanged: only `status='published'` |
| F3 | anon reads `ecosystem_projects` | unchanged: only `status='published'` |
| F4 | anon reads `site_content` | unchanged: published + visible only |
| F5 | anon reads `journeys` | unchanged: public statuses only |
| F6 | `/`, `/en`, `/hanh-trinh`, `/en/journey`, `/journeys`, `/en/journeys` after applying | render identically; no query selects the new tables yet |
| F7 | admin CMS (content, projects, posts, media, journeys) | unchanged behaviour |
| F8 | `private.can_edit()` / `private.has_role()` still `EXECUTE` for `authenticated` + `service_role` only | unchanged |
| F9 | replay 0015 → 0016 → 0017 a second time | succeeds, no duplicate policy, no widened grant |
| F10 | after replay, count unique indexes on `journey_updates(id, journey_id)` | exactly **one**, owned by constraint `journey_updates_id_journey_unique`; no second index under any other name |

## G. Execution notes

- Run anon cases with the publishable key against PostgREST; expect `[]` for DENY on SELECT and `42501` for DENY on write.
- Run editor/admin cases with real signed-in sessions on the external project; never with `service_role`, never by relaxing a policy.
- Use clearly-marked temporary fixtures and delete them through normal admin paths immediately after the run; record cleanup confirmation.
- Gate rule: every DENY row above must be observed as DENY before any Phase 8.2 UI work begins.
