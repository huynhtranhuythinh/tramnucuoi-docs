# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.Final — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — WU4 PRODUCTION FOUNDATION ACTIVE; RECRUITMENT REMAINS HOLD / CLOSED**

## 1. Objective

Cut over the already-PASS P17-WU4 volunteer staffing / application / assignment foundation into production without activating a real recruitment window.

Canonical flow now supported by production schema:

`Staffing Need -> ranked Department preferences -> BTC review / Waitlist / Approval -> Participant -> Journey Role -> Department / optional Team assignment -> Attendance`

Truth boundaries remain locked:

- application != participant;
- approval != assignment;
- assignment != attendance;
- Journey Role != Department != Team != Task != Skill;
- P16 legacy team strings remain historical compatibility only;
- operational truth does not imply public/social truth;
- no application or assignment state manufactures attendance;
- no WU4 operation creates Memory / Reflection / shared-experience evidence.

## 2. Starting truth

Product main before WU4.Final:

`24ca60628ba1289dc523b730d53cd7723c3498b6`

Completed before cutover:

- WU4.0 — Current-State Audit & Architecture Lock;
- WU4.1 — Staffing / Preference / Assignment Schema Source Contract;
- WU4.2 — Dynamic Staffing Needs + Volunteer Application Rebase;
- WU4.3 — Review / Waitlist / Approval Workflow Rebase;
- WU4.4 — Participant Role + Assignment Operations;
- WU4.5 — Legacy P16 Compatibility;
- WU4.6 — Security / Privacy / Mobile Regression QA.

Production preflight immediately before migration confirmed:

- WU3 Department / Team / Runbook / Official Update tables: present;
- WU4 staffing table: absent;
- WU4 participant assignment table: absent;
- P17 application structure columns: absent;
- `waitlisted` enum value: absent;
- public staffing RPC: absent;
- Journeys: 5;
- applications: 9;
- volunteer applications: 1;
- participants: 3;
- legacy `preferred_team` rows: 1;
- legacy `assigned_team` rows: 1;
- attendance resolved: 0;
- non-closed Application Windows: 0.

Therefore production was in the exact pre-cutover state expected by WU4.Final.

## 3. Production migration materialization

Added:

`database/migrations/0058_p17_wu4_final_volunteer_staffing_assignment_cutover.sql`

The migration is materialized directly from the locked source bodies of:

- `database/contracts/p17_wu4_1_staffing_preference_assignment.sql`;
- `database/contracts/p17_wu4_2_public_staffing_projection.sql`;
- `database/contracts/p17_wu4_4_participant_assignment_operations.sql`.

The source gate requires those canonical bodies to be present without semantic drift.

The migration does not:

- update Journey rows;
- set `application_state = open`;
- change Journey lifecycle phase;
- manufacture attendance;
- mutate Memory / Reflection / Community / shared-experience truth;
- seed staffing;
- seed assignment;
- convert legacy P16 team strings.

## 4. Activated production capability

### 4.1 Explicit Waitlist state

Production `journey_application_status` now includes:

`waitlisted`

This is application workflow truth only.

Waitlist remains distinct from:

- approval;
- participant creation;
- assignment;
- attendance.

### 4.2 Journey staffing needs

Production table active:

`public.journey_staffing_needs`

Canonical behavior:

- Journey-scoped;
- Department-based;
- target volunteer count;
- VI / EN requirement text;
- open/closed;
- sort order;
- creator/timestamps;
- same-Journey Department FK;
- RLS active;
- operational Admin mutation authority.

No staffing rows were seeded during cutover.

### 4.3 Ranked Department preferences

Production `journey_applications` now includes:

- `volunteer_structure_version`;
- `preferred_department_1_id`;
- `preferred_department_2_id`;
- `preferred_department_3_id`.

Forward P17 volunteer applications use:

`volunteer_structure_version = 'p17-wu4-v1'`

Preference 1 is required by the P17 contract.

Preferences 2/3 remain optional.

Duplicate Department preferences are blocked.

Each Department preference is constrained to the same Journey.

### 4.4 Approval independent from legacy assignment

The former P16 constraint requiring legacy `assigned_team` before accepted/confirmed was removed from the forward production contract.

Therefore:

> approval / confirmation no longer requires a P16 fixed-team assignment.

Final P17 Department/Team assignment remains a separate operational authority.

### 4.5 Participant assignment history

Production table active:

`public.journey_participant_assignments`

Canonical assignment model:

- Journey Role:
  - `btc`;
  - `tnv`;
  - `ban_dia`;
- optional Department;
- optional Team;
- assignment note;
- assigned by / assigned at;
- ended at;
- append-history model;
- only one current assignment per participant.

Same-Journey participant / Department / Team hierarchy is database-enforced.

Attendance fields on `journey_participants` remain untouched.

No assignment rows were seeded during cutover.

## 5. Public-safe staffing projection

Production RPC active:

`public.tnc_public_staffing_options(uuid)`

Architecture:

- privileged implementation:
  `private.tnc_public_staffing_options(uuid)`;
- public wrapper:
  `SECURITY INVOKER`;
- fixed empty search path;
- public wrapper exposes only the staffing option projection needed by applicant UX.

Visibility requires:

- staffing need open;
- Department active;
- Journey lifecycle phase `upcoming`;
- Application Window `open`.

All current production Application Windows remain CLOSED, so the real recruitment surface remains unavailable.

## 6. Assignment operations

Production public wrappers active:

- `public.tnc_set_journey_participant_assignment(...)`;
- `public.tnc_end_journey_participant_assignment(uuid)`;
- `public.tnc_ensure_volunteer_participant_role(uuid)`.

Privileged implementations remain in the non-exposed `private` schema.

Public wrappers are `SECURITY INVOKER`.

Assignment mutation requires authenticated Admin authority in the private implementation.

Anon does not have execute authority on assignment RPCs.

The volunteer role bootstrap remains idempotent:

- creates TNV role baseline only if there is no current assignment;
- does not overwrite a later BTC Department/Team decision.

## 7. RLS / grants production verification

Production verification confirms:

### Staffing

- RLS: enabled;
- public staffing SELECT grant: present per locked WU4 contract;
- Admin insert/update policies: present;
- public open-read policy: present;
- public-safe RPC execution for anon: present.

### Participant assignments

- RLS: enabled;
- anon table SELECT: false;
- anon assignment RPC execution: false;
- authenticated assignment table/API capability exists but row/function authorization remains Admin-gated.

### Privileged functions

Production inspection confirms:

- private privileged functions use `SECURITY DEFINER`;
- fixed `search_path = ''`;
- stable public wrappers are not SECURITY DEFINER;
- public assignment wrappers do not grant anon execution.

No Supabase Security Advisor database warning was introduced by WU4.Final.

## 8. Legacy P16 preservation

The production legacy volunteer record remains unchanged after cutover:

- legacy `preferred_team`: preserved;
- legacy `assigned_team`: preserved;
- `volunteer_structure_version`: NULL.

No automatic semantic mapping was performed.

WU4 continues to treat P16 values as historical compatibility truth, not P17 Journey Role / Department / Team authority.

## 9. Attendance / Memory preservation

Post-cutover verification:

- participants: 3;
- attendance resolved: 0.

WU4 migration did not write:

- `attended_party_size`;
- `attendance_recorded_at`;
- `attendance_recorded_by`.

No WU4 migration path mutates:

- Journey Memory;
- Reflection;
- Community;
- shared-experience evidence;
- Impact truth.

## 10. Production data after cutover

Immediately after migration:

- `journey_staffing_needs` rows: **0**;
- `journey_participant_assignments` rows: **0**;
- P17 structured application rows: **0**;
- legacy P16 volunteer record: **1** preserved;
- non-closed Application Windows: **0**;
- attendance resolved: **0**.

Therefore this cutover activates capability only.

It does not start operations or recruitment.

## 11. Rollback

Added:

`database/rollbacks/p17_wu4_final_volunteer_staffing_assignment_cutover.sql`

Rollback is explicitly:

**IMMEDIATE CUTOVER / PRE-DATA ONLY**

It aborts instead of deleting operational truth if any of these exist:

- staffing need rows;
- participant assignment history;
- P17 volunteer structure/preference data;
- waitlisted application rows.

If safe, rollback:

- removes WU4 assignment RPCs;
- removes public staffing projection;
- removes assignment table;
- removes WU4 application preference columns;
- removes staffing table;
- restores the exact pre-WU4 P16 volunteer-required constraint;
- restores legacy `assigned_team` approval coupling;
- restores pre-WU4 standard private-field constraint.

### Enum rollback rule

PostgreSQL enum labels are treated append-only.

The `waitlisted` label is intentionally retained as an inert compatibility residue during emergency rollback rather than rebuilding the enum type and risking dependency-heavy policy/index/type replacement.

Capability removal is controlled by tables/columns/RPCs and restored constraints.

## 12. WU4.Final QA

Added:

`scripts/p17-wu4-final-production-source-qa.ts`

`scripts/p17-wu4-final-db-qa.sql`

QA proves:

- actual production migration contains all locked WU4 source contract bodies;
- no Journey recruitment/lifecycle mutation is smuggled into migration;
- no attendance/Memory/social truth mutation;
- migration applies from a production-like pre-WU4 fixture;
- legacy P16 application survives cutover;
- staffing and assignment capability becomes active;
- closed Application Window remains closed;
- Admin staffing/assignment operation path works;
- ephemeral operational test rows can be rolled back;
- guarded rollback restores pre-WU4 structural capability;
- rollback preserves legacy data and attendance state.

The final gates are wired into:

- generic CI;
- dedicated P16-WU10B Volunteer Pilot Gate.

## 13. Exact-head release evidence

Branch:

`p17-wu4-final-production-cutover`

PR:

`#93 — P17-WU4.Final: Production volunteer staffing and assignment cutover`

Final PR head:

`88501c1fb95d76134c67e801dfe1de1fe2e33940`

Exact-head workflows:

- generic CI `36097116259`: **SUCCESS**;
- dedicated P16-WU10B gate `36097116266`: **SUCCESS**.

Passed:

- WU4.Final production migration source QA;
- WU4.Final actual migration + rollback DB QA;
- WU4.1–WU4.6 inherited gates;
- P16 volunteer privacy/Vault gate;
- all inherited P9–P17 source gates;
- inherited ephemeral DB regressions;
- build;
- typecheck;
- Cloudflare dry-run.

## 14. Merge / post-merge evidence

PR #93 was squash-merged.

Product main:

`a47c099c4b5f1af6c162e75a0f16c5059222a7cb`

Post-merge main CI:

`36097236961` — **SUCCESS**

Exact head:

`a47c099c4b5f1af6c162e75a0f16c5059222a7cb`

## 15. Production cutover

Production Supabase project:

`iwiqprhoohkxvjyxojto`

Applied migration:

`p17_wu4_final_volunteer_staffing_assignment_cutover`

Migration history version:

`20260925050939`

Apply result:

**SUCCESS**

No operational WU4 data was seeded.

## 16. Production verification

Verified after cutover:

- staffing table: present;
- assignment table: present;
- P17 preference columns: present;
- `waitlisted`: present;
- participant same-Journey unique authority: present;
- one-current-assignment unique index: present;
- public staffing RPC: present;
- Admin assignment RPCs: present;
- TNV-role bootstrap RPC: present;
- WU4 RLS policies: present;
- application forward constraints: present;
- legacy assignment constraint: absent as designed;
- staffing rows: 0;
- assignment rows: 0;
- P17 application rows: 0;
- legacy P16 volunteer preserved: 1;
- non-closed Application Windows: 0;
- attendance resolved: 0.

## 17. Supabase Advisors

### Security Advisor

Current warning:

`auth_leaked_password_protection`

Title:

**Leaked Password Protection Disabled**

This is the same pre-existing project-level Auth configuration warning already recorded before WU4.Final.

It is not:

- caused by WU4;
- a WU4 RLS failure;
- a Vault regression.

No new WU4-specific Security Advisor warning appeared after cutover.

### Performance Advisor

Performance Advisor reports non-blocking observations including:

- unindexed FK candidates on some new WU4 preference/assignment/staffing relationships;
- multiple permissive SELECT policies for authenticated users on `journey_staffing_needs`;
- existing application-policy initplan warnings;
- expected unused-index observations immediately after creating empty foundation tables.

These are performance observations, not security/correctness failures.

Current production scale is extremely small:

- staffing rows: 0;
- assignment rows: 0;
- P17 application rows: 0.

WU4.Final intentionally does not add speculative indexes/policy rewrites solely to silence informational lints. Future optimization should be evidence-driven or performed when real operational volume justifies it.

## 18. Cloudflare runtime scope

WU4.Final does not deploy a new Cloudflare Worker runtime.

Release evidence includes:

- source merged to main;
- build PASS;
- typecheck PASS;
- Cloudflare production configuration dry-run PASS.

The production cutover owned by this work unit is the Supabase operational foundation.

Real volunteer recruitment remains separately controlled by canonical lifecycle/application authority and is still CLOSED.

## 19. WU4 final decision

# **P17-WU4 — VOLUNTEER APPLICATION & ASSIGNMENT REBASE: COMPLETE / CLOSED / PASS**

Completed sequence:

- WU4.0 — Current-State Audit & Architecture Lock;
- WU4.1 — Staffing / Preference / Assignment Schema Source Contract;
- WU4.2 — Dynamic Staffing Needs + Volunteer Application Rebase;
- WU4.3 — Review / Waitlist / Approval Workflow Rebase;
- WU4.4 — Participant Role + Assignment Operations;
- WU4.5 — Legacy P16 Compatibility;
- WU4.6 — Security / Privacy / Mobile Regression QA;
- WU4.Final — Production Cutover & Canonical Closeout.

Production foundation is active.

Recruitment remains:

# **HOLD / CLOSED**

No Journey application window was opened by WU4.

## 20. Next canonical work unit

Next Phase 17 work unit:

# **P17-WU5 — PARTICIPANT PERSONAL JOURNEY WORKSPACE**

WU5 must build on the now-live WU3 + WU4 operational truth without conflating personal participant experience with Admin operational authority.
