# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 15 / P15-WU7 — POST-JOURNEY MEMORY, REFLECTION & RELATIONSHIP CONTINUITY

**Date:** 2026-09-02  
**Status:** COMPLETE / PASS — SOURCE & CANONICAL  
**Real populated post-Journey verification:** DEFERRED / PENDING REAL 2026-09-11 EVIDENCE  
**Production activation status:** NOT DECLARED / NOT VERIFIED IN THIS WU  
**P14-WU5 real-pilot operational verification:** REMAINS OPEN

## 1. Objective

P15-WU7 redesigns the AFTER-Journey personal experience so that a Journey can continue as a factual personal Memory, Reflection, Contribution and relationship chapter without manufacturing any relationship that canonical evidence does not support.

The governing experience principle is:

> After a Journey, the website helps a person remember what actually happened — it never invents attendance, Memory, Reflection, Contribution or relationship continuity.

## 2. Product evidence

- Product repository: `huynhtranhuythinh/tramnucuoi`
- Base product main: `d90b5b9be79fae4a83bb7ca84f2f48a44cfdf73b`
- Product branch: `p15-wu7-post-journey-continuity`
- Product PR: **#48 — P15-WU7: Post-Journey Memory, Reflection & Relationship Continuity**
- Final PR head: `86ffac68fd93b74b4d11f0a29166f12ca11180a0`
- PR-head CI: **#200**, Run ID `33551041752` — PASS
- Product main after merge: `28bff0a896fa9e51199c5a97a1418fd6181de420`
- Post-main CI: **#201**, Run ID `33551212552` — PASS

Final product PR changed only:

- `.github/workflows/ci.yml`
- `scripts/p15-wu4-my-tnc-relationship-home-qa.ts`
- `scripts/p15-wu5-journey-lifecycle-qa.ts`
- `scripts/p15-wu7-post-journey-continuity-qa.ts`
- `src/components/community/community-experience-shell.tsx`
- `src/components/journeys/journey-relationship-experience.tsx`
- `src/lib/community/post-journey-continuity.ts`

No temporary source-apply workflow/script remains in the merged PR.

## 3. Canonical truth gate introduced

New shared helper:

`src/lib/community/post-journey-continuity.ts`

The presentation layer now treats a Memory as evidence-backed only when all three conditions agree:

1. `attendance_state === "attended"`
2. `attended_party_size > 0`
3. canonical `memory_eligible === true`

Reflection availability is stricter still:

1. an evidence-backed Memory exists; and
2. Journey canonical status is `completed`.

This is presentation-side defense in depth. Database/domain rules remain authoritative.

The helper does not infer attendance from dates, registration, account state, participant claim, Journey status or any other proxy.

## 4. AFTER-Journey state semantics

Deterministic source QA explicitly covers:

1. past/completed Journey + unresolved attendance → no Memory / no Reflection eligibility
2. verified no-show → no attended Memory / no Reflection eligibility
3. verified attended but Memory eligibility false → no Memory presentation / no Reflection eligibility
4. verified attended + positive party size + Memory eligibility → evidence-backed Memory
5. evidence-backed Memory + Journey not completed → Reflection remains unavailable
6. evidence-backed Memory + completed Journey + no Reflection → Reflection invitation available
7. submitted Reflection pending → private submitted state
8. published Reflection → public-publication state remains distinct
9. rejected/not-published Reflection → remains private personal continuity
10. verified Contribution / verified relationship sources remain source-backed and unscored

Canonical attendance truth continues to outrank date-derived lifecycle presentation.

## 5. Memory experience

Memory is presented as a durable personal trace, not an achievement, unlock, badge or social post.

Authored VI language includes:

> Một Ký ức đã được nối với Journey này.

Authored EN language includes:

> A Memory is now part of this Journey.

My TNC now filters Memory continuity through the shared evidence-backed helper rather than relying on the eligibility boolean alone.

Journey detail uses the same helper before presenting Memory state.

## 6. Reflection experience

Reflection is positioned as an invitation to remember, not a review or rating.

Journey-detail heading:

- VI: **Điều còn ở lại**
- EN: **What remains**

Reflection submission and public publication remain separate states.

Human states include:

VI:
- `Đã gửi · Trạm đang đọc lại`
- `Đã được chọn để xuất hiện công khai`
- `Đang được giữ riêng trong My TNC`

EN:
- `Submitted · Trạm is reading it`
- `Selected to appear publicly`
- `Kept private in My TNC`

The public Living Community server still reads only `journey_reflection_publications`; it does not read the private `journey_reflections` source.

The public Reflection projection remains identity-minimized and selects:

`reflection_id, journey_id, body, locale, published_at`

No user UUID, private Memory record, participant list or moderator identity was introduced into the public read path.

## 7. Database-domain parity retained

Existing Reflection DB foundation remains unchanged and authoritative.

The canonical trigger continues to require:

- caller-owned Memory relationship;
- attended state;
- Memory eligibility;
- completed Journey;
- new submission forced into pending moderation state.

The public publication table remains separate from the private Reflection source.

No migration was created or modified in WU7.

## 8. Contribution continuity

Contribution remains personal relationship history, not a score.

Relationship Map continues to read only:

- own-user `community_contributions`
- `status = active`

No points, ranking, aggregated impact score, leaderboard or comparison mechanic was added.

Different contribution units are not collapsed into a vanity impact total.

## 9. Relationship continuity

Relationship descriptions remain sourced relationships rather than badges, trophies or permissions.

Own-user Relationship Map reads retain explicit filters for:

- `community_journey_memories`
- `community_contributions`
- `community_relationship_assignments`

Relationship assignments remain restricted to `status = verified` in the personal map.

Reader language clarifies that Explorer, Participant, Contributor, Host or partner representative are descriptions of how a person has genuinely journeyed with Trạm, not status badges and not CMS administration authority.

## 10. Privacy / own-data boundary

P15-WU1A/WU4/WU6 defense-in-depth contracts remain intact.

Personal Journey/My TNC reads retain explicit `.eq("user_id", authenticatedUserId)` scoping in addition to backend RLS authority.

Public Living Community continues to avoid private reads from:

- `profiles`
- `journey_reflections`
- `community_journey_memories`
- `community_relationship_assignments`
- `journey_participants`

No public participant directory, public profile list or social feed was introduced.

## 11. Bilingual editorial language

VI and EN were authored separately around the same truth model rather than exposing implementation vocabulary.

Key continuity concepts now include:

- Memory as a factual personal chapter
- Reflection as “what remains”
- private submission before any public publication
- Contribution as something genuinely contributed
- relationship continuity as sourced history

No rating/review pattern, points, followers, reaction count, leaderboard or achievement-unlock pattern was introduced.

## 12. CI and regression history

WU7 deliberately preserved prior P15 regression gates.

During implementation, three CI failures were treated as QA-contract reconciliation issues rather than ignored:

- **CI #196 / Run `33550338517`** — P15-WU4 stale exact-string QA expected the older completed-only Reflection filter. The QA was reconciled to require the stricter WU7 helper while preserving own-data and auth contracts.
- **CI #197 / Run `33550488205`** — P15-WU5 stale exact-string QA expected `completed && attended`. The QA was reconciled to require evidence-backed Memory + explicit Reflection continuity state while retaining attendance-over-date precedence.
- **CI #198 / Run `33550622039`** and **CI #199 / Run `33550855029`** — WU7 QA initially treated anti-gamification/source-comment wording as reader-facing violations. The QA was tightened to inspect the intended semantics without weakening the product guardrails.

Final PR-head CI **#200 / Run `33551041752`** PASS included:

- P9-WU7 abuse-protection source QA
- P10-WU3A runtime-context regression QA
- P13-WU6 Community activation QA
- P14-WU2 Community Auth activation QA
- P14-WU4 own-data QA
- P14-WU4A credential lifecycle QA
- P14-WU5A Vietnam date-authority source QA
- P15-WU1A Journey own-data QA
- P15-WU2 experience design system QA
- P15-WU3 public Story World QA
- P15-WU4 My TNC relationship-home QA
- P15-WU5 Journey lifecycle QA
- P15-WU6 Living Community QA
- P15-WU7 post-Journey continuity QA
- all retained ephemeral PostgreSQL domain/rollback gates
- build
- typecheck
- Cloudflare configuration dry-run

Post-main CI **#201 / Run `33551212552`** repeated the complete chain and PASSed.

## 13. No production/runtime mutation

WU7 did **not**:

- create or apply a database migration;
- change RLS policies;
- mutate Supabase production data;
- fabricate attendance;
- create fake Memory records;
- create fake Reflection records;
- create fake Contributions or relationship assignments;
- deploy the Cloudflare Worker;
- declare a new production Worker version.

The GitHub Cloudflare step is dry-run only.

Therefore **production activation is NOT DECLARED / NOT VERIFIED** in this WU.

## 14. Real-pilot boundary — 2026-09-11

As of this closeout, the real Journey dated **2026-09-11** has not yet produced the post-Journey evidence needed for populated verification.

P15-WU7 source implementation can be complete without falsifying field evidence.

The following remain deferred until real evidence exists:

- populated attended state verification;
- real Memory creation/eligibility verification;
- real Reflection submission/moderation/publication verification;
- real Contribution continuity verification, if such evidence exists;
- real relationship continuity verification, if such evidence exists;
- final P14-WU5 real-pilot operational closeout.

P14-WU5 therefore remains **OPEN** and independent of this source closeout.

## 15. Canonical declaration

**P15-WU7 — COMPLETE / PASS — SOURCE & CANONICAL.**

The AFTER-Journey source experience now has a stricter, shared evidence gate and authored personal continuity language while preserving privacy, attendance truth, Reflection publication separation, contribution provenance and relationship sourcing.

**Real populated post-Journey verification remains DEFERRED until actual 2026-09-11 Journey evidence exists.**

**Production activation remains NOT DECLARED / NOT VERIFIED.**

## 16. Next work unit

Next planned Phase 15 work:

**P15-WU8 — Bilingual, Mobile & Cross-Surface Integration QA**

WU8 may proceed before 2026-09-11, but it must preserve the same real-evidence boundary and must not manufacture post-Journey populated states.
