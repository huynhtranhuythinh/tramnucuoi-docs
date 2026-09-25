# TRẠM NỤ CƯỜI — PHASE 17 / P17-WU8.0
# DAY-OF OPERATIONS & ATTENDANCE — CANONICAL GAP AUDIT & ARCHITECTURE FREEZE

Date: 2026-09-25

Status: **COMPLETE / PASS — AUDIT & ARCHITECTURE FREEZE ONLY**

## 1. Purpose

P17-WU8.0 freezes the architecture for Journey day-of operations and attendance.

This WU is audit-only.

It does NOT:
- change production schema;
- record or infer attendance;
- open recruitment;
- change Journey lifecycle;
- create fake participant/evidence/Shared Journey truth;
- activate Community v2 runtime;
- create QR/GPS/NFC/biometric attendance;
- create a generic event-management product.

## 2. Canonical product authority

Authoritative source:
`canon/PRODUCT_REASSESSMENT_JOURNEY_OPERATING_MODEL_CANON_2026-09-22.md`

Locked invariants:

- registration != attendance
- approved participation != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance > 0 = verified attended
- participant claim != attendance
- social activity != evidence
- operational truth != public visibility
- same Journey context != proof of shared physical attendance
- no fake attendance
- no fake shared experience

Canonical Control Center areas include:
- Day-of Operations
- Attendance / Evidence

Mobile priority is explicit:
BTC/TNV should not need a desktop-style dashboard on Journey day.

## 3. Current production attendance truth

Existing canonical source:

`public.journey_participants`

Existing attendance fields:

- `attended_party_size integer null`
- `attendance_recorded_at timestamptz null`
- `attendance_recorded_by uuid null`

Existing constraints preserve:

- unresolved = all three attendance fields NULL;
- resolved = all three attendance fields populated;
- attended_party_size >= 0;
- attended_party_size <= participant party_size.

Existing date authority:

`private.tnc_guard_attendance_event_date()`

Current rule:
attendance cannot become recorded before Journey `start_date` using the Vietnam calendar date.

Production snapshot at WU8.0 audit:

- participant rows = 3
- unresolved attendance rows = 3
- verified no-show rows = 0
- verified attended rows = 0
- attendance-recorded rows = 0

Therefore production currently contains **no verified attendance truth**.

## 4. Existing source that must remain canonical

### 4.1 Attendance snapshot

Decision:

**KEEP `journey_participants.attended_party_size / attendance_recorded_at / attendance_recorded_by` as canonical current attendance truth.**

Do NOT create a second attendance snapshot table.

Reason:
- the semantic contract is already production-proven;
- existing Memory / Shared Journey / contribution logic depends on this participant truth;
- replacing it would create duplicate truth and migration risk.

### 4.2 Participant assignment

Existing P17 Journey Role / Department / Team assignment remains operational authority.

Attendance does not rewrite:
- Journey Role;
- Department;
- Team;
- participant confirmation;
- application status.

### 4.3 Runbook / Official Update / Resource Operations

WU8 day-of UI composes existing Journey-scoped sources:
- runbook;
- official updates;
- participant assignments;
- resources;
- lifecycle;
- attendance.

WU8 does NOT create a duplicate day-of task system.

## 5. Current gaps

The production foundation has attendance fields but lacks a complete P17 day-of operating layer.

Gaps:

1. no dedicated mobile-first attendance workspace;
2. no Journey-scoped BTC field authority contract;
3. current attendance truth is mutable but has no append-only correction/audit history;
4. existing date guard does not express the full P17 lifecycle contract;
5. no day-of projection aggregating "what matters now";
6. no explicit unresolved/no-show/attended reconciliation workspace;
7. no clean handoff from attendance completion into WU9 closeout readiness.

## 6. Architecture freeze — attendance truth

Canonical attendance model remains:

`Participant -> current attendance snapshot`

States:

### UNRESOLVED
`attended_party_size IS NULL`

Meaning:
attendance has not yet been verified.

This is not no-show.

### VERIFIED NO-SHOW
`attended_party_size = 0`

Meaning:
BTC has verified the participant/group did not attend.

### VERIFIED ATTENDED
`attended_party_size > 0`

Meaning:
BTC has verified that number of people from the participant/group actually attended.

Upper bound remains:

`attended_party_size <= party_size`

## 7. Attendance authority freeze

Initial WU8 operational authority:

### May record attendance

1. global Admin; or
2. authenticated user who is a current participant of the same Journey with current Journey Role:
   `btc`

### May NOT create attendance truth merely because they are

- Editor;
- TNV;
- Bản địa;
- ordinary participant;
- applicant;
- donor;
- Community member;
- participant claimant.

This preserves:

**Journey Role != CMS permission**

while allowing genuine Journey BTC to operate from mobile without giving them global Admin authority.

Future delegation to a specific TNV check-in operator requires separate evidence/business need and is not implicit in the TNV role.

## 8. Attendance lifecycle/date authority

Normal attendance recording is allowed only when:

- Journey start date exists;
- Vietnam calendar date is on/after start date;
- Journey lifecycle is `active` or `closeout_pending`;
- caller has WU8 attendance authority.

Interpretation:

### active
Primary day-of recording window.

### closeout_pending
Late verification / reconciliation remains allowed before closeout is finalized.

### memory / archived
Normal operational attendance mutation is closed.

Any later historical correction requires a separately controlled correction path; WU8 must not silently keep ordinary day-of mutation open forever.

### upcoming / draft
Attendance recording forbidden even if a client attempts to bypass UI.

## 9. Attendance audit trail

WU8 should add an append-only attendance event history while keeping the participant columns as canonical current snapshot.

Recommended source:

`journey_participant_attendance_events`

Purpose:
audit/correction history only.

It is NOT a second current-state source.

Each event should capture:

- Journey;
- participant;
- action:
  - `record`
  - `correct`
  - `clear_to_unresolved`
- previous attended count;
- new attended count;
- actor;
- timestamp;
- optional reason/note.

Rules:

- event rows append; they are not casually edited/deleted;
- snapshot update + event insert occur atomically;
- public never reads this ledger directly;
- a correction preserves the prior truth in history;
- clearing to unresolved is explicit and auditable;
- audit event itself does not create public evidence/Impact/Shared Journey truth.

## 10. Protected attendance mutation path

WU8 must stop treating direct client table UPDATE as the preferred operational interface.

Canonical write path should be a narrow RPC that:

1. authenticates the caller;
2. verifies Admin or same-Journey current BTC authority;
3. verifies participant belongs to the same Journey;
4. verifies participant is in an eligible current participant state;
5. enforces lifecycle/date authority;
6. validates count against `party_size`;
7. locks the participant row;
8. updates the canonical attendance snapshot;
9. appends the audit event in the same transaction;
10. returns the new attendance snapshot.

The RPC must not rely on:
- user_metadata;
- client-provided role strings;
- client-provided recorder ID;
- global Editor permission.

## 11. Day-of Operations workspace

WU8 activates the existing Control Center section:

`Ngày diễn ra`

The day-of workspace is a mobile-first composition, not a new generic project-management system.

It should answer:

1. Journey đang ở trạng thái nào?
2. Điều gì đang diễn ra bây giờ?
3. Runbook item tiếp theo là gì?
4. Có official update quan trọng nào?
5. Team/Bộ phận nào cần chú ý?
6. Bao nhiêu participant chưa có attendance truth?
7. Bao nhiêu người đã verified attended?
8. Bao nhiêu verified no-show?
9. Resource nào còn thiếu / chưa phân phối?
10. Việc nào cần BTC xử lý ngay?

## 12. Mobile-first field UX

Day-of screens prioritize actions above decoration.

Recommended mobile order:

1. Journey state + current time/context;
2. critical official update;
3. unresolved attendance;
4. participant quick search;
5. one-tap attendance actions;
6. now/next runbook;
7. team/resource alerts;
8. secondary detail.

Do not require:
- desktop tables;
- multi-column forms;
- deep navigation for each check-in;
- page refresh after every participant action.

## 13. Attendance UX

For each confirmed participant/group:

Display:
- name;
- party size;
- Journey Role / Team where available;
- attendance state.

Primary actions:

### unresolved
- `Có mặt` defaulting to party_size;
- `Chỉnh số người có mặt`;
- `Vắng mặt` -> 0.

### verified attended / no-show
- show current verified truth;
- correction requires an explicit correction action;
- correction may require a reason when changing an already-resolved state.

Avoid ambiguous labels such as merely "Check-in" if that could be mistaken for self-service presence.

## 14. Self check-in boundary

Initial WU8 does NOT allow participant self-check-in to become attendance truth.

A participant phone interaction, QR scan, Community activity, location signal, or claim may later become supporting evidence, but cannot independently mutate canonical attendance.

Therefore:

**self signal != verified attendance**

## 15. Evidence boundary

Attendance is operationally verified truth.

It does NOT automatically create:
- public Impact;
- Memory;
- Shared Journey relationship;
- Community publication;
- contribution history.

Downstream systems may consume verified attendance according to their own evidence rules.

WU9 owns closeout/result publication.

## 16. Shared Journey boundary

Two users being:
- confirmed participants;
- in the same team;
- active in the same Community;
- recorded on the same date

does NOT by itself prove shared real-world experience.

Shared Journey evidence remains separately governed.

WU8 attendance may become one necessary input, but WU8 must not manufacture shared-experience edges.

## 17. Closeout handoff

WU8 must expose a clear attendance readiness signal for WU9.

Recommended readiness metrics:

- total confirmed participant rows;
- unresolved attendance rows;
- verified no-show rows;
- verified attended rows;
- expected party-size people;
- verified attended people.

Canonical closeout expectation:

> unresolved attendance must be explicitly visible and cannot be silently treated as zero.

WU9 decides whether unresolved attendance blocks final closeout/Memory transition.

WU8 supplies truthful metrics; it does not silently transition lifecycle.

## 18. Security architecture

Implementation requirements:

- existing participant RLS remains enabled;
- no public direct attendance write;
- protected mutation RPC is narrow and Journey-scoped;
- actor identity is always server-derived;
- same-Journey checks are database-enforced;
- Admin/BTC authority is database-enforced;
- audit history is append-only through the attendance write path;
- no service_role in browser;
- no authorization from user_metadata;
- privileged helpers live in `private`, with explicit EXECUTE grants;
- public schema wrappers should prefer SECURITY INVOKER unless a reviewed privileged boundary is required;
- run Supabase Advisors after production cutover.

Current Supabase documentation was rechecked for RLS requirements:
public-schema tables require RLS and explicit grants, UPDATE requires appropriate SELECT policy, and `TO authenticated` alone is not sufficient authorization.

## 19. No-go scope

WU8 MUST NOT add:

- QR-code attendance as canonical truth;
- GPS geofencing;
- live location tracking;
- NFC;
- facial recognition;
- biometrics;
- device fingerprinting;
- generic shift management;
- payroll/timekeeping;
- warehouse workflows;
- generic incident-management platform;
- self-attendance as final authority;
- automatic attendance from Community activity;
- automatic Shared Journey edges;
- automatic Impact claims;
- automatic lifecycle transition based only on date.

## 20. Production invariants until WU8.Final

Until P17-WU8.Final passes:

- recruitment remains HOLD / CLOSED;
- existing attendance truth remains unchanged;
- current 3 unresolved production participant rows are not auto-filled;
- no fake attendance;
- no fake no-show;
- no fake evidence;
- no fake Shared Journey edge;
- no Community v2 activation as a side effect;
- no automatic Memory transition;
- no Impact mutation.

## 21. Recommended implementation sequence

### P17-WU8.1 — ATTENDANCE AUTHORITY & AUDIT CONTRACT
Define Journey BTC/Admin authorization, lifecycle/date gate, atomic attendance RPC and append-only audit events.

### P17-WU8.2 — DAY-OF OPERATIONS PROJECTION
Create a narrow Journey-scoped projection for now/next runbook, key updates, assignment/resource signals and attendance metrics.

### P17-WU8.3 — MOBILE ATTENDANCE WORKSPACE
Activate fast participant search and unresolved/attended/no-show actions optimized for mobile field use.

### P17-WU8.4 — DAY-OF CONTROL CENTER COMPOSITION
Activate `Ngày diễn ra` using existing Runbook, Official Updates, Team, Resource and Attendance sources.

### P17-WU8.5 — ATTENDANCE RESOLUTION & CLOSEOUT READINESS
Expose unresolved/no-show/attended metrics and handoff contract for WU9 without auto-transitioning lifecycle.

### P17-WU8.6 — SECURITY / PRIVACY / MOBILE REGRESSION QA
Verify RLS, scoped BTC authority, actor privacy, correction audit, mobile interaction and legacy attendance compatibility.

### P17-WU8.Final — PRODUCTION CUTOVER & CANONICAL CLOSEOUT
Production migration only after exact-head CI / ephemeral DB QA / rollback QA PASS.

## 22. Final decision

Architecture is frozen as:

**Existing `journey_participants` attendance columns remain canonical current attendance truth.**

WU8 adds:

**scoped Admin/BTC mutation authority + append-only attendance audit history + mobile-first Day-of Operations composition.**

It does NOT create a second attendance truth system.

# FINAL STATUS

**P17-WU8.0 — COMPLETE / PASS**

Next:

**P17-WU8.1 — ATTENDANCE AUTHORITY & AUDIT CONTRACT**
