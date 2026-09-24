# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.5 — OFFICIAL UPDATE FOUNDATION

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE MANAGEMENT READY; PRODUCTION EVENT TABLES STILL UNAPPLIED**

## 1. Objective

Add Journey-scoped Official Update management to the Journey Control Center as the foundation for operational communication.

Canonical distinction:

> Official Update is operational communication. It is not a Community post.

WU3.5 provides source/admin publication management only. Participant-facing delivery/read projection remains deferred.

Public recruitment remains **HOLD / CLOSED**.

## 2. Starting truth

Product base:

`e715dd5f0f40a472873f3c32a199ab3d25e0dc73`

WU3.0 through WU3.4 were already COMPLETE / PASS.

WU3.1 had already defined:

`public.journey_official_updates`

with Journey scope, optional Department / Team scope, audience rules, bilingual content, draft/published publication truth, timestamps, same-Journey foreign keys and Admin-only RLS.

Production Event Management DDL remained unapplied.

## 3. Source contract

Added:

`src/lib/journeys/official-updates.ts`

Capabilities:

- load Official Updates for one selected Journey;
- create an Official Update as Draft;
- edit Draft content;
- explicitly Publish;
- explicitly return Published content to Draft;
- support VI / EN title and body;
- support optional effective timestamp;
- validate audience scope;
- validate Department / Team against the same Journey;
- attribute new rows to the authenticated creator.

All reads/writes remain scoped by:

`journey_id`

The WU3.1 composite foreign keys remain the final database integrity authority.

## 4. Canonical audience model

Supported audience scopes:

- `all_participants`;
- `btc`;
- `tnv`;
- `ban_dia`;
- `department`;
- `team`.

Rules:

- broad audiences cannot carry Department or Team IDs;
- Department audience requires one Department and no Team;
- Team audience requires both Department and Team;
- Department must belong to the selected Journey;
- Team must belong to both the selected Journey and selected Department.

WU3.5 does not yet grant participant read/delivery authority for any audience vocabulary.

## 5. Publication authority

Publication is deliberately explicit.

New Official Updates are created with:

- `status = draft`;
- `published_at = null`.

Saving content does **not** auto-publish.

Publishing performs:

- `status = published`;
- `published_at = current timestamp`.

Returning to Draft performs:

- `status = draft`;
- `published_at = null`.

### Published-content edit protection

During QA, source was hardened so a Published Official Update cannot be silently edited while retaining the original publication timestamp.

Edit operations require:

`status = draft`

The UI therefore exposes Edit only for Draft records.

A Published record must explicitly return to Draft before its content can be changed.

This preserves the meaning of publication truth without introducing a full revision-history system.

## 6. Admin UI

Added:

`src/components/admin/journeys/journey-official-updates-manager.tsx`

The manager lives inside the selected Journey Control Center.

Supported UI actions:

- compose Official Update;
- save Draft;
- edit Draft;
- Publish;
- return to Draft;
- choose audience;
- choose Department / Team when applicable;
- enter VI / EN title and body;
- set optional effective timestamp.

The interface explicitly states that participant-facing delivery is not active in WU3.5.

## 7. Control Center integration

Updated:

`src/components/admin/journeys/journey-control-center.tsx`

and:

`src/lib/journeys/control-center.ts`

Control Center map now marks:

`Thông báo — ACTIVE — WU3.5`

Event Management source-ready areas now include:

- Department;
- Team;
- Runbook;
- Official Update.

Production tables still wait for WU3.Final.

## 8. Fail-closed capability

Production does not yet contain:

`journey_official_updates`

Missing-relation capability detection is inherited from the WU3 foundation.

A genuine missing table returns capability unavailable rather than a fake empty communication feed.

The Admin UI displays:

> Production capability chưa được kích hoạt.

Permission / RLS / authentication / network errors are not converted into a successful unavailable state.

## 9. Authority

Foundation-stage authority remains:

**Admin-only**

Global `editor` is not inferred as Journey BTC authority.

WU3.5 adds no participant-facing Data API read policy and no notification fan-out.

## 10. Scope protection

WU3.5 did not:

- create Community posts;
- reinterpret Official Update as Community content;
- deliver updates to participant accounts;
- create notification fan-out;
- mutate `journey_applications`;
- mutate `journey_participants`;
- create staffing or assignment truth;
- touch `preferred_team` / `assigned_team`;
- mutate attendance;
- mutate Memory / Reflection / Impact;
- expose hard delete in source/UI;
- apply production Event Management DDL;
- deploy Cloudflare production runtime;
- open recruitment.

## 11. QA

Added:

`scripts/p17-wu3-5-official-update-qa.ts`

and wired it into:

`.github/workflows/ci.yml`

The gate protects:

- Journey-scoped Official Update source;
- canonical audience vocabulary;
- audience/Department/Team scope validation;
- draft-by-default;
- explicit publish/unpublish;
- `published_at` publication contract;
- Draft-only content editing;
- authenticated creator attribution;
- missing-relation fail-closed behavior;
- no hard delete;
- no staffing/application/participant/attendance mutation;
- Official Update != Community / Reflection truth;
- Control Center integration;
- WU3.1 audience/publication/same-Journey FK/RLS/Admin-only DB invariants;
- no production migration smuggling.

## 12. CI investigation and repair

Initial exact-head verification exposed one real TypeScript regression after the Control Center map advanced `Thông báo` from `next` to `active`.

The render code still compared:

`section.status === "next"`

After WU3.5 activation, the inferred section-status union no longer contained `next`.

TypeScript correctly failed with:

`TS2367`

The stale render branch was removed.

No business/security gate was weakened.

The corrected exact head then passed all required gates.

## 13. Exact-head CI evidence

Branch:

`p17-wu3-5-official-update-foundation`

PR:

`#82 — P17-WU3.5: Official Update foundation`

Final PR head:

`09e5ca66da8c0e8947df8f9a2f6f8f4919a5f8fa`

Exact-head gates:

- generic CI `35996114164`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35996114159`: **SUCCESS**
- P17-WU3.5 Official Update source QA: **PASS**
- P17-WU3.4 Runbook QA: **PASS**
- P17-WU3.3 Department Team QA: **PASS**
- P17-WU3.2 Journey Control Center QA: **PASS**
- P17-WU3.1 Event Management source/schema QA: **PASS**
- all inherited source gates: **PASS**
- all inherited ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 14. Merge / post-merge evidence

PR #82 was squash-merged.

Product main:

`9df3676d85edb612ca691cba85f93b0cac9f13fe`

Post-merge main CI:

- run `35996283738`
- exact head `9df3676d85edb612ca691cba85f93b0cac9f13fe`
- conclusion: **SUCCESS**

## 15. Production verification

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

No production mutation was performed by WU3.5.

## 16. Decision

**P17-WU3.5 — COMPLETE / PASS.**

Next:

**P17-WU3.6 — EXISTING JOURNEY ADMIN RECOMPOSITION**

Public recruitment remains:

**HOLD / CLOSED**
