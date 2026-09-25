# TRẠM NỤ CƯỜI — PHASE 17 / P17-WU7.0
# DONATION & RESOURCE OPERATIONS — CANONICAL GAP AUDIT & ARCHITECTURE FREEZE

Date: 2026-09-25

Status: COMPLETE / PASS — AUDIT & ARCHITECTURE FREEZE ONLY

## 1. Purpose

P17-WU7.0 establishes the canonical product and architecture boundary for Journey-scoped Donation & Resource Operations.

This work unit is audit-only.

It does NOT:
- change production schema;
- create donation/resource rows;
- open recruitment;
- activate Journey Community v2 runtime;
- mutate attendance, Memory, Shared Journey, Impact, or participant truth;
- create payment custody;
- create accounting, ERP, inventory, warehouse, or crowdfunding functionality.

## 2. Canonical product authority

Authoritative source:
`canon/PRODUCT_REASSESSMENT_JOURNEY_OPERATING_MODEL_CANON_2026-09-22.md`

Locked product truths:

1. Donation/resources are a capability of a Journey.
2. Campaign is not a required core product object.
3. TNC commonly gathers material support to prepare a specific Journey and takes those resources into the field itself.
4. Journey Control Center contains a Resources & Donations area.
5. Before a Journey, resources/donations support preparation.
6. During a Journey, resource distribution may need operational tracking.
7. After a Journey, donation/resource reconciliation is part of closeout.
8. Cash and in-kind support are distinct flows.
9. Cash support links to an appropriate official external channel; TNC Web App is not payment/custody infrastructure.
10. In-kind truth must distinguish Need, Pledge, Received, Remaining, and later Distributed / surplus / unresolved.
11. Pledge != Received.
12. Received != Distributed.
13. Remaining need is based on verified received quantity, not promises.
14. Donation/resource transparency may be surfaced publicly where appropriate.
15. Non-participant TNC users may donate subject to Journey policy.
16. TNC-controlled crowdfunding/payment custody remains explicitly out of scope.

## 3. Current product/source audit

### Journey Control Center

Current production source already reserves:

`resources -> "Nguồn lực" -> status future -> owner WU7`

The section exists in the Control Center map but has no active manager/component.

Therefore WU7 should activate the reserved Journey-scoped area rather than create a separate top-level product.

### Existing WU3 operational foundation

Current Event Management foundation includes:
- departments;
- teams;
- runbook;
- official updates.

These are Journey-scoped operational sources and are Admin-controlled at the source layer.

WU7 should follow the same Journey-scoped operational architecture rather than introduce a generic organization-wide inventory subsystem.

## 4. Production database gap audit

Supabase project:
`iwiqprhoohkxvjyxojto`

Production inspection found no table or column whose current semantic contract represents:
- donation need;
- resource need;
- pledge;
- donor offer;
- receipt;
- distribution;
- resource reconciliation;
- cash support appeal;
- donor transaction.

Therefore Donation & Resource Operations is currently a real production capability gap.

## 5. Existing tables that MUST NOT be overloaded

### 5.1 community_contributions

Production table:
`public.community_contributions`

Relevant current semantics:
- belongs to a user;
- may be Journey- or Project-related;
- contribution types include `resource`;
- requires a verification method;
- status is only `active | revoked`;
- represents contribution history / verified contribution evidence.

Production row count at WU7.0 audit:
`0`

Decision:

**DO NOT use `community_contributions` as the operational pledge/receipt ledger.**

Reason:
- it has no Need/Pledge/Received/Distributed lifecycle;
- `user_id` is mandatory;
- its verification semantics are retrospective contribution truth;
- using it for promises would collapse "Pledge != Received";
- using it for distribution would collapse operational and impact truth.

Future WU9 may derive verified contribution history from WU7 receipt/distribution evidence if product policy allows, but WU7 must not silently write contribution history merely because a donor made a pledge.

### 5.2 journey_impact_items

Production table:
`public.journey_impact_items`

Current semantics:
- Journey-level impact/result item;
- evidence/verification governed;
- public visibility is tied to verified/legacy-public state and Memory lifecycle.

Production row count at WU7.0 audit:
`4`

Decision:

**DO NOT use `journey_impact_items` for live resource operations.**

Reason:
- impact/result truth is downstream of operations;
- resource need, pledge and receipt are not impact claims;
- WU9 closeout may later publish verified aggregate results from reconciled WU7 truth.

### 5.3 project_partner_relationships

Existing relationship types include:
- collaboration;
- funding;
- csr_support;
- institutional_support;
- in_kind;
- other.

Decision:

Partner relationships may identify a verified supporting organization, but they are **not** the resource ledger.

WU7 may optionally reference an existing partner relationship where a support commitment is organization-backed, but must not infer quantity received or distributed from the relationship row.

## 6. Architecture freeze

### 6.1 Primary object

The primary object remains:

**Journey**

Donation/resource objects MUST be Journey-scoped.

No generic top-level Campaign object.

### 6.2 Two canonical flows

#### A. Cash support

Purpose:
allow a Journey to explain a financial need and direct the user to an official external support channel.

WU7 Web App responsibility:
- display purpose/context;
- display approved support instructions;
- link to official external channel;
- optionally show an approved public target/summary if BTC chooses to publish it.

WU7 Web App MUST NOT:
- accept card/bank payment;
- custody funds;
- build wallet/balance;
- issue financial settlement;
- infer donor payment success from clicking the link;
- create a donor transaction record merely because the CTA was used.

Any received-money/accounting truth remains outside WU7 operational source unless Owner later approves separate financial architecture.

Journey-level financial summary remains a closeout/publication concern for WU9.

#### B. In-kind / service / operational resource support

Canonical truth chain:

`NEED -> PLEDGE -> RECEIVED -> DISTRIBUTED / REMAINING / SURPLUS / UNRESOLVED`

Each transition represents a different fact.

The system must never collapse them into one generic "donated" status.

## 7. Canonical domain model

WU7 implementation should use four distinct operational concepts.

### 7.1 Resource Need

Represents what the Journey actually needs.

Minimum semantic fields:
- Journey;
- title;
- description;
- category;
- requested quantity;
- unit;
- public/private visibility;
- active/closed state;
- optional target date;
- optional operational note;
- ordering.

Need categories should remain lightweight and extensible.

Recommended initial categories:
- `in_kind`
- `service`
- `transport`
- `food`
- `equipment`
- `venue`
- `other`

Do not hard-code charity-specific commodity taxonomies.

Cash support is represented separately as an external-support notice, not as a quantity-bearing warehouse item.

### 7.2 Resource Pledge

Represents an intention/commitment to support a specific Need.

Minimum semantic fields:
- Journey;
- Need;
- pledging identity;
- pledged quantity;
- note;
- status;
- created/updated timestamps.

Initial pledge lifecycle:

`pending -> accepted -> cancelled | rejected`

An accepted pledge is still **not received truth**.

A non-participant authenticated TNC user may pledge subject to Journey policy.

Operational donor identity is private by default.

Public donor recognition, if later desired, requires a separate explicit publication/consent decision and must not be inferred from pledge/receipt existence.

### 7.3 Resource Receipt

Represents BTC verification that resources were actually received.

Minimum semantic fields:
- Journey;
- Need;
- optional Pledge link;
- received quantity;
- received timestamp/date;
- verified by;
- note;
- optional evidence reference.

Receipt truth is BTC-controlled.

A user cannot self-certify that pledged resources were received.

Remaining need calculation:

`max(requested quantity - verified received quantity, 0)`

Accepted pledges MUST NOT reduce Remaining.

### 7.4 Resource Distribution

Represents actual operational distribution/use of received resources.

Minimum semantic fields:
- Journey;
- Need;
- optional receipt/source reference;
- distributed quantity;
- distribution time/date;
- verified by;
- destination/context note;
- optional evidence reference.

Distribution must not exceed valid received availability unless the reconciliation model explicitly records an approved adjustment.

Distribution does not automatically create public Impact truth.

## 8. Reconciliation rules

For each Need:

- `pledged_quantity` = active/accepted pledge aggregate;
- `received_quantity` = verified receipt aggregate;
- `distributed_quantity` = verified distribution aggregate;
- `remaining_need` = max(need - received, 0);
- `undistributed_received` = max(received - distributed, 0).

At closeout, BTC must be able to classify undistributed received resources as:
- surplus;
- retained for approved follow-up;
- returned;
- transferred under an approved operational note;
- unresolved.

WU7 should preserve this truth for WU9 closeout.

## 9. Public/private boundary

### Public may see, when explicitly published:
- active Journey resource needs;
- requested quantity/unit where appropriate;
- received progress aggregate where approved;
- remaining need aggregate;
- official external cash-support CTA;
- approved partner/supporter acknowledgement.

### Public must NOT automatically see:
- donor account/user ID;
- private donor contact data;
- internal BTC notes;
- rejected/cancelled pledges;
- receipt verification actor;
- internal evidence identifiers;
- distribution operational detail;
- bank reconciliation/accounting records.

### Authenticated donor may see:
- own pledges;
- status of own pledges;
- public Need context;
- receipt acknowledgement linked to their own pledge only if policy permits.

### Admin/BTC operational authority

Source mutation remains controlled by trusted Journey operations roles.

At WU7 foundation stage, default source authority should remain Admin-first unless an already-canonical Journey-scoped BTC authorization helper is proven safe for direct reuse.

Do not broaden Editor into operational donor/receipt authority merely for convenience.

## 10. Identity boundary

Donation/resource support is not participation truth.

A donor:
- does not become a Journey participant;
- does not gain attendance truth;
- does not gain Shared Journey evidence;
- does not gain Journey Community original-post rights merely from donating;
- does not automatically become a public supporter.

A Journey participant may also donate, but these are independent relationships.

## 11. Partner boundary

An organization may support a Journey through an existing verified partner relationship.

WU7 should allow an optional reference to that relationship.

However:
- Partner relationship != pledge.
- Partner relationship != receipt.
- Partner relationship != distribution.
- Funding/in-kind relationship labels do not prove quantity or completion.

## 12. Impact / contribution boundary

WU7 produces operational resource truth.

It does NOT directly produce:
- public impact claims;
- Community contribution history;
- beneficiary outcomes;
- attendance;
- Memory;
- Shared Journey evidence.

WU9 may consume reconciled WU7 truth to create verified closeout/results projections with explicit evidence and publication controls.

## 13. Security architecture

Required principles for implementation:

- all public-schema WU7 source tables use RLS;
- Data API grants are explicit;
- public reads use narrow projections/RPCs rather than exposing operational source tables broadly;
- donor ownership checks must be server-authoritative;
- do not authorize from `user_metadata`;
- never expose `service_role`;
- do not add `SECURITY DEFINER` merely to fix a permission error;
- any privileged helper must remain private, least-privilege, have an explicit authorization predicate, and have direct EXECUTE revoked unless intentionally exposed;
- admin/BTC verification actions must record actor/time;
- public aggregate views must not leak donor identity through joins or counts small enough to identify an individual.

## 14. Control Center UX freeze

WU7 should activate the existing Control Center section:

`Nguồn lực`

Admin/BTC Journey Resource workspace should answer:

1. Journey đang cần gì?
2. Cần bao nhiêu?
3. Đã có bao nhiêu người/đơn vị cam kết?
4. Đã nhận thực tế bao nhiêu?
5. Còn thiếu bao nhiêu?
6. Đã phân phối/sử dụng bao nhiêu?
7. Còn tồn/surplus/unresolved bao nhiêu?
8. Có pledge nào cần BTC xử lý?
9. Có Need nào sắp đến hạn nhưng vẫn thiếu?

This workspace is not:
- warehouse ERP;
- procurement system;
- accounting;
- payment processor;
- CRM.

## 15. Public Journey UX freeze

Before Journey:
- show approved active resource needs;
- show remaining need based on received truth;
- provide pledge/support action;
- provide external cash-support CTA where configured.

During Journey:
- public UX may continue showing approved support needs if still relevant;
- operational distribution remains private unless intentionally summarized.

After Journey:
- WU7 stops being the public "ask" surface once Journey policy/closeout closes support;
- WU9 owns reconciled results/financial/resource summary publication.

## 16. Lifecycle rules

Default behavior:

### draft
- BTC may prepare Needs privately.
- no public pledge action.

### upcoming
- Needs may be published.
- pledge flow may be open independently from volunteer recruitment.

### active
- pledge may remain open only if Journey policy allows.
- receipts/distributions are operationally active.

### closeout_pending
- new pledges default closed.
- receipt/distribution reconciliation continues.

### memory / archived
- operational mutation is restricted to controlled correction paths.
- public result presentation belongs to WU9 projections, not live WU7 operational tables.

Donation/resource support state must NOT be coupled to volunteer Application Window.

Recruitment can remain CLOSED while resource support is open, if Journey policy permits.

## 17. No-go architecture

WU7 MUST NOT introduce:
- Campaign as required top-level object;
- payment gateway;
- wallet;
- bank balance;
- payout;
- donor settlement;
- accounting ledger;
- expense ledger;
- invoice system;
- warehouse locations/bins;
- purchase orders;
- procurement workflow;
- stock valuation;
- generic sponsor CRM;
- public donor leaderboard;
- engagement ranking;
- "amount raised" inferred from external-link clicks;
- impact claims generated from pledges.

## 18. Production invariants for WU7 implementation

Until WU7.Final explicitly passes:

- recruitment remains HOLD / CLOSED;
- no fake resource/donation data;
- no fake donor identity;
- no attendance mutation;
- no Memory mutation;
- no Shared Journey evidence;
- no Community v2 runtime activation as a side effect of WU7;
- no Impact mutation;
- no external-payment custody;
- no public donor disclosure without explicit publication/consent policy.

## 19. Recommended implementation sequence

### P17-WU7.1 — Resource Need Source Contract
Create Journey-scoped Need + public/private visibility foundation.

### P17-WU7.2 — Pledge / Donor Intent Contract
Implement authenticated non-participant/participant pledge flow with private donor identity and BTC review.

### P17-WU7.3 — Receipt Verification
Implement BTC-controlled actual receipt truth and Remaining calculation.

### P17-WU7.4 — Distribution & Reconciliation
Implement distribution, available balance, surplus/unresolved closeout states.

### P17-WU7.5 — Journey Control Center Resource Workspace
Activate `Nguồn lực` in the existing Control Center.

### P17-WU7.6 — Public Journey Donation/Resource UX
Publish approved Needs, progress aggregates, pledge action, and external cash-support CTA.

### P17-WU7.7 — Security / Privacy / Mobile / VI-EN Regression QA
Verify RLS, projections, donor privacy, mobile and bilingual behavior.

### P17-WU7.Final — Production Cutover & Canonical Closeout
Only after exact-head source/DB QA PASS and production verification.

## 20. WU7.0 final decision

Architecture is frozen as:

**Journey-scoped Resource Operations with separate Need, Pledge, Receipt and Distribution truth.**

Cash support remains:

**External official channel only; no TNC payment/custody architecture.**

Existing `community_contributions`, `journey_impact_items` and partner relationship sources are preserved and not overloaded.

# FINAL STATUS

**P17-WU7.0 — COMPLETE / PASS**

Next:

**P17-WU7.1 — RESOURCE NEED SOURCE CONTRACT**
