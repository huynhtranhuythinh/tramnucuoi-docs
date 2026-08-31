# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI
# CANONICAL KICKOFF / FINAL STATUS

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Phase objective

Phase 12 established the trustworthy Living Community OS domain foundation.

Phase 13 turned that foundation into a coherent, emotional and useful bilingual experience for real people:

**Identity → Journey → Memory → Reflection → Contribution → Community Roles → Host Network → Partner / Impact Network**

The experience remains documentary/editorial rather than a generic social network.

## 2. Product principles preserved

### Experience before feature count
Existing verified capabilities were organized into understandable journeys before adding new primitives.

### Real-world truth remains authoritative
UI state follows Phase 12 semantics. Emotional presentation never weakens truth boundaries.

### Personal first, public second
My TNC established the personal relationship layer before wider public Community storytelling.

### Editorial, not social-feed
No infinite feed, follower mechanics, ranking or engagement gamification was introduced.

### Bilingual by architecture
Phase 13 surfaces are designed for VI / EN from the start.

## 3. Final Phase 13 experience architecture

### A. Public Story World
About / Projects / public Journeys / Field Journal / evidence / participation.

### B. Personal Community Home / My TNC
A person's own Journey history, Memories, Contributions, verified relationships and Reflections.

### C. Journey Relationship Experience
Before / During / After lifecycle based on truth:
- confirmed participation ≠ attendance;
- event time ≠ attendance evidence;
- unresolved / no-show / attended / Memory / Reflection remain distinct states.

### D. Public Living Community Surface
Public Journey facts, published Field Updates and identity-minimized Reflection publications without exposing private people data.

## 4. Phase sequence — final

### P13-WU1 — Community Experience Architecture — COMPLETE / PASS
Canonical record:
`canon/PHASE_13_WU1_COMMUNITY_EXPERIENCE_ARCHITECTURE_2026-08-31.md`

### P13-WU2 — Personal Community Home / My TNC — COMPLETE / PASS
Canonical:
`canon/PHASE_13_WU2_PERSONAL_COMMUNITY_HOME_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU2_MY_TNC_EVIDENCE_2026-08-31.md`

Product main after WU2:
`a6f3dfa4d3033d5855b1e3906d6d48beec7619ef`

### P13-WU3 — Journey Community Experience — COMPLETE / PASS
Canonical:
`canon/PHASE_13_WU3_JOURNEY_COMMUNITY_EXPERIENCE_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU3_JOURNEY_EXPERIENCE_EVIDENCE_2026-08-31.md`

Product main after WU3:
`75706af5b2dfa5e9b01b34150aa2e440406640e4`

### P13-WU4 — Community People & Relationship UI — COMPLETE / PASS
Canonical:
`canon/PHASE_13_WU4_COMMUNITY_PEOPLE_RELATIONSHIP_UI_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU4_COMMUNITY_RELATIONSHIP_UI_EVIDENCE_2026-08-31.md`

Product main after WU4:
`cb465399f4860e4dfa842e2008e62547dbce8fde`

### P13-WU5 — Living Community Surface — COMPLETE / PASS
Canonical:
`canon/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_2026-08-31.md`

Evidence:
`evidence/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_EVIDENCE_2026-08-31.md`

Product main after WU5:
`029d444a32529b23cc0171309e8bc81ae9792957`

### P13-WU6 — Public Activation & Polish — COMPLETE / PASS
Implementation record:
`canon/PHASE_13_WU6_PUBLIC_ACTIVATION_POLISH_2026-08-31.md`

Implementation evidence:
`evidence/PHASE_13_WU6_PUBLIC_ACTIVATION_POLISH_EVIDENCE_2026-08-31.md`

Production closeout addendum:
`canon/PHASE_13_WU6_PRODUCTION_CLOSEOUT_ADDENDUM_2026-08-31.md`

Production hotfix / Owner Review evidence:
`evidence/PHASE_13_WU6_PRODUCTION_HOTFIX_OWNER_REVIEW_EVIDENCE_2026-08-31.md`

Initial WU6 product main:
`1072c11366222847ca931ab392b04862c947cfca`

Owner Review found a duplicated Community masthead after the first production deployment. PR #32 removed the internal Community BrandMark/language masthead and retained `PageShell` / `SiteNav` as the single site chrome.

Final Phase 13 product main:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

Hotfix CI:
- PR CI #150: PASS
- post-merge main CI #151: PASS

Final production Worker Version ID:
`23bacfb3-5ec0-4aa8-88e7-df8008ba1b81`

Final Owner Review on desktop/mobile VI/EN: **PASS**.

## 5. Final activation decision

### Public Living Community
**ON**

Routes:
- `/cong-dong`
- `/en/community`

The public layer is:
- indexable / canonical / bilingual;
- present in desktop/mobile navigation and footer;
- sourced only from already-public or identity-minimized sources;
- not a public people directory or social feed.

### My TNC public onboarding
**OFF / EXPLICITLY GATED**

`VITE_APP_COMMUNITY_AUTH_ENABLED=false`

Also remains OFF:
- Email operational activation;
- Turnstile.

Phase 13 completion does not imply public Magic Link activation.

## 6. Guardrails preserved

- P11 live-pilot operational authority remains independent;
- Email OFF until separately activated;
- Turnstile OFF until separately activated;
- My TNC public Community Auth gate remains fail-closed;
- `pg_graphql` OFF;
- CMS roles exactly `admin | editor`;
- zero fabricated attendance/Memory/Contribution/relationship/impact facts;
- verified relationship existence is not automatic identity-publication consent;
- public Community reads only sources authorized for publication;
- role never equals CMS permission.

## 7. Final production data note

Final postflight showed no substantive Community personal-history facts:
- profiles 0
- participant links 0
- Memories 0
- Reflections 0
- public Reflection publications 0
- active Contributions 0
- verified relationship assignments 0

One zero-result claim audit row appeared from an already-authenticated operational Journey session. API logs and P13-WU3 source inspection show it came from `JourneyRelationshipExperience`, which intentionally invokes `claim_my_journey_participations()` for an existing verified session before reading the user's RLS-scoped Journey context. It linked nothing and created no profile/Memory/history fact.

## 8. Success criteria — final result

1. Community feels like a coherent product rather than technical panels — PASS.
2. A signed-in person can understand their relationship to Trạm — source implementation PASS; public onboarding remains intentionally gated.
3. Journey lifecycle is clear Before / During / After — PASS.
4. Memory remains emotionally valuable and evidence-backed — PASS.
5. Contribution and roles are meaningful without scorekeeping — PASS.
6. Public Community storytelling grows from verified/public facts without exposing private identity or becoming a social feed — PASS.
7. Experience remains warm, documentary, editorial, trustworthy and bilingual — PASS.
8. Production activation passes navigation/deployment/live visual review while My TNC Auth stays fail-closed — PASS.

## 9. Final canonical dependency

Read together with:
- `canon/PHASE_12_FINAL_CLOSEOUT_2026-08-31.md`
- `evidence/PHASE_12_LIVING_COMMUNITY_OS_EVIDENCE_2026-08-31.md`
- `handoff/PHASE_12_TO_PHASE_13_HANDOFF_2026-08-31.md`
- all P13 WU1 → WU6 canonical/evidence documents;
- `canon/PHASE_13_FINAL_CLOSEOUT_2026-08-31.md`.

The WU6 production closeout addendum supersedes the earlier pre-deploy PENDING status recorded in the original WU6 implementation document.

## 10. Current declaration

**PHASE 13 — COMMUNITY EXPERIENCE & LIVING UI: COMPLETE / PASS**

**P13-WU1 — COMPLETE / PASS**

**P13-WU2 — COMPLETE / PASS**

**P13-WU3 — COMPLETE / PASS**

**P13-WU4 — COMPLETE / PASS**

**P13-WU5 — COMPLETE / PASS**

**P13-WU6 — COMPLETE / PASS**

Public Living Community is live. My TNC public Auth remains deliberately OFF until a future explicit activation decision.
