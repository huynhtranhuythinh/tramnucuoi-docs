# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.1 — OPERATIONAL SCHEMA SOURCE CONTRACT

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE + EPHEMERAL DB CONTRACT; NO PRODUCTION DDL**

## Objective

Define and verify the additive database foundation for the Journey Control Center without touching production and without leaking later WU scope into Event Management Foundation.

## Product evidence

Base product main:

`2c37673680450c12742512945f870e2bece6e653`

Branch:

`p17-wu3-1-operational-schema-contract`

PR:

`#78 — P17-WU3.1: Event Management operational schema contract`

Final PR head:

`7076e578bad28cc6b0c671494a6a001f2469c9cb`

Exact-head verification:

- generic CI `35978076405`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35978076402`: **SUCCESS**

Squash-merged main:

`a3d77cfd8dcefff1c6c27d8bfacd006875259bd8`

Post-merge main CI:

- run `35978305151`
- exact head `a3d77cfd8dcefff1c6c27d8bfacd006875259bd8`
- conclusion: **SUCCESS**

## Schema contract

Source-only contract:

`database/contracts/p17_wu3_1_event_management_foundation.sql`

This is **not a production migration**.

It defines four Journey-scoped operational primitives:

1. `journey_departments`
2. `journey_teams`
3. `journey_runbook_items`
4. `journey_official_updates`

## Department / Team integrity

Department belongs to one Journey.

Team belongs to one Department in the same Journey.

No global Department/Team enum is introduced.

Historical operational structure is protected:
- referenced Department/Team rows are deletion-restricted;
- use `is_active=false` for deactivation;
- old P16 hard-coded `preferred_team/assigned_team` values are untouched.

WU4 owns application/participant assignment compatibility.

## Runbook

Minimal states:

- `todo`
- `doing`
- `done`

Minimal kinds:

- `task`
- `schedule`

Runbook may scope to Department/Team, with composite FKs enforcing same-Journey hierarchy.

No generic PM/ERP mechanics were introduced.

## Official Update

Official Update is an operational object, not Community content.

Reserved audiences:

- all participants
- BTC
- TNV
- Bản địa
- Department
- Team

Audience targeting is structurally validated.

A `published` Official Update requires `published_at`.

Participant-facing delivery is intentionally deferred to WU5.

## Security contract

For every WU3 foundation table:

- explicit Data API grants;
- RLS enabled;
- `anon` has no direct table grant;
- `authenticated` receives required DML table privileges;
- RLS authority at WU3 foundation stage is **Admin-only**;
- `service_role` explicitly granted;
- no new SECURITY DEFINER function.

Global `editor` is not silently converted into Journey-scoped BTC authority.

## Scope invariants

WU3.1 does not mutate:

- `journey_applications`;
- `journey_participants`;
- attendance fields;
- Memory;
- Reflection;
- Impact;
- recruitment/application state.

It does not create:

- staffing needs;
- participant assignments;
- Resource/Donation operations.

Those remain later WUs.

## QA

Added:

- `scripts/p17-wu3-1-event-management-contract-qa.ts`
- `scripts/p17-wu3-1-event-management-schema-qa.sql`

Ephemeral PostgreSQL QA proves:

- four foundation tables exist;
- RLS is enabled;
- explicit grants are correct;
- Editor cannot see/create operational rows;
- Admin can create operational structure;
- cross-Journey Department/Team references fail;
- cross-hierarchy Runbook references fail;
- invalid Official Update targeting fails;
- published-without-timestamp fails;
- referenced operational structure cannot be deleted;
- soft deactivation works;
- later-WU tables are absent.

One initial CI run produced a false positive because a static non-scope guard matched the word “Donation” in a comment. The guard was corrected to detect actual schema identifiers. The corrected exact head and post-merge main passed every inherited gate.

## Production boundary

WU3.1 made:

- **no production Supabase mutation**;
- **no recruitment activation**;
- **no Cloudflare deploy**;
- **no feature-flag mutation**.

Public recruitment remains:

**HOLD / CLOSED**

## Decision

**P17-WU3.1 — COMPLETE / PASS.**

Next:

**P17-WU3.2 — JOURNEY CONTROL CENTER ROUTE & OVERVIEW**
