# PHASE 15 — WU8
# BILINGUAL, MOBILE & CROSS-SURFACE INTEGRATION QA

Date: 2026-09-02

## STATUS

**COMPLETE / PASS — SOURCE & CANONICAL**

- Production activation: **NOT DECLARED / NOT VERIFIED**
- Real populated post-Journey verification: **DEFERRED / PENDING REAL 2026-09-11 EVIDENCE**
- P14-WU5 real-pilot operational verification: **REMAINS OPEN**

## PURPOSE

P15-WU8 is the integration and quality gate across the Phase 15 experience. It does not introduce a new business domain, change attendance truth, create Community evidence, or mutate production data.

The WU verifies that the experience established in P15-WU2 through P15-WU7 remains coherent across:

- Vietnamese and English routes and authored state language;
- desktop and mobile composition;
- touch interaction and accessibility baselines;
- public Story World, Journey, Community, My TNC and Auth surfaces;
- public/private data boundaries;
- Journey lifecycle and Vietnam date authority;
- Memory, Reflection and relationship continuity;
- empty/error/recovery states;
- SEO route parity for public bilingual Community surfaces.

## BASELINE

### Product

- Repository: `huynhtranhuythinh/tramnucuoi`
- Base `main`: `28bff0a896fa9e51199c5a97a1418fd6181de420`

### Canonical docs

- Repository: `huynhtranhuythinh/tramnucuoi-docs`
- Base `main`: `ebb32cd10c1da881ea6588dd5ffb59513f1be2b1`

## PRODUCT DELIVERY

### Branch

`p15-wu8-bilingual-mobile-cross-surface-qa`

### Pull request

PR #49 — `P15-WU8: Bilingual, Mobile & Cross-Surface Integration QA`

Final PR head:

`c57b8e9994e6c5fd11151d1012ada29bfbffa552`

Merged by squash to product `main`:

`a430b5d1cfac54d839e0fada354c0c96dc8ad00e`

## CONCRETE INTEGRATION DEFECTS FOUND AND CLOSED

### 1. Global error recovery did not preserve locale

Before WU8, the root error boundary was English-only and its home recovery path was hard-coded to `/`.

Impact:

- a failure under `/en/...` could fall out of the English experience;
- global recovery state was inconsistent with Phase 15 bilingual standards.

Fix:

- root error state now has authored VI/EN copy;
- locale is derived from the current pathname;
- recovery home uses `pageHref("home", locale)`;
- retry and recovery controls use the shared editorial/touch interaction system.

### 2. Public bilingual Community routes were absent from sitemap

Community is intentionally outside `NAV_PAGES` because its public layer and private My TNC continuation have different access semantics. The sitemap previously inherited this navigation separation and therefore omitted:

- `/cong-dong`
- `/en/community`

Fix:

- both public Community routes are now explicitly included in the sitemap;
- private My TNC remains independently authentication-gated and is not exposed as a public directory.

### 3. Mobile chrome controls did not consistently meet the 44×44 touch-target baseline

The shared P15 utility already defined `touch-target` as 2.75rem × 2.75rem, but several mobile controls were not using it.

Fix:

- mobile menu Open control uses `touch-target`;
- mobile menu Close control uses `touch-target`;
- VN/EN language links use `touch-target`.

No redesign of the visual language was introduced.

## WU8 DETERMINISTIC QA

Added:

`scripts/p15-wu8-bilingual-mobile-integration-qa.ts`

Added CI step:

`P15-WU8 bilingual mobile integration QA`

The QA locks the following contracts.

### Bilingual route and SEO parity

Verified route pairs include:

- `/` ↔ `/en`
- `/ve-tram` ↔ `/en/about`
- `/he-sinh-thai` ↔ `/en/ecosystem`
- `/hanh-trinh` ↔ `/en/journey`
- `/journeys` ↔ `/en/journeys`
- `/cong-dong` ↔ `/en/community`

Localized head remains `vi` / `en` / `x-default` hreflang-aware.

### Community information architecture

Both VI and EN Community routes retain the WU6 ordering:

1. public `LivingCommunitySurface` first;
2. authenticated/private My TNC continuation second.

Authentication does not hide the public Community story world.

### Mobile and responsive baseline

Verified:

- horizontal overflow protection;
- responsive editorial typography utilities;
- shared 44×44 touch target;
- reduced-motion support;
- mobile navigation touch target usage;
- VN/EN switch touch target usage;
- Journey registration CTA touch target continuity.

### Auth continuity

Verified authored VI/EN account copy and retained security semantics, including:

- explicit sign-in;
- explicit account creation;
- recovery language;
- Magic Link remains existing-account-only with `shouldCreateUser: false`.

Account continues to mean account identity only; it does not establish participant or attendance truth.

### Public/private boundaries

Verified explicit own-user filtering remains on personal datasets across:

- My TNC;
- Journey relationship experience;
- Relationship Map.

Public Living Community remains narrow and does not query private source tables such as:

- `journey_reflections`
- `community_journey_memories`
- `community_relationship_assignments`
- `journey_participants`
- `profiles`

Public Reflection continues to use the identity-minimized `journey_reflection_publications` projection.

### Journey date and post-Journey truth

Verified:

- `Asia/Ho_Chi_Minh` remains explicit calendar authority;
- P15 personal/Journey surfaces do not derive Vietnam lifecycle presentation using UTC `toISOString().slice(0, 10)`;
- Memory continuity still requires canonical attended truth, positive observed party size and canonical Memory eligibility;
- Reflection continuity still requires completed Journey plus evidence-backed Memory.

WU8 did not infer attendance from Journey dates or account/registration state.

## ENGLISH CMS FALLBACK POLICY

Current CMS localization still allows Vietnamese fallback when English content is missing. WU8 deliberately did **not** fabricate English translations.

The accepted interim contract is:

- fallback remains explicitly documented as temporary;
- admin-visible translation debt detectors remain available:
  - `projectTranslationStatus`
  - `postTranslationStatus`
  - `mediaTranslationStatus`
  - `blockTranslationStatus`
- missing authored English therefore remains detectable rather than silently becoming assumed-complete editorial parity.

This editorial debt may be reviewed during P15-WU9 Owner Review without falsifying content.

## CI EVIDENCE

### First WU8 run — test reconciliation only

CI #202

Run ID: `33580449744`

Result: **FAILED at the new WU8 assertion only**.

All inherited P15 source gates through WU7 had passed before the WU8 failure. The failing assertion demanded the literal source spelling `memory_eligible === true`, while the canonical helper correctly uses boolean truthiness `memory.memory_eligible` together with attended state and positive party size.

Resolution:

- reconciled the WU8 QA to assert the semantic three-part Memory gate;
- did **not** weaken or modify product Memory eligibility rules.

### Final PR-head CI

CI #203

Run ID: `33580549813`

Head: `c57b8e9994e6c5fd11151d1012ada29bfbffa552`

Result: **PASS**.

Confirmed PASS:

- P9/P10/P13/P14 retained source gates;
- P15-WU1A through P15-WU8 source QA;
- retained ephemeral database gates P9/P11/P12/P14;
- application build;
- typecheck;
- Cloudflare configuration **dry-run only**.

### Post-main CI

CI #204

Run ID: `33580664232`

Main SHA: `a430b5d1cfac54d839e0fada354c0c96dc8ad00e`

Result: **PASS**.

Confirmed PASS again on `main`:

- P15-WU8 integration QA;
- all retained source and database gates;
- build;
- typecheck;
- Cloudflare **dry-run**.

## CHANGE BOUNDARIES

WU8 made no changes to:

- Supabase production data;
- migrations;
- RLS policies;
- attendance records;
- Memory records;
- Reflection records/publication state;
- Contribution records;
- relationship assignments;
- Cloudflare production deployment.

No synthetic Journey, attendance, Memory, Reflection, Contribution, relationship or impact evidence was created.

## PRODUCTION BOUNDARY

The GitHub CI Cloudflare step is a configuration dry-run only.

Therefore:

**P15-WU8 SOURCE & CANONICAL = COMPLETE / PASS**

but:

**PRODUCTION ACTIVATION = NOT DECLARED / NOT VERIFIED**

No Worker version is claimed from this WU.

## REAL PILOT BOUNDARY

The real Journey is scheduled for **2026-09-11**.

P15-WU8 can verify deterministic source and integration contracts before that date, but cannot close populated AFTER-Journey operational evidence.

The following remain dependent on real evidence after the Journey:

- real attendance reconciliation;
- real Memory formation/eligibility;
- real Reflection submission/moderation/publication;
- real Contribution continuity if evidence exists;
- real relationship continuity if evidence exists;
- P14-WU5 real-pilot closeout.

P14-WU5 therefore remains **OPEN**.

## CLOSEOUT

P15-WU8 is **COMPLETE / PASS — SOURCE & CANONICAL**.

The Phase 15 experience is now ready to move to:

**P15-WU9 — OWNER REVIEW & FINAL EXPERIENCE GATE**
