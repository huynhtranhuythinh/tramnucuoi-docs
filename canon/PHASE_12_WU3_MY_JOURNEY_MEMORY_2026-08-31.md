# PHASE 12 — COMMUNITY OS CONTINUATION
# P12-WU3 — MY JOURNEY MEMORY

Date: 2026-08-31
Status: **COMPLETE / PASS — UX ACTIVATION INHERITS WU2 EMAIL GATE**

## Objective

Turn a verified Community Identity ↔ Journey participation relationship into a personal Journey archive without fabricating attendance, Memory, documentary evidence or impact.

The central semantic rule is:

- identity link proves ownership of a confirmed Journey participation record;
- `attended_party_size IS NULL` = attendance unresolved;
- `attended_party_size = 0` = verified no-show;
- `attended_party_size > 0` = verified attended;
- only verified attendance greater than zero makes a Journey `memory_eligible`.

Identity link alone never creates an attended Memory.

## Architecture

WU3 adds `public.community_journey_memories` as a read-only personal archive projection.

The projection contains:

- community user identity;
- participant source reference;
- Journey reference;
- attendance state;
- authoritative attended party size when resolved;
- Memory eligibility;
- identity-link timestamp;
- attendance-recorded timestamp;
- source update timestamp.

It does not copy applicant email, phone, full name, registration message or impact claims.

## Access model

The authoritative `journey_participants` table remains admin-only.

Community users do **not** receive participant-table SELECT permission.

`community_journey_memories`:

- RLS enabled;
- authenticated users may SELECT only their own rows;
- anon cannot SELECT;
- authenticated users cannot INSERT/UPDATE/DELETE projection rows;
- service role retains operational access.

Projection maintenance happens only through private trigger/internal helpers.

## Synchronization

Private function:

`private.tnc_sync_community_journey_memory(participant_id)`

Behavior:

1. no active identity link → remove projection;
2. participant missing or no longer confirmed → remove projection;
3. attendance NULL → `unresolved`, `memory_eligible=false`;
4. attendance 0 → `no_show`, `memory_eligible=false`;
5. attendance >0 → `attended`, `memory_eligible=true`;
6. ownership changes/revocation cannot leave stale projection rows.

Sync triggers react to:

- identity-link insert/delete;
- identity-link status/ownership reference changes;
- participant status changes;
- participant attendance count/timestamp changes.

All sync helpers are private and cannot be executed directly by anon or authenticated API roles.

The migration backfills only already-active verified identity links and never invents attendance. Production had zero identity links at application time, therefore it created zero Memory rows.

## UX

The bilingual Community Account source now includes:

- `Hành trình của tôi` / `My Journeys`;
- unresolved attendance state;
- verified attendance zero/no-show state;
- verified attended state;
- an evidence-backed Memory indicator only when attendance is >0;
- link back to the public Journey story where available.

The UX explicitly explains that confirmed registration, actual attendance and evidence-backed Memory are separate facts.

Community route activation remains inherited from P12-WU2. No Cloudflare Worker deployment was performed because production Community Auth email delivery is still activation-gated while canonical Email is OFF.

## Source / CI

Primary PR: `#21` — **P12-WU3 evidence-based My Journey Memory**

Primary merge SHA:
`627a0ffc406e45bf3f33ae4b57757cea80a16560`

PR CI `#111`: PASS, including:

- all prior regression gates;
- dedicated P12-WU3 PostgreSQL 17 database QA;
- build;
- typecheck;
- Cloudflare dry-run.

Dedicated WU3 QA proves:

- no identity link → no personal archive row;
- identity link + unresolved attendance does not become Memory eligible;
- verified zero attendance remains no-show and not Memory eligible;
- verified positive attendance becomes Memory eligible;
- own-user RLS isolation;
- participant source remains inaccessible to community users;
- community users cannot directly author projection rows;
- revoking identity ownership removes projection;
- non-confirmed participant removes projection;
- private sync helpers are not callable by API roles.

## Production migration

Primary production migration:

`20260831012010 p12_wu3_my_journey_memory_projection`

Initial post-migration Performance Advisor correctly identified two new foreign keys without leading indexes. WU3 remained open until corrected.

Follow-up PR: `#22` — **P12-WU3 add Memory foreign-key indexes**

Final product main SHA after follow-up:
`e32fedfda6bcba0773f7e21bccd0409aadb59215`

Follow-up PR CI `#113`: PASS.

Follow-up production migration:

`20260831012223 p12_wu3_memory_fk_indexes`

Added:

- `community_journey_memories_participant_idx(participant_id)`;
- `community_journey_memories_journey_idx(journey_id)`.

After this migration, the two WU3 unindexed-foreign-key Advisor findings disappeared. The indexes may currently appear as unused because production has zero Memory projection rows; that is expected before Community activation.

## Production postflight

After WU3 production migrations:

- community Journey Memory rows: `0`;
- Memory-eligible rows: `0`;
- community participant links: `0`;
- pilot Journey status: `registration_open`;
- pilot capacity: `30`;
- pilot confirmed participant rows: `1`;
- pilot confirmed people: `1`;
- pilot attendance-resolved rows: `0`;
- anon Memory SELECT privilege: `false`;
- authenticated Memory INSERT privilege: `false`;
- authenticated Memory UPDATE privilege: `false`;
- authenticated Memory DELETE privilege: `false`;
- authenticated direct private-sync EXECUTE: `false`;
- `pg_graphql`: OFF.

Therefore no production attendance, Memory, identity or impact fact was fabricated.

## Advisors

Security Advisor after WU3 shows no new WU3 security finding. The remaining security warning is the pre-existing:

- Leaked Password Protection Disabled.

Reference:
https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

Performance Advisor still contains pre-existing project warnings, including older unindexed foreign keys, RLS init-plan observations, unused indexes and the existing multiple-permissive-policy warning. WU3 does not claim those unrelated warnings are resolved.

Reference for unindexed FK lint:
https://supabase.com/docs/guides/database/database-linter?lint=0001_unindexed_foreign_keys

## Final declaration

**P12-WU3 — COMPLETE / PASS — UX ACTIVATION INHERITS WU2 EMAIL GATE**

P11-WU6 remains ACTIVE for the real pilot lifecycle. WU3 does not pre-empt the 2026-09-11 event or create attendance facts before field verification.

Next Community OS layer:

**P12-WU4 — Journey-native Community Interaction**

Interaction must remain anchored to real Journey context and verified community identity. It must not become a generic follower/feed/chat social network.
