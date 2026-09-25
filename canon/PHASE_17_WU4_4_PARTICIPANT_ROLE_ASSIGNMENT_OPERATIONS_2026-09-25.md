# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.4 — PARTICIPANT ROLE + ASSIGNMENT OPERATIONS

Date: 2026-09-25  
Status: **COMPLETE / PASS**

## 1. Objective

Implement the source-first operational assignment layer for confirmed Journey participants without collapsing application, participant, assignment and attendance truth.

WU4.4 owns:

- canonical Journey Role operations;
- current participant assignment;
- assignment history;
- Department / optional Team placement;
- volunteer TNV-role bootstrap at Confirm;
- Admin-only assignment management in Journey Control Center;
- security and regression evidence for those operations.

WU4.4 does not deploy WU4 production DDL. WU4.Final remains the production cutover gate.

## 2. Canonical truth boundaries

WU4.4 preserves:

- application != participant;
- approval != assignment;
- participant != assignment;
- assignment != attendance;
- Journey Role != Department != Team != Task != Skill;
- account Editor != Journey-scoped BTC authority.

Canonical Journey Roles remain exactly:

- `btc` — Ban tổ chức;
- `tnv` — Tình nguyện viên;
- `ban_dia` — Bản địa.

Legacy P16:

- `preferred_team`;
- `assigned_team`

remain compatibility history only and are not interpreted as P17 Department/Team assignment truth.

## 3. Source contract

Added:

`database/contracts/p17_wu4_4_participant_assignment_operations.sql`

The contract provides atomic Admin-authorized operations through a non-exposed privileged implementation plus stable public Data API wrappers.

### Set current assignment

Private implementation:

`private.tnc_set_journey_participant_assignment(...)`

Public wrapper:

`public.tnc_set_journey_participant_assignment(...)`

Behavior:

- participant must be CONFIRMED;
- Journey Role must be one of `btc / tnv / ban_dia`;
- Department, when supplied, must be active and belong to the participant Journey;
- Team, when supplied, requires Department;
- Team must be active and belong to the same Journey and selected Department;
- current row is ended;
- a new assignment row is appended;
- historical rows remain intact.

There is no assignment hard-delete path.

### End current assignment

Private/public pair:

`tnc_end_journey_participant_assignment(...)`

Behavior:

- Admin only;
- ends the one current assignment row when present;
- keeps history.

### Volunteer Role bootstrap

Private implementation:

`private.tnc_ensure_volunteer_participant_role(uuid)`

Public wrapper:

`public.tnc_ensure_volunteer_participant_role(uuid)`

Behavior:

- applies only to a `volunteer_v1` application;
- application must be accepted/confirmed;
- participant must already be CONFIRMED;
- if a current assignment exists, returns it unchanged;
- otherwise creates only:
  - Journey Role = `tnv`;
  - Department = NULL;
  - Team = NULL;
  - no inferred assignment note.

The bootstrap is idempotent and never overwrites a later BTC/Admin assignment decision.

## 4. Security architecture

Privileged functions:

- live in `private`;
- use `SECURITY DEFINER`;
- set empty `search_path`;
- self-authorize authenticated Admin via `private.has_role('admin')`.

Public Data API functions:

- use `SECURITY INVOKER`;
- preserve stable RPC names;
- do not themselves carry privileged execution.

WU4.4 does not add broad participant-table grants merely to support Admin assignment UI.

## 5. Source-first capability behavior

Added/extended:

`src/lib/journeys/participant-assignments.ts`

Operations include:

- `loadParticipantAssignmentWorkspace(...)`;
- `setParticipantAssignment(...)`;
- `endParticipantAssignment(...)`;
- `ensureVolunteerParticipantRole(...)`.

Before WU4.Final:

- missing WU4 table/function capability is represented explicitly;
- assignment workspace returns unavailable rather than fake empty operational truth;
- volunteer Confirm retains inherited participant behavior when the WU4 marker/schema is genuinely absent.

After WU4 schema exists:

- a missing required bootstrap RPC is treated as a real regression;
- the flow fails closed rather than silently creating a confirmed volunteer without Journey Role.

## 6. Confirm workflow integration

Updated:

`src/lib/journeys/admin-queries.ts`

The Confirm flow now re-reads:

`application_mode`

from current database application truth.

For a `volunteer_v1` application:

1. confirmed participant identity is created/reused through the inherited flow;
2. WU4.4 invokes idempotent TNV-role bootstrap;
3. final application status normalization proceeds.

The bootstrap:

- does not guess Department/Team;
- does not write attendance;
- does not reinterpret legacy P16 team strings;
- does not overwrite an existing current assignment.

## 7. Admin assignment workspace

Added:

`src/components/admin/journeys/journey-participant-assignment-manager.tsx`

Integrated in:

`src/components/admin/journeys/journey-control-center.tsx`

Admin workspace supports:

- confirmed participants only;
- Journey Role selection;
- optional Department;
- optional Team filtered by selected Department;
- assignment note;
- atomic append-history reassignment;
- end current assignment;
- visible historical assignment rows.

The UI explicitly communicates:

> Journey Role ≠ Bộ phận ≠ Nhóm ≠ Attendance

Editor receives no assignment management authority.

Before WU4.Final, the UI reports:

> Production capability chưa được kích hoạt.

It does not display missing production tables as legitimate empty data.

## 8. Attendance boundary

WU4.4 never mutates:

- `attended_party_size`;
- `attendance_recorded_at`;
- `attendance_recorded_by`.

The dedicated DB gate verifies attendance source truth independently from assignment authority.

No attendance privilege was broadened merely to simplify the assignment test or Admin workspace.

## 9. QA

Added:

`scripts/p17-wu4-4-participant-assignment-qa.ts`

Source gate locks:

- private privileged implementation + public SECURITY INVOKER wrappers;
- Admin self-authorization;
- confirmed-participant requirement;
- exact Journey Role vocabulary;
- active/same-Journey Department/Team validation;
- idempotent volunteer bootstrap;
- no bootstrap overwrite;
- no assignment hard delete;
- no attendance mutation;
- no legacy P16 team reinterpretation;
- capability-aware pre-cutover behavior;
- Control Center integration;
- Admin-only authority.

Added:

`scripts/p17-wu4-4-participant-assignment-schema-qa.sql`

Ephemeral DB gate proves:

- existing current assignment survives volunteer bootstrap unchanged;
- reassignment ends previous current row and appends history;
- exactly one current assignment remains;
- invalid Journey Role is rejected;
- cross-Journey Department is rejected;
- ending an assignment leaves no current row;
- bootstrap creates a TNV role-only current row;
- repeated bootstrap is idempotent;
- history remains;
- Editor/non-admin mutation is denied;
- attendance remains untouched.

Inherited WU4.3 source QA was advanced to require the new `application_mode` projection rather than preserving a stale exact string.

## 10. QA investigation evidence

Several CI failures were investigated rather than bypassed:

1. inherited WU4.3 exact projection assertion was stale after WU4.4 legitimately added `application_mode`;
2. early DB fixture assertion hardcoded assignment details instead of proving row preservation;
3. attendance assertion initially ran under an intentionally restricted authenticated role;
4. the moved attendance DO block suffered a JavaScript replacement-string `$$` collapse and was rewritten using literal line editing.

No production/security invariant was weakened to obtain PASS.

## 11. Exact-head evidence

Branch:

`p17-wu4-4-participant-role-assignment-operations`

PR:

`#89 — P17-WU4.4: Participant Role and assignment operations`

Final PR head:

`ca4541fd17c8d5f26e87d285c2a9f23acf70bac4`

Exact-head evidence:

- generic CI `36085608835`: **SUCCESS**
- dedicated P16-WU10B gate `36085608881`: **SUCCESS**
- WU4.4 source QA: **PASS**
- WU4.4 ephemeral DB QA: **PASS**
- WU4.1 schema contract QA: **PASS**
- WU4.2 public staffing projection QA: **PASS**
- WU4.3 inherited workflow QA: **PASS**
- inherited P9–P17 source/DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 12. Merge / post-merge evidence

PR #89 was squash-merged.

Product main:

`85eb570a015b300212f2d586006309d79c6e73a9`

Post-merge main CI:

- run `36085735594`;
- exact head `85eb570a015b300212f2d586006309d79c6e73a9`;
- conclusion: **SUCCESS**.

## 13. Production invariants

Production was verified immediately before merge:

- `journey_staffing_needs`: absent;
- `journey_participant_assignments`: absent;
- WU4 assignment RPC: absent;
- WU4 volunteer bootstrap RPC: absent;
- `volunteer_structure_version` marker: absent;
- non-closed application windows: **0**.

Therefore:

- no WU4 production DDL was applied;
- recruitment remains **HOLD / CLOSED**;
- WU4.Final remains the production cutover authority.

## 14. Decision

**P17-WU4.4 — COMPLETE / PASS.**

Next canonical work unit:

**P17-WU4.5 — LEGACY P16 COMPATIBILITY**

WU4.5 must preserve historical P16 values without automatic semantic migration into P17 Department / Team / Journey Role truth.
