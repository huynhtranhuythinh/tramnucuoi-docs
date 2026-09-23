# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2 — JOURNEY LIFECYCLE & INFORMATION ARCHITECTURE

Date: 2026-09-22  
Status: **OPEN — GATE 0 COMPLETE / PASS**  
Owner: Jean Huynh  
CTO / Product Architect / QA Lead: ChatGPT  
Builder: Lovable only when explicitly needed

## 1. Canonical input

P17-WU2 starts from:

- `canon/PRODUCT_REASSESSMENT_JOURNEY_OPERATING_MODEL_CANON_2026-09-22.md`;
- corrected P17-WU1 audit:
  `canon/PHASE_17_WU1_CANONICAL_PRODUCT_MODEL_CURRENT_STATE_GAP_AUDIT_2026-09-22.md`.

P17-WU1 correction commit:

`145e3420f4a2f9b34c81f77994c43970c030c442`

The WU1 correction establishes:

- actual product `main` before WU2 Gate 0 = `dc75bf19c15fdf39cf1ad95fdf650b1453ac97f3`;
- P16-WU10A is an ancestor, not a divergent history;
- exact WU10B migrations 0052/0053 and rollbacks are present under `database/migrations/`;
- the real Gate 0 drift was generated Supabase types + inherited CI coverage.

## 2. WU2 objective

Make **Journey / Hành Trình** the canonical public, operational and social object without rewriting the evidence/privacy foundation.

WU2 owns:

1. source canonicalization baseline;
2. Journey lifecycle authority;
3. application-window separation;
4. canonical public Journey route family;
5. Field Journal route separation;
6. Journey page information architecture;
7. top-level public navigation rebase;
8. lifecycle transition contract into Closeout / Memory.

WU2 does **not** build:

- Department / Team staffing engine;
- full Event Management Control Center;
- Participant Workspace;
- Donation/resource ledger;
- Journey participant publishing;
- final Closeout reconciliation model.

Those remain later P17 WUs.

## 3. Immutable truth boundaries

WU2 must preserve:

- account != participant;
- application != participant;
- confirmed participation != attendance;
- `attended_party_size = NULL` = unresolved;
- `attended_party_size = 0` = verified no-show;
- `attended_party_size > 0` = verified attended;
- participant claim != attendance;
- Journey Presence != attendance;
- interaction != attendance;
- Memory requires evidence;
- Reflection source != public publication;
- shared-experience edges require verified attendance + governed consent;
- operational/private truth is separate from public/social publication.

No elapsed date may manufacture attendance, Closeout, Memory, Contribution, relationship or impact truth.

## 4. Gate 0 — Source canonicalization

Product branch:

`p17-wu2-source-canonicalization`

PR:

`#66 — P17-WU2 Gate 0: canonicalize production schema contract`

Pre-merge head:

`52486b4d224b9571b2cfcefb020ec739633e8da5`

Changes:

- regenerated `src/integrations/supabase/types.ts` from production Supabase project `iwiqprhoohkxvjyxojto`;
- generic CI now inherits P16-WU10B source QA;
- generic CI now inherits P16-WU10B ephemeral DB / Vault QA;
- added `scripts/p17-wu2-source-canonicalization-qa.ts`.

Pre-merge evidence:

- generic CI: PASS;
- dedicated P16-WU10B Volunteer Pilot Gate: PASS;
- WU10B source/privacy invariants: PASS;
- WU10B schema/RLS/Vault invariants: PASS;
- all inherited P9–P16 gates: PASS;
- build: PASS;
- typecheck: PASS;
- Cloudflare production-config dry-run: PASS.

PR #66 squash merge:

`f78fabb329ab994510f9325ce644a30386b38673`

No production database mutation occurred in Gate 0.

## 5. Canonical route authority decision

### 5.1 Vietnamese

Operational Journey becomes canonical:

- `/hanh-trinh`
- `/hanh-trinh/:slug`

Field Journal moves to:

- `/nhat-ky`
- `/nhat-ky/:slug`

Legacy operational routes:

- `/journeys` → redirect to `/hanh-trinh`
- `/journeys/:slug` → redirect to `/hanh-trinh/:slug`

### 5.2 English

Keep the already-clean plural operational route:

- `/en/journeys`
- `/en/journeys/:slug`

Move editorial Field Journal from singular Journey route to:

- `/en/journal`
- `/en/journal/:slug`

Legacy editorial routes:

- `/en/journey` → redirect to `/en/journal`
- `/en/journey/:slug` → redirect to `/en/journal/:slug`

### 5.3 Vietnamese legacy slug collision rule

Because `/hanh-trinh/:slug` was historically Field Journal but becomes operational Journey:

1. operational Journey slug is authoritative when it exists;
2. if no operational Journey exists but a legacy published Field Journal post exists, route compatibility redirects to `/nhat-ky/:slug`;
3. no editorial post is deleted or silently repurposed.

This avoids broken indexed links while making Hành Trình the primary product object.

## 6. Lifecycle target model

Current legacy `journeys.status` conflates publication/lifecycle and recruitment.

WU2 target separates:

### Journey lifecycle phase

- `draft`
- `upcoming`
- `active`
- `closeout_pending`
- `memory`
- `archived`

### Application window

- `closed`
- `open`
- `paused`

The existing legacy `status` must remain during migration/compatibility until all source paths have moved to the new authority.

It must not be dropped or reinterpreted destructively in the first migration.

## 7. Lifecycle transition contract

Canonical transitions:

`draft → upcoming → active → closeout_pending → memory → archived`

Rules:

- application window is independently controlled;
- date alone does not transition phase;
- opening applications does not change attendance truth;
- Journey may be `upcoming` with applications closed;
- Journey may be `active` with applications closed;
- `closeout_pending` explicitly means operational reconciliation is not complete;
- `memory` is allowed only after manual Closeout authority confirms required reconciliation;
- `archived` is an administrative visibility/storage state, not evidence that Closeout passed.

## 8. Existing production rows — reconciliation rule

WU2 must not silently infer real-world state from dates.

Existing legacy rows remain historically intact while a controlled reconciliation maps them into the new phase/application model.

At minimum:

- no currently past Journey may remain publicly recruitable merely because legacy `status='registration_open'`;
- recruitment remains HOLD until lifecycle/application reconciliation and privacy release gates pass;
- unresolved attendance remains unresolved.

## 9. Canonical Journey page IA

One Journey page changes emphasis by lifecycle and viewer authority.

### Before Journey

Primary:

- objective;
- beneficiary/local context;
- date/location;
- activity plan;
- participant/TNV needs;
- resource needs;
- partners;
- participation CTA when application window is open.

### Approved participant

Same Journey page later exposes the Personal Journey Workspace entry point.

WU2 defines the IA slot only; Participant Workspace implementation is later.

### Active Journey

Primary:

- authoritative operational instructions;
- schedule/meeting point;
- official updates;
- Journey-scoped Community.

### Closeout pending

Primary:

- truthful “đang đối soát / reconciling” state;
- no premature impact/Memory claims.

### Memory

Primary:

- verified result;
- official story/media;
- reconciled resources/impact/financial summary where applicable;
- partners;
- Memory/Reflection continuity.

## 10. Public navigation target

Top-level direction:

- Trang chủ / Home
- TNC / About
- Dự án / Projects
- Hành Trình / Journeys
- Tác động / Impact
- Đồng hành / Get Involved
- My TNC when authenticated

“Cộng đồng / Community” remains a Journey capability and authenticated continuity layer, not a required global mental model for understanding the product.

## 11. Structured-field authority

Structured Journey fields are authoritative for:

- lifecycle phase;
- application state;
- start/end dates;
- location;
- capacity;
- project link.

Editorial story text may add context but may not become a second authority for those facts.

Source/UI should avoid duplicating date/location facts in uncontrolled narrative fields where drift can mislead participation.

## 12. Supabase implementation rule for Phase 17

Any new public-schema table/function introduced after Gate 0 must declare:

- explicit grants;
- RLS where applicable;
- narrow policies;
- function EXECUTE privileges explicitly;
- `SECURITY DEFINER` only when required and with fixed/empty `search_path`.

Phase 17 must not rely on implicit future Data API grants.

## 13. WU2 execution sequence

### Gate 0 — Source canonicalization
Status: **COMPLETE / PASS**.

Post-merge main:
`f78fabb329ab994510f9325ce644a30386b38673`

Post-merge main CI:
- workflow: `CI`
- run: `35733776365`
- event: `push`
- exact head: `f78fabb329ab994510f9325ce644a30386b38673`
- conclusion: **SUCCESS**

Gate 0 therefore closes on the actual production branch source, not only the PR head.

### WU2.1 — Lifecycle schema/source contract
Status: **COMPLETE / PASS — SOURCE & SCHEMA CONTRACT; NO PRODUCTION DDL**

Canonical record:
`canon/PHASE_17_WU2_1_LIFECYCLE_SCHEMA_SOURCE_CONTRACT_2026-09-22.md`

Product evidence:
- PR #67 merged;
- product main: `b870525511e346e2f06ed10c0270823c078b7131`;
- post-merge main CI `35736281689`: **SUCCESS**;
- lifecycle phase and application state are separately defined;
- existing-row bootstrap is fail-closed;
- legacy status remains untouched on production;
- no date-derived lifecycle transition;
- no production DDL / feature-flag / runtime mutation.


### WU2.2 — Canonical routes / redirects
Status: **COMPLETE / PASS**

Canonical record:
`canon/PHASE_17_WU2_2_CANONICAL_JOURNEY_ROUTES_FIELD_JOURNAL_REDIRECTS_2026-09-23.md`

Product evidence:
- PR #68 merged;
- product main: `cb18d0a3a8c030b347f215d9b8df7f7d6be485eb`;
- final PR-head generic CI `35802506826`: **SUCCESS**;
- P16-WU10B regression `35802506775`: **SUCCESS**;
- post-merge main CI `35802626064`: **SUCCESS**;
- VI operational Journey canonical: `/hanh-trinh/*`;
- EN operational Journey canonical: `/en/journeys/*`;
- VI Field Journal canonical: `/nhat-ky/*`;
- EN Field Journal canonical: `/en/journal/*`;
- permanent legacy redirects verified;
- no production DB / feature flag / runtime activation.


### WU2.3 — Journey index/detail composition
Status: **COMPLETE / PASS**

Canonical record:
`canon/PHASE_17_WU2_3_JOURNEY_INDEX_DETAIL_COMPOSITION_2026-09-23.md`

Product evidence:
- PR #69 merged;
- product main: `f463302adf98b0d009bbbc1be21e58e4283d129a`;
- final PR-head generic CI `35806619027`: **SUCCESS**;
- P16-WU10B regression `35806619166`: **SUCCESS**;
- post-merge main CI `35806736533`: **SUCCESS**;
- public index/detail now compose from Phase-17 lifecycle vocabulary;
- legacy `registration_open` is fail-closed for recruitment presentation;
- legacy `completed` maps to `closeout_pending`, not Memory;
- public Impact and Social Continuity are withheld before canonical Memory;
- no production lifecycle DDL / feature flag / runtime activation.


### WU2.4 — Public navigation rebase
Status: **COMPLETE / PASS**

Canonical record:
`canon/PHASE_17_WU2_4_PUBLIC_NAVIGATION_REBASE_2026-09-23.md`

Product evidence:
- PR #70 merged;
- product main: `1ed29ff7898a7e03b955a7cf5bde1c930c8540c9`;
- final PR-head CI `35808307142`: **SUCCESS**;
- WU10B regression `35808307185`: **SUCCESS**;
- post-merge main CI `35808410110`: **SUCCESS**;
- Journey is primary participation navigation;
- Impact has bilingual public routes;
- Field Journal is secondary editorial navigation;
- Community is de-emphasized globally; authenticated My TNC remains governed;
- no production DB / recruitment activation.


### WU2.5 — Admin lifecycle controls
- separate phase and application-window controls;
- explicit transition guards;
- past open Journey reconciliation.

### WU2.6 — VI/EN / mobile / regression QA
- reciprocal routes;
- redirect compatibility;
- no truth-boundary regression;
- no recruitment side effect.

## 14. Production gate

No WU2 production DB mutation is authorized merely by this design record.

Required before production migration:

1. source migration complete;
2. ephemeral DB QA;
3. full inherited CI PASS;
4. security review/advisors;
5. exact migration reviewed against current production truth;
6. explicit production migration gate.

Public recruitment remains HOLD throughout WU2 until later activation criteria are satisfied.
