# PHASE 16 — WU10A AUTHENTICATED JOURNEY SOCIAL HOME

Date: 2026-09-09
Status: SOURCE COMPLETE / CI PASS / MERGED TO PRODUCT MAIN

## Objective

Correct the authenticated Community entry experience so that Journey is the first social object for signed-in members, while preserving the existing public Living Community for signed-out visitors and preserving My TNC as the private personal-history layer.

## Canonical product semantics

Signed-out presentation:

1. Living Community public editorial surface.
2. My TNC sign-in / account entry.

Signed-in presentation:

1. Authenticated Journey Social Home.
2. My TNC private archive and relationship continuity.
3. Social safety controls.
4. Public Living Community remains available below the authenticated surfaces rather than replacing them.

Journey participation truth remains evidence-based:

- account != attendance;
- registration != attendance;
- confirmed participation before the event is not a Memory;
- attendance is shown only when canonical evidence exists;
- RLS and existing consent boundaries remain authoritative.

## Implementation

Product repository: `huynhtranhuythinh/tramnucuoi`

Merged PR: #62 — `P16-WU10A: authenticated Journey Social Home`

Product main commit:

`dadff1a2e2ffdaaa5b5a441660133178b9504117`

Primary implementation files:

- `src/components/community/authenticated-journey-social-home.tsx`
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`
- `scripts/p16-wu10a-authenticated-journey-social-home-qa.ts`
- `.github/workflows/ci.yml`

No database migration was required.

## Reused canonical foundations

WU10A reuses existing governed sources instead of creating a parallel social model:

- `community_journey_memories`
- published Journey updates
- governed Journey interactions
- Social Identity
- Journey social presence and consent boundaries
- My TNC private history

Production Supabase inspection before implementation confirmed the relevant Journey/community/social tables, private helper functions and RLS policies are present. The current participant-link projection was also checked and showed no linked participant row without a corresponding Journey memory at inspection time.

## QA evidence

Branch CI run #264 completed SUCCESS before merge.

All inherited source/database gates passed, including:

- P9 security QA
- P10 runtime-context regression
- P13/P14 Community activation and Auth gates
- P14 own-data and credential lifecycle
- P15 Journey own-data, experience, public Story World, My TNC, Journey lifecycle, Living Community, post-Journey continuity and bilingual/mobile gates
- P16 WU3–WU9 social/community/safety gates
- P16 WU10 auth confirmation and post-auth destination gates
- P16 WU10A authenticated Journey Social Home QA
- ephemeral PostgreSQL database gates
- application build
- TypeScript typecheck
- Cloudflare deployment dry-run

The initial WU10A CI attempt intentionally failed on the P15-WU6 source-order contract. That failure exposed that a naive component reorder would regress the signed-out public experience. The implementation was corrected with session-aware presentation semantics, and the superseded P15 ordering contract was updated without deleting the inherited Living Community safety checks.

## Production boundary

GitHub `main` is now canonical source truth at:

`dadff1a2e2ffdaaa5b5a441660133178b9504117`

This record does NOT claim Cloudflare production deployment because the current ChatGPT tool environment has no connected Cloudflare deployment control plane. Production runtime must only be declared PASS after the `tramnucuoi` Worker is deployed from this exact main commit and the live `/cong-dong` + `/en/community` flows are verified.

## Final WU10A decision

P16-WU10A implementation and repository integration: **COMPLETE / PASS**.

Cloudflare production cutover and live verification: **PENDING RUNTIME DEPLOYMENT EVIDENCE**.
