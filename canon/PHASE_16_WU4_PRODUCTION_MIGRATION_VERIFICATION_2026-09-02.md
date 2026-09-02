# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU4 — PRODUCTION MIGRATION VERIFICATION

Date: 2026-09-02
Status: **PRODUCTION DATABASE FOUNDATION COMPLETE / PASS**
Shared-Experience Graph UX: **OFF**
Worker activation: **NOT PERFORMED**

## Owner approval

Approved production gate:

> APPROVE P16-WU4 DIRECT PRODUCTION MIGRATION — apply 0046 under controlled QA, preserve all Journey truth, keep Shared-Experience Graph UX OFF until production verification PASS.

## Canonical source

Product main SHA used for the production migration:

`51f83eebc4794269e753d0b96f20a02905bf7798`

Canonical migration source:

`db/migrations/0046_p16_wu4_journey_presence_shared_experience_graph.sql`

Source blob SHA:

`cd20d68eae1b6533790c850dde98f8decefde619`

## Production migration ledger

Applied to Supabase project:

`iwiqprhoohkxvjyxojto`

Production migration ledger entry:

- version: `20260902112106`
- name: `0046_p16_wu4_journey_presence_shared_experience_graph`

Apply result: **SUCCESS**

## Pre-migration baseline

Immediately before 0046:

Pilot Journey `19539f36-3ed4-4a22-96b9-c8a9b73c5283`:

- participants: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended rows: 0
- verified attended people: 0

Truth ledger:

- participant links: 1
- Memories: 1
- Reflections: 0
- Reflection publications: 0
- Contributions: 0

Social foundation:

- social identities: 0
- social identity cards: 0
- Journey social presences: 0
- blocks: 0
- reports: 0
- consent events: 0

WU4 tables did not yet exist.

## Post-migration verification

Created and verified:

- `public.journey_relationship_consents`
- `public.journey_shared_experience_edges`

RLS:

- relationship consents: ON
- shared-experience edges: ON

Anonymous privileges:

- consents SELECT: denied
- edges SELECT: denied

Authenticated privileges:

- consents: SELECT / INSERT / UPDATE
- edges: SELECT only
- edges INSERT: denied
- edges UPDATE: denied
- edges DELETE: denied

Production row counts after migration:

- relationship consents: 0
- shared-experience edges: 0
- pilot Journey edges: 0

This is the required truthful result while the 2026-09-11 Journey attendance remains unresolved.

## Truth preservation

Post-migration pilot Journey truth remains exactly:

- participants: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended rows: 0
- verified attended people: 0

Truth ledger remains exactly:

- participant links: 1
- Memories: 1
- Reflections: 0
- Reflection publications: 0
- Contributions: 0

Social foundation remains 0 rows across identities/cards/presences/blocks/reports/consent events.

Therefore migration 0046 did not fabricate attendance, shared experience, Memory, relationship or impact.

## Trigger / function contract

Verified production triggers are enabled for:

- participant attendance/status changes -> shared-experience projection refresh
- participant-link changes -> projection refresh
- new social identity linkage -> projection refresh
- relationship-consent guard
- relationship-consent audit

Function ACL verification confirms:

- evidence refresh function is executable by `service_role`, not ordinary clients
- internal trigger/audit helpers are not client-executable
- authenticated users receive only the two RLS-safe authorization helpers required by the WU4 policies

## Advisor results

### Security Advisor

No WU4-created security warning.

One pre-existing warning remains:

- `auth_leaked_password_protection` — Leaked Password Protection Disabled

Reference:
https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

### Performance Advisor

No new WU4 RLS-initplan or missing-FK-index warning was introduced.

WU4 indexes are currently reported as `unused_index` INFO because the new tables contain zero real rows and the feature is not activated. These indexes are retained for the intended Journey-scoped graph query paths.

Existing older performance advisor findings remain independent.

Reference:
https://supabase.com/docs/guides/database/database-linter

## Release state

Database foundation: **COMPLETE / PASS**

Shared-Experience Graph UX: **OFF**

No Cloudflare Worker activation/deploy was performed as part of this production database gate.

The canonical truth remains:

> Journey Presence is digital consent only.
>
> Shared Experience requires verified positive attendance evidence.
>
> Evidence does not become socially visible without mutual relationship consent.
>
> Social visibility never rewrites operational Journey truth.

## Next gate

P16-WU4 may now be treated as **COMPLETE / PASS** for architecture, source and production database foundation.

Next product work:

**P16-WU5 — INTERACTION V1: JOURNEY QUESTION / REPLY / APPRECIATION**

Any later Shared-Experience Graph UX activation remains a separate release decision and must preserve the evidence + mutual-consent contract above.
