# PHASE 15 — WU1
# CANONICAL UX AUDIT & REDESIGN FOUNDATION

Date: 2026-09-01
Status: COMPLETE / OWNER APPROVED

## Mission

Move TRẠM NỤ CƯỜI from a technically correct social-impact platform toward a premium emotional living digital community experience while preserving evidence, privacy and Journey truth.

## Audit conclusion

The strongest current layer is the public editorial Story World. The largest experience gap is Community / My TNC, where the data model is relationship-rich but the interface remains too record-first and architecture-explanatory. Journey truth is strong but the lifecycle is fragmented across modules rather than experienced as one coherent before/during/after story.

## Priority findings

### P0

1. `JourneyRelationshipExperience` personal reads were RLS-dependent and did not explicitly scope four personal datasets to the authenticated `user_id`. This matches the privacy class already fixed in P14-WU4 My TNC and requires an independent hotfix before visual redesign.
2. Canonical docs stopped at P14-WU4A while product main already contained P14-WU5A attendance date-authority hardening. Documentation must be reconciled before Phase 15 source evolution.

### P1

- public Community and private My TNC are mixed into one heavy route
- My TNC is record-first rather than relationship-first
- Journey before/during/after is fragmented
- auth states are technically correct but still use technical/vendor language in places
- registration completion does not yet create a strong emotional continuity into Journey preparation
- operational Journey discovery and editorial Field Journal need clearer complementary roles
- mobile VI/EN has long technical labels and stacked-section density that should be redesigned intentionally, not merely made responsive

## Immutable truth rules for Phase 15

- registration != attendance
- confirmed registration != attendance
- attendance `NULL` = unresolved
- attendance `0` = verified no-show
- attendance `> 0` = verified attended
- participant claim != attendance
- participant claim != Memory eligibility
- account != participant
- account != attendance
- relationship role != CMS permission
- Memory only from real evidence
- Reflection remains evidence-gated
- no fake Community activity or impact
- no inferred attendance presented as fact
- no weakening of P14-WU5A attendance date authority

## Design principles

1. Relationship over records
2. Human language over database language
3. Truth can be quiet
4. Story first, status second
5. Documentary evidence is content, not decoration
6. Empty is a legitimate state
7. One Journey, one lifecycle
8. Personal does not mean social network
9. Community is presence, not activity volume
10. Mobile editorial composition, not desktop stacking
11. English is authored, not mechanically translated
12. Calm confidence: fewer cards/borders, stronger hierarchy, restrained motion

## Target experience architecture

### 1. Public Story World
Home → About → Ecosystem → Projects → Field Journal

Goal: help a visitor understand who Trạm is, why it exists and how it works.

### 2. Journey World
Discover → Understand → Decide → Register → Prepare → Participate → After

Goal: one living Journey lifecycle rather than disconnected modules.

### 3. My TNC — Personal Relationship Archive
Mental model: “Những lần tôi và Trạm đã gặp nhau.”

Order of meaning:
- today / current relationship state
- Journeys waiting or upcoming
- Journeys shared
- Memories documented
- contributions made
- reflections and continuity
- verified ongoing relationships

Only truthful sections appear.

### 4. Living Community
Public editorial presence through real Journeys, documentary moments, moderated Reflections and publication-approved Host/Partner context. No public member directory, feed, leaderboard, badges or vanity counters.

### 5. Trust & Identity
Shared human-language states for signup, email confirmation, login, Magic Link, recovery, MFA, loading, unresolved, no-show, pending, permission, error, Memory unavailable and Reflection unavailable.

## Approved Phase 15 sequence

- P15-WU1 — Canonical UX Audit & Redesign Foundation
- P15-WU1A — Privacy Ownership Hotfix & Canonical Integrity
- P15-WU2 — Experience Design System & Responsive Language
- P15-WU3 — Public Story World Editorial Elevation
- P15-WU4 — My TNC Entry, Identity & Personal Relationship Home
- P15-WU5 — Journey Lifecycle Experience Redesign
- P15-WU6 — Living Community Experience Redesign
- P15-WU7 — Post-Journey Memory, Reflection & Relationship Continuity
- P15-WU8 — Bilingual, Mobile & Cross-Surface Integration QA
- P15-WU9 — Owner Review & Final Experience Gate

P15 can proceed before the 2026-09-11 pilot Journey. Populated post-Journey verification must wait for real attendance / Memory / Reflection evidence; Phase 15 must not manufacture it.
