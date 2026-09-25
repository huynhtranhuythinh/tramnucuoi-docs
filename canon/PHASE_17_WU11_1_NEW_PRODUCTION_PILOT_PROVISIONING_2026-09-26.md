# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU11.1
# NEW PRODUCTION PILOT PROVISIONING — CANONICAL RESULT

Date: 2026-09-26

Status: **COMPLETE / PASS**

## Owner decisions

Owner chose:

1. Create a **NEW Journey pilot** instead of rewriting the historical `TRUNG THU EM VÀ CÂY 2026` Journey.
2. Volunteer staffing targets:
   - Media: 3
   - Hậu cần – Nấu ăn: 5
   - Cắt tóc: 2
   - Hoạt náo: 4
   - Truyền thông: 2
   - Điều phối: 2
   - Hỗ trợ chung: 4

Total target volunteers: **22**

## Provisioned production Journey

Slug:

`p17-production-pilot-2026`

Production id:

`5a942585-b2cf-4271-be97-081c3263cc5c`

Temporary internal title:

**P17 Production Pilot — Chờ chốt tên & ngày**

This title is intentionally non-public placeholder truth. It is not presented as the final Journey editorial name.

Production state:

- legacy status = `draft`
- lifecycle_phase = `draft`
- application_state = `closed`
- application_mode = `volunteer_v1`
- start_date = NULL
- end_date = NULL
- capacity = NULL

Therefore the new pilot is not publicly discoverable and cannot accept applications.

## Journey Departments / staffing

Production now contains seven active Journey-scoped Departments and seven open operational Staffing Needs:

| Department | Target |
| --- | ---: |
| Media | 3 |
| Hậu cần – Nấu ăn | 5 |
| Cắt tóc | 2 |
| Hoạt náo | 4 |
| Truyền thông | 2 |
| Điều phối | 2 |
| Hỗ trợ chung | 4 |
| **TOTAL** | **22** |

`is_open=true` on staffing needs means the Need itself is operationally active.

It does **not** open public recruitment.

Public staffing remains gated by canonical parent Journey lifecycle + Application Window authority.

## Historical Journey preservation

The historical Journey:

`trung-thu-em-va-cay-2026`

was not rewritten.

Verified after provisioning:

- title remains `TRUNG THU EM VÀ CÂY 2026`
- dates remain 2026-09-19 → 2026-09-20
- lifecycle remains `upcoming`
- application window remains `closed`
- existing applications remain 1
- existing participants remain 1

No historical attendance/application truth was moved to the new pilot.

## Source / QA

Provisioning source:

`database/operations/p17_wu11_pilot_provisioning.sql`

Guarded rollback:

`database/operations/p17_wu11_pilot_provisioning_rollback.sql`

Source QA:

`scripts/p17-wu11-pilot-provisioning-qa.ts`

PR:

`#128 — P17-WU11: Provision new production pilot draft`

Exact head:

`28849bfbbabf07fb5a2e31fc34d0fd103257a677`

Exact-head gates:

- CI `36175050311` — SUCCESS
- inherited Volunteer Pilot Gate `36175050409` — SUCCESS

Merged main:

`db6e6ded6a450478c9347cd51fae0b83533d4464`

Post-merge main CI:

`36175265273` — SUCCESS

## Production apply

Supabase production operation ledger:

`20260925184540_p17_wu11_pilot_provisioning`

Apply result:

**SUCCESS**

The control-plane SQL channel was read-only for direct operational DML, so provisioning used the versioned migration/apply channel with source and guarded rollback retained in GitHub.

No schema was changed by the provisioning SQL.

## Remaining activation blockers

### Business truth still required

Before the pilot can become UPCOMING/public:

- final Journey name should be confirmed or deliberately keep a pilot name;
- real start_date must be supplied;
- real end_date must be supplied when multi-day;
- location/content may remain minimal only if Owner intentionally wants a technical pilot.

### Runtime activation still required

Before Application Window opens:

- controlled P17 Community v2 runtime activation/deploy;
- runtime verification;
- Journey DRAFT → UPCOMING only after real date truth is set;
- protected Application Window OPEN;
- production application smoke test.

## Invariants

- No historical Journey rewrite.
- No fake date.
- No fake attendance.
- No fake participant.
- No fake public result.
- Staffing target is Owner-approved truth.
- Application remains CLOSED.
- Runtime social activation is not inferred from source existence.

# STATUS

**P17-WU11.1 — COMPLETE / PASS**

Next:

**P17-WU11.Final — PILOT DATE/EDITORIAL RECONCILIATION → RUNTIME ACTIVATION → APPLICATION WINDOW OPEN → PRODUCTION SMOKE TEST → CANONICAL CLOSEOUT**
