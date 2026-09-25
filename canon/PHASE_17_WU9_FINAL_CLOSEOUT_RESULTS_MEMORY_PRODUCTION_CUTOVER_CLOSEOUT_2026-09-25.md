# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU9.Final
# CLOSEOUT, RESULTS & MEMORY — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-25

Status: **COMPLETE / CLOSED / PASS**

## 1. Scope

P17-WU9 closes the post-Journey truth chain:

`CLOSEOUT_PENDING -> evidence/resource/result reconciliation -> Admin PASS -> MEMORY`

WU9 does not manufacture:
- attendance;
- Resource receipts/distributions;
- Impact/Result claims;
- Memory eligibility;
- Shared Journey evidence;
- reflections;
- lifecycle transition.

The transition into MEMORY remains an explicit Admin action after database readiness passes.

## 2. Starting gap

Production already contained:
- P11 closeout review tables/functions;
- P12 personal Memory projection and Reflections;
- verified/withheld/legacy Result/Impact workflow;
- P17 lifecycle authority;
- P17 Resource Operations;
- P17 Day-of Attendance.

The remaining gap was authority drift:

- legacy closeout still used `status=preparing/completed`;
- P17 canonical lifecycle is `closeout_pending -> memory`;
- old client closeout copy still referred to `completed`;
- personal Memory badge and Reflection eligibility were not consistently gated by canonical `lifecycle_phase=memory`.

WU9 rebases these surfaces without creating a parallel Results or Memory system.

## 3. WU9.1–WU9.6 source implementation

PR:

`#123 — P17-WU9.1-9.6: Closeout Results Memory`

Final exact head:

`df3377ba77585fd40ccf4dfd9cbfe211ad874427`

Exact-head gates:
- CI `36165335677` — **SUCCESS**
- inherited Volunteer Pilot Gate `36165335736` — **SUCCESS**

Squash-merged main:

`3df89c5b29fc1a51df9c00669102ab05e41190ff`

Post-merge main CI:

`36165548857` — **SUCCESS**

### Source contract

`database/contracts/p17_wu9_1_closeout_results_memory_authority.sql`

### QA

- `scripts/p17-wu9-source-privacy-qa.ts`
- `scripts/p17-wu9-closeout-memory-db-qa.sql`

## 4. Canonical Closeout Readiness

New Admin-only public wrapper:

`public.tnc_journey_closeout_readiness_v2(uuid)`

It aggregates truth from existing canonical sources:

### Attendance
- confirmed rows;
- unresolved rows;
- invalid rows.

### Resources
- Needs with verified Received truth;
- unresolved reconciliation;
- received-but-undistributed/unreconciled availability.

### Results / Impact
- draft;
- needs evidence;
- verified;
- withheld;
- legacy public;
- blocked snapshot state.

### Documentary / Field Updates
- draft Field Updates;
- documentary evidence completeness/trust state.

### Review freshness
A previous PASS is stale if relevant attendance/evidence/resource/result truth changed after review.

No Result value is calculated or created by this readiness function.

## 5. MEMORY gate

Canonical transition gate now enforces:

1. current lifecycle = `closeout_pending`;
2. Application Window = `closed`;
3. end date has passed in Asia/Ho_Chi_Minh;
4. all confirmed attendance resolved;
5. no invalid attendance;
6. no draft Field Updates;
7. no unsafe/incomplete documentary blocker;
8. Resource Operations reconciled;
9. Result/Impact claims fully reconciled into valid `verified` or valid `withheld` states;
10. no unresolved `legacy_public` result claims;
11. current Closeout Review = PASS;
12. review is not stale.

Only then:

`can_enter_memory = true`

The database still requires a deliberate Admin lifecycle transition.

## 6. Closeout Review rebase

`private.tnc_assert_closeout_review()` now uses canonical P17 authority.

PASS requires:
- Admin;
- Journey lifecycle = `closeout_pending`;
- end date passed;
- all three manual review attestations;
- no invalid attendance;
- zero unresolved confirmed attendance.

Important P17 rule:

**An attendance gap note cannot substitute for unresolved attendance truth.**

The note remains available for operational context, but PASS is blocked until attendance is resolved.

## 7. Results boundary

WU9 reuses the existing Result/Impact model.

Public Result/Impact RLS was already P17-hardened to require:

`journeys.lifecycle_phase = 'memory'`

and valid public result authority.

Therefore WU9 does not create:
- a second Results table;
- derived KPI claims;
- attendance-derived Impact;
- Resource-derived Impact.

Verified/withheld result claims remain human/evidence-governed.

## 8. Personal Memory UX rebase

Updated:

`src/components/community/community-account-page.tsx`

Attendance-derived `memory_eligible` remains an eligibility fact.

The UI badge:

`KÝ ỨC ĐỦ CĂN CỨ / EVIDENCE-BACKED MEMORY`

now requires both:

- attendance-based Memory eligibility; and
- parent Journey `lifecycle_phase = memory`.

Therefore:

**verified attendance != active Memory Mode**

## 9. Reflection rebase

Updated:

`src/components/community/community-reflections-panel.tsx`

Reflection composer eligibility now requires:

`lifecycle_phase = 'memory'`

instead of legacy:

`status = 'completed'`

Production database defense-in-depth was also verified:

`private.tnc_guard_journey_reflection()`

already enforces canonical `lifecycle_phase='memory'` on INSERT.

Public Reflection publication RLS also requires parent Journey lifecycle = MEMORY.

## 10. Admin UX

Updated Closeout Manager now presents:

`CLOSEOUT -> MEMORY`

rather than legacy:

`preparing -> completed`

It exposes:
- canonical blockers;
- attendance unresolved count;
- Resource reconciliation blockers;
- verified Result count;
- Result blockers;
- current PASS state.

PASS never auto-transitions lifecycle.

Admin still uses Lifecycle Authority for the explicit MEMORY transition after readiness is READY.

## 11. WU9.Final production release

PR:

`#124 — P17-WU9.Final: Closeout Results Memory production cutover`

Final exact head:

`b37b343eee85289cd615d8dae50047cb9194529a`

Exact-head gates:
- CI `36166005597` — **SUCCESS**
- inherited Volunteer Pilot Gate `36166005804` — **SUCCESS**

Squash-merged production main:

`d9add316981e00f7d2b10a716da2095cdf4e6909`

Post-merge main CI:

`36166223909` — **SUCCESS**

## 12. Production migration

Source:

`database/migrations/0064_p17_wu9_final_closeout_results_memory.sql`

Git blob SHA:

`54bd9dfc0e452703116c2d202e2fdae01543a5d2`

Authority rollback:

`database/rollbacks/p17_wu9_final_closeout_results_memory.sql`

Supabase project:

`iwiqprhoohkxvjyxojto`

Production migration ledger:

`20260925172001_p17_wu9_final_closeout_results_memory`

Apply result:

**SUCCESS**

## 13. Production preflight and post-cutover truth

Before 0064:

- Closeout Review rows = 0
- unresolved attendance rows = 3
- legacy-public Result items = 4
- verified Result items = 0
- attendance-derived Memory eligible rows = 0
- Journeys in MEMORY = 0
- non-closed Application Windows = 0

After 0064:

- Closeout Review rows = 0
- unresolved attendance rows = 3
- legacy-public Result items = 4
- verified Result items = 0
- attendance-derived Memory eligible rows = 0
- Journeys in MEMORY = 0
- non-closed Application Windows = 0

Therefore production cutover manufactured no truth and did not silently advance any Journey.

Current real production blockers remain visible as blockers rather than being bypassed:
- unresolved attendance exists;
- legacy-public Result claims still require reconciliation.

## 14. Production function / trigger verification

Verified production functions:

- private `tnc_journey_resource_closeout_state` — SECURITY DEFINER
- private `tnc_journey_result_closeout_state` — SECURITY DEFINER
- private `tnc_journey_closeout_readiness_v2` — SECURITY DEFINER with explicit Admin check
- public `tnc_journey_closeout_readiness_v2` — SECURITY INVOKER
- private `tnc_assert_closeout_review` — SECURITY INVOKER
- private `tnc_assert_journey_closeout_gate` — SECURITY INVOKER

Production trigger:

`journeys_closeout_gate`

now fires only on:

`lifecycle_phase -> memory`

The legacy `status -> completed` branch is removed from production authority.

## 15. Rollback

WU9 rollback is authority-only.

It:
- removes WU9 readiness helpers;
- restores pre-WU9 legacy closeout functions and trigger.

It does NOT delete:
- attendance;
- Closeout Reviews;
- Resources;
- Result/Impact rows;
- Memory projections;
- Reflections;
- Journey rows.

Migration + rollback ephemeral DB QA: PASS.

## 16. Supabase Advisors

### Security Advisor

No WU9-specific security regression.

Historical project warning remains:

`Leaked Password Protection Disabled`

### Performance Advisor

No new WU9 table/FK was introduced.

Historical unrelated performance findings remain outside WU9 scope.

## 17. Canonical invariants preserved

- Registration != attendance.
- Attendance eligibility != active Memory.
- Resource truth != Impact.
- Results are never inferred from counts.
- Legacy-public Results cannot silently pass canonical closeout.
- PASS != automatic MEMORY transition.
- Reflection requires actual MEMORY lifecycle.
- No fake attendance.
- No fake Results.
- No fake Memory.
- No fake Shared Journey.
- Recruitment remains HOLD / CLOSED.

# FINAL STATUS

**P17-WU9 — COMPLETE / CLOSED / PASS**

Production capability:

**Canonical Closeout -> Results reconciliation -> Memory authority ACTIVE, with current production truth left unchanged and blockers surfaced honestly.**

Next canonical roadmap item:

**P17-WU10 — PUBLIC DISCOVERY & TRUST REBASE**
