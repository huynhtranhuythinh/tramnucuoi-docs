# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.4 — PUBLIC NAVIGATION REBASE

Date: 2026-09-23  
Status: **COMPLETE / PASS — SOURCE NAVIGATION REBASE; NO PRODUCTION DB / RECRUITMENT ACTIVATION**

## Objective

Make Journey the primary public participation object in global navigation while preserving Field Journal as editorial documentation and Community/My TNC as governed continuity rather than the website's primary mental model.

## Product evidence

Base main: `f463302adf98b0d009bbbc1be21e58e4283d129a`  
Branch: `p17-wu2-4-public-navigation-rebase`  
PR: `#70`  
Final PR head: `377f4dee236f716291a9c4fe47f665b10ab41aee`  
Merged main: `1ed29ff7898a7e03b955a7cf5bde1c930c8540c9`

PR verification:
- generic CI `35808307142`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35808307185`: **SUCCESS**

Post-merge main:
- CI `35808410110`: **SUCCESS**
- exact head: `1ed29ff7898a7e03b955a7cf5bde1c930c8540c9`

## Canonical primary navigation

1. Trang chủ / Home
2. TNC / About
3. Dự án / Projects
4. Hành Trình / Journeys
5. Tác động / Impact
6. Đồng hành / Get Involved

Field Journal / Nhật ký is no longer a top-level participation route. It remains available as secondary editorial navigation.

Community is no longer a global primary-navigation concept.

My TNC is rendered in the public masthead/footer only for an authenticated session and continues to point to the governed authenticated continuity surface.

## Public Impact route

Added:
- VI: `/tac-dong`
- EN: `/en/impact`

The Impact page is a trust/navigation surface, not a KPI dashboard. It explains that:
- elapsed time does not manufacture impact truth;
- Journey is the unit where evidence/context are understood together;
- private operational truth does not become public merely because it exists.

It links readers back to canonical Journey and Project objects and reads no private participant/application/Memory sources.

## QA

Added:
`scripts/p17-wu2-4-public-navigation-qa.ts`

It verifies:
- exact primary nav order;
- reciprocal VI/EN Impact routes;
- Field Journal and Community absent from primary navigation;
- authenticated-only My TNC;
- secondary Field Journal discoverability;
- Impact page does not access private operational sources.

A first PR run correctly failed TypeScript because the VI Impact copy was placed at the wrong dictionary depth. The source was corrected before merge; the exact corrected head and post-merge main both passed full CI.

## Explicit non-scope

WU2.4 did not change:
- Supabase production schema/data;
- lifecycle/application state;
- recruitment state;
- attendance/Memory/Reflection truth;
- Community social foundations;
- feature flags;
- Cloudflare production deployment.

Public recruitment remains **HOLD**.

## Decision

**P17-WU2.4 — COMPLETE / PASS.**

Next: **P17-WU2.5 — ADMIN LIFECYCLE CONTROLS**.
