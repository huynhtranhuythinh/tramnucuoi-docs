# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU10.Final
# PUBLIC DISCOVERY & TRUST REBASE — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-26

Status: **COMPLETE / CLOSED / PASS**

## 1. Scope

P17-WU10 rebases public Journey discovery and trust around canonical P17 truth.

Public experience now explains and enforces the difference between:

- a Journey being discoverable;
- a Journey accepting volunteer applications;
- public-safe staffing/resource needs;
- published Field Updates;
- documentary evidence;
- verified/withheld Result claims;
- MEMORY publication;
- private operational/participant truth.

WU10 does not create popularity ranking, social ranking, inferred attendance or inferred Impact.

## 2. Starting gap

Source already had:
- canonical public Journey lifecycle;
- Journey index/detail routes;
- documentary trust workflow;
- Resource Support;
- WU4 public-safe staffing projection;
- Result/Impact publication restricted to MEMORY.

Audit identified two real gaps.

### UI discovery gap

The public site did not clearly explain the P17 lifecycle/trust model, and the existing WU4 public staffing projection was only exposed inside the application flow rather than as public Journey discovery information.

### Database authority drift

Production public-read RLS for:
- journey_updates;
- journey_media;
- journey_field_notes

still depended on legacy `journeys.status`, while public Journey authority had already moved to `journeys.lifecycle_phase`.

This could allow legacy status and canonical lifecycle to disagree about public visibility.

WU10 removes that authority drift.

## 3. Public Journey discovery UX

Added:

`src/components/journeys/journey-discovery-guide.tsx`

The Journey index now explains the four public lifecycle phases:

- UPCOMING;
- ACTIVE;
- CLOSEOUT_PENDING;
- MEMORY.

The guide explicitly states that dates, application counts and engagement do not determine real-world lifecycle truth.

No popularity/trending/ranking model was introduced.

## 4. Public Journey trust lens

Added:

`src/components/journeys/journey-public-trust.tsx`

Every public Journey detail can explain how to interpret the current phase.

Trust rules communicated in VI/EN include:

- application/support intent != attendance;
- event timing != presence;
- public media != automatic Result;
- participant count/media/post volume != Impact;
- Closeout is required before official Memory/Results;
- registration identity, volunteer records, attendance and operational data are not public content by default.

## 5. Public-safe volunteer staffing discovery

Added:

`src/components/journeys/journey-public-staffing-needs.tsx`

This reuses the existing WU4 projection path:

`public.tnc_public_staffing_options(uuid)`

No direct public read of operational Department/Application/Participant tables was introduced.

Public surface contains only:
- Department name;
- current volunteer target;
- public requirements.

The UI explicitly states:

**target need != approved volunteers != attendance != achieved Result**

The public wrapper remains SECURITY INVOKER.
The private helper remains SECURITY DEFINER.

## 6. Result / Memory publication boundary

WU10 does not create a new Results system.

Existing Journey Impact/Result publication remains restricted to canonical MEMORY and evidence-governed public states.

WU10 source QA locks this invariant:

- no Result/Impact in UPCOMING;
- no Result/Impact in ACTIVE;
- no Result/Impact in CLOSEOUT_PENDING;
- MEMORY publication only.

## 7. Public trust RLS rebase

Source contract:

`database/contracts/p17_wu10_public_discovery_trust_rls.sql`

Rebased six policies:

1. `journey updates public read`
2. `journey updates staff read`
3. `journey media public read`
4. `journey media staff read`
5. `journey field notes public read`
6. `journey field notes staff read`

Canonical parent Journey predicate:

`lifecycle_phase in ('upcoming','active','closeout_pending','memory')`

Legacy `journeys.status` no longer controls these public Journey-derived surfaces.

Content-specific gates remain unchanged.

### Field Updates

Must remain `status='published'`.

### Journey Media

Must retain:
- parent Journey is public lifecycle;
- media `is_public=true`;
- `evidence_status='documentation'`;
- `trust_status in ('legacy_public','approved')`;
- linked Field Update, when present, must be published.

### Related Field Notes

Must retain:
- parent Journey is public lifecycle;
- post is published;
- post kind is Journey.

## 8. Negative leakage proof

WU10 DB QA intentionally created mismatched legacy/canonical states.

It proved:

- legacy `status=draft` + canonical `lifecycle_phase=upcoming` follows canonical public lifecycle;
- legacy `status=completed` + canonical `lifecycle_phase=draft` does NOT leak;
- legacy `status=completed` + canonical `lifecycle_phase=archived` does NOT leak.

Therefore a stale legacy completed status can no longer reopen public Updates/Media/Field Notes.

## 9. WU10 source release

PR:

`#125 — P17-WU10: Public Discovery & Trust Rebase`

Final exact head:

`748afc0961f0cb9a6467909355e25b9d373bdc30`

Exact-head gates:

- CI `36167666887` — **SUCCESS**
- inherited Volunteer Pilot Gate `36167666853` — **SUCCESS**

Squash-merged main:

`024ff8c5501cb1b7adb68065958e0b41397276d0`

Post-merge main CI:

`36167849764` — **SUCCESS**

## 10. WU10.Final release

PR:

`#126 — P17-WU10.Final: Public Discovery & Trust production cutover`

Final exact head:

`50d757e921d1374e008d803e34f353b5e93a461d`

Exact-head gates:

- CI `36168510801` — **SUCCESS**
- inherited Volunteer Pilot Gate `36168510748` — **SUCCESS**

Two earlier CI attempts failed only in the ephemeral rollback-test fixture because of malformed SQL dollar quoting. Production was not touched. The fixture was corrected to a named dollar quote and the complete migration+rollback QA subsequently passed.

Squash-merged production main:

`6e679c67b9a5878227e6c5466260a0883f85eaa0`

Post-merge main CI:

`36168761376` — **SUCCESS**

## 11. Production migration

Source:

`database/migrations/0065_p17_wu10_final_public_discovery_trust.sql`

Git blob SHA:

`1a74395fd5e7c1c0b31263eb9e774748bd8469df`

Rollback:

`database/rollbacks/p17_wu10_final_public_discovery_trust.sql`

Supabase project:

`iwiqprhoohkxvjyxojto`

Production ledger:

`20260925174405_p17_wu10_final_public_discovery_trust`

Apply result:

**SUCCESS**

## 12. Production truth preservation

Pre-cutover:

- Journey Updates = 5
- Journey Media relations = 11
- Journey Field Notes relations = 0
- Journeys = 5
- Impact items = 4

Post-cutover:

- Journey Updates = 5
- Journey Media relations = 11
- Journey Field Notes relations = 0
- Journeys = 5
- Impact items = 4

Therefore the cutover changed authority only and mutated no product truth.

## 13. Production policy verification

All six target policies were queried from `pg_policies` after cutover.

Verified:

- canonical `lifecycle_phase` controls parent Journey public visibility;
- no target public policy relies on legacy `journeys.status`;
- documentary media gates remain present;
- trust-status gates remain present;
- linked update publication guard remains present;
- Field Note post publication/kind guards remain present.

The Journey public policy itself remains canonical lifecycle based.

Result/Impact publication remains separately MEMORY-gated.

## 14. Runtime verification note

Main source and production DB authority are both cut over and all CI/runtime-configuration dry-run gates pass.

External search indexing still returned the older cached Field Journal representation for `/hanh-trinh` during verification, and direct browser retrieval was not available through the verification tool at this moment.

Therefore WU10 does **not** use search-engine cache as proof of current runtime UI deployment.

Full browser/UI verification remains part of the already-planned:

**P17 FINAL PRODUCT QA / OWNER UAT**

after Phase 17 implementation is complete.

## 15. Supabase Advisors

### Security

No WU10-specific security regression.

Historical project warning remains:

`Leaked Password Protection Disabled`

### Performance

WU10 created no table, FK or index.

Existing historical performance advisor findings remain outside WU10 scope.

## 16. Canonical invariants

- Journey is the primary public discovery object.
- Public discovery follows explicit lifecycle truth.
- Dates do not auto-promote lifecycle.
- No popularity/trending ranking.
- Application != Confirmation != Attendance.
- Staffing need != approved staffing != attendance.
- Pledge != Receipt != Distribution != Impact.
- Public media != Result.
- Result is never inferred from counts/media/posts.
- Results remain MEMORY-gated.
- Operational PII remains private.
- Legacy status cannot reopen canonical private lifecycle content.
- Trust/evidence/publication predicates remain defense-in-depth.

# FINAL STATUS

**P17-WU10 — COMPLETE / CLOSED / PASS**

Production capability:

**Canonical lifecycle-based Public Discovery & Trust authority is ACTIVE at source + database layers.**

Next canonical roadmap item:

**P17-WU11 — PRODUCTION PILOT RE-ACTIVATION**

After remaining Phase 17 work:

**P17 FINAL PRODUCT QA / OWNER UAT — real UI, role-by-role, mobile + desktop.**
