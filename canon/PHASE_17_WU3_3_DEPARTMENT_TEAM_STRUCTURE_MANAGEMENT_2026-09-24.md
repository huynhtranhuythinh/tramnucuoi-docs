# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.3 — DEPARTMENT / TEAM STRUCTURE MANAGEMENT

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE MANAGEMENT READY; PRODUCTION EVENT TABLES STILL UNAPPLIED**

## 1. Objective

Implement Journey-scoped Department / Team management inside the Journey Control Center while preserving the WU3 foundation boundary:

- Department is per Journey;
- Team is an optional subgroup under a Department;
- Department / Team are not Journey Roles;
- P16 volunteer `preferred_team` / `assigned_team` remain legacy pilot compatibility only;
- volunteer / participant assignment remains WU4;
- production Event Management DDL remains WU3.Final.

Public recruitment remains **HOLD / CLOSED**.

## 2. Starting truth

Base product main:

`7ae0c3452bbf3e02a4d533ab0b7e307199420bcf`

WU3.0 / WU3.1 / WU3.2 were already COMPLETE / PASS.

Production Supabase:

`iwiqprhoohkxvjyxojto`

Pre-WU3.3 production verification:

- `journey_departments`: absent;
- `journey_teams`: absent;
- `journey_runbook_items`: absent;
- `journey_official_updates`: absent;
- non-closed application windows: `0`;
- Memory Journeys: `0`;
- Journeys: `5`.

Therefore WU3.3 could not truthfully behave as if Event Management structure already existed in production.

## 3. Supabase security contract retained

Current Supabase platform guidance was rechecked before implementation.

WU3.3 continues the WU3.1 contract:

- explicit Data API grants;
- RLS enabled on exposed `public` tables;
- authenticated table privileges do not replace row authorization;
- current operational row authority is Admin-only;
- `anon` receives no table access;
- no new SECURITY DEFINER function.

No production schema mutation was performed in WU3.3.

## 4. Product result

### 4.1 Journey Structure Manager

Added:

`src/components/admin/journeys/journey-structure-manager.tsx`

The manager lives inside the selected Journey Control Center.

Capabilities when production schema is available:

- create Department;
- edit Department;
- deactivate / reactivate Department;
- create Team under Department;
- edit Team;
- deactivate / reactivate Team;
- VI / EN name fields;
- VI / EN descriptions;
- explicit integer sort order.

The Control Center map now marks:

`Cơ cấu nhóm — ACTIVE — WU3.3`

### 4.2 No hard delete

WU3.3 deliberately exposes no hard-delete helper or UI.

Historical operational structure is preserved through:

- `is_active=true/false`;
- deactivate / reactivate actions.

This aligns with the WU3.1 contract, where referenced structure is protected by `ON DELETE RESTRICT`.

### 4.3 Team parent immutability in UI

A Team must be created under one Department.

After creation, the UI does not allow changing the Team's Department.

Reason:

- moving an existing Team would reinterpret historical operating structure;
- later Runbook / Official Update / assignment references may depend on the original hierarchy;
- the database same-Journey composite FK remains the final integrity authority.

## 5. Journey integrity

Added:

`src/lib/journeys/structure.ts`

Every Department / Team operation remains scoped by:

`journey_id`

Team creation/update performs an application-layer check that its Department belongs to the same Journey before the database composite FK verifies the relationship again.

Insert attribution uses the current authenticated user as:

`created_by`

No WU3.3 helper reads or writes:

- `journey_applications`;
- `journey_participants`;
- attendance fields;
- participant assignment;
- Memory / Reflection / Impact.

## 6. Production capability fail-closed

Added missing-relation capability detection in:

`src/lib/journeys/schema-capability.ts`

Recognized genuine missing-schema conditions include:

- PostgreSQL `42P01`;
- PostgREST `PGRST205`;
- equivalent missing relation/table schema-cache messages.

Important:

Only genuine missing-relation responses downgrade the feature to unavailable.

WU3.3 does not swallow:

- permission failures;
- RLS failures;
- auth failures;
- network failures.

When production Event Management tables are absent, the Control Center renders:

> Production capability chưa được kích hoạt.

It explains that:

- Department / Team source contract and management UI are ready;
- production DDL remains gated by WU3.Final;
- no fake structure is created.

An empty real Department list and a missing production table are therefore not conflated.

## 7. Authority

Foundation-stage Department / Team authority remains:

**Admin-only**

A global `editor` account:

- is not treated as Journey BTC;
- does not receive operational structure read authority;
- does not receive mutation authority.

Journey-scoped BTC authority remains a later canonical concern.

## 8. Scope protection

WU3.3 did not:

- reinterpret `preferred_team`;
- reinterpret `assigned_team`;
- create staffing needs;
- create participant assignments;
- mutate volunteer applications;
- mutate Journey participants;
- mutate attendance;
- implement Runbook;
- implement Official Update;
- implement participant-facing operational delivery;
- apply production WU3 DDL;
- deploy Cloudflare production runtime;
- activate recruitment.

## 9. QA

Added:

`scripts/p17-wu3-3-department-team-qa.ts`

The source gate verifies:

- Department / Team CRUD source contract exists;
- every write remains Journey-scoped;
- Team parent is checked against the same Journey;
- authenticated creator attribution exists;
- missing-relation fail-closed behavior exists;
- no hard-delete helper exists;
- P16 team fields are not reused;
- applications / participants are outside WU3.3 structure management;
- Control Center structure area is active;
- Admin authority is preserved;
- Team Department is frozen after creation in UI;
- WU3.1 same-Journey FK / RLS / Admin-only DB invariants remain inherited.

## 10. CI evidence

Branch:

`p17-wu3-3-department-team-management`

PR:

`#80 — P17-WU3.3: Department and Team structure management`

Final PR head:

`a22ef54790b5c537cebccbec846d8adbdff9ea81`

Exact-head verification:

- generic CI `35989822898`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35989822948`: **SUCCESS**
- P17-WU3.3 Department Team source QA: **PASS**
- P17-WU3.1 Event Management ephemeral schema QA: **PASS**
- all inherited P9–P17 source/DB gates: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

One initial CI run failed in inherited WU3.2 static QA because the wording that described the pre-DDL Event Management hold state had been refined by WU3.3.

The invariant itself remained true.

The inherited assertion was changed from exact old copy to the semantic production invariant:

`production tables vẫn chờ WU3.Final`

The corrected exact head then passed all gates.

## 11. Merge / post-merge evidence

Squash-merged product main:

`61a1c8b052546b418518bf0cf08017ca2d6da848`

Post-merge main CI:

- run `35989990911`
- exact head `61a1c8b052546b418518bf0cf08017ca2d6da848`
- conclusion: **SUCCESS**

## 12. Production boundary

WU3.3 made:

- **no production Supabase mutation**;
- **no production migration**;
- **no generated production type mutation**;
- **no Cloudflare production deploy**;
- **no feature flag mutation**;
- **no recruitment activation**.

Production still has no WU3 Event Management tables.

The source therefore remains intentionally fail-closed until WU3.Final.

## 13. Decision

**P17-WU3.3 — COMPLETE / PASS.**

Next:

**P17-WU3.4 — RUNBOOK FOUNDATION**

Public recruitment remains:

**HOLD / CLOSED**
