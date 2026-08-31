# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI
# CANONICAL KICKOFF

Date: 2026-08-31
Status: **ACTIVE — WU1/WU2 COMPLETE / PASS; WU3 NEXT**

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

At Phase 13 kickoff, Community routes were:
- `/cong-dong`
- `/en/community`

The Vietnamese route composed four large surfaces sequentially:
1. `CommunityAccountPage`
2. `CommunityRolesPanel`
3. `CommunityContributionsPanel`
4. `CommunityReflectionsPanel`

That source was functionally correct but still a foundation/demo surface rather than a coherent Personal Community Home.

P13-WU2 has now replaced route ownership with a shared `CommunityExperienceShell` and established MY TNC as the signed-in personal Community home while preserving the same stable bilingual routes and Phase 12 truth semantics.

Public Journey detail still requires the next experience layer: Before / During / After Community lifecycle integration.

## 4. Phase sequence

### P13-WU1 — Community Experience Architecture — COMPLETE / PASS

Defined:
- signed-out vs signed-in experience;
- Community route ownership;
- personal-home hierarchy;
- Journey lifecycle UX direction;
- empty/pending/resolved states;
- relationship/privacy presentation rules;
- bilingual visual/content architecture;
- source/component refactor boundary;
- activation boundary while Email remains OFF.

Canonical record:
`canon/PHASE_13_WU1_COMMUNITY_EXPERIENCE_ARCHITECTURE_2026-08-31.md`

### P13-WU2 — Personal Community Home / My TNC — COMPLETE / PASS

Implemented one coherent signed-in personal home around verified relationship data.

Canonical closeout:
`canon/PHASE_13_WU2_PERSONAL_COMMUNITY_HOME_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU2_MY_TNC_EVIDENCE_2026-08-31.md`

Product main after WU2:
`a6f3dfa4d3033d5855b1e3906d6d48beec7619ef`

Public activation remains gated while Email is OFF.

### P13-WU3 — Journey Community Experience — NEXT

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

WU2 materially satisfies criteria 1, 2, 4 and 5 at source level behind the activation gate. WU3 is the next dependency for criterion 3.

## 7. Canonical dependency

Phase 13 must read together with:
- `canon/PHASE_12_FINAL_CLOSEOUT_2026-08-31.md`
- `evidence/PHASE_12_LIVING_COMMUNITY_OS_EVIDENCE_2026-08-31.md`
- `handoff/PHASE_12_TO_PHASE_13_HANDOFF_2026-08-31.md`
- `canon/PHASE_13_WU1_COMMUNITY_EXPERIENCE_ARCHITECTURE_2026-08-31.md`
- `canon/PHASE_13_WU2_PERSONAL_COMMUNITY_HOME_2026-08-31.md`
- `evidence/PHASE_13_WU2_MY_TNC_EVIDENCE_2026-08-31.md`

## 8. Current declaration

**PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI: ACTIVE**

**P13-WU1 — COMMUNITY EXPERIENCE ARCHITECTURE: COMPLETE / PASS**

**P13-WU2 — PERSONAL COMMUNITY HOME / MY TNC: COMPLETE / PASS**

**P13-WU3 — JOURNEY COMMUNITY EXPERIENCE: NEXT**

Community public activation remains gated while Email is OFF.
