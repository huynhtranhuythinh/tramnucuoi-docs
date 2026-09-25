# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU11.0
# PRODUCTION PILOT RE-ACTIVATION — READINESS AUDIT & FAIL-CLOSED FOUNDATION

Date: 2026-09-26

Status: **READINESS FOUNDATION PASS / PRODUCTION RE-ACTIVATION HOLD**

Owner: Jean Huỳnh
CTO / Product Architect / QA Lead: ChatGPT

Product repo: `huynhtranhuythinh/tramnucuoi`
Docs repo: `huynhtranhuythinh/tramnucuoi-docs`
Production: `https://tramnucuoi.com`
Supabase: `iwiqprhoohkxvjyxojto`
Cloudflare Worker: `tramnucuoi`

## 1. Canonical purpose

P17-WU11 is the final Phase 17 roadmap step:

**Production Pilot Re-activation**

Its job is not to reopen recruitment blindly.

Before public recruitment is reactivated, production truth must be reconciled across:

- Journey lifecycle;
- actual Journey date;
- application window;
- volunteer staffing;
- participant truth;
- registration protection;
- runtime flags;
- privacy/security;
- mobile operational capability.

The Product Reassessment Canon explicitly keeps public recruitment HOLD until a new explicit activation gate passes.

## 2. Pilot candidate found in production

The existing volunteer pilot candidate is:

`trung-thu-em-va-cay-2026`

Title:

**TRUNG THU EM VÀ CÂY 2026**

Production state at WU11 audit:

- lifecycle_phase = `upcoming`
- application_state = `closed`
- application_mode = `volunteer_v1`
- legacy status = `registration_open`
- structured start_date = `2026-09-19`
- structured end_date = `2026-09-20`
- story text says `18 – 20/09/2026`
- applications = 1
- confirmed applications = 1
- participants = 1
- unresolved attendance = 1
- active Departments = 0
- open staffing needs = 0
- Runbook items = 0
- Official Updates = 0
- Resource Needs = 0

The application window remains CLOSED, which is currently the correct fail-closed state.

## 3. Real production drift

Current date is after the stored Journey dates.

Therefore opening recruitment without correcting business truth would expose a stale Journey.

The canonical reassessment explicitly states that the previously discussed 30/09/2026 date was illustrative and must not be treated as an automatic production correction.

WU11 therefore does not mutate Journey dates without Owner-confirmed real operating truth.

## 4. Existing volunteer structure evidence

The public Journey story already names seven recruiting groups:

1. Ban Media
2. Ban Hậu cần – Nấu ăn
3. Ban Cắt tóc
4. Ban Hoạt náo
5. Ban Truyền thông
6. Ban Điều phối
7. Ban Hỗ trợ chung

This is sufficient evidence to reuse these labels as Journey-scoped Departments if the same pilot Journey is reactivated.

However, canonical production contains no approved target volunteer count for any of them.

The `journey_staffing_needs.target_volunteers` schema default of 1 is a technical default only and MUST NOT be treated as business truth.

## 5. WU11 fail-closed activation hardening

Before WU11, the protected Application Window activation checked:

- Admin authorization;
- lifecycle = UPCOMING;
- application state = CLOSED/PAUSED;
- registration protection probe;
- activation secret/runtime contract.

It did not reject:

- a stale/past Journey date;
- a volunteer Journey with no active Department/open staffing need.

WU11 fixes this.

The protected activation server now fails closed with:

### `date_not_ready`

Application Window cannot open when:

- start_date is missing;
- start_date is before the current Vietnam calendar date;
- end_date is earlier than start_date.

Dates remain explicit business/lifecycle truth.

This guard does NOT auto-transition Journey phase from dates.

### `staffing_not_ready`

For `application_mode=volunteer_v1`, Application Window cannot open until production contains:

- at least one open `journey_staffing_need`;
- linked to an active Journey Department.

This makes the WU4 canonical model enforceable at the final activation boundary.

## 6. Admin UX

Admin activation now surfaces distinct errors for:

- stale/missing date;
- missing Journey staffing structure.

The system no longer collapses these real operating blockers into a generic activation failure.

## 7. Controlled P17 runtime activation scripts

Added:

`scripts/p17-wu11-pilot-activate.sh`

Activation pins:

- `VITE_APP_COMMUNITY_AUTH_ENABLED=true`
- `VITE_APP_JOURNEY_COMMUNITY_V2_ENABLED=true`
- `VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED=true`
- compatibility Journey Room / Interaction foundations = true

BEFORE-stage evidence/post-Journey surfaces remain false:

- Shared Experience Graph
- Social Notifications v1
- Journey Social Continuity

This preserves the rule that BEFORE-stage access never manufactures attendance, shared experience or post-Journey Memory truth.

Added rollback:

`scripts/p17-wu11-pilot-rollback.sh`

Rollback fails Community v2/social interaction surfaces closed without deleting product or operational truth.

Both scripts default to dry-run.

## 8. Source release

PR:

`#127 — P17-WU11: Production Pilot Re-activation readiness`

Final exact head:

`3fa885d383c9efd076601f27d2492deb5cb32f94`

Exact-head gates:

- CI `36172223403` — **SUCCESS**
- inherited Volunteer Pilot Gate `36172223437` — **SUCCESS**

Merged main:

`d88c48141cf6d883d7fbba03475cbf384c84f684`

Post-merge main CI:

`36172440154` — **SUCCESS**

## 9. Production mutations in WU11.0

None.

WU11.0 deliberately did NOT:

- alter Journey dates;
- create Departments;
- invent staffing targets;
- open Application Window;
- modify existing applicant/participant history;
- record attendance;
- deploy P17 runtime activation flags.

## 10. Existing applicant/participant truth

The pilot already contains:

- one confirmed application;
- one participant;
- attendance unresolved.

Therefore changing the date of this same Journey would also change the calendar context attached to that existing confirmed participation.

Whether that is correct depends on real-world operating truth.

WU11 must not decide this from technical inference alone.

## 11. Remaining hard blockers to actual production re-activation

### Blocker A — Pilot identity/date truth

Owner must confirm either:

1. the same `TRUNG THU EM VÀ CÂY 2026` Journey is being rescheduled, including the actual new start/end dates; or
2. the old pilot should remain historical/closed and a new Journey should be created for the next production pilot.

### Blocker B — Volunteer target truth

If the same Trung Thu pilot is reused, the actual target TNV count for each recruiting Department must be known before the application window opens.

Department names already have source evidence.

Target counts do not.

### Blocker C — Runtime activation

After A/B are resolved and production data is reconciled, WU11 must still:

1. run final production readiness audit;
2. dry-run the controlled P17 runtime activation;
3. deploy exact runtime flags;
4. verify runtime;
5. explicitly open Application Window through protected activation;
6. run public volunteer-application smoke test;
7. verify applicant/admin/participant/mobile truth;
8. record closeout evidence.

## 12. Security advisor note

Supabase currently reports:

`Leaked Password Protection Disabled`

Current public Community account flow uses passwordless email Magic Link / OTP rather than a password sign-in flow.

The warning remains project security debt and is not silently ignored, but WU11 does not invent an Auth configuration mutation through unavailable control-plane permissions.

Any later password-based public auth should resolve this before activation.

## 13. WU11 status

### Foundation

**PASS**

The source/runtime activation boundary is now safer than before WU11.

### Production pilot reactivation

**HOLD**

This is a business-truth hold, not a technical CI failure.

The system is intentionally preventing activation against stale dates and empty staffing configuration.

## NEXT GATE

Once Owner provides the actual pilot/date decision and TNV target counts, continue directly with:

**P17-WU11.Final — PILOT DATA RECONCILIATION → RUNTIME ACTIVATION → APPLICATION WINDOW OPEN → PRODUCTION SMOKE TEST → CANONICAL CLOSEOUT**

Do not reopen discovery.
