# PHASE 15 — WU1A
# PRIVACY OWNERSHIP HOTFIX & CANONICAL INTEGRITY

Date: 2026-09-01
Status: IMPLEMENTED ON BRANCH / CI GATE PENDING AT DOCUMENT CREATION

## Trigger

P15-WU1 found that `JourneyRelationshipExperience` queried four personal Community datasets by Journey only and relied on RLS for ownership isolation. P14-WU4 had already established that Community staff/admin-capable accounts may have broader read policies, so Journey personal context requires the same explicit own-user boundary as My TNC.

## Product branch

`p15-wu1a-privacy-hotfix`

Base main SHA:
`fbe8f0d85f8b28b13760b1f84307342d6c2d9fc0`

Product PR:
`#42 — P15-WU1A: harden Journey personal ownership boundary`

Initial PR head SHA:
`084bc15454b955b31e888593997f7a572f2544b8`

## Source hardening

`src/components/journeys/journey-relationship-experience.tsx` now scopes every personal read to the authenticated `userId` before Journey/status filters:

- `community_journey_memories`
- `community_contributions`
- `community_relationship_assignments`
- `journey_reflections`

The authenticated user id continues to come from the active auth session. The existing evidence-backed `claim_my_journey_participations` reconciliation remains unchanged.

## Regression gate

Added:
`scripts/p15-wu1a-journey-own-data-qa.ts`

The QA fails if any of the four explicit `user_id = signed-in user` filters disappears. It also asserts that Journey personal loading remains keyed from the authenticated user and that the existing claim reconciliation RPC was not removed.

Repository CI now runs:
`P15-WU1A Journey own-data boundary QA`

## Non-scope

- no Supabase migration
- no RLS mutation
- no production DB mutation
- no attendance data mutation
- no visual redesign

## Truth semantics preserved

- registration != attendance
- confirmation != attendance
- attendance `NULL` = unresolved
- attendance `0` = verified no-show
- attendance `> 0` = verified attended
- participant claim != attendance
- Memory remains evidence-gated
- Reflection remains evidence-gated
- P14-WU5A attendance date authority remains intact

## Closeout gate

WU1A may be declared COMPLETE / PASS only after:
1. PR CI PASS
2. PR merged to main
3. post-main CI PASS
4. exact main SHA recorded
5. canonical docs merged

P15-WU2 may begin after the privacy/canonical gate is clean. Visual redesign must not be mixed into this hotfix.
