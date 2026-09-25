# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU7.Final
# DONATION & RESOURCE OPERATIONS — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-25

Status: **COMPLETE / CLOSED / PASS**

## 1. Scope

P17-WU7 closes the Journey-scoped Donation & Resource Operations foundation from Resource Need through Pledge, verified Receipt, Distribution, Reconciliation, Journey Control Center operations, public Resource Support UX, privacy/security/mobile regression, and production database cutover.

The canonical operational truth remains:

`Need != Pledge != Received != Distributed != Contribution != Impact`

Cash support remains external-channel only. TNC does not custody or reconcile money.

## 2. Canonical WU sequence

Completed and canonicalized:

- P17-WU7.0 — Donation & Resource Operations gap audit / architecture freeze
- P17-WU7.1 — Resource Need source contract
- P17-WU7.2 — Pledge / Donor Intent contract
- P17-WU7.3 — Receipt Verification
- P17-WU7.4 — Distribution & Reconciliation
- P17-WU7.5 — Journey Control Center Resource Workspace
- P17-WU7.6 — Public Resource Support UX
- P17-WU7.7 — Security / Privacy / Mobile / VI-EN regression QA
- P17-WU7.Final — Production cutover

## 3. Source PR evidence

### WU7.1

PR #115 — Resource Need source contract

Merged main:
`c715f7eda59a3672b438a3f5f64acc81d4b39c18`

### WU7.2

PR #116 — Pledge donor intent contract

Final head:
`9754203d7650b0b0668fd2af7941fd85f81f7231`

Merged main:
`4240c1e5294b4f95ffdfc0bf19386311a4b342e1`

Post-merge CI:
`36146552830` — SUCCESS

### WU7.3

PR #117 — Receipt verification contract

Final head:
`74a8655c8be9f217d6b674405f277b6cfacf71d6`

Merged main:
`e5a64b677c7f396e6d9235d2051e227307c84779`

Post-merge CI:
`36146920541` — SUCCESS

### WU7.4

PR #118 — Distribution and reconciliation contract

Final head:
`f63c470e0ecc078eff267d1c09aead77214f1139`

Exact-head:
- CI `36147583159` — SUCCESS
- inherited dedicated gate `36147583080` — SUCCESS

Merged main:
`3f6fd53a3003f85cf9f2d5f0a7e8e27b0fdbf3c9`

Post-merge CI:
`36147815707` — SUCCESS

### WU7.5–WU7.7

PR #119 — Resource operations experience

Final head:
`ef19964b3236095e1d062187d6b19e2cdc7a7fbf`

Exact-head:
- CI `36148904512` — SUCCESS
- inherited dedicated gate `36148904427` — SUCCESS

Merged main:
`f30097d56c4c7f55eef7114c7b016aef1ca070e7`

Post-merge CI:
`36149106668` — SUCCESS

### WU7.Final

PR #120 — Donation Resource Operations production cutover

Final exact head:
`a2690492fdfb9475f8bceb21044a6ceb70cb6870`

Exact-head:
- CI `36149493112` — SUCCESS
- inherited dedicated gate `36149493186` — SUCCESS

Squash-merged production main:
`caf25d7d4f110e79939c32f8eb92512202454b8e`

Post-merge main CI:
`36159379194` — SUCCESS

Post-merge CI includes:
- WU7.1–WU7.7 source/DB regressions;
- WU7.Final production source gate;
- WU7.Final migration + guarded rollback ephemeral DB QA;
- inherited P9–P17 DB regressions;
- build;
- typecheck;
- Cloudflare dry-run.

## 4. Production migration

Source:
`database/migrations/0062_p17_wu7_final_donation_resource_operations.sql`

Git blob SHA:
`1cb543a38f938c5ca5219fcf558b34df63c6dc77`

Guarded rollback:
`database/rollbacks/p17_wu7_final_donation_resource_operations.sql`

Supabase project:
`iwiqprhoohkxvjyxojto`

Production migration ledger:
`20260925161548_p17_wu7_final_donation_resource_operations`

Apply result:
**SUCCESS**

## 5. Production source tables

Production now contains:

- `journey_resource_needs`
- `journey_resource_pledges`
- `journey_resource_receipts`
- `journey_resource_distributions`
- `journey_resource_reconciliations`
- `journey_cash_support_notices`

All six tables have Row Level Security enabled.

Each table has three intended operational policies for its scoped read/write model.

No normal hard-delete workflow is exposed for operational resource truth.

## 6. Resource Need truth

Resource Need is Journey-scoped.

Initial categories:
- in_kind
- service
- transport
- food
- equipment
- venue
- other

A Need has:
- requested quantity;
- unit;
- VI/EN copy;
- internal/public visibility;
- open/closed state;
- optional target date;
- operational audit fields.

A public Need may exist while volunteer Application Window remains CLOSED.

Resource Support is intentionally independent from volunteer recruitment.

## 7. Pledge / Donor Intent truth

Pledge is authenticated donor intent.

A donor:
- can create a Pledge;
- sees only their own Pledge rows through RLS;
- cannot self-accept or self-reject;
- may cancel a pending Pledge.

Admin/BTC review owns accepted/rejected transitions.

Donor identity remains private operational truth.

Pledge does not reduce Remaining Need.

## 8. Receipt truth

Receipt is BTC/Admin-verified actual receipt.

Server records the verifier identity.

Same-Journey / same-Need foreign-key authority prevents a receipt from being attached to an unrelated Pledge.

Canonical calculation:

`Remaining Need = max(Requested - Verified Received, 0)`

Pledge quantity is intentionally excluded.

## 9. Distribution truth

Distribution is verified actual use/distribution of received resources.

Canonical invariant:

`Distributed <= Verified Received`

The distribution guard locks the Resource Need row and calculates already-distributed quantity transactionally before accepting a write.

Therefore concurrent distribution writes cannot legitimately overspend the verified received resource balance.

Derived available quantity:

`Available = max(Received - Distributed, 0)`

## 10. Reconciliation truth

Reconciliation is available only in closeout/memory/archived lifecycle authority.

Allowed dispositions:

- surplus
- retained
- returned
- transferred
- unresolved

Reconciliation records the actual undistributed verified balance.

After a Need is reconciled:
- later Distribution is blocked;
- later Receipt mutation/addition for the reconciled Need is blocked.

This preserves closeout truth.

## 11. Public Resource Support

Public source access is through narrow projection functions rather than direct anon table reads.

Public wrappers:

- `public.tnc_public_journey_resource_needs(uuid)`
- `public.tnc_public_journey_resource_support(uuid)`
- `public.tnc_public_journey_cash_support(uuid)`

Public wrappers are SECURITY INVOKER.

Narrow private projection helpers are pinned SECURITY DEFINER functions.

Anonymous callers have EXECUTE authority for the public resource-support and cash-support projections, while anon has no direct SELECT authority on the Resource Need source table.

Public Resource Support may expose:
- explicitly public/open Needs;
- requested quantity;
- unit;
- optional Received/Remaining aggregate only when progress visibility is explicitly public.

It never exposes:
- donor identity;
- Pledge identity;
- internal notes;
- verifier identity;
- operational Distribution detail.

## 12. Cash support boundary

`journey_cash_support_notices` contains an optional Journey-owned external support notice.

Rules:
- only HTTPS external URL;
- BTC/Admin controls the notice;
- public sees only an enabled notice on eligible Journey lifecycle;
- TNC does not infer payment success from link usage;
- TNC does not create wallet/balance/payout/settlement/accounting truth.

Cash support remains:

**External official channel only.**

## 13. Journey Control Center

The previously-reserved Control Center section:

`Nguồn lực`

is now active.

The admin workspace supports operational Resource Need / Pledge / Receipt / Distribution / reconciliation management according to source authority.

This remains Journey operations, not:
- ERP;
- warehouse management;
- procurement;
- accounting;
- CRM;
- crowdfunding.

## 14. Production data truth at cutover

Immediately after migration 0062:

- Resource Needs rows = 0
- Pledge rows = 0
- Receipt rows = 0
- Distribution rows = 0
- Reconciliation rows = 0
- Cash Support Notice rows = 0

Therefore no fake resource, donor, receipt or distribution truth was seeded.

Existing truth remains:

- `community_contributions` rows = 0
- `journey_impact_items` rows = 4
- Shared Journey experience edges = 0
- participant rows with attendance truth = 0
- non-closed volunteer Application Windows = 0

WU7 production cutover did not manufacture:
- Participation;
- Attendance;
- Memory;
- Shared Journey;
- Contribution;
- Impact.

Recruitment remains:

**HOLD / CLOSED**

## 15. RLS / privilege verification

Verified:
- RLS ON for all 6 WU7 production tables;
- anon direct SELECT on Resource Need source = false;
- authenticated source grants exist only where required, with row authorization enforced through RLS;
- service_role retains required operational authority;
- anon EXECUTE on public Resource Support projection = true;
- anon EXECUTE on public Cash Support projection = true.

Privileged projection helpers remain in `private`.

Trigger guards are SECURITY INVOKER and enforce operational actor/transition rules.

## 16. Performance hardening

WU7.Final proactively adds 17 covering indexes for WU7 foreign-key paths.

Production verification:
- expected WU7 covering indexes = 17
- present = 17

Supabase Performance Advisor after cutover does not report WU7 Resource tables among the unindexed-FK findings.

Historical unrelated performance findings remain outside WU7 scope and were not silently bundled into this cutover.

## 17. Security Advisor

Post-cutover Security Advisor reports no WU7-specific security regression.

Pre-existing warning remains:

`Leaked Password Protection Disabled`

This is not caused by WU7 and was not silently changed.

## 18. Product invariants preserved

WU7 does not turn Donation into participation.

A donor does not automatically become:
- Journey participant;
- attendee;
- Shared Journey member;
- Community poster;
- public supporter;
- Impact contributor.

Partner relationship is not receipt truth.

Contribution history is not pledge truth.

Impact is downstream and remains separately verified/publication-controlled.

## 19. Final decision

P17-WU7 production source, UX, security, database cutover, RLS and production verification all pass.

# FINAL STATUS

**P17-WU7 — COMPLETE / CLOSED / PASS**

Production capability:

**Donation & Resource Operations foundation ACTIVE, empty-by-default, Journey-scoped, privacy-preserving, with recruitment still HOLD/CLOSED.**

Next canonical roadmap item:

**P17-WU8 — DAY-OF OPERATIONS & ATTENDANCE**
