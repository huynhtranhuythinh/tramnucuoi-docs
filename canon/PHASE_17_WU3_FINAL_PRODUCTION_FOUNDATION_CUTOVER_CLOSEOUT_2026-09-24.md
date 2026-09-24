# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.Final — PRODUCTION FOUNDATION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-24  
Status: **COMPLETE / PASS — WU3 EVENT MANAGEMENT FOUNDATION CLOSED**

## 1. Objective

Cut over the locked P17-WU3 Event Management foundation into production only after source, security, regression, mobile, ephemeral DB and exact-head CI evidence had passed.

WU3.Final owns:

- production migration materialization of the WU3.1 source contract;
- rollback proof;
- exact-head release gate;
- production Supabase cutover;
- production RLS / grants / FK / trigger verification;
- confirmation that recruitment remains closed;
- canonical closeout of WU3.

## 2. Starting truth

Product main before WU3.Final:

`5a31cab2378a63bed6e541cbf8dcfe3a4396c208`

WU3.0 through WU3.7 were COMPLETE / PASS.

Production preflight confirmed:

- `journey_departments`: absent;
- `journey_teams`: absent;
- `journey_runbook_items`: absent;
- `journey_official_updates`: absent;
- Event Management tables: **0/4**;
- non-closed application windows: **0**;
- Journeys: **5**;
- `public.set_updated_at()`: present;
- `private.has_role(public.app_role)`: present;
- `public.content_status`: present;
- `public.app_role`: present;
- `gen_random_uuid()`: present.

Supabase security advisor had no WU3-specific database warning. The only remaining warning was the pre-existing Auth setting:

`auth_leaked_password_protection`

## 3. Production migration

Added:

`database/migrations/0057_p17_wu3_final_event_management_foundation.sql`

Canonical source:

`database/contracts/p17_wu3_1_event_management_foundation.sql`

The production migration contains the locked WU3.1 contract body without semantic reinterpretation.

It creates exactly four Journey-scoped operational source tables:

1. `public.journey_departments`
2. `public.journey_teams`
3. `public.journey_runbook_items`
4. `public.journey_official_updates`

No other WU3 domain table is introduced.

## 4. Production schema contract now active

### Department

Journey-scoped operational structure with:

- VI / EN name and description;
- `is_active`;
- `sort_order`;
- creator/audit timestamps;
- unique `(id, journey_id)`.

Department is not:

- Journey Role;
- Skill taxonomy;
- global mandatory organization structure.

### Team

Optional Team under exactly one Department.

Database same-Journey authority is enforced with the composite foreign key:

`(department_id, journey_id) -> journey_departments(id, journey_id)`

Team also exposes composite uniqueness required by downstream Runbook / Official Update scope checks.

### Runbook

Lightweight Journey-specific task/schedule foundation:

- `item_kind in ('task','schedule')`;
- `status in ('todo','doing','done')`;
- optional Department;
- optional Team;
- Team requires Department;
- Department / Team must belong to the same Journey;
- no staffing assignment truth.

### Official Update

Operational communication source distinct from Community:

- VI / EN title/body;
- Draft / Published publication truth;
- explicit `published_at`;
- optional `effective_at`;
- audience:
  - all participants;
  - BTC;
  - TNV;
  - Bản địa;
  - Department;
  - Team;
- Department / Team targeting must remain inside the same Journey/hierarchy.

Participant-facing delivery remains deferred to later WU authority.

## 5. Security

All four production tables now have:

- explicit Data API grants;
- RLS enabled;
- no direct `anon` table privilege;
- authenticated CRUD table privilege subject to RLS;
- service-role table privilege;
- four Admin-only RLS policies per table:
  - SELECT;
  - INSERT;
  - UPDATE;
  - DELETE.

Foundation-stage row authority remains:

`private.has_role('admin'::public.app_role)`

UPDATE policies retain both:

- `USING`;
- `WITH CHECK`.

No WU3 production policy uses user-editable Auth metadata for authorization.

No SECURITY DEFINER function was added by migration 0057.

## 6. Historical truth / hierarchy integrity

Production has five same-Journey/hierarchy foreign keys:

1. Team -> Department / Journey
2. Runbook -> Department / Journey
3. Runbook -> Team / Journey / Department
4. Official Update -> Department / Journey
5. Official Update -> Team / Journey / Department

Referenced Department / Team rows use `ON DELETE RESTRICT` where operational history depends on them.

The application/UI model continues to prefer deactivate/reactivate rather than hard deletion.

## 7. Updated-at ownership

Each of the four production tables now has one database-owned update trigger using the existing:

`public.set_updated_at()`

Production verification confirmed one non-internal trigger per WU3 table.

## 8. Rollback

Added:

`database/rollbacks/p17_wu3_final_event_management_foundation.sql`

Rollback order is reverse dependency order:

1. Official Update
2. Runbook
3. Team
4. Department

The rollback is explicitly destructive and is intended only for immediate cutover recovery before real operational data exists.

Once production operational history exists, canonical direction is forward repair rather than deleting history.

## 9. WU3.Final QA

Added:

`scripts/p17-wu3-final-production-source-qa.ts`

The source gate locks:

- migration contains the canonical WU3.1 contract body;
- all four tables present;
- all four RLS statements present;
- Admin authority retained;
- explicit grants retained;
- anon not granted operational table access;
- no application/participant mutation;
- no legacy P16 team-field reinterpretation;
- no attendance mutation;
- no Memory / Reflection / Impact mutation;
- no lifecycle/application activation;
- rollback covers all four tables in reverse dependency order.

Added:

`scripts/p17-wu3-final-db-qa.sql`

The ephemeral DB gate executes the real production migration, then verifies:

- four-table schema shape;
- RLS/grants;
- Editor cannot read/create operational rows;
- Admin can create operational hierarchy;
- cross-Journey Team is rejected;
- invalid Runbook hierarchy is rejected;
- invalid Official Update targeting is rejected;
- Published Official Update requires `published_at`;
- referenced structure cannot be deleted;
- soft deactivation works;
- no later-WU tables leak into WU3;
- rollback removes all four WU3 tables.

## 10. QA harness repair evidence

The first WU3.Final DB gate failure was not a schema/product failure.

The migration successfully:

- created all tables;
- created policies;
- passed hierarchy tests;
- passed Admin / Editor RLS tests;
- executed rollback and dropped all four tables.

The final rollback-verification DO block had been generated as:

`do $ ... $;`

instead of:

`do $$ ... $$;`

A first attempted patch still collapsed `$$` because JavaScript replacement strings interpret `$$` specially.

The harness was then repaired by writing literal lines directly.

No migration/security invariant was weakened.

Final exact-head DB migration/rollback gate passed.

## 11. Exact-head release evidence

Branch:

`p17-wu3-final-production-cutover`

PR:

`#85 — P17-WU3.Final: Production Event Management foundation cutover`

Final PR head:

`685af0eeef536ed083e2020ce48d6f59e932f42a`

Exact-head evidence:

- generic CI `36002419718`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `36002419767`: **SUCCESS**
- P17-WU3.Final production migration source QA: **PASS**
- P17-WU3.Final migration + rollback ephemeral DB QA: **PASS**
- P17-WU3.1 schema contract QA: **PASS**
- P17-WU3.2–WU3.7 inherited gates: **PASS**
- all inherited P9–P17 source gates: **PASS**
- all inherited ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 12. Merge / post-merge evidence

PR #85 was squash-merged.

Product main:

`3ad24711c3fbbc473f821042968fae835dcf5edc`

Post-merge main CI:

- run `36002611763`;
- exact head `3ad24711c3fbbc473f821042968fae835dcf5edc`;
- conclusion: **SUCCESS**.

## 13. Production cutover

Production Supabase project:

`iwiqprhoohkxvjyxojto`

Applied migration:

`p17_wu3_final_event_management_foundation`

Migration history version:

`20260924125937`

Apply result:

**SUCCESS**

No seed operational data was inserted.

## 14. Production verification

Post-cutover production verification:

### Tables

- `journey_departments`: present
- `journey_teams`: present
- `journey_runbook_items`: present
- `journey_official_updates`: present

Event Management tables:

**4/4**

### Initial data state

All four WU3 tables start with:

**0 rows**

This is intentional. WU3.Final activates capability; it does not invent operational structure.

### RLS / grants

Each table:

- RLS: **enabled**
- anon SELECT privilege: **false**
- authenticated CRUD table grant: **true**
- service_role CRUD table grant: **true**
- Admin-only RLS policies: **4**
- updated_at trigger: **1**

Total WU3 policies:

**16**

### Hierarchy integrity

Verified same-Journey FK count:

**5**

### Recruitment invariant

Non-closed application windows:

**0**

Journeys:

**5**

Public recruitment therefore remains:

**HOLD / CLOSED**

## 15. Security advisor after cutover

Post-cutover Supabase security advisor reports no WU3 schema/RLS finding.

The remaining unrelated warning is:

`Leaked Password Protection Disabled`

This is a Supabase Auth account configuration item, not a WU3 Event Management regression.

## 16. Scope explicitly preserved

WU3.Final did not:

- mutate `journey_applications`;
- mutate `journey_participants`;
- reinterpret `preferred_team`;
- reinterpret `assigned_team`;
- create staffing assignment truth;
- mutate attendance;
- mutate Memory / Reflection / Impact;
- create Community content;
- create participant-facing Official Update delivery;
- open recruitment;
- change Journey lifecycle state;
- deploy a new Cloudflare Worker runtime.

## 17. WU3 closeout decision

**P17-WU3 — EVENT MANAGEMENT FOUNDATION: COMPLETE / CLOSED / PASS.**

Completed sequence:

- WU3.0 — Current-State Audit & Architecture Lock
- WU3.1 — Operational Schema Source Contract
- WU3.2 — Journey Control Center Route & Overview
- WU3.3 — Department / Team Structure Management
- WU3.4 — Lightweight Runbook / Task / Schedule Management
- WU3.5 — Official Update Foundation
- WU3.6 — Existing Journey Admin Recomposition
- WU3.7 — Security / Regression / Mobile Admin QA
- WU3.Final — Production Foundation Cutover & Canonical Closeout

## 18. Next canonical work unit

Next Phase 17 work unit:

**P17-WU4 — VOLUNTEER APPLICATION & ASSIGNMENT REBASE**

Locked direction carried forward from the Phase 17 roadmap:

- dynamic registration -> approval -> assignment;
- staffing needs may generate volunteer demand;
- applicant may choose preferred Department;
- BTC owns final operational assignment;
- Department / Team remains distinct from Journey Role;
- canonical Journey Roles remain:
  - Ban tổ chức;
  - Tình nguyện viên;
  - Bản địa.

WU4 must build on the now-live WU3 production structure rather than reinterpret legacy P16 volunteer fields.

Public recruitment remains:

**HOLD / CLOSED**
