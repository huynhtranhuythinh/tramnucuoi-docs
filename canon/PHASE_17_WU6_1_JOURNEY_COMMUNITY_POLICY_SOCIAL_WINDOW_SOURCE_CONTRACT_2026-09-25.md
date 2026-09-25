# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.1
# JOURNEY COMMUNITY POLICY / SOCIAL WINDOW SOURCE CONTRACT

Date: 2026-09-25  
Status: **COMPLETE / PASS — SOURCE CONTRACT MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Introduce the canonical Journey-owned Community policy source contract required by P17-WU6 without activating social publishing in production.

WU6.1 establishes:

- independent Publishing Window;
- independent Interaction Window;
- visitor interaction policy;
- participant-post publication policy;
- fail-closed behavior when no Journey Community policy exists;
- Admin-only policy management;
- public-safe derived policy projection.

No fixed ±N-day product rule is hardcoded.

## 2. Product source truth

Product repository:

`huynhtranhuythinh/tramnucuoi`

WU6.1 PR:

`#104 — P17-WU6.1: Journey Community policy and social windows`

Final PR head:

`b2e62ed432dd90ca63dbe9c3670c64826c51f4fd`

Squash-merged product `main` SHA:

`ade399456fbf50de4e3649483532abd7b3dd0859`

Source contract:

`database/contracts/p17_wu6_1_journey_community_policy.sql`

Source QA:

`scripts/p17-wu6-1-journey-community-policy-source-qa.ts`

Ephemeral PostgreSQL QA:

`scripts/p17-wu6-1-journey-community-policy-db-qa.sql`

## 3. Canonical policy semantics

Journey Community policy is Journey-scoped.

Conceptual policy fields:

- publishing enabled;
- publishing opens at;
- publishing closes at;
- interaction enabled;
- interaction opens at;
- interaction closes at;
- visitor interaction enabled;
- post publication mode.

Canonical publication modes:

- `participants_only`;
- `review_required`;
- `participant_choice`.

Default when no policy row exists:

- publishing disabled;
- interaction disabled;
- visitor interaction disabled;
- publication mode `participants_only`.

Therefore absence of configuration is fail closed.

## 4. Independent social windows

WU6.1 proves Publishing and Interaction are separate authorities.

Example QA contract:

- before both configured windows: both closed;
- Interaction may open before Publishing;
- both may be open concurrently;
- Publishing may later close while Interaction remains open;
- Interaction may be intentionally open-ended by leaving its close timestamp null.

This implements the Owner-locked Memory/community model without relying on Journey calendar date alone.

## 5. Authorization / security

The source contract:

- enables RLS on the future production policy table;
- revokes broad default table access;
- grants only the minimum authenticated operations required for the Admin management path;
- restricts read/insert/update policy management to Admin;
- server-derives `created_by` / `updated_by`;
- prevents mutation of immutable policy identity/audit fields;
- keeps privileged helpers in `private`;
- uses pinned `search_path = ''`;
- exposes only a public-safe `SECURITY INVOKER` projection wrapper.

Current Supabase guidance on Data API security continues to support the combined explicit-grant + RLS pattern used here.

## 6. Regression boundaries

WU6.1 does not:

- create default/fake policy rows;
- open any Journey Application Window;
- change Journey lifecycle;
- create participant records;
- create attendance;
- create Memory;
- create Journey social presence;
- create posts/interactions;
- create Shared Journey evidence;
- enable any feature flag.

Recruitment remains closed.

## 7. Corrective QA history

Early CI iterations exposed test-harness issues only:

1. source QA expected one more Admin-check occurrence than the contract actually contains;
2. Editor denial was raised by the server guard before the fixture's narrower expected RLS exception;
3. the first temporal assertion incorrectly expected Interaction to be closed on a date after its configured opening;
4. one edited PostgreSQL test block contained a malformed dollar-quote delimiter.

The production/source contract did not need to be weakened or semantically changed to resolve these failures.

Final exact-head QA proves the intended contract.

## 8. Exact-head verification

Final PR exact head:

`b2e62ed432dd90ca63dbe9c3670c64826c51f4fd`

Final PR checks:

- P16-WU10B Volunteer Pilot Gate: **SUCCESS**
- Generic CI: **SUCCESS**
- WU6.1 source QA: **PASS**
- WU6.1 ephemeral PostgreSQL social-window QA: **PASS**
- Build: **PASS**
- Typecheck: **PASS**
- Cloudflare dry-run / inherited release checks: **PASS**

Post-merge `main` exact SHA:

`ade399456fbf50de4e3649483532abd7b3dd0859`

Post-merge verification:

- main CI verify: **SUCCESS**
- Cloudflare staging build: **SUCCESS**
- Cloudflare production build pipeline: **SUCCESS**

The WU6.1 delta contains no runtime Community UI code and no production DB migration, so this build does not activate the new social policy behavior.

## 9. Production database state

WU6.1 performs **no production Supabase migration**.

Production migration ledger remains at:

`20260925062925 / p17_wu5_final_participant_workspace_projections`

No `journey_community_policies` production table exists yet from WU6.1.

WU6.Final remains the production cutover owner.

## 10. Final decision

# **P17-WU6.1 — COMPLETE / CLOSED / PASS**

Proceed directly to:

# **P17-WU6.2 — ORIGINAL JOURNEY POST + PUBLISHING AUTHORIZATION FOUNDATION**
