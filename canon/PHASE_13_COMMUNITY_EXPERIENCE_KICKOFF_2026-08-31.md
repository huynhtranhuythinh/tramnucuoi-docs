# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI
# CANONICAL KICKOFF

Date: 2026-08-31
Status: **ACTIVE — WU1/WU2/WU3/WU4 COMPLETE / PASS; WU5 NEXT**

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

## 3. Existing source reality and progress

At Phase 13 kickoff, Community routes were:
- `/cong-dong`
- `/en/community`

The Vietnamese route composed four large surfaces sequentially:
1. `CommunityAccountPage`
2. `CommunityRolesPanel`
3. `CommunityContributionsPanel`
4. `CommunityReflectionsPanel`

That source was functionally correct but still a foundation/demo surface rather than a coherent Personal Community Home.

P13-WU2 replaced route ownership with a shared `CommunityExperienceShell` and established MY TNC as the signed-in personal Community home while preserving the same stable bilingual routes and Phase 12 truth semantics.

P13-WU3 then integrated a personal Before / During / After lifecycle into the operational Journey detail routes:
- `/journeys/$slug`
- `/en/journeys/$slug`

The Field Journal/editorial routes remain separate. Signed-out operational Journey pages do not promote Community authentication while Email remains OFF.

P13-WU4 added a privacy-safe person-centered Relationship Map to My TNC. It presents Explorer / Participant / Contributor / Host / Partner representative from existing verified source truth without creating a public people directory or interpreting internal relationship verification as consent to publish identity.

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

### P13-WU3 — Journey Community Experience — COMPLETE / PASS

Implemented authenticated personal Journey lifecycle context on the operational Journey detail while preserving registration/attendance/Memory separation.

Canonical closeout:
`canon/PHASE_13_WU3_JOURNEY_COMMUNITY_EXPERIENCE_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU3_JOURNEY_EXPERIENCE_EVIDENCE_2026-08-31.md`

Product main after WU3:
`75706af5b2dfa5e9b01b34150aa2e440406640e4`

Key lifecycle rule:
- Before: confirmed participation is not attendance or Memory;
- During: requires operational `preparing` status plus Vietnam date window; event time alone is not attendance evidence;
- After: unresolved / no-show / attended / Memory / Reflection states remain evidence-backed and distinct.

### P13-WU4 — Community People & Relationship UI — COMPLETE / PASS

Implemented a private-by-default Relationship Map inside My TNC.

Canonical closeout:
`canon/PHASE_13_WU4_COMMUNITY_PEOPLE_RELATIONSHIP_UI_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU4_COMMUNITY_RELATIONSHIP_UI_EVIDENCE_2026-08-31.md`

Product main after WU4:
`cb465399f4860e4dfa842e2008e62547dbce8fde`

Key relationship rules:
- Explorer remains the base authenticated relationship only;
- Participant is derived from evidence-backed attended Memory;
- Contributor is derived from active verified Contribution;
- Host / Partner representative come only from verified relationship assignments;
- role never grants CMS permission;
- own display name is not legal identity proof and is not automatically public;
- verified internal relationship does not itself authorize public identity publication;
- no public Community member directory was created.

Public activation remains gated while Email is OFF.

### P13-WU5 — Living Community Surface — NEXT

Build a wider editorial Community surface that reveals real activity and relationships without becoming a generic feed.

WU5 must preserve the WU4 publication boundary. It may use public Journey/Project facts, approved documentary evidence, moderated public Reflections and privacy-safe relationship narratives, but must not expose a person's identity merely because an internal relationship is verified.

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
- zero fabricated attendance/Memory/Contribution/relationship/impact facts;
- verified relationship existence is not automatic identity-publication consent.

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

WU2 materially satisfies criteria 1, 2, 4 and 5 at source level behind the activation gate.

WU3 materially satisfies criterion 3 at source level while preserving the same activation gate and truth semantics.

WU4 strengthens criteria 2, 5 and 6 by making the personal relationship graph legible while explicitly separating internal verification from public identity publication.

WU5 is the next dependency for criterion 6: the wider Living Community surface.

## 7. Canonical dependency

Phase 13 must read together with:
- `canon/PHASE_12_FINAL_CLOSEOUT_2026-08-31.md`
- `evidence/PHASE_12_LIVING_COMMUNITY_OS_EVIDENCE_2026-08-31.md`
- `handoff/PHASE_12_TO_PHASE_13_HANDOFF_2026-08-31.md`
- `canon/PHASE_13_WU1_COMMUNITY_EXPERIENCE_ARCHITECTURE_2026-08-31.md`
- `canon/PHASE_13_WU2_PERSONAL_COMMUNITY_HOME_2026-08-31.md`
- `evidence/PHASE_13_WU2_MY_TNC_EVIDENCE_2026-08-31.md`
- `canon/PHASE_13_WU3_JOURNEY_COMMUNITY_EXPERIENCE_2026-08-31.md`
- `evidence/PHASE_13_WU3_JOURNEY_EXPERIENCE_EVIDENCE_2026-08-31.md`
- `canon/PHASE_13_WU4_COMMUNITY_PEOPLE_RELATIONSHIP_UI_2026-08-31.md`
- `evidence/PHASE_13_WU4_COMMUNITY_RELATIONSHIP_UI_EVIDENCE_2026-08-31.md`

## 8. Current declaration

**PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI: ACTIVE**

**P13-WU1 — COMMUNITY EXPERIENCE ARCHITECTURE: COMPLETE / PASS**

**P13-WU2 — PERSONAL COMMUNITY HOME / MY TNC: COMPLETE / PASS**

**P13-WU3 — JOURNEY COMMUNITY EXPERIENCE: COMPLETE / PASS**

**P13-WU4 — COMMUNITY PEOPLE & RELATIONSHIP UI: COMPLETE / PASS**

**P13-WU5 — LIVING COMMUNITY SURFACE: NEXT**

Community public activation remains gated while Email is OFF.
