# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU5 — PARTICIPANT PERSONAL JOURNEY WORKSPACE
# ARCHITECTURE & CANONICAL WORK PLAN

Date: 2026-09-25  
Status: **WU5.0 COMPLETE / PASS — ARCHITECTURE LOCKED; SOURCE IMPLEMENTATION NEXT**

## 1. Purpose

P17-WU5 builds the private, Journey-centric operational workspace for a verified confirmed participant.

The participant should be able to answer:

- Which Journey am I participating in?
- What is the Journey lifecycle phase?
- What is my participation status?
- What is my Journey Role?
- Which Department / optional Team am I assigned to?
- What participant-safe plan applies to me?
- Which Official Updates are addressed to me?
- What time/location/preparation context am I permitted to see?

This is the **Participant Personal Journey Workspace**.

It is not:

- a generic account dashboard;
- a social profile;
- a Community feed;
- the Admin Journey Control Center;
- staff ERP;
- attendance evidence;
- a Memory page;
- Reflection or Impact.

## 2. Reconciled starting truth

WU4.Final is already production-active and canonically closed.

Product main at WU5 start:

`a47c099c4b5f1af6c162e75a0f16c5059222a7cb`

WU4.Final production migration:

`20260925050939 / p17_wu4_final_volunteer_staffing_assignment_cutover`

Production verification at WU5.0 audit:

- Journeys: 5;
- participants: 3;
- confirmed participants: 3;
- active verified participant links: 2;
- personal Memory projection rows: 2;
- current P17 participant assignments: 0;
- Departments: 0;
- Teams: 0;
- Runbook items: 0;
- Official Updates: 0;
- non-closed Application Windows: 0.

Recruitment remains:

**HOLD / CLOSED**

No operational data is to be seeded by WU5.

## 3. Existing truth to reuse

### 3.1 Verified participant ownership

Existing P12 identity authority:

`public.community_participant_links`

An active link binds:

`auth user -> verified Journey participant`

The self-claim path is already constrained by verified email and confirmed application/participant truth.

WU5 reuses this identity ownership evidence.

WU5 does **not** infer participant authority from:

- account role;
- CMS/editor role;
- social identity;
- Community presence;
- Memory eligibility.

### 3.2 Operational participant truth

Canonical operational participant source:

`public.journey_participants`

Workspace eligibility requires the linked participant to remain:

`status = confirmed`

A stale identity link does not override current operational participant status.

### 3.3 Assignment truth

Canonical current Journey assignment source:

`public.journey_participant_assignments`

Current assignment means:

`ended_at IS NULL`

It carries:

- Journey Role: `btc | tnv | ban_dia`;
- optional Department;
- optional Team.

WU5 never reads legacy `preferred_team` / `assigned_team` as P17 assignment authority.

### 3.4 Event Management truth

WU3 production sources remain authoritative:

- `journey_departments`;
- `journey_teams`;
- `journey_runbook_items`;
- `journey_official_updates`.

Those source tables remain operational/Admin surfaces.

WU5 must not broaden them into general participant-readable tables.

## 4. Critical P12 / P16 boundary

Existing P12:

`community_journey_memories`

is an attendance-derived personal archive projection.

It can contain:

- unresolved attendance;
- verified no-show;
- verified attended / Memory eligibility.

It is **not** the WU5 operational access source.

Reason:

> Participant Workspace authority comes from verified participant ownership + current confirmed participant status, not from Memory/attendance state.

Existing P16:

`AuthenticatedJourneySocialHome`

is a Community/social surface.

It remains a separate capability.

WU5 does not reinterpret social activity as operational authority.

## 5. Participant access contract

A caller may receive WU5 Journey workspace data only when all are true:

1. caller is authenticated;
2. an active `community_participant_links` row belongs to `auth.uid()`;
3. the linked `journey_participants` row belongs to the requested Journey;
4. participant status is `confirmed`.

Global account role is irrelevant to this decision.

An Admin/Editor who is not a verified confirmed participant does not gain participant workspace access by role.

A verified link for a participant that later becomes withdrawn does not keep operational workspace access.

## 6. Projection architecture

WU5 uses narrow participant-safe read projections/RPCs.

Source-table RLS remains unchanged and Admin-controlled.

### 6.1 My Journey workspace projection

Canonical public API:

`public.tnc_my_journey_workspaces()`

Returns only the caller's eligible Journey workspace rows.

Participant-safe fields may include:

- participant id;
- Journey id / slug;
- Journey title VI/EN;
- summary VI/EN;
- location VI/EN;
- start/end dates;
- canonical lifecycle phase;
- participant status;
- joined timestamp;
- current Journey Role;
- current Department id/name VI/EN;
- current Team id/name VI/EN;
- current assignment timestamp.

Must not expose:

- application email/phone;
- date of birth;
- identity document fields;
- emergency contact;
- health notes;
- application review notes;
- assignment note;
- assigned_by;
- ended assignment history;
- attendance fields;
- Memory / Reflection / Impact fields.

No current assignment is a valid state.

A confirmed participant without current P17 assignment still sees the Journey workspace, with Role / Department / Team unset.

### 6.2 Participant-safe Plan projection

Canonical public API:

`public.tnc_my_journey_plan(uuid)`

Access requires caller eligibility for the requested Journey.

Visibility rules:

- Journey-wide Runbook item: visible to every eligible participant;
- Department item: visible only when the caller's current Department matches;
- Team item: visible only when the caller's current Team matches.

If caller has no current assignment, only Journey-wide items are visible.

Safe fields:

- id;
- item kind;
- title VI/EN;
- note VI/EN;
- scheduled_at;
- due_at;
- status;
- sort order;
- scope label/type needed for UX.

No Admin actor metadata is exposed.

Runbook remains source operational truth; WU5 creates a read projection only.

### 6.3 Participant Official Update projection

Canonical public API:

`public.tnc_my_journey_official_updates(uuid)`

Access requires caller eligibility for the requested Journey.

Only:

`status = published`

is deliverable.

An update with future `effective_at` is not delivered before that time.

Audience rules:

- `all_participants`: every eligible participant;
- `btc | tnv | ban_dia`: matching current Journey Role;
- `department`: matching current Department;
- `team`: matching current Team.

Safe fields:

- id;
- title VI/EN;
- body VI/EN;
- effective_at;
- published_at;
- audience scope.

Do not expose:

- created_by;
- Admin-only source metadata;
- unrelated Department / Team operational rows.

Official Update remains distinct from Community post.

## 7. Database security shape

Preferred pattern:

- privileged implementation in non-exposed `private` schema;
- fixed `search_path = ''`;
- implementation self-authorizes with `auth.uid()` + verified participant-link/current-participant checks;
- narrow public wrapper remains `SECURITY INVOKER`;
- EXECUTE only to `authenticated` and `service_role`;
- no anon execution;
- no new broad table grants;
- no participant mutation RPC in WU5.

This follows the existing hardened TNC pattern and preserves least privilege.

## 8. UX composition

Authenticated `/cong-dong` and `/en/community` already own the account/session gateway.

WU5 reuses that gateway rather than creating a second authentication system.

Composition after authentication:

1. **Participant Personal Journey Workspace — WU5**
2. existing **Authenticated Journey Social Home — P16**
3. existing My TNC / Community capabilities

This keeps:

`Operational Workspace != Community`

The WU5 component is a distinct product boundary even when composed on the same authenticated page.

Primary heading:

- VI: **Hành Trình của tôi**
- EN: **My Journey**

The workspace is mobile-first and Journey-centric, not account-centric.

## 9. Workspace sections

### Today / Hiện tại

- Journey title;
- lifecycle phase;
- dates/location;
- participation status;
- Journey Role;
- Department;
- Team.

### My Plan / Kế hoạch của tôi

Only participant-safe Runbook projection rows.

No full Admin Runbook table access.

### Official Updates / Thông báo chính thức

Only audience-matched, published, effective participant-safe rows.

### My Team / Nhóm của tôi

Displays assignment context only:

- Journey Role;
- Department;
- Team.

Team is not automatically a social group and does not prove shared physical attendance.

### Journey Info / Thông tin Hành Trình

Participant-safe time/location/preparation context.

## 10. Lifecycle behavior

WU5 may display an eligible participant's linked Journey across canonical lifecycle phases:

- draft;
- upcoming;
- active;
- closeout_pending;
- memory;
- archived.

Workspace access is participation-governed, not public-discovery-governed.

WU5 itself does not transition lifecycle state.

## 11. Explicit privacy exclusions

WU5 must not expose:

- CCCD/passport;
- DOB;
- emergency contact;
- health notes;
- application review notes;
- application PII;
- internal assignment notes/history;
- Admin actor identifiers;
- internal Runbook items outside participant scope;
- draft or future-effective Official Updates;
- another participant's private data.

## 12. Non-goals and truth boundaries

WU5 does not:

- create Community truth;
- create social presence;
- create shared-experience claims;
- write attendance;
- treat assignment as attendance;
- treat participant status as attendance;
- create Memory;
- create Reflection;
- create Impact;
- infer two Team members met in person;
- open recruitment;
- change application windows.

## 13. Canonical WU5 sequence

### WU5.0 — Current-State Audit & Participant Workspace Architecture Lock
**COMPLETE / PASS**

### WU5.1 — Participant Access / Projection Source Contract
Next.

### WU5.2 — My Journey Entry / Journey Workspace Shell

### WU5.3 — My Role / Department / Team

### WU5.4 — Participant-safe Plan / Runbook Projection

### WU5.5 — Participant Official Update Delivery / Read Model

### WU5.6 — Mobile / Privacy / Security Regression QA

### WU5.Final — Production Cutover & Canonical Closeout

## 14. Release strategy

WU5.1-WU5.6 are source-first.

No production DDL is authorized merely by source readiness.

WU5.Final owns:

- explicit production migration;
- guarded rollback;
- ephemeral migration/rollback QA;
- exact-head release gate;
- production apply;
- post-cutover security/data verification;
- canonical closeout.

## 15. Production invariants

Until WU5.Final cutover:

- WU3/WU4 production truth remains authoritative;
- WU5 source must fail closed if projection RPCs are absent;
- recruitment remains HOLD / CLOSED;
- all current Application Windows remain CLOSED;
- attendance remains untouched;
- Memory / Reflection / Impact remain untouched;
- no fake participant, Department, Team, Runbook or Official Update data is seeded.

## 16. WU5.0 decision

# **P17-WU5.0 — COMPLETE / PASS**

Architecture is locked.

Proceed directly to:

# **P17-WU5.1 — PARTICIPANT ACCESS / PROJECTION SOURCE CONTRACT**
