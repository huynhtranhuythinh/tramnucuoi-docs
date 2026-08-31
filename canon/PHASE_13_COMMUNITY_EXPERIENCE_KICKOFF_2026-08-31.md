# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI
# CANONICAL KICKOFF

Date: 2026-08-31
Status: **ACTIVE — WU1/WU2/WU3/WU4/WU5 COMPLETE / PASS; WU6 NEXT**

## 1. Phase objective

Phase 12 established the trustworthy Living Community OS domain foundation.

Phase 13 turns that foundation into a coherent, emotional and useful experience for real people.

The goal is not to add more database concepts by default. The goal is to make existing verified relationships legible through UI/UX:

**Identity → Journey → Memory → Reflection → Contribution → Community Roles → Host Network → Partner / Impact Network**

Phase 13 should make a person feel:

> “Tôi biết mình đã đi cùng Trạm ở đâu, đã góp điều gì, đang có mối liên hệ nào, và những điều đó trở thành một phần ký ức sống của cộng đồng.”

## 2. Product principles

### Experience before feature count
Phase 13 first organizes existing capabilities into understandable journeys before creating new primitives.

### Real-world truth remains authoritative
UI state follows Phase 12 semantics. Emotional presentation never weakens truth boundaries.

### Personal first, public second
The first high-value experience is a person's own relationship with Trạm. Public Community surfaces follow only after the personal experience is coherent.

### Editorial, not social-feed
Community remains documentary/editorial and human. No infinite feed, follower mechanics or engagement gamification.

### Bilingual by architecture
Every user-facing Phase 13 surface is designed for VI / EN from the start.

## 3. Progress

At kickoff, `/cong-dong` and `/en/community` exposed Phase 12 capabilities as separate technical panels.

P13-WU2 replaced that with one `CommunityExperienceShell` and established **My TNC** as the signed-in personal home.

P13-WU3 added an authenticated Before / During / After relationship layer to operational Journey detail while keeping Field Journal editorial routes separate.

P13-WU4 added a private-by-default Relationship Map that presents Explorer / Participant / Contributor / Host / Partner representative from source truth without creating a public people directory.

P13-WU5 added the wider **Living Community Surface**, composed only from already-public RLS-backed Journey facts, published Field Updates and identity-minimized Reflection publications. It deliberately does not expose private people/relationship data merely to make the Community look populated.

## 4. Phase sequence

### P13-WU1 — Community Experience Architecture — COMPLETE / PASS
Canonical record:
`canon/PHASE_13_WU1_COMMUNITY_EXPERIENCE_ARCHITECTURE_2026-08-31.md`

### P13-WU2 — Personal Community Home / My TNC — COMPLETE / PASS
Canonical closeout:
`canon/PHASE_13_WU2_PERSONAL_COMMUNITY_HOME_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU2_MY_TNC_EVIDENCE_2026-08-31.md`

Product main after WU2:
`a6f3dfa4d3033d5855b1e3906d6d48beec7619ef`

### P13-WU3 — Journey Community Experience — COMPLETE / PASS
Canonical closeout:
`canon/PHASE_13_WU3_JOURNEY_COMMUNITY_EXPERIENCE_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU3_JOURNEY_EXPERIENCE_EVIDENCE_2026-08-31.md`

Product main after WU3:
`75706af5b2dfa5e9b01b34150aa2e440406640e4`

Key lifecycle rule:
- Before: confirmed participation is not attendance or Memory;
- During: event time alone is not attendance evidence;
- After: unresolved / no-show / attended / Memory / Reflection remain distinct evidence-backed states.

### P13-WU4 — Community People & Relationship UI — COMPLETE / PASS
Canonical closeout:
`canon/PHASE_13_WU4_COMMUNITY_PEOPLE_RELATIONSHIP_UI_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU4_COMMUNITY_RELATIONSHIP_UI_EVIDENCE_2026-08-31.md`

Product main after WU4:
`cb465399f4860e4dfa842e2008e62547dbce8fde`

Key relationship rules:
- Participant derives from evidence-backed attended Memory;
- Contributor derives from active verified Contribution;
- Host / Partner representative come only from verified assignments;
- role never grants CMS permission;
- internal relationship verification does not authorize public identity publication;
- no public Community member directory was created.

### P13-WU5 — Living Community Surface — COMPLETE / PASS

Implemented the wider editorial Community layer on the existing bilingual Community routes.

Canonical closeout:
`canon/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_EVIDENCE_2026-08-31.md`

Product main after WU5:
`029d444a32529b23cc0171309e8bc81ae9792957`

Key public-surface rules:
- source only from public operational Journeys, published Journey Updates and `journey_reflection_publications`;
- public Reflection remains identity-minimized and is never reverse-joined to a person;
- no public people directory;
- no feed/follower/rank/impact-score mechanics;
- missing Reflection/Contribution/relationship facts render as truthful empty states;
- no fake Community activity is seeded;
- VI/EN share the same architecture;
- Reflection text is not machine-translated or rewritten.

WU5 production audit at implementation time showed real public Journey/Field content but zero public Reflection publications and zero verified Community relationship/Contribution rows, so the empty states are a canonical part of the real experience.

### P13-WU6 — Public Activation & Polish — NEXT

WU6 is the final Phase 13 gate.

It must review, not blindly enable:
- responsive/mobile polish;
- accessibility;
- bilingual parity;
- loading/error/empty states;
- navigation placement;
- SEO / `noindex` decision;
- Supabase Auth Site URL / redirect URLs;
- production Magic Link / email delivery readiness;
- privacy copy and identity-publication boundary;
- Cloudflare production deployment readiness;
- production smoke QA;
- Owner Review.

Email/Auth activation and Community navigation promotion remain gated until WU6 explicitly passes the required checks.

## 5. Phase 13 guardrails

Phase 13 must preserve:
- P11 live-pilot operational authority;
- Email OFF until separately activated;
- Turnstile OFF until separately activated;
- Community Auth activation gate;
- `pg_graphql` OFF;
- CMS roles exactly `admin | editor`;
- zero fabricated attendance/Memory/Contribution/relationship/impact facts;
- verified relationship existence is not automatic identity-publication consent;
- public Community presentation reads only sources already authorized for publication.

## 6. Success criteria

Phase 13 is successful when:
1. Community feels like a coherent product rather than technical panels;
2. a signed-in person understands their relationship to Trạm;
3. Journey lifecycle is clear Before / During / After;
4. Memory remains emotionally valuable and evidence-backed;
5. Contribution and roles are meaningful without scorekeeping;
6. public Community storytelling grows from verified/public facts without exposing private identity or becoming a social feed;
7. the experience remains warm, documentary, editorial, trustworthy and bilingual;
8. production activation passes explicit auth/email/privacy/navigation/deployment review.

WU2 materially satisfies criteria 1, 2, 4 and 5 at source level.
WU3 materially satisfies criterion 3.
WU4 strengthens criteria 2, 5 and the privacy half of criterion 6.
WU5 materially satisfies the public editorial half of criterion 6.
WU6 is the remaining activation/polish gate for criteria 7–8 and Phase 13 closeout.

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
- `canon/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_2026-08-31.md`
- `evidence/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_EVIDENCE_2026-08-31.md`

## 8. Current declaration

**PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI: ACTIVE**

**P13-WU1 — COMMUNITY EXPERIENCE ARCHITECTURE: COMPLETE / PASS**

**P13-WU2 — PERSONAL COMMUNITY HOME / MY TNC: COMPLETE / PASS**

**P13-WU3 — JOURNEY COMMUNITY EXPERIENCE: COMPLETE / PASS**

**P13-WU4 — COMMUNITY PEOPLE & RELATIONSHIP UI: COMPLETE / PASS**

**P13-WU5 — LIVING COMMUNITY SURFACE: COMPLETE / PASS**

**P13-WU6 — PUBLIC ACTIVATION & POLISH: NEXT**

Community public activation remains gated while Email is OFF.
