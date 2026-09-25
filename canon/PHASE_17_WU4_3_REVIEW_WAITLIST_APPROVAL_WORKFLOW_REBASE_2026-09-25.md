# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.3 — REVIEW / WAITLIST / APPROVAL WORKFLOW REBASE

Date: 2026-09-25  
Status: **COMPLETE / PASS**

## Objective

Rebase application review semantics so Waitlist is a first-class state, approval is independent from operational assignment, and confirmation creates a participant only from current database approval/lifecycle truth.

## Starting truth

Product main:

`8e497c2e4de625f7c3f21dad22f92f9ed893cdce`

WU4.0 / WU4.1 / WU4.2:

**COMPLETE / PASS**

Production remains pre-WU4 cutover and recruitment remains HOLD / CLOSED.

## Canonical transition graph

Added:

`src/lib/journeys/application-workflow.ts`

Transitions:

- submitted -> reviewing / rejected
- reviewing -> waitlisted / accepted / rejected
- waitlisted -> reviewing / accepted / rejected
- accepted -> confirmed / rejected
- rejected -> terminal
- confirmed -> terminal

Positive progression requires canonical Journey phase:

- upcoming; or
- active.

Rejection remains an Admin terminal decision even when positive progression is no longer allowed.

Existing application review therefore does not depend on the public Application Window remaining open.

## Server-side review authority

`setApplicationStatus(...)` now:

- re-reads current application id / Journey / status;
- re-reads current Journey lifecycle;
- validates the canonical transition graph;
- records reviewer id and reviewed_at;
- updates only if the application still has the expected current status;
- fails on stale concurrent state instead of blindly overwriting it.

The Admin browser is no longer the sole transition authority.

## Waitlist capability

`waitlisted` is now part of the TypeScript application status domain.

However production enum activation remains WU4.Final.

Added:

`applicationWaitlistCapabilityAvailable(journeyId)`

This probes the WU4 structural marker column.

Before WU4.Final:

- Waitlist UI remains hidden;
- existing Review / Approve / Confirm flow remains usable;
- missing P17 column is treated as capability unavailable, not as an empty waitlist.

After WU4.Final:

- the same source automatically enables Waitlist.

## Confirmation hardening

`confirmApplication(...)` no longer trusts the JourneyApplication object supplied by the browser.

Before participant creation it re-reads:

- application id;
- Journey id;
- user id;
- participant type;
- party size;
- current status;
- Journey lifecycle.

Participant creation requires current DB state:

`accepted`

or remains idempotent for already:

`confirmed`

Positive confirmation also requires canonical application workflow phase.

The source explicitly preserves:

**Waitlist != approval**

**Assignment != approval**

**Confirmed != attendance**

No attendance field is written by review/confirmation.

## Generic Admin workflow

Generic application manager now supports:

- Review
- Waitlist
- Approve
- Reject
- Re-review Waitlist
- Approve from Waitlist
- Confirm

Waitlist actions are source-first and hidden until WU4 production capability exists.

## Volunteer Admin workflow

Volunteer workspace now:

- supports Waitlist;
- approval no longer requires `assigned_team`;
- confirmation no longer requires `assigned_team`;
- distinguishes P16 compatibility from P17 operational model.

Legacy fixed-team assignment is now exposed through:

`assignLegacyVolunteerTeam(...)`

The helper hard-blocks:

`volunteer_structure_version = p17-wu4-v1`

from writing to legacy `assigned_team`.

Fixed-team selector is labelled:

`P16 LEGACY`

and:

`Compatibility-only`

P17 application UI explicitly defers Journey Role + Department / Team assignment to WU4.4.

## Pre-DDL Admin compatibility

`listVolunteerApplications(...)` now attempts the P17 application projection first.

If WU4 columns are genuinely absent before WU4.Final:

- it falls back to the historical P16 projection;
- returns `p17StructureAvailable = false`;
- UI does not pretend P17 nulls are real production data.

Permission / other database errors still surface.

## P16 compatibility gate

The inherited P16 volunteer QA was advanced semantically:

- P16 preferred vs assigned distinction remains required;
- current compatibility wording may explicitly say legacy;
- P17 does not rewrite the historical migration contract.

No historical P16 schema invariant was weakened.

## QA

Added:

`scripts/p17-wu4-3-review-waitlist-approval-qa.ts`

Locks:

- waitlisted domain consistency;
- transition graph;
- lifecycle progression authority;
- rejection semantics;
- reviewer attribution;
- optimistic concurrency;
- confirmation DB re-read;
- participant creation only after accepted decision;
- no attendance fabrication;
- generic Waitlist UX;
- pre-Final fail-closed waitlist capability;
- volunteer approval/confirmation independent of legacy assignment;
- P17 hard-block from legacy assigned_team writes;
- fixed-team compatibility-only presentation.

The first WU4.3 QA run failed because the new assertion scanned all of
`admin-queries.ts` and therefore found legitimate attendance code in a separate module section.

The gate was corrected to inspect only the review/confirmation source slice.

No product/security invariant was weakened.

## Exact-head evidence

Branch:

`p17-wu4-3-review-waitlist-approval-rebase`

PR:

`#88 — P17-WU4.3: Review waitlist and approval workflow rebase`

Final PR head:

`83b6390045e1b81500bc80fb45f8c2e8ecd31ba0`

Exact-head:

- generic CI `36081022951`: **SUCCESS**
- dedicated P16-WU10B `36081023049`: **SUCCESS**
- WU4.3 source gate: **PASS**
- WU4.2 source / DB gates: **PASS**
- WU4.1 source / DB gates: **PASS**
- inherited regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## Merge / post-merge

PR #88 squash-merged.

Product main:

`c2b4b2fc0e48e708e39f3d87e16e6c38ff2ebb16`

Post-merge main CI:

`36081155906` -> **SUCCESS**

## Production invariant

No WU4 production DDL was applied.

Verified after merge:

- staffing table: absent
- assignment table: absent
- P17 Department preference columns: absent
- waitlisted enum: absent
- non-closed Application Windows: **0**

Recruitment:

**HOLD / CLOSED**

## Decision

**P17-WU4.3 — COMPLETE / PASS**

Next:

**P17-WU4.4 — PARTICIPANT ROLE + ASSIGNMENT OPERATIONS**
