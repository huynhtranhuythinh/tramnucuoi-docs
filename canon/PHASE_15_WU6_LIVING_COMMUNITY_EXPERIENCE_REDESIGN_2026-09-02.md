# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 15 / WU6 — LIVING COMMUNITY EXPERIENCE REDESIGN

Date: 2026-09-02

## STATUS

**COMPLETE / PASS — SOURCE & CANONICAL**

Production activation status:

**NOT DECLARED / NOT VERIFIED IN THIS WU**

This work unit redesigns the Community experience and source orchestration only. It does not declare that the resulting source has been deployed to the production Cloudflare Worker.

---

## 1. OBJECTIVE

P15-WU6 converts the Community experience from a technically valid collection of Community surfaces into a calm, documentary, relationship-aware living Community experience.

The intended model is:

> Community is made visible through real Journeys, trusted field records, intentionally public voices and relationship continuity — not through activity volume.

The work unit explicitly avoids building:

- a social/news feed;
- a public member directory;
- public people profiles generated from private Community relationships;
- followers/following/reactions;
- badges, points or leaderboards;
- vanity activity or impact counters;
- inferred attendance, contribution or impact.

---

## 2. CANONICAL PRODUCT EVIDENCE

Product repository:

`huynhtranhuythinh/tramnucuoi`

Product branch:

`p15-wu6-living-community-experience`

Base product main before WU6:

`ff80421c2a173da1af8d72888193efc87c285dea`

Product PR:

**#47 — P15-WU6: Living Community Experience Redesign**

Final PR head:

`1a8d0a8e89c74962f44411d722539167f94e0911`

Final PR-head CI:

- CI run number: **#194**
- Run ID: `33545686164`
- Conclusion: **PASS**

Product merge/main SHA:

`d90b5b9be79fae4a83bb7ca84f2f48a44cfdf73b`

Post-main CI:

- CI run number: **#195**
- Run ID: `33545866942`
- Conclusion: **PASS**

Both final-head and post-main gates passed:

- P13-WU6 Community activation source QA;
- P14-WU2 controlled Community Auth activation QA;
- P14-WU4 My TNC own-data boundary QA;
- P14-WU4A account credential lifecycle QA;
- P14-WU5A attendance date-authority source QA;
- P15-WU1A Journey own-data boundary QA;
- P15-WU2 experience design system QA;
- P15-WU3 public Story World editorial QA;
- P15-WU4 My TNC relationship-home QA;
- P15-WU5 Journey lifecycle experience QA;
- P15-WU6 Living Community experience QA;
- all existing ephemeral database gates;
- production build;
- TypeScript typecheck;
- Cloudflare configuration dry-run.

The Cloudflare gate is a **dry-run only** and is not deployment evidence.

---

## 3. COMMUNITY INFORMATION ARCHITECTURE RECONCILIATION

### Previous condition

The Community Auth gateway could wrap the full `/cong-dong` / `/en/community` experience.

That created an experience-level inversion:

- signed-out visitors could be blocked from the public Living Community story;
- signed-in users encountered private My TNC / relationship material before the shared Community world;
- private identity/account behavior became the apparent entrypoint to Community.

The source remained technically valid, but the IA did not match the Phase 15 experience principle:

> Community = presence, not account state or activity volume.

### WU6 canonical structure

Both Community routes now use one public site shell:

1. `PageShell`
2. `LivingCommunitySurface` — always first
3. private continuation:
   - when Community Auth is enabled: `CommunityAccountGateway` → `CommunityExperienceShell` + `CommunityRelationshipMap`
   - when Community Auth is not enabled: `CommunityAccessGate`

Therefore:

- Living Community is readable independent of My TNC sign-in state;
- My TNC remains a private relationship archive rather than becoming the definition of Community;
- Auth activation remains fail-closed;
- the public and private layers coexist without duplicate site chrome.

No route split or new account domain was introduced.

---

## 4. LIVING COMMUNITY EXPERIENCE

### Core VI statement

> **Cộng đồng hiện ra qua những lần chúng ta cùng đi.**

### Core EN statement

> **Community becomes visible in the times we journey together.**

The public Community experience is organized around real, publishable source material rather than user activity mechanics.

Key editorial chapters now include:

### VI

- Một Journey để cùng hướng tới
- Những lần cộng đồng gặp nhau
- Những điều đã được nhìn thấy
- Những điều người tham gia chọn giữ lại
- Con người trước dữ liệu
- Được nhìn thấy không có nghĩa là bị phơi bày.

### EN

- A Journey to look toward
- The times community meets
- What has been witnessed
- What participants choose to carry forward
- People before data
- Being visible does not mean being exposed.

The language intentionally avoids presenting backend implementation vocabulary as reader-facing concepts.

---

## 5. PUBLIC DATA BOUNDARY

The Living Community server source remains deliberately narrow.

Public Living Community may use:

- public Journeys through the existing public Journey reader;
- published Journey field updates;
- identity-minimized public Reflection publications.

The public Community server does **not** read private Community source records such as:

- profiles;
- private `journey_reflections`;
- Community Journey Memories;
- Community Relationship Assignments;
- Journey participant PII/attendance source rows.

A private fact becoming known to the system does not automatically make that fact public content.

Identity publication remains a separate publication decision.

---

## 6. RELATIONSHIP MAP PRIVACY DEFENSE

During WU6 audit, `CommunityRelationshipMap` was found to rely on RLS as the final privacy authority while lacking the explicit user-filter defense already standardized elsewhere by P15-WU1A/P15-WU4.

WU6 adds explicit ownership filters to the personal Relationship Map reads:

- `community_journey_memories` → `.eq("user_id", userId)`
- `community_contributions` → `.eq("user_id", userId)`
- `community_relationship_assignments` → `.eq("user_id", userId)`

This is defense-in-depth.

It does not replace RLS and does not imply that an observed production privacy leak existed before this change.

Existing My TNC own-data filters remain intact.

---

## 7. DATE / LIFECYCLE TRUTH

WU6 reuses the shared Vietnam date/lifecycle presentation authority established and hardened in P14-WU5A and P15-WU5.

Public Community date-relative Journey labels use:

`journeyPresentationPhase(...)`

This prevents a generic `preparing` status from being presented as proof that a Journey is currently happening.

Presentation states now distinguish:

- before / looking ahead;
- within the authored Journey dates;
- after / Journey has passed.

This remains a presentation layer only.

It does not infer attendance.

Canonical attendance truth remains separate and unchanged.

---

## 8. MY TNC / AUTH CONTINUITY

WU6 changes orchestration and page composition, not the authentication domain contract.

Preserved:

- explicit account signup;
- Magic Link existing-account-only behavior;
- `shouldCreateUser: false`;
- locale-aware callback paths;
- recovery flow;
- MFA/AAL2 behavior;
- My TNC own-data filters;
- participant claim boundaries;
- Reflection evidence gates.

Account continues to mean identity continuity only.

It does not mean:

- participant;
- attendance;
- contribution;
- Memory eligibility;
- public Community membership status.

---

## 9. HUMAN FAIL-CLOSED PRIVATE BOUNDARY

When My TNC/Auth is unavailable, the page no longer makes the public Community experience feel unavailable.

The public Community story remains complete and readable.

The private continuation is explained quietly:

VI:

> **Living Community đã mở. My TNC vẫn được giữ riêng.**

EN:

> **Living Community is open. My TNC remains private.**

The closed private layer is treated as an intentional privacy/activation boundary, not as missing Community data.

---

## 10. WU6 DETERMINISTIC QA

Added:

`scripts/p15-wu6-living-community-experience-qa.ts`

CI gate:

`P15-WU6 Living Community experience QA`

The gate asserts at minimum:

- public Living Community exists on both VI/EN Community routes;
- Living Community is source-ordered before private My TNC/Auth continuation;
- Community Auth activation remains present;
- private gateway, My TNC, Relationship Map and fail-closed AccessGate remain available at their canonical boundaries;
- duplicate private site chrome is absent;
- VI/EN Living Community language is authored and relationship-first;
- shared Vietnam Journey lifecycle presentation authority is used;
- `preparing` does not masquerade as live presence;
- prohibited social/gamification mechanics remain absent;
- public Community server does not read private Community source tables;
- Relationship Map explicit owner filters remain present;
- My TNC exact own-data filters remain present;
- Magic Link cannot silently create an account;
- the private fail-closed boundary remains bilingual and human-readable.

---

## 11. CI RECONCILIATION DURING THE WU

An earlier PR CI run (#189) passed all source, privacy, database and build gates but failed TypeScript because the new `ExperienceState` loading/error usages omitted the required `body` prop.

This was a UI type-contract issue, not a privacy, domain-truth, database or auth regression.

The component contract was corrected.

Temporary transformation/fix workflow artifacts used during implementation were removed from the final branch before the final-head gate.

The final clean PR head `1a8d0a8e89c74962f44411d722539167f94e0911` then passed CI #194 completely.

Post-merge product main `d90b5b9be79fae4a83bb7ca84f2f48a44cfdf73b` passed CI #195 completely.

---

## 12. DATABASE / RUNTIME BOUNDARY

P15-WU6 introduced:

- no database migration;
- no RLS mutation;
- no Supabase production mutation;
- no attendance mutation;
- no Memory/Reflection data mutation;
- no authentication-domain mutation;
- no Cloudflare Worker deployment.

Production activation is therefore:

**NOT DECLARED / NOT VERIFIED IN THIS WU**

The current source may be deployment-ready according to CI, but source readiness and runtime production activation are separate facts.

---

## 13. TRUTH CONTRACT AFTER WU6

The following remain canonical:

- registration != attendance
- confirmed registration != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance > 0 = verified attended
- participant claim != attendance
- participant claim != Memory eligibility
- account != participant
- account != attendance
- relationship role != CMS permission
- Memory requires real evidence
- Reflection is evidence-gated
- contribution is not inferred from attendance
- public Community does not imply public personal identity
- no fake Community activity or impact
- no inferred attendance presented as fact

---

## 14. NEXT CANONICAL STEP

Next Phase 15 work unit:

**P15-WU7 — Post-Journey Memory, Reflection & Relationship Continuity**

WU7 may continue source/experience design work before the 2026-09-11 Journey, but populated operational verification must not fabricate evidence.

Real post-Journey Memory / Reflection / attendance-dependent verification remains constrained by P14-WU5 and must use real pilot evidence after the Journey when such evidence exists.
