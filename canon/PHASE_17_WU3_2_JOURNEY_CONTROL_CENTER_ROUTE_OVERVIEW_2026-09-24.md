# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.2 — JOURNEY CONTROL CENTER ROUTE & OVERVIEW

Date: 2026-09-24  
Status: **COMPLETE / PASS — SOURCE CONTROL CENTER ACTIVE; NO WU3 PRODUCTION DDL**

## Objective

Replace the fragmented Admin mental model of one Journey row plus many independent managers with a dedicated per-Journey operating context.

Canonical operating question:

> What is missing or needs action now?

WU3.2 activates only the Control Center shell and Overview. It does not pretend later-WU operational modules are already live.

## Starting truth

Base product main:

`a3d77cfd8dcefff1c6c27d8bfacd006875259bd8`

WU3.0 and WU3.1 were already COMPLETE / PASS.

WU3.1 Event Management tables remain a source/ephemeral DB contract only; no WU3 production DDL is applied in WU3.2.

Public recruitment remains **HOLD / CLOSED**.

## Product result

### Journey portfolio

`/admin/journeys` remains the portfolio/list entry.

Each Journey now has a prominent:

`CONTROL CENTER`

entry.

Existing manager buttons remain temporarily available until WU3.6 recomposes them into the dedicated Journey context.

### Dedicated Control Center route

Added canonical Admin route:

`/admin/journeys/:journeyId`

The route is deliberately non-nested under the list UI so a selected Journey opens one independent operating context rather than rendering under the full portfolio.

### Overview

The Overview exposes existing production truth only:

- Lifecycle Phase;
- Application Window;
- VI/EN translation completeness;
- application count for Admin;
- confirmed participant rows / people for Admin;
- unresolved attendance rows / people when attendance capability is available;
- past-UPCOMING reconciliation signal;
- Closeout Pending signal;
- OPEN application signal.

Dates may create attention cues but never lifecycle transitions.

Application, Participant and Attendance remain distinct truths.

## RLS-aware metric handling

Application / participant / attendance data remain restricted operational truth.

For a non-Admin Editor, the Control Center does not treat RLS-hidden data as zero.

Instead it explicitly reports that restricted metrics are unavailable to the current role.

This prevents a privacy boundary from becoming a false operational fact.

## Canonical Control Center map

The shell records the Journey Operating System sections:

1. Tổng quan
2. Thiết lập
3. Người & đăng ký
4. Cơ cấu nhóm
5. Kế hoạch
6. Nguồn lực
7. Thông báo
8. Cộng đồng
9. Ngày diễn ra
10. Điểm danh & Tư liệu
11. Đóng sổ & Kết quả

Only **Tổng quan / Overview** is activated by WU3.2.

Other sections are labelled by ownership/status:

- existing foundation / recomposition in WU3.6;
- next WU3 module;
- later WU.

No fake functionality is exposed.

## Truth and scope protection

WU3.2 does not:

- query source-only `journey_departments`, `journey_teams`, `journey_runbook_items`, or `journey_official_updates` as if they existed in production;
- reinterpret P16 `preferred_team` / `assigned_team` as canonical Department / Team;
- mutate attendance truth;
- mutate Memory / Reflection / Impact truth;
- create staffing assignment truth;
- deploy WU3 production schema;
- open recruitment.

Application OPEN continues to use the protected WU2 activation server path.

Lifecycle controls reuse the existing WU2 authority and database gates rather than creating a new transition implementation.

## Product evidence

Branch:

`p17-wu3-2-journey-control-center`

PR:

`#79 — P17-WU3.2: Journey Control Center route and overview`

Final PR head:

`42cfbc6454471ef86ce66217565bbe65f0c0a73c`

Exact-head verification:

- generic CI `35987060599`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35987060508`: **SUCCESS**

Squash-merged main:

`7ae0c3452bbf3e02a4d533ab0b7e307199420bcf`

Post-merge main CI:

- run `35987238110`
- exact head `7ae0c3452bbf3e02a4d533ab0b7e307199420bcf`
- conclusion: **SUCCESS**

## QA

Added:

`scripts/p17-wu3-2-control-center-qa.ts`

The gate verifies:

- dedicated per-Journey route;
- portfolio → Control Center entry;
- canonical operating framing;
- reuse of WU2 lifecycle/application authority;
- date-non-authoritative action signals;
- RLS-hidden metrics never represented as zero;
- later-WU Event Management tables are not treated as production-ready;
- legacy P16 team vocabulary is not promoted;
- attendance is read-only;
- protected Application OPEN path is preserved.

An initial QA run produced a false positive because the static attendance guard matched `==` as if it were assignment. The guard was corrected to distinguish comparison from mutation.

A subsequent exact-head run then reached build successfully but TypeScript correctly rejected an explicit `undefined` optional prop under `exactOptionalPropertyTypes`. The component was corrected before merge.

Final exact-head and post-merge main both passed all inherited source/DB gates, build, typecheck and Cloudflare dry-run.

## Production boundary

WU3.2 made:

- **no production Supabase mutation**;
- **no WU3 schema deployment**;
- **no Cloudflare production deploy**;
- **no feature flag mutation**;
- **no recruitment activation**.

## Decision

**P17-WU3.2 — COMPLETE / PASS.**

Next:

**P17-WU3.3 — DEPARTMENT / TEAM STRUCTURE MANAGEMENT**

Public recruitment remains:

**HOLD / CLOSED**
