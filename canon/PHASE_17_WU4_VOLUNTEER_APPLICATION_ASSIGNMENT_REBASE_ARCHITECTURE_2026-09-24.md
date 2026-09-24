# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4 — VOLUNTEER APPLICATION & ASSIGNMENT REBASE

Date: 2026-09-24  
Status: **IN PROGRESS — WU4.0 CURRENT-STATE AUDIT & ARCHITECTURE LOCK COMPLETE / PASS**

## 1. Objective

Rebase the P16 pilot-specific volunteer model onto the canonical Phase 17 Journey Operating Model now that WU3 Event Management Foundation is active in production.

Canonical flow:

`Staffing Need -> Department Preferences -> BTC Review / Waitlist / Approval -> Participant -> Journey Role -> Department / Team Assignment -> Attendance`

Truth boundaries remain explicit:

- application != participant;
- approval != assignment;
- assignment != attendance;
- Journey Role != Department != Team != Task != Skill;
- social activity != operational truth;
- claim must never rewrite role, assignment or attendance.

Public recruitment remains **HOLD / CLOSED** throughout WU4.

## 2. Starting production truth

Product main:

`3ad24711c3fbbc473f821042968fae835dcf5edc`

Production Supabase:

`iwiqprhoohkxvjyxojto`

WU3 production foundation:

- `journey_departments`: active / empty;
- `journey_teams`: active / empty;
- `journey_runbook_items`: active / empty;
- `journey_official_updates`: active / empty;
- non-closed application windows: 0.

Current Journeys:

- 4 standard / closed;
- 1 volunteer_v1 / closed.

Current applications:

- total: 9;
- volunteer_v1: 1;
- submitted: 5;
- reviewing: 0;
- accepted: 0;
- confirmed: 3;
- rejected: 1.

Legacy volunteer pilot truth:

- one `preferred_team = general_support`;
- one `assigned_team = communications`.

Current participants:

- 3;
- all 3 linked by `journey_participants.application_id`;
- all 3 confirmed;
- all 3 attendance unresolved.

No production row is modified by WU4.0.

## 3. Canonical product authority

Phase 17 product canon requires:

### Journey Roles

Exactly three primary Journey roles:

- `btc` — Ban tổ chức;
- `tnv` — Tình nguyện viên;
- `ban_dia` — Bản địa.

Journey Role is Journey-scoped and is not the existing account/CMS role.

Existing `journey_participant_type` values:

- individual;
- family;
- contributor;
- partner_rep

must not be reinterpreted as Journey Role.

### Staffing

Keep separate:

- Journey Role;
- Department / Team;
- Task / Assignment;
- Skill.

Departments are Journey-configurable and must not be globally hardcoded.

### Application

Volunteer application is generated from Journey staffing needs.

Applicant may express Department preferences:

- Preference 1;
- Preference 2;
- optional Preference 3.

Applicant preference is not final assignment.

Final operational assignment belongs to BTC.

Application truth must distinguish:

- Pending;
- Approved;
- Waitlist;
- Declined;
- Assigned;
- Attended / No-show.

Approval and assignment are logically separate.

## 4. Existing P16 model that must be preserved but deprecated as authority

P16 added directly onto `journey_applications`:

- `preferred_team text`;
- `assigned_team text`;
- fixed CHECK vocabulary:
  - media;
  - logistics_cooking;
  - haircutting;
  - activities_entertainment;
  - communications;
  - coordination;
  - general_support;
- assignment audit columns.

The browser/server also hardcodes the same `VOLUNTEER_TEAMS` vocabulary.

This was valid for the pilot but is no longer canonical.

WU4 must **not delete or rewrite** existing pilot values.

Legacy fields become compatibility/history input only.

New P17 application and assignment source must not write them.

## 5. Existing workflow coupling that WU4 must remove

Current P16 database constraint requires:

- volunteer application in `accepted` or `confirmed`;
- therefore `assigned_team IS NOT NULL`.

This incorrectly couples:

`approval -> legacy fixed-team assignment`

and conflicts with canonical:

`approval != assignment`.

WU4 will remove this forward-write requirement while preserving the old column/value.

Current volunteer Admin UI also disables approval/confirmation until `assigned_team` is selected.

That UI coupling must be removed.

## 6. Existing lifecycle coupling that WU4 must rebase

Production submission RLS already uses WU2 authority:

- `lifecycle_phase = 'upcoming'`;
- `application_state = 'open'`.

However inherited workflow functions still check legacy:

`journeys.status = 'registration_open'`

for application progression / participant confirmation.

This is stale lifecycle authority.

WU4 will rebase review/confirmation guards onto WU2 lifecycle authority.

Important:

> Closing or pausing public applications must not strand applications already submitted.

Therefore:

- `application_state` governs new submission only;
- review / waitlist / approval / confirmation may continue while Journey phase permits application workflow progression;
- WU4 must not require application_state OPEN for existing application review.

## 7. Locked WU4 data architecture

### 7.1 Journey Staffing Need

New source table:

`journey_staffing_needs`

Journey-scoped.

One current staffing need per Department.

Foundation fields:

- Journey;
- Department;
- target volunteer count;
- VI / EN requirements;
- open/closed;
- sort order;
- timestamps.

No hard delete for normal Admin flow.

Closing a need preserves operational history.

### 7.2 Department preferences on application

P17 volunteer application stores up to three ranked Department references directly on the application:

- `preferred_department_1_id`;
- `preferred_department_2_id`;
- `preferred_department_3_id`.

Why direct columns instead of a separate preference-child write:

- public registration remains one protected application INSERT;
- P9 registration gate / dedupe / Vault flow remains intact;
- no second anonymous write can leave a partially submitted application;
- rank 1–3 is canonical and bounded;
- same-Journey composite FKs can enforce Department ownership.

Rules:

- Preference 1 required for P17 volunteer contract;
- Preference 2/3 optional;
- no duplicate Department preference;
- preference must belong to same Journey;
- new P17 submissions must reference currently open staffing needs;
- legacy `preferred_team` is preserved but not written by P17 flow.

### 7.3 Waitlist

Add `waitlisted` to the existing application workflow enum.

Canonical presentation mapping:

- submitted / reviewing -> Pending;
- accepted -> Approved;
- waitlisted -> Waitlist;
- rejected -> Declined;
- confirmed -> participation confirmed / participant row exists.

Assignment remains a separate truth and is never encoded by application status.

### 7.4 Participant operational assignment history

New source table:

`journey_participant_assignments`

Append-history model.

Fields:

- participant;
- Journey;
- Journey Role;
- optional Department;
- optional Team;
- assigned by;
- assigned at;
- ended at.

Rules:

- at most one current assignment per participant;
- Team requires Department;
- participant / Department / Team must all belong to same Journey;
- ended assignment rows remain historical truth;
- ordinary Admin UI has no hard-delete operation.

A security-invoker Admin RPC/helper may atomically:

1. end the current assignment;
2. insert the replacement assignment.

No SECURITY DEFINER public assignment API is required.

### 7.5 Journey Role bootstrap for volunteers

When a volunteer application becomes a confirmed participant:

- create/ensure a current operational assignment row with Journey Role = `tnv`;
- Department / Team may still be null;
- BTC may later set the final Department / Team.

Therefore participant access can know the Journey Role before final Department assignment.

WU5 will own the participant-safe projection/workspace.

## 8. Legacy compatibility rule

Existing P16 values:

- `preferred_team`;
- `assigned_team`;
- assignment audit fields

remain stored exactly as historical pilot truth.

WU4 will not guess a Department mapping because WU3 production structure currently contains no Department rows.

No automatic `communications -> Media`, `general_support -> ...`, or other semantic mapping is allowed.

A later Admin compatibility surface may show:

- legacy preference;
- legacy assignment;
- current P17 Department/Team assignment

side-by-side until BTC explicitly maps/reassigns.

## 9. Sensitive-data rule

Keep existing private volunteer safety/identity foundation:

- DOB;
- CCCD/passport type;
- encrypted full number in Vault;
- last4;
- expiry;
- emergency contact;
- health notes;
- explicit consent.

Do not place sensitive values in:

- staffing needs;
- Department/Team tables;
- participant assignment table;
- Community;
- Memory;
- public Journey surfaces.

Full-document reveal hardening from WU3.7 remains authoritative.

## 10. Application form direction

WU4 application UX will become progressive/mobile-first:

1. Journey context / current staffing needs;
2. personal/contact data actually required;
3. ranked Department preferences;
4. operational/safety fields that are actually required;
5. consent / submit.

WU4 does not build a generic form builder.

Conditional skill/experience questions are deferred unless a concrete staffing need requires them.

## 11. Source/runtime sequence

### WU4.0 — Current-State Audit & Architecture Lock
Status: **COMPLETE / PASS**

### WU4.1 — Staffing / Preference / Assignment Schema Source Contract
Status: **NEXT**

Source-only contract. No production DDL.

### WU4.2 — Dynamic Staffing Needs + Volunteer Application Rebase
Status: PLANNED

- Admin staffing needs management;
- public staffing need read;
- ranked Department preference submission;
- remove browser hardcoded-team authority;
- lifecycle authority from WU2.

### WU4.3 — Review / Waitlist / Approval Workflow Rebase
Status: PLANNED

- waitlist;
- approval independent of assignment;
- generic + volunteer Admin semantics.

### WU4.4 — Participant Role + Assignment Operations
Status: PLANNED

- TNV role bootstrap;
- Department / optional Team assignment;
- assignment history;
- current-assignment management.

### WU4.5 — Legacy P16 Compatibility
Status: PLANNED

- preserve legacy values;
- no automatic semantic migration;
- Admin compatibility presentation.

### WU4.6 — Security / Privacy / Mobile Regression QA
Status: PLANNED

- RLS / grants;
- PII;
- lifecycle;
- attendance;
- Memory;
- recruitment HOLD;
- mobile form/Admin.

### WU4.Final — Production Cutover & Canonical Closeout
Status: PLANNED

Production DDL only after exact-head/full inherited gates PASS.

## 12. Explicit non-goals

WU4 does not implement:

- Participant Personal Journey Workspace — WU5;
- Journey Community rebase — WU6;
- Donation / resources — WU7;
- Day-of attendance rewrite — WU8;
- Closeout/Memory expansion — WU9;
- public trust/discovery rebase — WU10;
- recruitment activation — WU11;
- generic HR/ERP;
- generic form builder;
- skill marketplace.

## 13. Production invariants

Throughout WU4 until WU4.Final:

- WU3 Event Management tables remain active;
- WU4 source tables do not exist production;
- application windows remain closed;
- public recruitment remains HOLD / CLOSED;
- attendance truth is untouched;
- Memory/claim/shared-experience truth is untouched.

## 14. WU4.0 decision

**P17-WU4.0 — COMPLETE / PASS.**

Proceed to:

**P17-WU4.1 — STAFFING / PREFERENCE / ASSIGNMENT SCHEMA SOURCE CONTRACT**
