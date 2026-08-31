# TRẠM NỤ CƯỜI — PHASE 12
# P12-WU4 — JOURNEY-NATIVE COMMUNITY INTERACTION

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

P12-WU4 adds the first constrained community interaction that is native to a real Journey relationship rather than a generic social network.

Implemented capability: **Journey Reflections**.

Canonical relationship:

**Verified Person → Verified Attendance → Evidence-backed Memory → Completed Journey → Reflection → Staff Moderation → Identity-minimized Public Publication**

Explicitly out of scope:
- generic/infinite feed;
- follower graph;
- private chat;
- engagement gamification;
- reaction counts detached from Journey context;
- public community profiles;
- fabricated attendance, Memory or Reflection data.

P11-WU6 remains ACTIVE for the real pilot on 2026-09-11. WU4 does not pre-empt or manufacture pilot facts.

## 2. Product source evidence

Product repository:
`huynhtranhuythinh/tramnucuoi`

PR:
- #23 — `P12-WU4 Journey-native community reflections`

Merged product main SHA:
`b94692c544d5703f7052971ac818b69c5e1e1eb8`

Canonical source migration:
`db/migrations/0037_p12_wu4_journey_reflections_foundation.sql`

Dedicated QA:
`scripts/p12-wu4-db-qa.sql`

Community UX:
- `src/components/community/community-reflections-panel.tsx`
- `/cong-dong`
- `/en/community`

Staff moderation UX:
- `src/routes/_authenticated/admin.reflections.tsx`
- Admin navigation includes `Reflections`.

No Cloudflare production deployment occurred in WU4.

## 3. Data model

### 3.1 `public.journey_reflections`

Author-visible private reflection source.

Core fields:
- `id`
- `user_id`
- `memory_id`
- `journey_id`
- `body`
- `locale`
- `status`
- timestamps

Rules:
- one reflection per `(user_id, journey_id)`;
- body must be 20–1200 trimmed characters;
- locale is `vi` or `en`;
- status is `pending`, `published`, or `rejected`;
- community authors may SELECT only their own rows;
- staff may read the moderation source;
- community authors can INSERT but cannot moderate or DELETE;
- source content and ownership become immutable after submission.

### 3.2 `public.journey_reflection_moderation_events`

Separate staff-only append-only moderation audit.

Contains:
- `reflection_id`
- moderation decision (`published` / `rejected`)
- `moderated_by`
- `moderated_at`

Reason for separation:
community authors must not receive moderator UUIDs or internal moderation audit data simply because they can read their own Reflection source row.

### 3.3 `public.journey_reflection_publications`

Identity-minimized public projection.

Contains only:
- `reflection_id`
- `journey_id`
- `body`
- `locale`
- publication timestamps

It deliberately excludes:
- auth user UUID;
- Memory ID;
- email / phone / applicant PII;
- moderator UUID;
- internal moderation information.

Only `published` Reflection source rows are projected publicly. A later `rejected` decision removes the public projection.

## 4. Authoritative authorship gate

Trigger guard:
`private.tnc_guard_journey_reflection()`

A Reflection INSERT succeeds only when all conditions are true:
1. caller is authenticated;
2. `user_id = auth.uid()`;
3. referenced Memory belongs to the same caller;
4. Memory belongs to the same Journey;
5. Memory attendance state is exactly `attended`;
6. `memory_eligible = true`;
7. operational Journey status is exactly `completed`.

Therefore:
- identity link alone is insufficient;
- confirmed registration alone is insufficient;
- unresolved attendance is insufficient;
- verified no-show is insufficient;
- verified attendance before Journey closeout is still insufficient.

On submission the database forcibly resets status to `pending`, so a client cannot self-publish by sending `status='published'`.

## 5. Moderation and publication gates

Only `admin` / `editor` staff may update moderation status.

Submitted source content is immutable:
- author identity cannot change;
- Memory cannot change;
- Journey cannot change;
- body cannot change;
- locale cannot change;
- original creation timestamp cannot change.

Each publish/reject transition appends a staff-only moderation event using the authenticated moderator identity.

Publication synchronization is trigger-driven:
- `published` → upsert public projection;
- `rejected` → remove public projection.

Community authors do not write the publication table directly.

## 6. Security model

All three WU4 public-schema tables have RLS enabled.

### Private source
`journey_reflections`
- anon SELECT: false
- authenticated INSERT: granted but governed by RLS + DB trigger gate
- authenticated UPDATE: granted but RLS restricts moderation to staff
- authenticated DELETE: false

### Moderation audit
`journey_reflection_moderation_events`
- anon SELECT: false
- community author SELECT: denied by staff-only RLS
- authenticated mutation: false

### Public projection
`journey_reflection_publications`
- anon SELECT: true
- authenticated SELECT: true
- authenticated mutation: false

Private trigger helpers:
- `private.tnc_guard_journey_reflection()`
- `private.tnc_audit_journey_reflection_moderation()`
- `private.tnc_sync_journey_reflection_publication()`

All are:
- in schema `private`;
- `SECURITY DEFINER` for controlled trigger-only cross-RLS work;
- `search_path=''`;
- direct EXECUTE revoked from PUBLIC, anon and authenticated.

## 7. Dedicated PostgreSQL QA

PR CI run #122: **PASS**.

Dedicated WU4 QA proved:
- eligible attended Memory + completed Journey can submit;
- attempted self-publish is normalized to `pending`;
- attended Memory before Journey completion is rejected;
- unresolved/non-eligible attendance is rejected;
- another user's Memory cannot be borrowed;
- unrelated community user cannot read Reflection source;
- author cannot change moderation status after submission;
- author cannot read staff moderation audit;
- staff can publish;
- publish creates an audit event with staff actor;
- publish creates public projection;
- public projection schema contains no user/Memory/moderator fields;
- anon can read publication but not source/audit;
- staff cannot rewrite submitted body while moderating;
- reject removes public projection;
- moderation history remains append-only across decisions;
- private trigger helpers are not directly executable by API roles;
- authenticated users cannot directly mutate moderation audit or public projection.

The same run also passed all inherited regression gates, build, typecheck and Cloudflare dry-run.

Post-merge main CI run #123: **PASS** with the same gate set.

## 8. Production migration

Supabase project:
`iwiqprhoohkxvjyxojto`

Applied migration:
`20260831014537 p12_wu4_journey_reflections_foundation`

Migration was schema-only. It did not intentionally create:
- Reflection rows;
- moderation events;
- public publications;
- Community identities;
- Memories;
- attendance;
- impact records;
- Journey lifecycle changes.

## 9. Production preflight and postflight

### Before migration
- pilot status: `registration_open`
- capacity: 30
- confirmed participant rows: 1
- confirmed people: 1
- attendance-resolved rows: 0
- community link rows: 0
- Memory rows: 0
- Memory-eligible rows: 0
- WU4 tables existing: 0
- pg_graphql: OFF

### After migration
- `journey_reflections`: 0 rows
- `journey_reflection_moderation_events`: 0 rows
- `journey_reflection_publications`: 0 rows
- community links: 0
- Memory rows: 0
- Memory-eligible rows: 0
- pilot status: `registration_open`
- capacity: 30
- confirmed participant rows: 1
- confirmed people: 1
- attendance-resolved rows: 0
- pg_graphql: OFF

Thus WU4 changed capability/schema only; it did not manufacture real-world facts.

## 10. Advisor result

### Security Advisor
No WU4-specific security regression was reported.

Existing project warning remains:
- `Leaked Password Protection Disabled`
- remediation: https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

### Performance Advisor
No WU4 foreign-key indexing gap or new multiple-permissive-policy warning was introduced.

WU4 indexes are currently reported as `unused`, which is expected immediately because all WU4 production tables contain zero rows and Community activation has not started.

Existing project performance advisories remain and are not claimed as solved by WU4.

## 11. UX outcome

### Community member
On `/cong-dong` and `/en/community`:
- public approved Reflections can be read from the identity-minimized publication projection;
- signed-in users may see a Reflection composer only for an evidence-backed Memory whose operational Journey is already completed;
- submitted Reflection status is visible as pending/published/rejected;
- no moderator identity is exposed.

### Staff
At `/admin/reflections`:
- admin/editor can read moderation queue;
- publish or reject Reflection;
- database, not the browser, records moderator identity and synchronizes public publication.

## 12. Activation gate

The WU4 capability is production-schema ready, but Community onboarding remains operationally gated by P12-WU2 conditions.

Current controls remain:
- Email OFF;
- no Community Auth activation;
- no Cloudflare production deploy for WU4;
- 0 real Community identity links;
- 0 real Memories;
- 0 real Reflections.

Real Reflection flow must only become populated after legitimate identity claim, real attendance, completed Journey closeout and actual member submission.

## 13. Final declaration

**P12-WU4 — Journey-native Community Interaction: COMPLETE / PASS.**

The implemented interaction primitive is deliberately narrow: Journey Reflection, not a generic social network.

Next canonical Phase 12 work:
**P12-WU5 — Contribution History**.

P11-WU6 remains **ACTIVE** until the real 2026-09-11 pilot is executed and evidence-based post-event closeout completes.
