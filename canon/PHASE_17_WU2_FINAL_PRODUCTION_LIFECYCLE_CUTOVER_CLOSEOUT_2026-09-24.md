# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.Final — PRODUCTION LIFECYCLE CUTOVER & CLOSEOUT

Date: 2026-09-24  
Status: **COMPLETE / PASS — PRODUCTION LIFECYCLE AUTHORITY ACTIVE; RECRUITMENT CLOSED / HOLD**

## 1. Objective

Complete the Phase-17 Journey lifecycle rebase in production after WU2.1–WU2.6 had already established:

- canonical Journey routes;
- lifecycle-aware public composition;
- public navigation;
- Admin lifecycle/application controls;
- VI/EN/mobile regression gates;
- fail-closed recruitment behavior.

WU2.Final makes the new lifecycle model authoritative in production without fabricating real-world truth from dates or legacy status.

## 2. Canonical authority after cutover

### Journey lifecycle

`journeys.lifecycle_phase`:

- `draft`
- `upcoming`
- `active`
- `closeout_pending`
- `memory`
- `archived`

### Application window

`journeys.application_state`:

- `closed`
- `open`
- `paused`

The two authorities are independent.

Application `open/paused` is valid only while lifecycle phase is `upcoming`.

Legacy `journeys.status` remains temporarily as a compatibility projection for inherited P9–P16 code paths. It is no longer recruitment/publication authority.

## 3. Product release evidence — Final cutover

### PR #75

Title:
`P17-WU2.Final: production lifecycle cutover`

Final PR head:
`e866c1cc0b9f834f2f0fa3b6d1e0905c5a7f41ef`

PR evidence:

- generic CI `35954487810`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35954487799`: **SUCCESS**
- P17-WU2.Final source release QA: **PASS**
- P17-WU2.Final production-like ephemeral DB cutover QA: **PASS**
- all inherited P9–P16 gates: **PASS**
- WU2.1–WU2.6 gates: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare production-config dry-run: **PASS**

Squash-merged main:

`f9445a87491a6bad658897a2256ac1a02796e046`

Post-merge main CI:

- run `35954654960`
- exact head `f9445a87491a6bad658897a2256ac1a02796e046`
- conclusion: **SUCCESS**

## 4. Production migration 0054

Production Supabase:

`iwiqprhoohkxvjyxojto`

Applied migration:

- ledger version: `20260924074254`
- name: `0054_p17_wu2_final_journey_lifecycle_cutover`

Result:

**SUCCESS**

### 4.1 Conservative bootstrap

Existing production rows were mapped from explicit legacy status only:

| Legacy status | Canonical lifecycle |
| --- | --- |
| draft | draft |
| registration_open | upcoming |
| preparing | active |
| completed | closeout_pending |
| archived | archived |

No start/end date was used to infer lifecycle.

Every existing Journey bootstrapped:

`application_state = closed`

Therefore the migration did not reopen recruitment.

### 4.2 Production Journey truth after bootstrap

Verified production state:

- archived QA Journey → `archived / closed`
- two former legacy `registration_open` Journeys → `upcoming / closed`
- legacy draft Journey → `draft / closed`
- legacy completed Journey → `closeout_pending / closed`

Important:

Legacy `completed` did **not** become Memory.

Production had:

- `0` open/paused application windows;
- `0` invalid lifecycle/application combinations;
- `0` Memory Journeys;
- `1` closeout-pending Journey.

## 5. Lifecycle transition enforcement

Production trigger:

`journeys_lifecycle_contract_guard`

Admin-only canonical transition graph:

- draft → upcoming / archived
- upcoming → draft / active / archived
- active → closeout_pending
- closeout_pending → active / memory
- memory → archived

No date-driven transition exists.

Legacy status is synchronized as compatibility projection:

- draft → draft
- upcoming → registration_open
- active → preparing
- closeout_pending → preparing
- memory → completed
- archived → archived

This projection is compatibility only.

## 6. Recruitment / Application Window authority

Production application submission policies now require:

- parent Journey `lifecycle_phase='upcoming'`;
- parent Journey `application_state='open'`;
- matching application mode;
- all inherited Phase-9/P16 path, replay, assignment and submission constraints.

Opening the application window is not a generic database update.

Production activation guard:

`public.tnc_journey_activation_guard_version() = 'p17-wu2-final-v1'`

OPEN remains protected by:

- authenticated Admin authority;
- Phase-9 activation secret;
- registration protection probes;
- canonical `upcoming` phase.

Release invariant:

**all application windows remain CLOSED.**

Public recruitment remains:

**HOLD**

## 7. Cutoff and Closeout continuity

WU2.Final reuses existing authoritative operational gates instead of replacing them.

### UPCOMING → ACTIVE

Runs the existing registration cutoff validator.

This preserves checks such as:

- Admin-only close;
- accepted-but-unconfirmed application blocker;
- roster/capacity safeguards.

### CLOSEOUT_PENDING → MEMORY

Runs the existing full P11 closeout validator.

Memory therefore still requires the existing reconciliation contract, including:

- current passed Closeout Review;
- attendance reconciliation;
- Field Update review;
- documentary review;
- impact reconciliation;
- evidence-staleness checks;
- Admin authority.

Memory cannot be created merely because a date elapsed or legacy status was completed.

## 8. Post-cutover defense-in-depth audit

After migration 0054, production audit found remaining database publication paths that still used legacy:

`status='completed'`

The application server/UI already failed closed, but direct Data API policy authority also had to be corrected.

Observed before HF1:

- 4 impact items with public-ish verification state on a Journey now canonical `closeout_pending`;
- 1 impact snapshot with public-ish verification state on that same non-Memory Journey;
- 0 currently published Reflections;
- Reflection publication policy was still unconditional;
- Reflection insertion guard still checked legacy completed.

WU2.Final was therefore **not closed** until the DB publication layer was hardened.

## 9. WU2.Final HF1 — Memory Authority DB Hardening

### PR #76

Title:
`P17-WU2.Final HF1: Memory authority DB hardening`

Final PR head:

`812d90d9938522ccb9e644863c41bb82f06ffb2e`

PR evidence:

- generic CI `35971728924`: **SUCCESS**
- P16-WU10B dedicated gate `35971728938`: **SUCCESS**
- HF1 Memory authority QA: **PASS**
- all inherited source/DB gates: **PASS**
- build/typecheck/Cloudflare dry-run: **PASS**

Squash-merged main:

`47bf6591cc1ec170de9aaa0574fd6040e5992a1c`

Post-merge main CI:

- run `35971886726`
- exact head `47bf6591cc1ec170de9aaa0574fd6040e5992a1c`
- conclusion: **SUCCESS**

### Production migration 0055

Applied migration:

- ledger version: `20260924075257`
- name: `0055_p17_wu2_final_memory_authority_hardening`

Result:

**SUCCESS**

### HF1 canonical result

The following are now canonical Memory-only publication paths:

- Journey Impact public policy;
- Journey Impact authenticated compatibility policy;
- Journey Impact Snapshot public policy;
- Journey Impact Snapshot authenticated compatibility policy;
- `impact_network_verified_claims`;
- Journey Reflection creation guard;
- published Journey Reflection public policy.

They now require parent:

`lifecycle_phase='memory'`

Post-HF1 audit:

- publication policies using legacy `completed`: **0**
- Reflection guard using legacy `completed`: **0**

Private own-data attendance/Memory projection remains unchanged because it is evidence/personal archive truth, not public Memory publication authority.

## 10. Production advisors

Supabase security/performance advisors were run before and after the WU2 production DDL.

No new WU2-specific security advisor finding was introduced.

Existing baseline warnings remain:

1. authenticated access to the intentionally protected `tnc_reveal_volunteer_identity_document` SECURITY DEFINER RPC from P16-WU10B;
2. Supabase Auth leaked-password protection disabled.

Existing performance advisory backlog also remains unchanged, including unindexed foreign-key / RLS init-plan / unused-index notices outside WU2 scope.

These are not regressions introduced by WU2.Final.

## 11. Production generated types sync

After 0054/0055, Supabase generated types confirmed new Journey columns:

- `application_state: string`
- `lifecycle_phase: string`

### PR #77

Title:
`P17-WU2.Final: sync production lifecycle types`

Final PR head:

`5534fc21de6503db249f62562d5f95adc9fc88e3`

PR evidence:

- generic CI `35972295559`: **SUCCESS**
- P16-WU10B dedicated gate `35972295564`: **SUCCESS**

Squash-merged final product main:

`2c37673680450c12742512945f870e2bece6e653`

Post-merge final main CI:

- run `35972476693`
- exact head `2c37673680450c12742512945f870e2bece6e653`
- conclusion: **SUCCESS**

Source, production schema and generated TypeScript database types are now reconciled.

## 12. Production/runtime boundary

WU2.Final changed production Supabase schema/policies intentionally.

It did **not**:

- open any application window;
- fabricate lifecycle from dates;
- fabricate attendance;
- mark a Journey Memory automatically;
- deploy a new Cloudflare production runtime;
- enable public recruitment;
- change social-consent/publication principles.

Cloudflare verification remained dry-run only.

## 13. Final decision

**P17-WU2 — COMPLETE / PASS.**

All WU2 stages are closed:

1. Gate 0 Source Canonicalization — COMPLETE / PASS
2. WU2.1 Lifecycle Schema & Source Contract — COMPLETE / PASS
3. WU2.2 Canonical Journey Routes & Field Journal Redirects — COMPLETE / PASS
4. WU2.3 Journey Index / Detail Composition — COMPLETE / PASS
5. WU2.4 Public Navigation Rebase — COMPLETE / PASS
6. WU2.5 Admin Lifecycle Controls — COMPLETE / PASS
7. WU2.6 VI/EN / Mobile / Regression QA — COMPLETE / PASS
8. WU2.Final Production Lifecycle Cutover & Closeout — COMPLETE / PASS
9. WU2.Final HF1 Memory Authority DB Hardening — COMPLETE / PASS
10. Production generated types reconciliation — COMPLETE / PASS

Canonical production invariant at close:

> Journey lifecycle and application state are explicit independent authorities.  
> Dates do not manufacture truth.  
> Recruitment is CLOSED unless explicitly opened through the protected Admin path.  
> Impact/Reflection public Memory publication requires canonical Memory phase.

Public recruitment remains **HOLD / CLOSED** after WU2 closeout.
