# PHASE 15 — WU4
# MY TNC ENTRY, IDENTITY & PERSONAL RELATIONSHIP HOME

Date: 2026-09-02
Status: COMPLETE / PASS — SOURCE & CANONICAL
Production activation status: NOT DECLARED / NOT VERIFIED IN THIS WU

## Purpose

Transform My TNC from an account/record center into a private personal relationship archive with Trạm while preserving all canonical identity, ownership, attendance, Memory and Reflection truth boundaries.

Core experience statement:

**VI:** `Những lần tôi và Trạm đã gặp nhau.`

**EN:** `The times Trạm and I have met.`

My TNC is not a social network, achievement dashboard or proof of attendance. It is a private continuity surface built from records that genuinely belong to the signed-in person.

## Product implementation

Product branch:
`p15-wu4-my-tnc-relationship-home`

Base product main SHA:
`f1d98844b7c2b15245ce73e9b975551674f83055`

Product PR:
`#45 — P15-WU4: My TNC Entry, Identity & Personal Relationship Home`

Final PR head SHA:
`be972e34e33c7a79637be5eb49a1f0931de0a372`

Merged product main SHA:
`4efa77bf5a9dff39121ff4fbabe74380c4c9f218`

## Experience work

### 1. My TNC entry before sign-in

`src/components/community/community-account-gateway.tsx`

The account gateway is reframed around the private relationship archive rather than an account product.

The entry experience now explains:
- an account helps Trạm recognise the right person
- account != participant
- account != attendance
- registration != attendance
- Memory and Reflection require their own evidence

Login, signup, email confirmation, Magic Link, password recovery and MFA are presented as human experience states serving this continuity, not as vendor-oriented utility screens.

### 2. Identity as continuity

`src/components/community/community-experience-shell.tsx`

The signed-in identity surface is now secondary to the personal story. It shows:
- verified email
- how Trạm should address the person
- display-name editing
- sign out

It explicitly avoids presenting identity as achievement, score or legal identity verification.

### 3. Personal Relationship Home

The former dashboard-like hero and relationship counters are removed.

The primary statement is the personal archive itself. Journey history is presented as a chronological relationship timeline rather than a stack of status cards.

Status remains evidence-first:
- unresolved: `Chưa có dữ liệu xác minh` / `Not yet verified`
- no-show: `Đã xác minh không tham dự` / `Verified as not attended`
- attended: `Đã xác minh tham dự` / `Attendance verified`

The label changes are presentation-only. The underlying canonical attendance semantics are unchanged.

### 4. Empty and system states

P15-WU2 `ExperienceState` is used for loading, empty, error and unavailable states. Empty is treated as legitimate; the UI does not manufacture activity, impact, Memory or relationships to make My TNC appear populated.

### 5. Contributions, relationships and Reflections

Existing personal data remains on the same signed-in surface but is editorially quieter:
- contributions are not scored or aggregated into vanity impact numbers
- relationship roles are sourced descriptions, not badges or CMS permissions
- Reflection remains evidence-gated and moderated

## Security and truth preserved

The exact P14-WU4 own-data contracts remain in `CommunityExperienceShell`:
- `community_journey_memories` explicitly filtered by signed-in `user_id`
- `community_contributions` explicitly filtered by signed-in `user_id`
- `community_relationship_assignments` explicitly filtered by signed-in `user_id`
- `journey_reflections` explicitly filtered by signed-in `user_id`
- display-name mutation explicitly filtered by signed-in `user_id`

P14-WU4A account lifecycle remains intact:
- password login
- signup + one-way email-confirmation pending state
- Magic Link with `shouldCreateUser: false`
- password recovery
- MFA challenge/verify
- AAL2 enforcement where required
- Auth callback work deferred to avoid synchronous auth/PostgREST deadlock

Canonical truth remains:
- registration != attendance
- confirmed registration != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance >0 = verified attended
- participant claim != attendance
- participant claim != Memory eligibility
- account != participant
- account != attendance
- relationship role != CMS permission
- Memory remains driven by evidence-backed `memory_eligible`
- Reflection eligibility remains Memory-eligible + completed Journey
- P14-WU5A attendance date authority remains authoritative

## P14-WU2 regression-contract reconciliation

The first WU4 PR run, CI #184, correctly failed at the older `P14-WU2 controlled Community Auth activation QA`.

The failure was not a runtime auth failure. The old WU2 source gate still required legacy Magic Link onboarding to live in `CommunityExperienceShell` with `shouldCreateUser: true`. P14-WU4A had already superseded that architecture by moving canonical account lifecycle into `CommunityAccountGateway`, where signup is explicit and Magic Link is existing-account-only with `shouldCreateUser: false`.

WU4 therefore reconciled `scripts/p14-wu2-controlled-activation-qa.ts` to the active canonical architecture instead of restoring obsolete implicit signup behavior. The reconciled gate now requires:
- the Community activation feature gate remains fail-closed by default
- VI/EN routes retain `CommunityAccountGateway`, `CommunityExperienceShell` and Living Community composition
- explicit `auth.signUp` remains in the gateway
- Magic Link remains in the gateway
- locale-specific auth callbacks remain
- Magic Link must use `shouldCreateUser: false`
- neither gateway nor signed-in shell may restore `shouldCreateUser: true`
- My TNC must not grant CMS/admin roles

CI #185 and post-main CI #186 both pass the reconciled P14-WU2 gate together with P14-WU4A, confirming the reconciliation did not weaken the canonical account lifecycle.

## Regression QA

Added:
`scripts/p15-wu4-my-tnc-relationship-home-qa.ts`

CI step:
`P15-WU4 My TNC relationship-home QA`

The WU4 gate checks:
- relationship-first VI/EN entry and home language
- absence of dashboard-style relationship counters
- human unresolved/no-show/attended labels
- use of P15-WU2 editorial/state primitives
- exact P14 own-data ownership filters
- Magic Link cannot silently create an account
- Reflection eligibility remains evidence-first
- prohibited social/gamification patterns do not enter Personal Home

## CI evidence

- Product PR #45: MERGED
- Initial PR CI #184: FAIL at stale P14-WU2 legacy source contract; no merge occurred
- Final PR head: `be972e34e33c7a79637be5eb49a1f0931de0a372`
- PR CI #185: PASS
- Merged product main: `4efa77bf5a9dff39121ff4fbabe74380c4c9f218`
- Post-main CI #186: PASS
- P15-WU4 My TNC relationship-home QA: PASS
- P15-WU3 public Story World editorial QA: PASS
- P15-WU2 experience design system QA: PASS
- P15-WU1A Journey own-data boundary QA: PASS
- P14-WU5A attendance date-authority source + DB QA: PASS
- P14-WU4 own-data boundary QA: PASS
- P14-WU4A account credential lifecycle QA: PASS
- reconciled P14-WU2 controlled Community Auth activation QA: PASS
- all earlier database regression gates: PASS
- build: PASS
- typecheck: PASS
- Cloudflare dry-run: PASS

## Non-scope

- no database migration
- no RLS mutation
- no production data mutation
- no attendance mutation
- no Memory eligibility mutation
- no Community Relationship Map redesign
- no Living Community redesign
- no social feed or member directory
- no points, badges, leaderboard or vanity counters

## WU5 lifecycle follow-up

WU4 deliberately does not expand into Journey lifecycle authority. P15-WU5 must reconcile the remaining lifecycle presentation details as part of its broader Journey redesign, including:
- ensuring all date-relative UI decisions use the canonical Vietnam date perspective rather than generic UTC client-date shortcuts
- making explicit canonical attendance states take precedence over any purely date-derived presentation label
- removing remaining implementation-field vocabulary from Journey-facing human copy where it does not add value

These are WU5 presentation/lifecycle follow-ups. They do not change WU4 ownership, auth, attendance, Memory or Reflection source truth.

## Production boundary

The product repository has no GitHub production deployment workflow. CI's Cloudflare command is a configuration dry-run only; it does not upload or deploy the Worker.

Therefore WU4 does **not** declare production activation or live visual verification. No Worker version is inferred or fabricated.

## Closeout

P15-WU4 is COMPLETE / PASS for source and canonical architecture.

NEXT:
**P15-WU5 — Journey Lifecycle Experience Redesign**
