# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU7.4 — DISTRIBUTION & RECONCILIATION

Date: 2026-09-25
Status: **COMPLETE / CLOSED / PASS — SOURCE CONTRACT MERGED; NO PRODUCTION DATABASE CUTOVER**

## Canonical truth

`Received != Distributed`

Distribution is BTC/Admin-verified operational truth and may never exceed verified Received quantity.

At closeout, undistributed received resources are classified as:
- surplus;
- retained;
- returned;
- transferred;
- unresolved.

Reconciliation quantity is server-derived from actual undistributed Received truth.

## Evidence

PR: `#118 — P17-WU7.4: Distribution and reconciliation contract`

Final exact head:
`f63c470e0ecc078eff267d1c09aead77214f1139`

Exact-head:
- generic CI `36147583159` — **SUCCESS**
- inherited dedicated gate `36147583080` — **SUCCESS**

Squash-merged main:
`3f6fd53a3003f85cf9f2d5f0a7e8e27b0fdbf3c9`

Post-merge main CI:
`36147815707` — **SUCCESS**

Source:
- `database/contracts/p17_wu7_4_distribution_reconciliation.sql`
- `scripts/p17-wu7-4-distribution-source-qa.ts`
- `scripts/p17-wu7-4-distribution-db-qa.sql`

## Transactional safety

Distribution guard:
- locks the Resource Need row;
- sums existing Distribution rows;
- derives verified Received quantity;
- rejects any write whose aggregate Distributed quantity would exceed Received.

This prevents over-distribution from being accepted as valid operational truth, including concurrent paths serialized by the Need row lock.

Reconciliation:
- allowed only in `closeout_pending | memory | archived`;
- locks the Need row;
- derives available quantity as `max(received - distributed, 0)`;
- server-stamps reconciliation actor/time;
- locks later Receipt and Distribution writes for a reconciled Need.

## Corrective QA history

An early exact-head CI run failed because the self-contained ephemeral fixture omitted SELECT/UPDATE grants on the Resource Need source table while the production WU7.1 contract grants those operations to the authenticated role and relies on RLS for authorization.

The fixture was corrected to match production grant parity. The operational guard was not weakened.

The final exact head passed all WU7.4 and inherited gates.

## Production invariant

No WU7.4 production migration was applied.

Recruitment remains **HOLD / CLOSED**.

No Distribution/Reconciliation, Attendance, Memory, Shared Journey or Impact truth was fabricated.

# FINAL STATUS

**P17-WU7.4 — COMPLETE / CLOSED / PASS**

Next:
**P17-WU7.5 — JOURNEY CONTROL CENTER RESOURCE WORKSPACE**
