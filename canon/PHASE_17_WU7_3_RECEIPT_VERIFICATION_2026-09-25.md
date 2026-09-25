# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU7.3 — RECEIPT VERIFICATION

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE CONTRACT MERGED; NO PRODUCTION DATABASE CUTOVER**

## Canonical truth

`Pledge != Received`

Receipt is BTC/Admin-verified actual receipt truth. Remaining Need is derived as:

`max(requested_quantity - verified_received_quantity, 0)`

Pledge quantity is intentionally excluded.

## Evidence

PR: `#117 — P17-WU7.3: Receipt verification contract`

Final exact head:
`74a8655c8be9f217d6b674405f277b6cfacf71d6`

Exact-head:
- CI `36146682861` — **SUCCESS**
- inherited dedicated gate `36146683068` — **SUCCESS**

Squash-merged main:
`e5a64b677c7f396e6d9235d2051e227307c84779`

Post-merge main CI:
`36146920541` — **SUCCESS**

Source:
- `database/contracts/p17_wu7_3_resource_receipt.sql`
- `scripts/p17-wu7-3-receipt-source-qa.ts`
- `scripts/p17-wu7-3-receipt-db-qa.sql`

## Contract

Planned table:
`public.journey_resource_receipts`

Locked rules:
- positive received quantity;
- same-Journey Need FK;
- optional Pledge FK must match the same Journey and Need;
- Admin-only source read/write;
- verifier identity server-derived;
- Receipt identity/verification fields immutable;
- no normal DELETE grant;
- donor cannot self-verify receipt;
- no public donor identity exposure.

Derived private helpers:
- `private.tnc_journey_resource_received_quantity(uuid)`
- `private.tnc_journey_resource_remaining_quantity(uuid)`

## Production invariant

No WU7.3 production migration was applied.

Recruitment remains **HOLD / CLOSED**.

No receipt, participant, attendance, Memory, Shared Journey or Impact truth was fabricated.

# FINAL STATUS

**P17-WU7.3 — COMPLETE / CLOSED / PASS**

Next:
**P17-WU7.4 — DISTRIBUTION & RECONCILIATION**
