# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI
# P13-WU3 — JOURNEY COMMUNITY EXPERIENCE

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

P13-WU3 turns the operational Journey detail into a truthful personal lifecycle experience for an already authenticated Community member:

**Before → During → After**

The implementation must never improve emotional presentation by weakening the Phase 12 truth model.

This work unit does not activate Community Auth publicly. It makes the source ready behind the existing Email/Auth activation gate.

## 2. Architectural boundary

The product contains two distinct Journey surfaces and WU3 preserves that distinction.

### Editorial / public Field Journal world

Routes such as:
- `/hanh-trinh/$slug`
- `/en/journey/$slug`

remain documentary/editorial storytelling surfaces.

### Operational Journey world

Routes:
- `/journeys/$slug`
- `/en/journeys/$slug`

are the canonical operational Journey detail surfaces with registration, field updates, evidence and impact.

P13-WU3 adds the personal lifecycle layer only to the operational Journey world.

This prevents private relationship state from being mixed into the Field Journal story model and preserves the separation defined in P13-WU1.

## 3. Implementation

Product branch:
`p13-wu3-journey-community-experience`

New source:
`src/components/journeys/journey-relationship-experience.tsx`

Updated source:
`src/components/journeys/journey-detail-page.tsx`

The operational Journey detail now mounts `JourneyRelationshipExperience` after canonical Journey metadata and before the main editorial body.

## 4. Authentication and activation boundary

The personal layer uses the existing browser backend client and existing authenticated session.

If the visitor is signed out, the component returns no Community surface.

Therefore WU3 does **not** introduce:
- a public Community sign-in CTA on Journey pages;
- new Magic Link activation;
- a navigation entry to Community;
- a production Email enablement dependency.

Public signed-out Journey experience remains unchanged while Email is OFF.

## 5. Existing Phase 12 truth reused

WU3 adds no competing Journey relationship model and no database migration.

It reuses:
- `claim_my_journey_participations`;
- `community_journey_memories`;
- `community_contributions`;
- `community_relationship_assignments`;
- `journey_reflections`.

All personal reads remain subject to the existing Phase 12 RLS model.

WU3 deliberately does not broaden application/registration PII read access merely to make the UI show more states.

If the existing safe Community projection cannot prove a state, the interface does not infer it.

## 6. Before / During / After semantics

### Before

A linked confirmed participation may be described as a confirmed participation relationship.

It is explicitly **not** described as:
- attendance;
- presence;
- Memory.

A signed-in person without a verified participation relationship does not receive a fabricated registration state.

### During

A Journey is treated as `during` only when both conditions are satisfied:
1. operational Journey status is `preparing`;
2. current Vietnam date falls inside the Journey date window.

Calendar time alone is not operational authority.

A confirmed participant whose attendance is still unresolved remains unresolved. The event being active never converts confirmation into attendance.

### After

The personal layer keeps the Phase 12 attendance distinctions explicit:

- attendance `NULL` / unresolved → attendance not yet recorded;
- attendance `0` / `no_show` → verified no-show, not an attended Memory;
- attendance `>0` with attended state → verified attendance;
- `memory_eligible` is presented only with evidence-backed attendance;
- Reflection status remains pending / published / rejected and eligibility remains dependent on completed Journey + verified attendance.

## 7. Contribution and relationship context

For the current Journey, the signed-in person may see only facts already authorized by the existing Community model:

- active verified Contributions;
- verified Host assignments;
- verified Partner-representative assignments;
- owned Reflection moderation state.

No score, inferred role or permission is created.

**Role does not equal Permission** remains unchanged.

## 8. Bilingual experience

The lifecycle component ships with VI / EN copy from the same source boundary.

The wording deliberately explains truth boundaries instead of hiding them:
- confirmation is not attendance;
- Journey time is not attendance evidence;
- unresolved remains unresolved;
- no-show does not become Memory;
- Reflection remains moderated.

## 9. Pilot continuity

Live pilot Journey:
`19539f36-3ed4-4a22-96b9-c8a9b73c5283`

Title:
`Trạm Cơm Chay Yêu Thương — Đổi Nụ Cười · Mùng 1 Tháng 8`

Event date:
`2026-09-11`

At WU3 closeout the Journey remains:
- status `registration_open`;
- before the event date;
- confirmed registration remains distinct from attendance;
- attendance remains unresolved unless operations records otherwise.

WU3 does not mutate the pilot.

## 10. QA history

### Initial PR CI — #141

Run:
`33366575674`

Results before correction:
- P9→P12 database/regression suite: PASS;
- build: PASS;
- strict TypeScript typecheck: FAIL;
- Cloudflare dry-run: not reached because typecheck failed.

The failure was TS18047: the nullable backend client was correctly guarded in the effect, but TypeScript did not preserve that narrowing inside a nested async closure.

The correction introduced a non-null local constant after the existing guard:
`const backend = client`

All nested calls then use that narrowed constant.

No compiler option, permission, RLS policy, lifecycle rule or product semantics were weakened.

### Final PR CI — #142

PR head:
`6442c7f746118981959c296a6be1f08e2a6c784a`

Run:
`33366762068`

PASS:
- P9-WU7 source abuse-protection QA;
- P10-WU3A runtime-context regression;
- P9 DB gate/rollback;
- P11-WU11 capacity/cutoff;
- P12-WU1 through P12-WU7 DB QA;
- build;
- strict typecheck;
- Cloudflare dry-run.

## 11. Merge and main verification

Product PR:
`#28 — P13-WU3 Journey Community Experience`

Merged product main SHA:
`75706af5b2dfa5e9b01b34150aa2e440406640e4`

Main push CI:
- run #143;
- run ID `33366949521`;
- result: **PASS**;
- full P9→P12 regression, build, typecheck and Cloudflare dry-run all PASS.

## 12. Production mutation statement

P13-WU3 performed no:
- Supabase migration;
- production database mutation;
- fake-data seed;
- Cloudflare production deploy;
- Email activation;
- Turnstile activation;
- public Community Auth activation;
- application PII permission expansion.

## 13. Closeout declaration

**P13-WU3 — JOURNEY COMMUNITY EXPERIENCE: COMPLETE / PASS**

Canonical product main:
`75706af5b2dfa5e9b01b34150aa2e440406640e4`

Phase 13 remains ACTIVE.

Next gated work unit:
**P13-WU4 — COMMUNITY PEOPLE & RELATIONSHIP UI**

WU4 is not implemented by this closeout.
