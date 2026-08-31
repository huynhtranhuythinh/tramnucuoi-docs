# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 / P13-WU6
# PRODUCTION CLOSEOUT ADDENDUM

Date: 2026-08-31
Status: **COMPLETE / PASS**

This addendum supersedes the pending production status recorded in:
`canon/PHASE_13_WU6_PUBLIC_ACTIVATION_POLISH_2026-08-31.md`.

## 1. Final product state

Product repository:
`huynhtranhuythinh/tramnucuoi`

Initial WU6 product merge:
`1072c11366222847ca931ab392b04862c947cfca`

Owner Review found a real production UI regression after the first WU6 deploy:
- `/cong-dong` and `/en/community` rendered the global `PageShell` header;
- `CommunityAccessGate` also rendered its own BrandMark + language masthead;
- result: duplicated logo/header block on desktop and mobile.

Hotfix PR:
- #32 — `P13-WU6 hotfix: remove duplicate Community header`

Hotfix merged product main:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

The hotfix removed the internal Community masthead and made `PageShell` / `SiteNav` the single public site chrome.

A permanent regression assertion was added to `scripts/p13-wu6-activation-source-qa.ts` so `CommunityAccessGate` cannot reintroduce a BrandMark or direct VI/EN Community masthead.

## 2. CI evidence

Hotfix PR CI #150: **PASS**.

Post-merge main CI #151: **PASS** on:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

Passed gates include:
- P13-WU6 Community activation source QA;
- P9 → P12 regression suite;
- build;
- strict TypeScript typecheck;
- Cloudflare dry-run.

## 3. Production deployment evidence

Owner deployed the hotfix from canonical product main with:
`VITE_APP_COMMUNITY_AUTH_ENABLED=false`.

Cloudflare Worker:
`tramnucuoi`

Final production Worker Version ID shown in Owner Terminal evidence:
`23bacfb3-5ec0-4aa8-88e7-df8008ba1b81`

Deployment output showed successful asset upload and successful Worker deployment.

## 4. Live production visual review

Owner supplied final production screenshots for:
- VI desktop `/cong-dong`;
- EN desktop `/en/community`;
- VI mobile `/cong-dong`;
- EN mobile `/en/community`.

Final review result:
- one global logo/header only;
- no duplicate Community masthead;
- desktop navigation is coherent;
- mobile header uses the canonical single logo + menu pattern;
- VI/EN content renders in the correct locale;
- Living Community activation messaging remains visible;
- My TNC public sign-in remains clearly gated;
- no Magic Link form appears while Auth is OFF.

**Owner visual review: PASS.**

## 5. Auth / privacy boundary remains unchanged

Phase 13 closes with split activation:

### Public Living Community
**ON**
- `/cong-dong`
- `/en/community`
- indexable / canonical / bilingual;
- visible in public navigation/footer;
- reads only already-public or identity-minimized sources.

### My TNC public onboarding
**OFF**
- `VITE_APP_COMMUNITY_AUTH_ENABLED=false`;
- no public Magic Link onboarding;
- Email remains OFF;
- Turnstile remains OFF;
- public Community Auth activation remains a future explicit gate.

Phase 13 completion does not imply My TNC Auth activation.

## 6. Final production postflight nuance

A final read-only Supabase postflight showed:
- profiles: 0
- Community participant links: 0
- Community Journey Memories: 0
- Journey Reflections: 0
- public Reflection publications: 0
- active Contributions: 0
- verified Community relationship assignments: 0
- Community claim requests: 1

The single claim request was investigated rather than discarded.

It was created at `2026-08-31 10:10:26+00` (17:10 ICT) and recorded:
- eligible_count = 0
- newly_linked_count = 0
- already_linked_count = 0

Supabase API logs show the request came from an already-authenticated Chrome session on the operational pilot Journey path, where P13-WU3 `JourneyRelationshipExperience` intentionally invokes `claim_my_journey_participations()` for an existing verified session before reading that user's RLS-scoped Journey context.

This is consistent with the WU3 source contract and is not evidence that the public `/cong-dong` access-gate page mounted My TNC Auth.

No profile, participant link, Memory, Reflection, Contribution or verified relationship was created by that request.

## 7. Final declaration

**P13-WU6 — PUBLIC ACTIVATION & POLISH: COMPLETE / PASS**

The production deploy, live desktop/mobile smoke review, Owner Review and post-deploy side-effect investigation are complete.
