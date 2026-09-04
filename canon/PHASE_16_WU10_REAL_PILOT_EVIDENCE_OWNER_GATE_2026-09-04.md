# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU10 — REAL PILOT EVIDENCE & OWNER GATE

Date opened: 2026-09-04
Status: **ACTIVE — WU10-A PILOT READINESS COMPLETE / WU10-B REAL EVIDENCE PENDING**

## 1. Purpose

P16-WU10 is the final Phase 16 gate. It does not add social features by assumption. It validates whether the Journey-Based Social Network is ready to be piloted around a real Journey and closes Phase 16 only after real evidence exists.

WU10 is split into two evidence stages:

- **WU10-A — Pilot Readiness & BEFORE-stage activation decision**: can be completed before the Journey.
- **WU10-B — Real Pilot Evidence Reconciliation & Owner Gate**: can only be completed after the real Journey and attendance reconciliation.

The real Journey is dated **2026-09-11**.

## 2. Immutable evidence rules

The following remain authoritative:

- registration != attendance;
- confirmed registration != attendance;
- attendance NULL = unresolved;
- attendance 0 = verified no-show;
- attendance > 0 = verified attended;
- Journey Presence != attendance;
- interaction != attendance;
- notification != attendance;
- shared-experience edge requires appropriate verified attendance evidence;
- Memory only becomes eligible from real evidence;
- Reflection remains evidence-gated;
- Contribution/impact are never inferred from social activity;
- public visibility remains separate from private operational truth.

WU10 must never manufacture evidence to make the pilot look successful.

## 3. Canonical product source at WU10 open

Production product branch:

`main`

Canonical product SHA:

`b25546fbea4514f51432436aa4e5e8f2e7f2a6de`

Tree SHA:

`788f731235cb179f024a8a848e17d91594c910d7`

P16-WU9 post-merge CI #247 / run `33753660608`: **SUCCESS**.

WU1 through WU9 are COMPLETE / PASS.

Repository default branch is still `develop`; all WU10 source truth must continue to pin explicitly to `main`.

## 4. Pilot Journey production snapshot — 2026-09-04

Supabase project:

`iwiqprhoohkxvjyxojto`

Journey:

- id: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- slug: `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`
- VI title: `Trạm Cơm Chay Yêu Thương — Đổi Nụ Cười · Mùng 1 Tháng 8`
- EN title: `Trạm Cơm Chay Yêu Thương — Share a Smile · First Day of the Eighth Lunar Month`
- status: `registration_open`
- start/end date: `2026-09-11`
- capacity: 30

Current production evidence:

- applications: 1
- participant rows: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended rows: 0
- verified attended people: 0
- active Journey social presences: 0
- active interactions: 0
- active notifications: 0
- verified shared-experience edges: 0
- Memory projection rows: 1
- Memory eligible rows: 0
- Memory unresolved rows: 1

Canonical interpretation:

> The single Memory projection row is unresolved state only. It is **not** an eligible Memory and must not be presented as proof of attendance.

## 5. Activation architecture

The source activation chain is fail closed.

### Community Auth

`VITE_APP_COMMUNITY_AUTH_ENABLED`

Journey Community Room cannot activate unless Community Auth is enabled.

Community Auth itself requires an independently verified production Auth delivery path, including Site URL, redirect allowlist and Magic Link/email delivery.

### Journey Community Room

`VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED`

Requires Community Auth.

### Social Safety

`VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED`

Requires Journey Community Room.

### Journey Interaction v1

`VITE_APP_JOURNEY_INTERACTION_V1_ENABLED`

Requires Journey Community Room **and** Social Safety.

### Social Notifications v1

`VITE_APP_SOCIAL_NOTIFICATIONS_V1_ENABLED`

Requires Journey Interaction v1.

### Shared-Experience Graph

`VITE_APP_SHARED_EXPERIENCE_GRAPH_ENABLED`

Requires Journey Community Room, but its user-facing meaning remains evidence + mutual-consent gated.

### Memory / Reflection / Contribution Social Continuity

`VITE_APP_JOURNEY_SOCIAL_CONTINUITY_ENABLED`

Requires Journey Community Room and only composes existing governed truth. It must not generate post-Journey truth from social behavior.

## 6. WU10-A CTO activation decision

### Decision: DO NOT activate the full P16 social chain yet.

Reasons:

1. Production currently has only one participant for the 2026-09-11 Journey. This is insufficient to validate a multi-person social-network loop today.
2. There are zero social identities, Journey Presences, interactions and notifications in production.
3. WU10 has not yet re-verified the production Community Auth delivery path. Since every Journey social surface depends on Community Auth, enabling downstream features without re-proving Auth delivery would violate the existing release boundary.
4. Shared-experience, Memory and post-Journey continuity cannot be validated before real attendance evidence exists.
5. Phase 16 success must be evidence-backed, not achieved by switching on all flags.

Therefore the correct pre-Journey state is:

- P16 source/database foundation: READY;
- production social activation: HOLD;
- no Worker deployment required by WU10-A;
- no feature flag changes required by WU10-A;
- continue real Journey registration/operational preparation independently.

## 7. BEFORE-stage pilot readiness criteria

If Owner later chooses to activate a BEFORE-stage pilot before 2026-09-11, all of the following must be re-verified immediately before activation:

1. production `main` SHA and CI remain canonical;
2. production Supabase schema/RLS match WU2–WU8 closeout;
3. Community Auth Site URL/redirect allowlist/Magic Link email delivery PASS;
4. Journey Room mobile VI/EN routes PASS in deployed runtime;
5. Social Safety must be enabled before Interaction v1;
6. no Shared-Experience claim is shown before verified attendance;
7. all pilot people must opt into Social Identity/Journey Presence themselves;
8. no participant is automatically exposed merely because they registered or appear in `journey_participants`;
9. social email/push remains OFF unless separately approved;
10. Owner must explicitly approve any Worker deployment / runtime activation.

## 8. Minimum evidence required for WU10-B after the real Journey

WU10-B must collect and reconcile, at minimum:

### Operational evidence

- participant rows;
- attendance unresolved count;
- verified no-show count;
- verified attended rows/people;
- attendance recording timestamps/actors;
- P14-WU5 real-evidence reconciliation result.

### Social evidence

- Social Identity opt-ins;
- Journey Presence opt-ins/withdrawals;
- Questions/Replies/Appreciations, if activation occurred;
- block/report/moderation events, if any;
- notification events, if any;
- shared-experience edges only after verified attendance + consent.

### Continuity evidence

- Memory eligibility after attendance reconciliation;
- Reflection eligibility/submission/publication, if real;
- verified Contribution, if real;
- return/reconnection signal, if actually observed;
- no inferred impact.

## 9. WU10-B evaluation questions

After real evidence exists, CTO/Owner must answer:

1. Did Journey Room improve preparation or relationship continuity?
2. Did anyone opt into social presence voluntarily?
3. Did social interactions remain Journey-contextual rather than generic status posting?
4. Did safety/privacy controls work without exposing operational participant truth?
5. Did evidence-gated AFTER transitions behave correctly?
6. Did real shared-experience relationships appear only after verified attendance?
7. Was the experience useful enough to justify broader activation?
8. Which features should stay OFF even if technically ready?
9. Does the product thesis still hold: offline meaningful experience first, digital relationship continuity second?

## 10. Owner gates

WU10 has two separate Owner decisions.

### Owner Gate A — optional BEFORE-stage runtime activation

This is **not automatically approved** by opening WU10.

If Owner wants the social pilot live before 2026-09-11, a separate activation approval must explicitly authorize the runtime deployment/feature flags after Community Auth delivery is re-verified.

### Owner Gate B — Phase 16 final closeout

Only after real Journey evidence is reconciled may Owner approve:

`APPROVE P16 FINAL — Journey-Based Social Network pilot evidence accepted; close Phase 16.`

Until then, WU10 and Phase 16 remain OPEN.

## 11. Current WU10 status

As of 2026-09-04:

**WU10-A PILOT READINESS = COMPLETE / PASS**

**BEFORE-STAGE PRODUCTION SOCIAL ACTIVATION = HOLD**

**WU10-B REAL PILOT EVIDENCE = PENDING 2026-09-11 JOURNEY EVIDENCE**

**P16-WU10 = ACTIVE / NOT COMPLETE**

**PHASE 16 = ACTIVE / NOT COMPLETE**

Independent evidence lane:

`P14-WU5` remains OPEN and authoritative for real Journey attendance/evidence reconciliation.
