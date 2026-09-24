# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.5 — ADMIN LIFECYCLE CONTROLS

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE CONTROLS READY; PRODUCTION DDL STILL UNAPPLIED**

## Objective

Separate Journey lifecycle authority from the application window in Admin, while keeping all controls fail-closed before the production lifecycle contract exists.

## Product evidence

Base main:
`1ed29ff7898a7e03b955a7cf5bde1c930c8540c9`

Branch:
`p17-wu2-5-admin-lifecycle-controls`

PR:
`#71 — P17-WU2.5: Admin lifecycle and application-window controls`

Final PR head:
`85edc9602470ec6c1696ba1e68144e84cd48ac5b`

PR generic CI:
- run `35945656693`
- conclusion: **SUCCESS**

Dedicated P16-WU10B Volunteer Pilot Gate:
- run `35945656692`
- conclusion: **SUCCESS**

Squash-merged main:
`4c4691f8de65138d1dad08bfe56e5051ac51a567`

Post-merge main CI:
- run `35945795145`
- conclusion: **SUCCESS**

## Admin authority model

Admin now sees two explicit authorities:

1. **Lifecycle Phase**
   - draft
   - upcoming
   - active
   - closeout_pending
   - memory
   - archived

2. **Application Window**
   - closed
   - open
   - paused

The generic Journey content editor no longer exposes legacy `status` as a content field.

Content CRUD strips:
- lifecycle_phase
- application_state
- legacy status

from ordinary edits.

New Journey creation remains fail-closed at legacy `draft`; the future database contract supplies lifecycle defaults `draft / closed`.

## Pre-DDL capability behavior

Admin queries attempt lifecycle columns first.

If production reports a genuine missing-column error:
- Admin falls back to the legacy Journey projection;
- lifecycle controls are visibly locked;
- application window remains closed;
- no legacy `registration_open` row is interpreted as an open application window.

## Lifecycle transition controls

The Admin workspace:
- exposes only transitions allowed by the canonical lifecycle graph;
- does not infer a transition from dates;
- surfaces a reconciliation warning when an `upcoming` Journey has a past end date;
- requires Closeout Readiness before offering `closeout_pending → memory`;
- keeps database enforcement as the final authority.

## Application Window

Closing/pausing is a direct Admin lifecycle operation.

Opening is not a generic update.

`OPEN` goes through the protected activation server path which:
- verifies current phase is `upcoming`;
- requires current application state `closed` or `paused`;
- preserves the Phase-9 registration negative/positive probes;
- requires the lifecycle-aware activation guard version expected from WU2.Final;
- therefore fails closed before the production migration.

## Runtime readers

Public Journey readers and registration now use:
- lifecycle columns first when available;
- conservative missing-column fallback before DDL.

The protected registration write accepts new applications only when canonical:
`phase = upcoming AND application_state = open`.

## Application / attendance workspaces

Standard and Volunteer workspaces now receive the canonical Journey phase.

Existing application review may progress only in:
- upcoming
- active

Attendance may be recorded/corrected only in:
- active
- closeout_pending
- memory

No date alone creates attendance or lifecycle truth.

## QA

Added:
`scripts/p17-wu2-5-admin-lifecycle-qa.ts`

It verifies:
- lifecycle-first / legacy fallback behavior;
- fail-closed invalid application state;
- content editor separation;
- Admin-only lifecycle UI;
- past-upcoming reconciliation warning;
- Closeout readiness requirement for Memory;
- protected OPEN application path;
- public/registration lifecycle authority;
- application and attendance workflow phases.

Full inherited P9–P16 QA, ephemeral DB QA, build, typecheck and Cloudflare dry-run all PASS.

## Non-scope

WU2.5 did not:
- mutate production Supabase schema/data;
- open any application window;
- reconcile existing production rows;
- deploy Cloudflare production runtime;
- change attendance/Memory/Reflection evidence truth.

Public recruitment remains **HOLD**.

## Decision

**P17-WU2.5 — COMPLETE / PASS.**

Next:
**P17-WU2.6 — VI/EN / MOBILE / REGRESSION QA**
