# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU11.2
# PILOT DATE RECONCILIATION & ACTIVATION AUTHORITY GATE

Date: 2026-09-26

Status: **DATE RECONCILIATION PASS / FINAL ACTIVATION HOLD**

## Owner-approved date

The new production pilot is a single-day Journey on:

**10/10/2026**

Canonical dates:

- start_date = 2026-10-10
- end_date = 2026-10-10

## Production Journey

Slug:

`p17-production-pilot-2026`

ID:

`5a942585-b2cf-4271-be97-081c3263cc5c`

Current title:

**P17 Production Pilot — 10/10/2026**

Current summary is deliberately technical and neutral; no unapproved location/theme/story was invented.

Current authority state:

- legacy status = `draft`
- lifecycle_phase = `draft`
- application_state = `closed`
- application_mode = `volunteer_v1`

## Staffing truth

Seven active Departments / open staffing needs remain:

- Media — 3
- Hậu cần – Nấu ăn — 5
- Cắt tóc — 2
- Hoạt náo — 4
- Truyền thông — 2
- Điều phối — 2
- Hỗ trợ chung — 4

Total target: **22 TNV**

Application Window remains CLOSED.

## Source release

PR #129 initially prepared a combined date + DRAFT->UPCOMING operation.

Exact head:
`9f111d8e53559c1b781ab9ba4d42dd20c875f655`

Exact-head gates:
- CI `36177674100` — SUCCESS
- Volunteer Pilot Gate `36177674030` — SUCCESS

Merged main:
`8bf2c4217b2c557ae3f56b28d0cd757465222df9`

Post-merge CI:
`36177875184` — SUCCESS

## Authority discovery during production cutover

The first production apply attempt was correctly rejected by:

`private.tnc_guard_journey_lifecycle_contract()`

with:

**Journey lifecycle/application state changes are admin-only**

This proved that the migration/control-plane channel cannot impersonate the authenticated Admin lifecycle authority.

No lifecycle or application state changed during the failed attempt.

The gate was respected rather than bypassed.

## HF1 — preserve Admin lifecycle authority

PR #130 corrected the operation so production provisioning sets only approved content/date truth and leaves lifecycle/application authority untouched.

Exact head:
`de909983bc81f19b73d510217255897ed1f8061f`

Exact-head gates:
- CI `36178195576` — SUCCESS
- Volunteer Pilot Gate `36178195575` — SUCCESS

Merged main:
`8c2d44a3362aa728a5b1787e1f3e913771389b05`

Post-merge CI:
`36178392379` — SUCCESS

## Production date reconciliation

Source:

`database/operations/p17_wu11_final_set_date_upcoming.sql`

The filename is historical from the initial attempt; the current source explicitly preserves DRAFT and Admin lifecycle authority.

Production operation ledger:

`20260925191551_p17_wu11_final_date_reconciliation`

Apply result:

**SUCCESS**

Postflight verified:

- title = P17 Production Pilot — 10/10/2026
- start_date = 2026-10-10
- end_date = 2026-10-10
- lifecycle_phase = draft
- application_state = closed
- seven staffing needs remain correct
- total staffing target remains 22

## Remaining WU11.Final authority gates

### 1. Authenticated Admin lifecycle action

DRAFT -> UPCOMING must run from an authenticated Admin session through the canonical application authority.

The current Supabase MCP migration/SQL control plane does not carry the Owner/Admin user session and must not bypass this guard.

### 2. Cloudflare runtime activation

The controlled activation source is ready:

`scripts/p17-wu11-pilot-activate.sh`

Required BEFORE-stage flags include:

- COMMUNITY_AUTH = true
- JOURNEY_COMMUNITY_V2 = true
- SOCIAL_SAFETY_HARDENING = true
- compatible Journey Community/Interaction foundations = true
- Shared Experience / post-Journey notification/continuity surfaces = false

Repository CI proves Cloudflare dry-run only.

Canonical infrastructure evidence confirms there is no repository production-deploy workflow and the available ChatGPT environment has no Cloudflare deployment connector.

Therefore Cloudflare production activation is **NOT CLAIMED** until an actual Worker deploy/version is evidenced.

### 3. Application Window

Application Window must remain CLOSED until:

- lifecycle = UPCOMING via authenticated Admin;
- runtime activation is actually deployed and verified;
- protected activation probe passes.

The WU11 activation guard now also validates:
- real current/future date;
- active Department + open Staffing Need.

## Security

No security authority was weakened to complete the date operation.

Supabase Security Advisor still reports the historical:

`Leaked Password Protection Disabled`

The current public Community onboarding path is passwordless; the warning remains security debt.

# STATUS

**P17-WU11.2 — PASS**

**P17-WU11.Final — HOLD at authenticated Admin + Cloudflare runtime authority gates.**

No fake activation, no direct DB OPEN, no lifecycle bypass.
