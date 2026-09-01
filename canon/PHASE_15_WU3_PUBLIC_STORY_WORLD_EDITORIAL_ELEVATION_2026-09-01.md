# PHASE 15 — WU3
# PUBLIC STORY WORLD EDITORIAL ELEVATION

Date: 2026-09-01
Status: COMPLETE / PASS — SOURCE & CANONICAL
Production activation status: NOT DECLARED / NOT VERIFIED IN THIS WU

## Purpose

Elevate the public TRẠM NỤ CƯỜI Story World from a set of technically correct pages into one calm, documentary, bilingual editorial experience while preserving all existing content, privacy and evidence truth boundaries.

Public scope:
- Home
- About entry via the shared public PageHero
- Ecosystem index
- Project detail

This is an elevation of the existing Living Ecosystem Editorial identity, not a rebrand.

## Product implementation

Product branch:
`p15-wu3-public-story-world`

Base product main SHA:
`d5edaf1014df28178a0942be065a4cf3a16a1bbc`

Product PR:
`#44 — P15-WU3: Public Story World Editorial Elevation`

PR head SHA:
`5fa9b9bd18e1603c75ad2c10620de28081ddf5d0`

Merged product main SHA:
`f1d98844b7c2b15245ce73e9b975551674f83055`

## Editorial elevation implemented

### 1. Shared public opener

`src/components/pages/page-hero.tsx`

The public page opener now uses the P15-WU2 semantic roles:
- `story-title`
- `story-lede`
- `section-rhythm`
- `ed-context`
- image-only `documentary-frame`

This also elevates the About and Ecosystem page entry without changing their CMS content or truth behavior.

### 2. Home as one editorial narrative

Updated:
- `src/components/home/origin.tsx`
- `src/components/home/shift.tsx`
- `src/components/home/ecosystem.tsx`
- `src/components/home/journey.tsx`

The chapters now share consistent hierarchy, measures, responsive rhythm and contextual labels. Existing CMS data and approved literal fallbacks remain authoritative.

The Journey chapter continues to prefer real CMS journal entries when available. No event date, location or impact metric is invented.

### 3. Ecosystem as a field-story index

`src/components/pages/ecosystem-page.tsx`

The former catalog-like project list is now an alternating documentary composition with:
- project code and pillar as quiet context
- project title as the primary editorial voice
- authored tagline and summary with readable measures
- documentary media composition
- mobile-safe CTA targets

`cover_suppressed` remains fail-closed. A suppressed media record is never replaced by stock/fallback imagery.

A no-image project is also a valid composition: when media is suppressed, the text reflows into a deliberate editorial column instead of leaving a broken grid.

### 4. Project detail as documentary dossier

`src/components/pages/project-detail-page.tsx`

The project experience is now explicitly ordered as:
1. project identity / story opener
2. real cover documentation when publishable
3. summary + authored narrative
4. populated evidence only
5. evidence gallery when linked/public
6. relationship back to the Living Loop
7. related projects

Empty narrative/evidence still stays empty. No content is invented to make a page look fuller.

Documentary framing is applied to imagery rather than enclosing real CMS captions/metadata in card chrome.

### 5. Mobile and bilingual resilience

Long VI/EN contextual labels use `ed-context` rather than over-tracked microtext. Primary CTAs use a minimum touch height. Typography and whitespace use P15-WU2 semantic responsive primitives instead of per-page hardcoded sizing wherever WU3 touched the surface.

## Truth preserved

- registration != attendance
- confirmed registration != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance >0 = verified attended
- participant claim != attendance
- participant claim != Memory eligibility
- Memory remains evidence-gated
- Reflection remains evidence-gated
- P14-WU5A attendance date authority unchanged
- P15-WU1A explicit own-user boundary unchanged
- public project evidence renders only when populated
- suppressed project media remains suppressed

## Non-scope

- no database migration
- no RLS mutation
- no auth/identity mutation
- no attendance/Memory/Reflection mutation
- no My TNC redesign
- no Journey lifecycle redesign
- no Community redesign
- no social feed
- no member directory
- no gamification, points, badges or leaderboards
- no vanity impact counters
- no fabricated team, founder attribution, partner, beneficiary or impact data

## Regression QA

Added:
`scripts/p15-wu3-public-story-world-qa.ts`

CI step:
`P15-WU3 public Story World editorial QA`

The gate verifies:
- WU2 semantic editorial roles are applied to the public Story World
- Home chapter rhythm/context language
- Ecosystem field-story architecture
- Project documentary dossier architecture
- `cover_suppressed` fail-closed behavior
- evidence content gating
- bilingual shared architecture
- prohibited social/gamification patterns are absent from WU3 public surfaces

## CI evidence

Product PR #44: MERGED

PR CI:
- `#182`: PASS
- head SHA: `5fa9b9bd18e1603c75ad2c10620de28081ddf5d0`

Post-main CI:
- `#183`: PASS
- main SHA: `f1d98844b7c2b15245ce73e9b975551674f83055`

Verified PASS in both final gates:
- P15-WU3 public Story World editorial QA
- P15-WU2 experience design system QA
- P15-WU1A Journey own-data boundary QA
- P14-WU5A attendance date-authority source QA
- P14-WU5A attendance date-authority DB QA
- earlier source and ephemeral DB regression suites
- production build
- TypeScript typecheck
- Cloudflare configuration dry-run

## Production boundary

The product repository still has no GitHub production deployment workflow. CI's Cloudflare step is a dry-run only and does not upload or deploy the Worker.

Therefore this WU declares the source/main and canonical editorial gate as PASS. It does **not** claim that the Worker production or live website has been updated or visually verified with WU3 source.

No Worker version is inferred or fabricated here.

## Closeout

P15-WU3 is COMPLETE / PASS for source and canonical architecture.

NEXT:
**P15-WU4 — My TNC Entry, Identity & Personal Relationship Home**

WU4 must preserve the privacy ownership boundary established in P15-WU1A and all attendance/Memory/Reflection truth semantics established before Phase 15.
