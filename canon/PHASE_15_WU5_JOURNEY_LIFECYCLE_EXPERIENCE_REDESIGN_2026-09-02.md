# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 15 / WU5 — JOURNEY LIFECYCLE EXPERIENCE REDESIGN

**Date:** 2026-09-02  
**Status:** COMPLETE / PASS — SOURCE & CANONICAL  
**Production activation status:** NOT DECLARED / NOT VERIFIED IN THIS WU

## 1. Canonical scope

P15-WU5 redesigns the Journey experience as one continuous, truth-safe lifecycle rather than disconnected public, registration and personal surfaces.

Lifecycle model:

**BEFORE**  
Discover → Trust → Understand → Register → Confirmation → Prepare

**DURING**  
Participate → Experience → Documentary / field-evidence context

**AFTER**  
Attendance truth → Memory → Reflection → Contribution → Relationship continuity

The model is presentation architecture only. It does not create attendance, Memory, Reflection, Contribution, Relationship or Impact truth.

## 2. Canonical source evidence

Product repository: `huynhtranhuythinh/tramnucuoi`

- Product base before WU5: `4efa77bf5a9dff39121ff4fbabe74380c4c9f218`
- WU5 branch: `p15-wu5-journey-lifecycle-experience`
- Product PR: `#46 — P15-WU5: Journey Lifecycle Experience Redesign`
- Final PR head: `03eb37257f39590cedd7338dd4737b8b34523c14`
- PR-head CI: `#187`, run ID `33543315734` — PASS
- Product main after merge: `ff80421c2a173da1af8d72888193efc87c285dea`
- Post-main CI: `#188`, run ID `33543471308` — PASS

## 3. Source changes

Primary WU5 files:

- `src/lib/journeys/attendance-date-authority.ts`
- `src/components/journeys/journey-lifecycle-guide.tsx`
- `src/components/journeys/journey-detail-page.tsx`
- `src/components/journeys/journey-meta.ts`
- `src/components/journeys/journeys-page.tsx`
- `src/components/journeys/registration-form.tsx`
- `src/components/journeys/journey-relationship-experience.tsx`
- `src/components/community/community-experience-shell.tsx`
- `scripts/p15-wu5-journey-lifecycle-qa.ts`
- `.github/workflows/ci.yml`

No database migration, RLS change, Supabase mutation, auth mutation or production runtime mutation was introduced.

## 4. One Journey, one lifecycle

### BEFORE

The public Journey now leads with story, purpose and trustworthy factual metadata before participation mechanics.

A bilingual editorial lifecycle guide explains that:

- registration helps Trạm prepare;
- registration is not attendance;
- confirmation of registration is not attendance;
- no Memory or Reflection is promised from registration alone.

Registration success now provides a calm continuation: Trạm has received the registration, review/contact is the next preparation step, and registration or confirmation never proves presence.

### DURING

The Journey guide describes the real-world experience without creating a fake live product surface.

It explicitly states that documentary material may be created subject to appropriate policy and consent, while the website does not simulate live activity or infer attendance from event timing.

No social feed, live-presence claim, participant counter, gamification or inferred attendance was introduced.

### AFTER

Personal Journey context keeps post-Journey truth states distinct:

- unresolved remains unresolved;
- verified no-show remains verified as not attended;
- attended appears only from canonical attendance evidence;
- Memory appears only when canonical eligibility exists;
- Reflection remains evidence-gated and requires completed Journey + verified attendance;
- Contributions and Relationships appear only from their existing canonical sources.

## 5. Vietnam date authority

WU5 extends the existing P14-WU5A date-authority module instead of creating a competing date system.

`src/lib/journeys/attendance-date-authority.ts` now provides shared presentation helpers:

- `VIETNAM_TIME_ZONE = "Asia/Ho_Chi_Minh"`
- `vietnamCalendarDate()`
- `journeyPresentationPhase()`
- `formatVietnamCalendarDate()`

Date-relative Journey presentation is therefore independent of the visitor/operator device timezone.

P14-WU5A attendance-control authority remains unchanged in semantics and remains protected by its existing source and DB QA.

## 6. Canonical attendance precedence

WU5 establishes the presentation rule:

**explicit canonical attendance truth outranks date-derived lifecycle presentation.**

In both authenticated Journey context and My TNC Journey history:

1. canonical attended / no-show state is evaluated first;
2. only unresolved records may use date-derived labels such as upcoming/before/during/after.

This prevents a date-derived UI label from conceptually masking a real canonical attended or no-show state.

## 7. Human system language

WU5 removes implementation-field vocabulary from Journey-facing human copy.

In particular, `memory_eligible` remains a required source/domain field but is no longer exposed as explanatory copy to visitors or My TNC users.

No raw `ATTENDANCE: 0` notation is used for no-show; the human state is presented as verified not attended.

## 8. Privacy and own-data boundary retained

P15-WU1A defense-in-depth ownership filters remain intact for personal Journey data:

- `community_journey_memories`
- `community_contributions`
- `community_relationship_assignments`
- `journey_reflections`

Each continues to include an explicit authenticated `user_id` filter in addition to RLS.

The existing participant-claim RPC remains `claim_my_journey_participations`.

WU5 does not broaden Community authentication, CMS permissions, relationship permissions or public exposure.

## 9. Deterministic QA

Added:

`P15-WU5 Journey lifecycle experience QA`

Script:

`scripts/p15-wu5-journey-lifecycle-qa.ts`

The gate verifies, among other contracts:

- shared Vietnam calendar authority is present and used;
- Journey detail renders the shared lifecycle guide;
- VI/EN registration copy distinguishes registration from attendance;
- DURING copy does not simulate live activity or infer attendance;
- My TNC uses Vietnam date perspective instead of UTC device-date shortcuts;
- canonical no-show/attended state precedes upcoming presentation;
- human copy does not expose `memory_eligible`;
- exact own-user filters remain;
- Reflection eligibility remains completed-Journey + evidence-backed attendance;
- no raw attendance-zero notation;
- no leaderboard, points or member-directory patterns;
- earlier P14/P15 regression gates remain in CI.

## 10. CI evidence

### PR-head CI #187 / run 33543315734

PASS:

- P9-WU7 source abuse protection
- P10-WU3A runtime-context regression
- P13-WU6 Community activation
- P14-WU2 controlled Community Auth activation
- P14-WU4 My TNC own-data boundary
- P14-WU4A credential lifecycle
- P14-WU5A attendance date-authority source QA
- P15-WU1A Journey own-data boundary
- P15-WU2 experience design system
- P15-WU3 public Story World editorial
- P15-WU4 My TNC relationship home
- **P15-WU5 Journey lifecycle experience**
- all retained ephemeral DB gates
- build
- typecheck
- Cloudflare dry-run

### Post-main CI #188 / run 33543471308

The same complete gate set passed on product main SHA:

`ff80421c2a173da1af8d72888193efc87c285dea`

## 11. Production boundary

WU5 does **not** establish that these source changes are live on `https://tramnucuoi.com`.

The repository CI performs Cloudflare configuration/build dry-run only. It does not upload or deploy the production Worker.

Therefore:

**Production activation status: NOT DECLARED / NOT VERIFIED IN THIS WU.**

No Worker version is inferred or invented.

## 12. Relationship to P14-WU5 / real pilot evidence

P14-WU5 Post-Journey Memory & Reflection Operations remains independently OPEN until real post-Journey pilot evidence exists after the Journey scheduled for 2026-09-11.

WU5 changes the experience architecture and truth-safe presentation. It does not fabricate populated attendance, Memory, Reflection, Contribution, Relationship or Impact evidence ahead of that Journey.

## 13. Gate to next work unit

P15-WU5 is COMPLETE / PASS — SOURCE & CANONICAL.

Next canonical work unit:

**P15-WU6 — Living Community Experience Redesign**

WU6 may redesign how people, Journeys, relationships and memories are perceived as a living community, but must not become a social network, activity feed, leaderboard, member directory or vanity-metric surface.
