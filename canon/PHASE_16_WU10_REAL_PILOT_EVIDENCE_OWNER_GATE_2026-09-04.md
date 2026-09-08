# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU10 — REAL PILOT EVIDENCE & OWNER GATE

Date opened: 2026-09-04
Last evidence update: 2026-09-08
Status: **ACTIVE — WU10-A PILOT READINESS COMPLETE / REAL PRE-JOURNEY EVIDENCE ACCUMULATING / WU10-B POST-JOURNEY EVIDENCE PENDING**

## 1. Purpose

P16-WU10 is the final Phase 16 gate. It does not add social features by assumption. It validates whether the Journey-Based Social Network is ready to be piloted around a real Journey and closes Phase 16 only after real evidence exists.

- **WU10-A — Pilot Readiness & BEFORE-stage activation decision**: pre-Journey.
- **WU10-B — Real Pilot Evidence Reconciliation & Owner Gate**: only after the real Journey and attendance reconciliation.

The real Journey is dated **2026-09-11**.

## 2. Immutable evidence rules

- registration != attendance;
- confirmed registration != attendance;
- rejected registration != verified no-show;
- attendance NULL = unresolved;
- attendance 0 = verified no-show;
- attendance > 0 = verified attended;
- Journey Presence != attendance;
- interaction/notification != attendance;
- shared-experience requires appropriate verified evidence + consent;
- Memory only becomes eligible from real evidence;
- Reflection remains evidence-gated;
- Contribution/impact are never inferred from social activity;
- public visibility remains separate from private operational truth.

WU10 must never manufacture evidence to make the pilot look successful.

## 3. Canonical product source at WU10 open

Production product branch: `main`

Canonical product SHA at WU10 open:
`b25546fbea4514f51432436aa4e5e8f2e7f2a6de`

Tree SHA:
`788f731235cb179f024a8a848e17d91594c910d7`

P16-WU9 post-merge CI #247 / run `33753660608`: **SUCCESS**.

Repository default branch remains `develop`; WU10 source truth must pin explicitly to production `main`.

## 4. Pilot Journey

Supabase project: `iwiqprhoohkxvjyxojto`

Journey:
- id: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- slug: `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`
- VI: `Trạm Cơm Chay Yêu Thương — Đổi Nụ Cười · Mùng 1 Tháng 8`
- EN: `Trạm Cơm Chay Yêu Thương — Share a Smile · First Day of the Eighth Lunar Month`
- status: `registration_open`
- date: `2026-09-11`
- capacity: 30

Initial 2026-09-04 snapshot:
- applications: 1
- participant rows: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended: 0
- Journey social presences: 0
- interactions: 0
- notifications: 0
- shared-experience edges: 0
- Memory rows: 1
- Memory eligible: 0
- Memory unresolved: 1

The Memory projection row is unresolved state only and is **not** proof of attendance.

## 5. Activation architecture

The activation chain remains fail closed:

- `VITE_APP_COMMUNITY_AUTH_ENABLED`
- `VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED` requires Community Auth
- `VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED` requires Journey Room
- `VITE_APP_JOURNEY_INTERACTION_V1_ENABLED` requires Journey Room + Social Safety
- `VITE_APP_SOCIAL_NOTIFICATIONS_V1_ENABLED` requires Interaction v1
- `VITE_APP_SHARED_EXPERIENCE_GRAPH_ENABLED` is independently evidence/consent constrained
- `VITE_APP_JOURNEY_SOCIAL_CONTINUITY_ENABLED` composes governed truth and may not infer post-Journey truth

No WU10 production social activation has been authorized yet.

## 6. WU10-A CTO activation decision

**DO NOT activate the full P16 social chain merely because source/database foundation is ready.**

Community Auth delivery must be re-verified before any downstream runtime activation, and post-Journey truth cannot be validated before real attendance evidence exists.

Current activation state remains HOLD pending a separate explicit Owner gate.

## 7. Real pre-Journey evidence — registration cohort

By 2026-09-06 the Owner reported 8 production applications for the pilot Journey, providing a useful small real-user registration cohort.

On 2026-09-08 the Owner deliberately rejected one of two registrations belonging to the same real person in order to test negative registration lifecycle behavior while leaving the person's other registration intact.

### Production DB audit — 2026-09-08

Target contact had two applications with the same phone `0395968270`:

1. `43c541f8-0d21-5904-9b10-73f3d0305719`
   - email: `tuthien.jun@gmail.com`
   - status: `rejected`
   - party size: 1
   - reviewed_at: `2026-09-08T11:43:40.041Z`
   - record remains preserved in production history.

2. `d702b008-4a90-5604-adc1-9f52495d60d4`
   - email: `tramnucuoivietnam@gmail.com`
   - status: `submitted`
   - party size: 1
   - reviewed_at: NULL
   - unaffected by rejection of the sibling application.

Neither target application has a `journey_participants` row.

Journey application snapshot after rejection:
- total applications: 8
- submitted: 6
- approved: 0
- rejected: 1
- remaining lifecycle state includes the separately confirmed original pilot registration.

Journey participant truth after rejection:
- participant rows: 1
- confirmed: 1
- confirmed party size: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended rows: 0

Capacity remains 30 and only the confirmed participant consumes one confirmed place.

Downstream truth after rejection:
- Memory rows: 1
- Memory eligible: 0
- Memory unresolved: 1
- Memory no-show: 0
- Memory attended: 0
- rejected target Memory rows: 0
- Journey Presence rows: 0
- shared-experience edges: 0
- Journey interactions: 0
- social notifications: 0
- social blocks: 0
- social reports: 0
- moderation controls: 0
- social identities total: 0
- participant links for the Journey: 1 (the pre-existing confirmed pilot participant linkage only)

### Evidence verdict

**REGISTRATION REJECTION LIFECYCLE = PASS**

The real production test demonstrates:

- rejection preserves application history;
- rejecting one application does not mutate the person's separate submitted application;
- rejection does not create a participant row;
- rejection does not create `attendance = 0` / verified no-show;
- rejection does not create Memory eligibility;
- rejection does not create social presence, interaction, notification or shared-experience truth;
- the original confirmed participant remains unresolved until real attendance evidence is recorded.

This is a valid negative-path real-pilot evidence item for WU10.

## 8. BEFORE-stage pilot readiness criteria

Before any optional social activation:

1. production `main` SHA and CI remain canonical;
2. production Supabase schema/RLS match WU2–WU8 closeout;
3. Community Auth Site URL/redirect allowlist/Magic Link email delivery PASS;
4. Journey Room mobile VI/EN deployed routes PASS;
5. Social Safety enabled before Interaction v1;
6. no Shared-Experience claim before verified attendance;
7. pilot people opt into Social Identity/Journey Presence themselves;
8. registration/participant rows never automatically expose a person socially;
9. social email/push stays OFF unless separately approved;
10. Owner explicitly approves any Worker/runtime activation.

## 9. Minimum evidence required after the real Journey

WU10-B must reconcile:

### Operational
- participant rows;
- attendance unresolved/no-show/attended counts;
- attendance timestamps/actors;
- P14-WU5 real-evidence result.

### Social
- Social Identity opt-ins;
- Journey Presence opt-ins/withdrawals;
- Questions/Replies/Appreciations if activated;
- block/report/moderation events if any;
- notifications if any;
- shared-experience only after verified attendance + consent.

### Continuity
- Memory eligibility after attendance reconciliation;
- Reflection eligibility/submission/publication if real;
- verified Contribution if real;
- real return/reconnection signal if observed;
- no inferred impact.

## 10. Owner gates

### Gate A — optional BEFORE-stage social activation
Not automatically approved. Requires fresh Auth/runtime verification and explicit Owner authorization.

### Gate B — Phase 16 final closeout
Only after the real 2026-09-11 Journey evidence is reconciled may Owner approve:

`APPROVE P16 FINAL — Journey-Based Social Network pilot evidence accepted; close Phase 16.`

## 11. Current status — 2026-09-08

**WU10-A PILOT READINESS = COMPLETE / PASS**

**REAL REGISTRATION COHORT = ACTIVE**

**REAL REJECTION NEGATIVE-PATH EVIDENCE = PASS**

**BEFORE-STAGE PRODUCTION SOCIAL ACTIVATION = HOLD**

**WU10-B POST-JOURNEY REAL EVIDENCE = PENDING 2026-09-11**

**P16-WU10 = ACTIVE / NOT COMPLETE**

**PHASE 16 = ACTIVE / NOT COMPLETE**

`P14-WU5` remains OPEN and authoritative for real Journey attendance/evidence reconciliation.
