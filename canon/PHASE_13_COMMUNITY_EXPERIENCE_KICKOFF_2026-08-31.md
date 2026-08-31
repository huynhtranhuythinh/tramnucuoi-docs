# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI
# CANONICAL KICKOFF

Date: 2026-08-31
Status: **ACTIVE**

## 1. Phase objective

Phase 12 established the trustworthy Living Community OS domain foundation.

Phase 13 turns that foundation into a coherent, emotional and useful experience for real people.

The goal is not to add more database concepts by default. The goal is to make existing verified relationships legible through UI/UX:

**Identity → Journey → Memory → Reflection → Contribution → Community Roles → Host Network → Partner / Impact Network**

Phase 13 should make a person feel:

> “Tôi biết mình đã đi cùng Trạm ở đâu, đã góp điều gì, đang có mối liên hệ nào, và những điều đó trở thành một phần ký ức sống của cộng đồng.”

## 2. Product principles

### Experience before feature count

Phase 13 should first organize existing capabilities into understandable journeys before creating new primitives.

### Real-world truth remains authoritative

UI state must follow Phase 12 semantics. Do not make the interface more emotionally impressive by weakening truth boundaries.

### Personal first, public second

The first high-value experience is a person's own relationship with Trạm: their Journey history, Memory, Contribution and roles.

Public Community surfaces come after the personal experience is coherent.

### Editorial, not social-feed

The visual language remains documentary/editorial and human. Community does not mean infinite feed, follower mechanics or engagement gamification.

### Bilingual by architecture

Every user-facing Phase 13 surface must be designed for VI / EN from the start.

## 3. Existing source reality at kickoff

Current Community routes:
- `/cong-dong`
- `/en/community`

Current Vietnamese route composes four large surfaces sequentially:
1. `CommunityAccountPage`
2. `CommunityRolesPanel`
3. `CommunityContributionsPanel`
4. `CommunityReflectionsPanel`

The current Community Account component also owns:
- Magic Link sign-in;
- participant claim flow;
- My Journey archive projection;
- attendance-state explanation;
- sign-out.

This implementation is functionally correct but is still a **foundation/demo surface**, not yet a coherent Personal Community Home.

Current public Journey detail remains primarily an editorial story page. It does not yet express a full Community lifecycle of Before / During / After for a signed-in person.

Therefore Phase 13 starts with information architecture and experience hierarchy, not a database migration.

## 4. Phase sequence

### P13-WU1 — Community Experience Architecture — ACTIVE

Define:
- signed-out vs signed-in experience;
- Community route ownership;
- personal-home hierarchy;
- Journey lifecycle UX;
- empty/pending/resolved states;
- relationship/privacy presentation rules;
- bilingual visual/content architecture;
- source/component refactor boundary;
- activation boundary while Email remains OFF.

No production mutation required.

### P13-WU2 — Personal Community Home / My TNC

Build the coherent signed-in personal home around verified relationship data.

### P13-WU3 — Journey Community Experience

Integrate Before / During / After Journey states while preserving registration/attendance/Memory semantics.

### P13-WU4 — Community People & Relationship UI

Present verified Host / Contributor / Participant / Partner-representative relationships with privacy-safe defaults.

### P13-WU5 — Living Community Surface

Build a wider editorial Community surface that reveals real activity and relationships without becoming a generic feed.

### P13-WU6 — Public Activation & Polish

Close responsive, bilingual, accessibility, SEO/noindex, motion, production activation and Owner Review gates.

## 5. Phase 13 guardrails

Phase 13 must preserve:
- P11-WU6 pilot operational authority;
- Email OFF until separately activated;
- Turnstile OFF until separately activated;
- Community Auth activation gate;
- `pg_graphql` OFF;
- CMS roles exactly `admin | editor`;
- zero fabricated attendance/Memory/Contribution/relationship/impact facts.

UI work may use truthful empty states and clearly labeled preview/development treatment, but must not seed fake production facts.

## 6. Success criteria

Phase 13 is successful when:

1. `/cong-dong` and `/en/community` feel like a real Community product rather than four technical panels stacked together.
2. A signed-in person can immediately understand their relationship to Trạm.
3. Journey lifecycle state is clear before, during and after participation.
4. Memory is emotionally valuable while still evidence-backed.
5. Contribution and roles feel meaningful without scorekeeping.
6. Public Community storytelling can grow from verified facts without exposing private identity or becoming a social feed.
7. The experience remains distinctly TRẠM NỤ CƯỜI: warm, documentary, editorial, trustworthy and bilingual.

## 7. Canonical dependency

Phase 13 must read together with:
- `canon/PHASE_12_FINAL_CLOSEOUT_2026-08-31.md`
- `evidence/PHASE_12_LIVING_COMMUNITY_OS_EVIDENCE_2026-08-31.md`
- `handoff/PHASE_12_TO_PHASE_13_HANDOFF_2026-08-31.md`

## 8. Kickoff declaration

**PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI: ACTIVE**

**P13-WU1 — COMMUNITY EXPERIENCE ARCHITECTURE: ACTIVE**

No Phase 13 production mutation has occurred at kickoff.
