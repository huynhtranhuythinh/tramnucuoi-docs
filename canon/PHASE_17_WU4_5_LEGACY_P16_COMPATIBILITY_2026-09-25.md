# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.5 — LEGACY P16 COMPATIBILITY

Date: 2026-09-25  
Status: **COMPLETE / PASS**

## Objective

Preserve historical P16 volunteer preference/assignment truth while making the P17 participant assignment authority explicit.

WU4.5 must not semantically migrate legacy fixed team strings into P17 Journey Role / Department / Team truth.

## Canonical behavior

Historical P16 application fields remain preserved:

- `preferred_team`
- `assigned_team`
- `assignment_updated_by`
- `assignment_updated_at`

They are compatibility/history only.

P17 canonical assignment authority remains:

- Journey Role
- optional Department
- optional Team
- participant assignment history

New P17 flows do not write legacy `assigned_team`.

## Admin compatibility presentation

The participant assignment workspace now shows historical P16 preference/assignment side-by-side when applicable, labeled:

`P16 LEGACY · HISTORICAL ONLY`

The UI explicitly states:

- legacy values are not automatically mapped to Journey Role;
- legacy values are not automatically mapped to Department/Team;
- current P17 assignment is the new authority;
- only an explicit Admin/BTC assignment decision changes canonical assignment truth.

The volunteer review workspace also continues to label the fixed P16 team field as compatibility-only.

## Source protections

Added:

`scripts/p17-wu4-5-legacy-p16-compatibility-qa.ts`

The gate proves:

- legacy fields remain typed/projected;
- P17 applications are rejected by the legacy assignment writer;
- legacy assignment helper remains explicitly P16-only;
- no automatic text-to-Department mapping exists;
- P17 schema/assignment operations do not mutate legacy fields;
- attendance is not touched;
- lifecycle/recruitment is not activated;
- Memory/social evidence truth is not touched.

Inherited WU4.4 UI QA was advanced semantically instead of preserving stale WU labels.

## Evidence

PR:

`#90 — P17-WU4.5: Legacy P16 compatibility`

Final PR head:

`a72ab23fc3f1d07a79707d9cd9ff84a4148b21ee`

Exact-head gates:

- generic CI `36086103120`: **SUCCESS**
- dedicated P16-WU10B gate `36086103144`: **SUCCESS**
- WU4.5 legacy compatibility QA: **PASS**
- inherited WU4.1–WU4.4 gates: **PASS**
- inherited P9–P17 source/DB gates: **PASS**
- build/typecheck/Cloudflare dry-run: **PASS**

Merged product main:

`a993980d5984a7725a1ecacde0a7a0c694070b33`

Post-merge main CI:

`36087373624` → **SUCCESS**

## Production invariants

WU4.5 applies no production DDL.

Production remains:

- WU4 source contract capability not cut over;
- recruitment HOLD / CLOSED;
- non-closed application windows: 0;
- historical P16 volunteer data preserved.

## Decision

**P17-WU4.5 — COMPLETE / PASS.**

Next:

**P17-WU4.6 — SECURITY / PRIVACY / MOBILE REGRESSION QA**
