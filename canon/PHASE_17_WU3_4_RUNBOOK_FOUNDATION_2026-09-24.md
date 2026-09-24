# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.4 — RUNBOOK FOUNDATION

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE MANAGEMENT READY; PRODUCTION EVENT TABLES STILL UNAPPLIED**

## 1. Objective

Add a lightweight, Journey-scoped operational Runbook inside the Journey Control Center without turning TNC into a generic project-management product.

WU3.4 owns only:

- task / schedule items;
- VI / EN title and note;
- optional Department / Team scope;
- scheduled time and due time;
- todo / doing / done state;
- explicit sort order;
- Admin-only foundation management.

Public recruitment remains **HOLD / CLOSED**.

## 2. Starting truth

Product base:

`61a1c8b052546b418518bf0cf08017ca2d6da848`

WU3.0 / WU3.1 / WU3.2 / WU3.3 were already COMPLETE / PASS.

The WU3.1 source contract already defined:

`public.journey_runbook_items`

with Journey scoping, optional Department / Team references, task/schedule kind, todo/doing/done status, timestamps, same-Journey composite foreign keys and Admin-only RLS.

Production Event Management DDL remained unapplied.

## 3. Product result

Added:

`src/lib/journeys/runbook.ts`

Source capabilities:

- load Journey Runbook;
- create a Runbook item;
- edit a Runbook item;
- change todo / doing / done state;
- validate Department against the selected Journey;
- validate Team against both selected Journey and Department;
- attribute new records to the authenticated creator.

Every Runbook read/write remains scoped by:

`journey_id`

Database composite foreign keys from WU3.1 remain the final integrity authority.

## 4. Control Center UI

Added:

`src/components/admin/journeys/journey-runbook-manager.tsx`

The selected Journey Control Center now contains a Runbook management area.

Supported fields/actions:

- Task or Schedule;
- title VI;
- title EN optional;
- note VI;
- note EN optional;
- optional Department;
- optional Team under that Department;
- scheduled datetime;
- due datetime;
- todo / doing / done;
- sort order;
- edit existing items.

Control Center map:

`Kế hoạch — ACTIVE — WU3.4`

## 5. Historical-truth protection

WU3.4 exposes no hard-delete helper or UI.

Runbook history is preserved by editing operational content/state rather than deleting rows from the management surface.

WU3.4 deliberately does not add:

- assignee;
- responsible participant;
- staffing assignment;
- applicant assignment;
- `preferred_team`;
- `assigned_team`;
- participant/application mutation;
- attendance mutation.

Governed person/participant assignment remains a later authority concern.

## 6. Fail-closed production capability

Production does not yet contain `journey_runbook_items`.

The source recognizes genuine missing-relation responses and returns capability unavailable rather than representing missing schema as a real empty Runbook.

The Control Center explicitly renders:

> Production capability chưa được kích hoạt.

No fake task/schedule data is created.

Permission, RLS, authentication and network failures are not converted into capability-unavailable success states.

## 7. Admin authority

Foundation-stage Runbook authority remains:

**Admin-only**

Global `editor` is not inferred to be Journey BTC authority.

Participant-facing Runbook delivery is not part of WU3.4.

## 8. Scope protection

WU3.4 did not:

- implement Official Update;
- mutate volunteer applications;
- mutate Journey participants;
- create staffing truth;
- change attendance truth;
- change lifecycle authority;
- change Memory / Impact / Reflection;
- apply production Event Management DDL;
- deploy Cloudflare production runtime;
- open recruitment.

## 9. QA

Added:

`scripts/p17-wu3-4-runbook-qa.ts`

and wired it into:

`.github/workflows/ci.yml`

The gate locks:

- Runbook source contract presence;
- Journey-scoped reads and writes;
- Department / Team same-Journey validation;
- current-user creator attribution;
- missing-relation fail-closed behavior;
- no hard delete;
- no application / participant / attendance assignment truth;
- Official Update remains outside WU3.4;
- Control Center integration;
- WU3.1 same-Journey FK, RLS and Admin-only DB invariants;
- no production migration smuggling.

## 10. Exact-head CI evidence

Branch:

`p17-wu3-4-runbook-foundation`

PR:

`#81 — P17-WU3.4: Lightweight Journey Runbook foundation`

Final PR head:

`7e9e1b03cc7f191e55a89792da5875b216c4fb6f`

Exact-head gates:

- generic CI `35994354911`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35994354853`: **SUCCESS**
- P17-WU3.4 Runbook source QA: **PASS**
- P17-WU3.3 Department Team QA: **PASS**
- P17-WU3.2 Journey Control Center QA: **PASS**
- P17-WU3.1 Event Management source/schema QA: **PASS**
- all inherited source gates: **PASS**
- all inherited ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 11. Merge / post-merge evidence

PR #81 was squash-merged.

Product main:

`e715dd5f0f40a472873f3c32a199ab3d25e0dc73`

Post-merge main CI:

- run `35994580462`
- exact head `e715dd5f0f40a472873f3c32a199ab3d25e0dc73`
- conclusion: **SUCCESS**

## 12. Production verification

Production Supabase:

`iwiqprhoohkxvjyxojto`

Post-merge read-only verification:

- `journey_departments`: absent;
- `journey_teams`: absent;
- `journey_runbook_items`: absent;
- `journey_official_updates`: absent;
- WU3 Event Management production tables: **0/4**;
- non-closed application windows: **0**;
- Journeys: **5**.

No production mutation was performed by WU3.4.

## 13. Decision

**P17-WU3.4 — COMPLETE / PASS.**

Next:

**P17-WU3.5 — OFFICIAL UPDATE FOUNDATION**

Public recruitment remains:

**HOLD / CLOSED**
