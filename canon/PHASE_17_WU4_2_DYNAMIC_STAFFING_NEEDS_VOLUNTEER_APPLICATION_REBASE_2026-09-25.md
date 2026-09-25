# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.2 — DYNAMIC STAFFING NEEDS + VOLUNTEER APPLICATION REBASE

Date: 2026-09-25  
Status: **COMPLETE / PASS**

## Objective

Replace the P16 fixed volunteer-team authority on the new application path with dynamic Journey-scoped Department staffing needs and ranked Department preferences, while keeping WU4 source-first until WU4.Final.

## Starting truth

Product main at start:

`0a967b091d8214d34d0313df3756794eda63acdf`

WU4.0 / WU4.1:

**COMPLETE / PASS**

Production before and after WU4.2 remains pre-WU4-cutover:

- `journey_staffing_needs`: absent
- `journey_participant_assignments`: absent
- P17 Department preference columns on applications: absent
- public staffing projection RPC: absent
- `waitlisted` application enum: absent
- non-closed application windows: **0**
- recruitment: **HOLD / CLOSED**

## Admin staffing needs

Added:

`src/lib/journeys/staffing.ts`

`src/components/admin/journeys/journey-staffing-needs-manager.tsx`

Foundation behavior:

- Journey-scoped;
- Admin-only;
- one current staffing need per Department;
- target volunteer count;
- VI / EN requirements;
- sort order;
- create/edit;
- close/reopen instead of normal hard delete;
- active same-Journey Department required;
- genuine missing WU4 relation becomes explicit capability-unavailable state;
- Editor is not inferred as Journey BTC authority.

Control Center integration is limited to volunteer journeys.

## Public-safe staffing projection

Added source-only contract:

`database/contracts/p17_wu4_2_public_staffing_projection.sql`

Architecture:

- privileged implementation:
  `private.tnc_public_staffing_options(uuid)`
- public Data API wrapper:
  `public.tnc_public_staffing_options(uuid)`

Rules:

- private helper uses SECURITY DEFINER, fixed empty search_path;
- public wrapper uses SECURITY INVOKER;
- no anonymous SELECT is granted on `journey_departments`;
- projection returns only public-safe staffing data:
  - staffing need id;
  - Journey id;
  - Department id;
  - VI / EN Department label;
  - target volunteers;
  - VI / EN staffing requirements;
  - sort order;
- result requires:
  - staffing need open;
  - Department active;
  - canonical Journey lifecycle/application authority accepts public staffing.

This avoids exposing operational Department rows merely to render volunteer application choices.

## Volunteer application rebase

New forward path no longer treats:

`VOLUNTEER_TEAMS`

as browser/server application authority.

Application form now supports ranked Department preferences:

1. preferred Department 1 — required
2. preferred Department 2 — optional
3. preferred Department 3 — optional

Preferences must be distinct.

The UI explicitly states:

**preference != final assignment**

The server re-reads current public staffing options and validates every submitted Department id before write.

## WU2 lifecycle authority

Volunteer submission no longer relies on:

`journey.status === registration_open`

The trusted server path now reuses:

- `journeyLifecycleState(...)`
- `acceptsNewApplications(...)`

Therefore new submission authority remains:

- lifecycle = upcoming;
- application window = open.

Missing WU2 lifecycle columns are treated as capability regression, not permission to fall back to legacy status.

## P17 forward write contract

New volunteer writes use:

`volunteer_structure_version = p17-wu4-v1`

and:

- `preferred_department_1_id`
- `preferred_department_2_id`
- `preferred_department_3_id`

Legacy fields are explicitly left null for new P17 writes:

- `preferred_team = null`
- `assigned_team = null`

Legacy P16 constants and persisted values remain available for compatibility/history and are not rewritten.

Replay/dedupe material now includes the P17 structure version and ranked Department preference ids.

## Privacy / safety boundaries

Preserved:

- full identity document remains transient-to-Vault;
- identity / DOB / health data are not added to notification email;
- Department preferences remain operational Admin data;
- application != participant;
- preference != assignment;
- assignment != attendance;
- no attendance truth is manufactured;
- no Memory / Reflection / Community / shared-experience truth is mutated.

## QA

Added:

`scripts/p17-wu4-2-dynamic-staffing-application-qa.ts`

`scripts/p17-wu4-2-public-staffing-projection-schema-qa.sql`

The DB gate proves:

- private helper SECURITY DEFINER;
- public wrapper SECURITY INVOKER;
- anonymous user cannot directly SELECT operational Department source;
- public projection returns only active/open staffing needs;
- inactive Department is hidden;
- closed need is hidden;
- closed Application Window hides all staffing options;
- attendance remains untouched.

Inherited WU3.2 / WU3.4 / WU3.5 / WU3.6 assertions were advanced from stale pre-WU3.Final copy checks to the already-canonical post-WU3.Final production truth. No underlying WU3 product/security invariant was weakened.

## Exact-head evidence

Branch:

`p17-wu4-2-dynamic-staffing-application-rebase`

PR:

`#87 — P17-WU4.2: Dynamic staffing needs and volunteer application rebase`

Final PR head:

`3b9b402ed6605b100312c0ef16b37ea02787c007`

Exact-head:

- generic CI `36080159902`: **SUCCESS**
- dedicated P16-WU10B gate `36080159916`: **SUCCESS**
- WU4.2 source QA: **PASS**
- WU4.2 public staffing projection DB QA: **PASS**
- WU4.1 contract/schema gates: **PASS**
- inherited source and ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## Merge / post-merge

PR #87 squash-merged.

Product main:

`8e497c2e4de625f7c3f21dad22f92f9ed893cdce`

Post-merge main CI:

`36080307053` → **SUCCESS**

## Production invariant after WU4.2

No WU4 DDL was applied.

Verified:

- staffing table absent;
- assignment table absent;
- Department preference application columns absent;
- public staffing RPC absent;
- `waitlisted` enum absent;
- non-closed application windows: **0**.

Recruitment remains:

**HOLD / CLOSED**

## Decision

**P17-WU4.2 — COMPLETE / PASS**

Next:

**P17-WU4.3 — REVIEW / WAITLIST / APPROVAL WORKFLOW REBASE**
