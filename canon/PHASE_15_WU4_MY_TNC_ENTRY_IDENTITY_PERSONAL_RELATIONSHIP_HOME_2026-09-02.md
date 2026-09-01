# PHASE 15 — WU4
# MY TNC ENTRY, IDENTITY & PERSONAL RELATIONSHIP HOME

Date: 2026-09-02
Status: SOURCE GATE IN PROGRESS
Production activation status: NOT DECLARED / NOT VERIFIED IN THIS WU

## Purpose

Transform My TNC from an account/record center into a private personal relationship archive with Trạm while preserving all canonical identity, ownership, attendance, Memory and Reflection truth boundaries.

Core experience statement:

**VI:** `Những lần tôi và Trạm đã gặp nhau.`

**EN:** `The times Trạm and I have met.`

My TNC is not a social network, achievement dashboard or proof of attendance. It is a private continuity surface built from records that genuinely belong to the signed-in person.

## Product scope

Product branch:
`p15-wu4-my-tnc-relationship-home`

Base product main SHA:
`f1d98844b7c2b15245ce73e9b975551674f83055`

Product PR:
`#45 — P15-WU4: My TNC Entry, Identity & Personal Relationship Home`

Current PR head:
`c581b5142ead187c22401c448e203f3e91cdc135`

Merged product main SHA:
`PENDING SOURCE GATE`

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

## CI evidence

PENDING SOURCE GATE.

## Production boundary

The product repository has no GitHub production deployment workflow. CI's Cloudflare command is a configuration dry-run only; it does not upload or deploy the Worker.

Therefore WU4 must not declare production activation or live visual verification without a separate real deployment path and evidence. No Worker version will be inferred or fabricated.

## Closeout

PENDING SOURCE GATE.

NEXT after PASS:
**P15-WU5 — Journey Lifecycle Experience Redesign**
