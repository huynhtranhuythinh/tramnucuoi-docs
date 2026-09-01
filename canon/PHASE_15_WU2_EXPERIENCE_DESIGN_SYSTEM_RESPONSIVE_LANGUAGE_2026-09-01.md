# PHASE 15 — WU2
# EXPERIENCE DESIGN SYSTEM & RESPONSIVE LANGUAGE

Date: 2026-09-01
Status: COMPLETE / PASS — SOURCE & CANONICAL
Production activation status: NOT DECLARED / NOT VERIFIED IN THIS WU

## Purpose

Establish the shared experience language required before page-level Phase 15 redesign. This work evolves the existing Living Ecosystem Editorial system rather than replacing the TRẠM NỤ CƯỜI visual identity.

## Product implementation

Product branch:
`p15-wu2-experience-design-system`

Base product main SHA:
`be6ed2d505faf903002f132a498235ee9695ce9e`

Product PR:
`#43 — P15-WU2: Experience Design System & Responsive Language`

PR head SHA:
`79de2ceb88708007e6f2361976b4f1d0e6a3d624`

Merged product main SHA:
`d5edaf1014df28178a0942be065a4cf3a16a1bbc`

## Foundation implemented

### 1. Responsive editorial typography and rhythm

`src/styles.css` now includes reusable editorial roles and responsive layout measures:
- `story-title`
- `chapter-title`
- `section-title`
- `story-lede`
- `story-body`
- `section-rhythm`
- `chapter-rhythm`
- `editorial-cluster`
- responsive section/chapter/cluster spacing tokens
- readable body/lede/label measures

The existing canonical brand colors and Noto Serif Display + Be Vietnam Pro pairing remain unchanged.

### 2. Bilingual/mobile label resilience

`ed-kicker` no longer uses a fixed 0.6875rem / 0.24em treatment. Font size and tracking now scale with `clamp()` and long VI/EN labels may wrap naturally instead of becoming unreadable microtext.

Added `ed-context` for longer contextual labels that should not be forced into highly tracked uppercase UI language.

### 3. Documentary image language

Added `documentary-frame` so image treatments remain editorial/documentary rather than glossy SaaS cards.

### 4. Human system-state primitive

Added:
`src/components/site/experience-state.tsx`

`ExperienceState` provides calm editorial hierarchy for loading, empty, pending, unavailable, permission and error experiences without dashboard alert-card chrome. It preserves live-region semantics and an optional accessible next action.

### 5. Truth-state language vocabulary

Added:
`src/lib/experience/system-state-language.ts`

Shared VI/EN labels cover:
- loading
- empty
- unresolved
- confirmed
- attended
- verified no-show
- completed
- pending
- permission
- error
- email confirmation
- MFA
- Memory unavailable
- Reflection unavailable

The module is explicitly presentation-only: domain components must first determine state from canonical evidence. The design system must never infer attendance, Memory eligibility or Reflection eligibility.

### 6. Accessibility continuity

Preserved:
- visible keyboard focus
- `prefers-reduced-motion` handling
- live-region support
- mobile touch target primitive (`touch-target`)

## Regression QA

Added:
`scripts/p15-wu2-experience-system-qa.ts`

The gate verifies:
- responsive spacing/measure/type tokens
- responsive `ed-kicker`
- long-copy wrapping
- reduced-motion contract
- shared state primitive/live-region semantics
- required truth-state vocabulary in VI/EN
- exact unresolved and verified no-show language
- evidence-first authority comment

CI includes:
`P15-WU2 experience design system QA`

## CI evidence

- Product PR #43: MERGED
- PR CI #177: PASS
- Post-main CI #178: PASS
- Product main SHA: `d5edaf1014df28178a0942be065a4cf3a16a1bbc`
- P15-WU2 experience design system QA: PASS
- P15-WU1A own-data boundary QA: PASS
- P14-WU5A attendance date-authority source QA: PASS
- P14-WU5A attendance date-authority DB QA: PASS
- build: PASS
- typecheck: PASS
- Cloudflare dry-run: PASS

## Production boundary

The product repository currently contains only `.github/workflows/ci.yml`; there is no GitHub production deployment workflow. The CI Cloudflare step is explicitly a dry-run and does not upload or deploy the Worker.

Therefore this WU declares only the source/main and canonical design-system gate as PASS. It does **not** claim that Worker production or the live website has been updated or visually verified with this WU2 source.

Production activation and visual verification must be recorded separately through an actual deployment path. No Worker version is fabricated or inferred here.

## Truth preserved

- registration != attendance
- confirmation != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance >0 = verified attended
- Memory remains evidence-gated
- Reflection remains evidence-gated
- P14-WU5A attendance date authority unchanged
- P15-WU1A explicit own-user boundary unchanged

## Non-scope

- no database migration
- no RLS mutation
- no attendance/Memory/Reflection data mutation
- no public Community fabrication
- no page-level redesign yet
- no social feed, directory, gamification or vanity counters

## Closeout

P15-WU2 is COMPLETE / PASS for source and canonical architecture.

NEXT:
**P15-WU3 — Public Story World Editorial Elevation**

P15-WU3 must continue to use this foundation and must not silently redefine truth states or bypass the privacy/date-authority gates established in P15-WU1A and P14-WU5A.
