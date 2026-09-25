# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU8.Final
# DAY-OF OPERATIONS & ATTENDANCE — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-25

Status: **COMPLETE / CLOSED / PASS**

## 1. Scope

P17-WU8 closes the Journey-scoped Day-of Operations & Attendance foundation.

Canonical current attendance truth remains on:

`public.journey_participants`

- `attended_party_size IS NULL` = unresolved
- `attended_party_size = 0` = verified no-show
- `attended_party_size > 0` = verified attended

WU8 does not create a second attendance snapshot.

WU8 adds:
- scoped Admin/current-Journey-BTC attendance authority;
- append-only attendance audit history;
- protected atomic attendance RPC;
- lifecycle/date authority;
- mobile-first Day-of workspace;
- attendance closeout-readiness projection;
- production database cutover.

## 2. WU8.0 architecture

Canonical audit/freeze:

`canon/PHASE_17_WU8_0_DAY_OF_OPERATIONS_ATTENDANCE_GAP_AUDIT_ARCHITECTURE_FREEZE_2026-09-25.md`

Locked boundaries:
- registration != attendance;
- approved participation != attendance;
- attendance NULL != no-show;
- participant self signal != verified attendance;
- social activity != attendance evidence;
- attendance does not automatically create Memory, Shared Journey, Contribution or Impact truth;
- no QR/GPS/NFC/biometric attendance authority;
- no fake attendance.

## 3. P17-WU8.1–WU8.6 source implementation

PR:

`#121 — P17-WU8.1-8.6: Day-of operations and attendance`

Final exact head:

`22441948de14b5629cfd43760b625cdf0d74d09e`

Exact-head gates:
- CI `36161865316` — **SUCCESS**
- inherited Volunteer Pilot Gate `36161865165` — **SUCCESS**

Squash-merged main:

`b52122c8c516f084290b43d8ae364899fcc4bec5`

Post-merge main CI:

`36162055679` — **SUCCESS**

### Source contracts

- `database/contracts/p17_wu8_1_attendance_authority_audit.sql`
- `database/contracts/p17_wu8_2_day_of_projection.sql`

### Source / DB QA

- `scripts/p17-wu8-source-mobile-privacy-qa.ts`
- `scripts/p17-wu8-attendance-day-of-db-qa.sql`

### UI / client

- `src/lib/journeys/day-of.ts`
- `src/components/journeys/journey-day-of-workspace.tsx`
- Journey Control Center composition
- Journey BTC personal-workspace composition

## 4. Attendance authority

Attendance mutation authority is:

1. global Admin; or
2. authenticated current participant of the same Journey whose current Journey Role is `btc`.

The authority helper derives Journey BTC status from:
- verified user ↔ participant link;
- confirmed participant;
- current P17 Journey assignment;
- same Journey;
- current role `btc`.

Global Editor is not attendance authority.

TNV / Bản địa / ordinary participant / applicant / donor / Community member do not automatically receive attendance authority.

Actor identity is server-derived.

## 5. Attendance lifecycle/date authority

Normal attendance mutation requires:
- Journey `start_date`;
- Vietnam calendar date on/after `start_date`;
- Journey lifecycle in:
  - `active`; or
  - `closeout_pending`;
- valid Admin/current-BTC authority.

Therefore normal day-of mutation is closed in:
- draft;
- upcoming;
- memory;
- archived.

Date alone does not automatically transition lifecycle.

## 6. Protected attendance mutation

Public RPC:

`public.tnc_set_journey_participant_attendance(uuid, integer)`

Public wrapper is SECURITY INVOKER.

Private implementation:
`private.tnc_set_journey_participant_attendance(uuid, integer)`

The mutation:
- authenticates caller;
- locks confirmed participant row;
- verifies same-Journey authority;
- validates count `0..party_size`;
- updates existing canonical snapshot;
- uses server-derived timestamp/actor;
- atomically triggers attendance audit history.

No direct self-check-in becomes attendance truth.

## 7. Append-only audit history

New production table:

`public.journey_participant_attendance_events`

Audit actions:
- `record`
- `correct`
- `clear_to_unresolved`

Each event captures:
- Journey;
- participant;
- previous attended count;
- new attended count;
- actor;
- timestamp.

The audit table is not a second current-state source.

RLS:
**ON**

Direct privileges verified:
- anon SELECT = false
- authenticated SELECT = true, then RLS limits rows to Admin policy
- authenticated INSERT = false

Audit rows are appended through the reviewed trigger path rather than client INSERT.

The private audit trigger function is SECURITY DEFINER only so it can append to the protected audit ledger; it contains explicit authentication + attendance-authority checks, is in the private schema and has no public direct API role.

## 8. Day-of workspace

Public wrapper:

`public.tnc_journey_day_of_workspace(uuid)`

Authority:
Admin or current same-Journey BTC.

The projection includes only operational fields required on Journey day:
- Journey identity/lifecycle;
- attendance metrics;
- confirmed participant name + party size + current attendance state;
- Journey Role / Department / Team;
- unfinished now/next Runbook items;
- effective Official Updates.

It intentionally excludes:
- health notes;
- emergency-contact details;
- identity-document vault data;
- unrelated application secrets.

The mobile UI exposes:
- unresolved;
- verified present;
- verified no-show;
- custom party-size attendance;
- explicit correction back to unresolved;
- now/next runbook;
- Official Updates.

## 9. Closeout-readiness handoff

RPC:

`public.tnc_journey_attendance_readiness(uuid)`

Metrics:
- confirmed rows;
- expected people;
- unresolved rows;
- unresolved people;
- no-show rows;
- attended rows;
- attended people;
- attendance readiness boolean.

The boolean is true only when unresolved attendance rows are zero.

WU8 does NOT automatically transition Journey lifecycle.

WU9 owns final closeout/result/Memory policy.

## 10. P17-WU8.Final source evidence

PR:

`#122 — P17-WU8.Final: Day-of Operations Attendance production cutover`

Final exact head:

`50d0998828b16e1f939cebaa71388f2aeba5872a`

Exact-head gates:
- CI `36162539376` — **SUCCESS**
- inherited Volunteer Pilot Gate `36162539442` — **SUCCESS**

Squash-merged production main:

`f755fd0f4adfd4712e015a306e7f2240a5e4cae0`

Post-merge main CI:

`36162737197` — **SUCCESS**

Post-merge CI passes:
- WU8 source/mobile/privacy QA;
- WU8 attendance/day-of DB QA;
- WU8.Final production source QA;
- WU8.Final migration + rollback DB QA;
- inherited P9–P17 regressions;
- build;
- typecheck;
- Cloudflare dry-run.

## 11. Production migration

Source:

`database/migrations/0063_p17_wu8_final_day_of_attendance.sql`

Git blob SHA:

`9bdc993076bb46c4412ab23869dfaad0ab3b9ddf`

Guarded rollback:

`database/rollbacks/p17_wu8_final_day_of_attendance.sql`

Supabase project:

`iwiqprhoohkxvjyxojto`

Production migration ledger:

`20260925164710_p17_wu8_final_day_of_attendance`

Apply result:

**SUCCESS**

## 12. Production preflight truth

Immediately before migration 0063:

- attendance audit table absent;
- participant rows = 3;
- unresolved attendance rows = 3;
- verified no-show rows = 0;
- verified attended rows = 0;
- attendance-recorded rows = 0;
- non-closed Application Windows = 0.

Thus production contained no verified attendance before WU8 cutover.

## 13. Production post-cutover truth

Immediately after migration 0063:

- attendance audit table exists;
- attendance audit rows = 0;
- participant rows = 3;
- unresolved attendance rows = 3;
- verified no-show rows = 0;
- verified attended rows = 0;
- attendance-recorded rows = 0;
- non-closed Application Windows = 0.

Existing downstream truth also remains:

- community contribution rows = 0;
- Journey Impact rows = 4;
- Shared Journey experience edges = 0.

Therefore WU8 cutover manufactured no:
- attendance;
- no-show;
- Shared Journey evidence;
- Contribution;
- Impact;
- recruitment opening.

Recruitment remains:

**HOLD / CLOSED**

## 14. RLS / function verification

Production verification:

Attendance audit table:
- RLS = ON

Expected WU8 audit indexes:
- `journey_participant_attendance_events_journey_participant_idx`
- `journey_participant_attendance_events_actor_idx`
- `journey_participant_attendance_events_participant_fk_idx`

All three are present.

Public WU8 wrappers are SECURITY INVOKER:
- `tnc_set_journey_participant_attendance`
- `tnc_journey_day_of_workspace`
- `tnc_journey_attendance_readiness`

Private privileged helpers are kept in `private` with explicit authorization predicates.

## 15. Guarded rollback

Rollback refuses to destroy WU8 attendance history when audit-event rows exist.

If the WU8 audit ledger is empty, rollback:
- removes WU8 RPCs/projections/audit source;
- restores the pre-WU8 P14 attendance date-authority function/trigger;
- leaves existing participant attendance columns intact.

This protects real attendance history from accidental destructive rollback.

## 16. Supabase Advisors

### Security Advisor

No WU8-specific security finding is reported.

Historical project warning remains:

`Leaked Password Protection Disabled`

This is unrelated to WU8 and was not silently modified.

### Performance Advisor

WU8 attendance audit source does not appear among the current unindexed foreign-key findings.

The new covering indexes are present.

Historical unrelated performance findings remain outside WU8 scope.

## 17. Canonical invariants preserved

- Registration does not imply attendance.
- Confirmation does not imply attendance.
- NULL remains unresolved.
- Zero remains verified no-show.
- Positive count remains verified attended.
- Participant claim does not rewrite attendance.
- BTC authority is Journey-scoped, not global CMS permission.
- Attendance is not automatically public.
- Attendance does not automatically create Memory.
- Attendance does not automatically create Shared Journey edges.
- Attendance does not automatically create Impact.
- No fake attendance.
- Recruitment remains HOLD/CLOSED.

# FINAL STATUS

**P17-WU8 — COMPLETE / CLOSED / PASS**

Production capability:

**Day-of Operations & Attendance foundation ACTIVE, with existing attendance truth preserved, scoped Admin/Journey-BTC verification, append-only audit history, mobile Day-of workspace, and zero manufactured attendance.**

Next canonical roadmap item:

**P17-WU9 — CLOSEOUT, RESULTS & MEMORY**
