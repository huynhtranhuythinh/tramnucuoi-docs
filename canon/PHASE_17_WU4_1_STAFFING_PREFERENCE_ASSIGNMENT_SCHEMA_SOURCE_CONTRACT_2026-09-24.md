# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.1 — STAFFING / PREFERENCE / ASSIGNMENT SCHEMA SOURCE CONTRACT

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE CONTRACT ONLY; NO PRODUCTION DDL**

## 1. Objective

Define and continuously test the P17 volunteer staffing/application/assignment source contract before any production migration exists.

Canonical flow:

`Staffing Need -> ranked Department preferences -> BTC review/waitlist/approval -> Participant -> Journey Role -> Department/optional Team assignment -> Attendance`

Truth boundaries:

- application != participant;
- approval != assignment;
- assignment != attendance;
- Journey Role != Department != Team != Task != Skill;
- legacy P16 team strings remain historical compatibility data only.

## 2. Product base

WU4.1 started from product main:

`3ad24711c3fbbc473f821042968fae835dcf5edc`

Branch:

`p17-wu4-1-staffing-preference-assignment-contract`

PR:

`#86 — P17-WU4.1: Staffing preference assignment schema source contract`

Final PR head:

`5d6027179c366e1328822b10b2dbe25ab57dccc3`

## 3. Source contract

Canonical non-production fixture:

`database/contracts/p17_wu4_1_staffing_preference_assignment.sql`

### 3.1 Waitlist

Adds planned application enum value:

`waitlisted`

This makes Waitlist an explicit application workflow state.

Assignment remains a separate truth and is not encoded as an application status.

### 3.2 Journey staffing needs

Defines:

`journey_staffing_needs`

Journey-scoped, Department-based demand with:

- target volunteer count;
- VI / EN requirements;
- open/closed;
- sort order;
- creator/timestamps;
- one need per Journey + Department;
- same-Journey Department FK;
- no normal hard-delete grant.

### 3.3 Ranked Department preferences

Planned additions to `journey_applications`:

- `volunteer_structure_version`;
- `preferred_department_1_id`;
- `preferred_department_2_id`;
- `preferred_department_3_id`.

Rules:

- Preference 1 required for `p17-wu4-v1`;
- Preference 2/3 optional;
- duplicate Department preferences rejected;
- every preferred Department must belong to the same Journey;
- standard application rows must keep these fields NULL.

### 3.4 Legacy compatibility

Existing P16 fields are preserved:

- `preferred_team`;
- `assigned_team`;
- assignment audit fields.

WU4.1 does not write or reinterpret them.

Legacy P16 volunteer rows remain valid through an explicit compatibility branch:

- `volunteer_structure_version IS NULL`;
- legacy `preferred_team` remains the historical preference source.

New P17 rows use:

- `volunteer_structure_version = 'p17-wu4-v1'`;
- Department preference IDs.

The legacy constraint that required `assigned_team` before `accepted/confirmed` is intentionally not recreated.

Therefore:

> approval no longer requires a legacy team assignment.

## 4. Participant operational assignment

Defines:

`journey_participant_assignments`

Append-history model with:

- participant;
- Journey;
- Journey Role;
- optional Department;
- optional Team;
- assignment note;
- assigned by / assigned at;
- ended at;
- audit timestamps.

Journey Role vocabulary is:

- `btc`;
- `tnv`;
- `ban_dia`.

Rules:

- one current assignment per participant;
- ended rows remain history;
- Team requires Department;
- participant / Department / Team must belong to the same Journey/hierarchy;
- no normal DELETE grant.

A composite uniqueness key is added to `journey_participants(id, journey_id)` solely to enforce same-Journey assignment FK authority.

Attendance fields are untouched.

## 5. Public staffing visibility security

The first contract version used an RLS policy containing a direct subquery against `public.journeys`.

Ephemeral DB QA correctly failed for anon with:

`permission denied for table journeys`

This exposed a real API/security design issue: public staffing visibility must not require a broad anon SELECT grant on the complete Journey source merely to inspect lifecycle authority.

WU4.1 was hardened with one narrow helper:

`private.tnc_journey_accepts_public_staffing(uuid)`

Properties:

- schema: `private`;
- `SECURITY DEFINER`;
- `STABLE`;
- fixed `search_path = ''`;
- no exposed public-schema privileged function;
- default execute explicitly revoked;
- execute granted only to roles that need the boolean lifecycle predicate.

The helper returns only whether the requested Journey is:

- `lifecycle_phase = 'upcoming'`;
- `application_state = 'open'`.

The public staffing RLS policy now requires:

- staffing need `is_open = true`;
- private lifecycle predicate = true.

This preserves least-privilege access without opening `journeys` broadly to anon.

## 6. RLS / grants

### Staffing needs

Planned grants:

- anon/authenticated: SELECT;
- authenticated: INSERT / UPDATE;
- service_role: full.

RLS:

- public open-needs read uses the narrow lifecycle predicate;
- Admin read;
- Admin insert;
- Admin update.

There is no normal DELETE grant.

### Participant assignments

Planned grants:

- authenticated: SELECT / INSERT / UPDATE;
- service_role: full.

All authenticated row operations are Admin-only through RLS.

There is no normal DELETE grant.

## 7. QA

Added:

`scripts/p17-wu4-1-staffing-assignment-contract-qa.ts`

Source gate locks:

- staffing table;
- assignment table;
- ranked preferences;
- waitlist state;
- same-Journey FKs;
- Journey Role vocabulary;
- one-current-assignment rule;
- legacy compatibility;
- removal of legacy assigned-team approval coupling;
- no attendance/Memory/shared-experience mutation;
- no recruitment activation;
- narrow private SECURITY DEFINER exception only.

Added:

`scripts/p17-wu4-1-staffing-assignment-schema-qa.sql`

Ephemeral DB gate proves:

- legacy volunteer row survives unchanged;
- P17 accepted application can exist without legacy `assigned_team`;
- cross-Journey Department preference is rejected;
- duplicate ranked preference is rejected;
- current assignment uniqueness is enforced;
- assignment history can be ended and replaced;
- Team/Department hierarchy is enforced;
- Editor cannot see/write assignment history;
- anon only sees staffing needs while canonical lifecycle/application authority permits;
- closing application window hides public staffing needs;
- no attendance truth is manufactured.

## 8. Exact-head evidence

Final PR head:

`5d6027179c366e1328822b10b2dbe25ab57dccc3`

Exact-head gates:

- generic CI `36006509038`: **SUCCESS**
- dedicated P16-WU10B Volunteer Pilot Gate `36006509051`: **SUCCESS**
- WU4.1 source contract QA: **PASS**
- WU4.1 ephemeral DB schema QA: **PASS**
- all inherited P9–P17 source gates: **PASS**
- inherited ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 9. Merge / post-merge

PR #86 was squash-merged.

Product main:

`0a967b091d8214d34d0313df3756794eda63acdf`

Post-merge main CI:

`36006719456` — **SUCCESS**

## 10. Production invariants

WU4.1 is source-only.

Production verification after merge confirms:

- `journey_staffing_needs`: absent;
- `journey_participant_assignments`: absent;
- `preferred_department_1_id`: absent;
- `waitlisted` enum value: absent;
- non-closed application windows: **0**.

Therefore no WU4 DDL was smuggled into production.

Recruitment remains:

**HOLD / CLOSED**

## 11. Decision

**P17-WU4.1 — COMPLETE / PASS.**

Proceed to:

**P17-WU4.2 — DYNAMIC STAFFING NEEDS + VOLUNTEER APPLICATION REBASE**

WU4.2 remains source/application work only unless a later explicit WU4.Final production cutover gate is reached.
