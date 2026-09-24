# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3 — EVENT MANAGEMENT FOUNDATION

Date: 2026-09-24  
Status: **IN PROGRESS — WU3.0 / WU3.1 / WU3.2 COMPLETE / PASS**  
Owner: Jean / Owner  
CTO / Product Architect / QA Lead: ChatGPT  
Builder: Lovable only when needed

## 1. Canonical starting point

P17-WU3 starts only after:

- P17-WU1 audit baseline;
- P17-WU2 lifecycle / information architecture;
- P17-WU2 production lifecycle cutover;
- P17-WU2 HF1 Memory publication hardening;
- production generated-types reconciliation.

Canonical product source at WU3 start:

`huynhtranhuythinh/tramnucuoi@2c37673680450c12742512945f870e2bece6e653`

Canonical docs source at WU3 start:

`huynhtranhuythinh/tramnucuoi-docs@afab13780ce4018e47fe5bf608db9915147edde3`

Production Supabase:

`iwiqprhoohkxvjyxojto`

Public recruitment at WU3 start:

**HOLD / CLOSED**

No WU3 work may reopen recruitment as a side effect.

---

## 2. Canonical product mandate

The Product Reassessment canon defines:

> BTC operates a Journey through one Journey Control Center.

The Control Center must remain Journey-specific and must not become a generic enterprise project-management product.

Canonical operational areas are:

1. Journey Setup
2. People & Applications
3. Teams & Assignments
4. Plan & Runbook
5. Resources & Donations
6. Communications
7. Community
8. Day-of Operations
9. Attendance / Evidence
10. Closeout & Results

The overview must answer:

> What is missing or needs action now?

This WU creates the **foundation** for that operating model.

---

## 3. WU3 scope boundary

WU3 owns:

1. one Journey Control Center information architecture;
2. Journey-scoped Department structure;
3. optional Journey-scoped Team structure;
4. lightweight Journey Runbook;
5. Official Update foundation distinct from Community content;
6. Control Center Overview / action signals;
7. recomposition of existing Journey Admin tools into one Journey context;
8. schema/security contracts required for the above.

WU3 explicitly does **not** own:

- volunteer application redesign;
- staffing-need → application generation;
- Waitlist workflow;
- applicant Department preferences;
- final volunteer/participant assignment migration;
- Participant Journey Workspace;
- participant-facing Official Update delivery;
- Community publishing rebase;
- Donation / Need → Pledge → Received → Distributed;
- Day-of operations redesign;
- attendance truth changes;
- expanded Closeout / Results / Memory model;
- public discovery/trust rebase;
- public pilot reactivation.

Those remain later canonical WUs.

---

## 4. Current-state audit

### 4.1 Admin UX is fragmented

Current route:

`/admin/journeys`

is a Journey list plus multiple independent action buttons.

Existing managers include:

- Journey content editor;
- lifecycle/application controls;
- applications;
- volunteer recruitment;
- Field Updates;
- media;
- impact;
- Field Journal relations;
- closeout.

Managers are opened below the full Journey list rather than inside a dedicated Journey operating context.

There is no canonical per-Journey Control Center route.

### 4.2 Existing database foundations to KEEP

Production already has strong truth boundaries for:

- `journeys`;
- `journey_applications`;
- `journey_participants`;
- attendance evidence;
- `journey_updates`;
- media / documentary evidence;
- impact;
- closeout reviews;
- social identity / Journey social presence;
- Reflection / Memory;
- verified Contribution foundation.

Production counts at WU3 audit:

- Journeys: 5
- Applications: 9
- Participants: 3
- Journey Updates: 5
- Closeout Reviews: 0
- Community Contributions: 0

These foundations are not to be rewritten by WU3.

### 4.3 Missing Event Management objects

Production currently has no canonical tables for:

- Journey Departments;
- Journey Teams;
- Journey Runbook;
- Official Journey Updates / operational communications.

There is also no per-Journey Control Center aggregate.

### 4.4 P16 volunteer team model is legacy pilot compatibility

Current volunteer pilot stores:

- `preferred_team`;
- `assigned_team`;

directly on `journey_applications`.

Source hard-codes:

- media;
- logistics/cooking;
- haircutting;
- activities/entertainment;
- communications;
- coordination;
- general support.

Database CHECK constraints hard-code the same vocabulary.

This remains historical pilot truth.

WU3 must **not** reinterpret those text values as the new canonical Department model and must not rewrite historical assignments.

WU4 will own compatibility mapping and application/assignment rebase.

### 4.5 Participant type is not Journey Role

Existing `participant_type` remains historical participation metadata:

- individual;
- family;
- contributor;
- partner_rep.

It must not be redefined as the canonical Journey Roles:

- BTC;
- TNV;
- Bản địa.

Journey Role modeling is not smuggled into WU3 through participant_type.

### 4.6 Current admin authorization

Global staff roles currently include:

- admin;
- editor.

Journey applications/participants and sensitive volunteer data remain Admin-only.

WU3 must not repurpose global `editor` as Journey-scoped BTC authority.

Until a later Journey-scoped role/participant authority exists, mutation of new operational structures should be **Admin-only**.

This is deliberately conservative.

---

## 5. Canonical WU3 domain model

### 5.1 Journey Department

A Department is configured per Journey.

Examples are data, not global product enums.

Conceptual fields:

- id;
- journey_id;
- name;
- name_en optional;
- description optional;
- description_en optional;
- active state;
- sort order;
- audit timestamps / creator.

Rules:

- Department belongs to exactly one Journey;
- no global mandatory Department list;
- deleting/replacing Department structure must not rewrite historical assignment truth;
- WU4 may later attach staffing needs and applicant preferences.

### 5.2 Journey Team

A Team is an optional subgroup under a Department.

Conceptual fields:

- id;
- journey_id;
- department_id;
- name;
- name_en optional;
- description optional;
- description_en optional;
- active state;
- sort order;
- audit timestamps / creator.

Rules:

- Team and Department must belong to the same Journey;
- Team is not Journey Role;
- Team Lead / participant assignment is deferred to WU4/WU5 authority.

### 5.3 Journey Runbook

The Runbook is lightweight and Journey-specific.

It is not Asana/Jira.

A Runbook item may represent a preparation task or chronological operating step.

Minimal conceptual fields:

- id;
- journey_id;
- optional department_id;
- optional team_id;
- title;
- title_en optional;
- note;
- note_en optional;
- kind: task / schedule;
- scheduled_at optional;
- due_at optional;
- status: todo / doing / done;
- sort order;
- created_by;
- timestamps.

Rules:

- no Gantt;
- no story points;
- no sprint;
- no deep dependency graph;
- no payroll/time tracking;
- no generic document drive;
- no ERP/procurement engine;
- no generic CRM.

A responsible participant/person link is **not required in WU3 schema** because canonical Journey participant/assignment authority is being rebased in WU4.

WU4 may extend the Runbook with governed assignee references.

### 5.4 Official Journey Update

Official Update is operational communication.

It is **not** a Community post.

Foundation fields:

- id;
- journey_id;
- optional department_id;
- optional team_id;
- audience scope;
- title;
- title_en optional;
- body;
- body_en optional;
- draft/published state;
- effective/published timestamp;
- created_by;
- timestamps.

Audience vocabulary may reserve:

- all participants;
- BTC;
- TNV;
- Bản địa;
- Department;
- Team.

WU3 does not yet grant participant Data API read access.

Participant delivery/read projection belongs to WU5 after Journey participant authority is ready.

### 5.5 Journey Control Center Overview

The overview is a derived operational surface, not a new source of truth.

WU3 foundation may safely derive:

- lifecycle phase;
- application state;
- start/end date display;
- existing application count;
- existing confirmed participant count;
- unresolved attendance count;
- Runbook incomplete count;
- Department / Team structure count;
- official update state;
- Closeout readiness when relevant.

Dates may produce display cues such as “X days remaining” but never perform lifecycle transitions.

Future WUs may add:

- required vs approved volunteers — WU4;
- unassigned participants — WU4;
- missing resources — WU7;
- richer day-of signals — WU8;
- complete closeout readiness — WU9.

---

## 6. Journey Control Center IA

Canonical Admin direction:

`/admin/journeys`
= Journey list / portfolio entry.

A selected Journey opens one dedicated Control Center route.

Recommended conceptual sections:

1. **Tổng quan**
2. **Thiết lập**
3. **Người & đăng ký**
4. **Cơ cấu nhóm**
5. **Kế hoạch**
6. **Nguồn lực**
7. **Thông báo**
8. **Cộng đồng**
9. **Ngày diễn ra**
10. **Điểm danh & Tư liệu**
11. **Đóng sổ & Kết quả**

WU3 implements the Control Center shell and the areas for which authority already exists.

Existing managers should be reused where sound rather than rewritten.

Later-WU domains may appear only as clearly non-operative future slots or remain hidden until implemented; no fake functionality.

---

## 7. Security & Supabase contract

WU3 schema changes must follow current Supabase platform requirements.

For every new table in exposed `public` schema:

- explicit grants;
- RLS enabled;
- explicit policies;
- no reliance on automatic Data API exposure;
- no browser service-role usage;
- no authorization via user-editable metadata;
- views, if any, use `security_invoker=true`.

Supabase announced in 2026 that new public tables require explicit grants as Data API auto-exposure is being removed. WU3 migrations must therefore be declarative about grants.

WU3 operational mutation authority at foundation stage:

**Admin-only**

until later Journey-scoped BTC participant authority is explicitly designed.

No new sensitive PII table is required by WU3.

---

## 8. Truth invariants inherited from WU1/WU2

WU3 must preserve:

- account != participant;
- application != participant;
- participant claim != attendance;
- attendance NULL / 0 / >0 semantics;
- Journey Role != Department != Team != Task != Skill;
- beneficiary != participant;
- operational/private truth != public/social publication;
- date != lifecycle authority;
- approved application != attendance;
- social presence != attendance;
- Memory requires Closeout authority;
- recruitment remains explicit and protected.

---

## 9. WU3 implementation sequence

### WU3.0 — Current-State Audit & Architecture Lock
Status: **COMPLETE / PASS**

Outputs:

- current Admin composition audit;
- production schema audit;
- hard-coded P16 staffing finding;
- Event Management boundary;
- canonical WU3 domain model;
- security contract;
- sub-WU sequence.

No product/database mutation.

### WU3.1 — Operational Schema Source Contract

Design and QA source contracts for:

- Journey Departments;
- Journey Teams;
- Journey Runbook;
- Official Updates.

No production DDL yet.

### WU3.2 — Journey Control Center Route & Overview
Status: **COMPLETE / PASS**

Canonical record:
`canon/PHASE_17_WU3_2_JOURNEY_CONTROL_CENTER_ROUTE_OVERVIEW_2026-09-24.md`

Product evidence:
- PR #79 merged;
- product main: `7ae0c3452bbf3e02a4d533ab0b7e307199420bcf`;
- post-merge main CI `35987238110`: **SUCCESS**;
- `/admin/journeys` remains the Journey portfolio;
- dedicated `/admin/journeys/:journeyId` Control Center is active in source;
- Overview uses existing production truth only;
- RLS-hidden Admin metrics are never represented as zero;
- WU2 lifecycle/application authority is reused;
- no WU3 production DDL / recruitment activation.

### WU3.3 — Department / Team Structure Management
Status: **NEXT**

Admin-only management of Journey-specific structure.

No volunteer assignment migration yet.

### WU3.4 — Runbook Foundation

Admin-only lightweight planning and chronological operations.

### WU3.5 — Official Update Foundation

Admin-only compose/publish operational updates.

No participant-facing delivery yet.

### WU3.6 — Existing Journey Admin Recomposition

Move/reuse existing Journey tools into the Control Center context without rewriting their truth models.

### WU3.7 — Security / Regression / Mobile Admin QA

Verify:

- RLS/grants;
- no PII leakage;
- inherited WU2 gates;
- no attendance/Memory regression;
- no recruitment activation;
- responsive Control Center.

### WU3.Final — Production Foundation Cutover & Canonical Closeout

Only after source/ephemeral DB/full CI/security evidence PASS.

Production migration is a separate final gate.

---

## 10. Explicit non-goals

WU3 must not become:

- Jira;
- Asana;
- ERP;
- HR/payroll;
- procurement suite;
- donor CRM;
- form builder;
- public social network redesign;
- attendance rewrite.

The operating model remains:

> lightweight, Journey-specific, actionable, evidence-safe.

---

## 11. WU3.0 decision

**P17-WU3.0 — COMPLETE / PASS.**

Architecture is locked sufficiently to begin:

**P17-WU3.1 — OPERATIONAL SCHEMA SOURCE CONTRACT**

Public recruitment remains:

**HOLD / CLOSED**
