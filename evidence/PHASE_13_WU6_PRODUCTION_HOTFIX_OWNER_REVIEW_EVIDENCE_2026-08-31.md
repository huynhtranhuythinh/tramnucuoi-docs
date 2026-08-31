# TRẠM NỤ CƯỜI — P13-WU6 PRODUCTION EVIDENCE
# HOTFIX + OWNER REVIEW

Date: 2026-08-31
Status: **PASS**

## 1. Why this evidence exists

The first WU6 production deployment exposed a real UI regression: Community had two header/logo layers because `PageShell` and `CommunityAccessGate` both rendered masthead chrome.

Owner screenshots proved the issue on:
- desktop VI;
- desktop EN;
- mobile VI;
- mobile EN.

The issue was treated as a release blocker rather than visually waived.

## 2. Hotfix evidence

Product PR:
- #32 — `P13-WU6 hotfix: remove duplicate Community header`

Scope:
- remove internal `BrandMark` from `CommunityAccessGate`;
- remove its internal VI/EN masthead switch;
- use `PageShell` / `SiteNav` as the only site header;
- preserve Community hero, Living Community, SEO and Auth-OFF boundary;
- extend WU6 source QA to prohibit reintroduction of duplicate masthead chrome.

Diff at PR creation:
- 2 files changed;
- +6 / -17.

Hotfix product main:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

## 3. CI evidence

PR CI #150: **SUCCESS**.

Post-merge main CI #151: **SUCCESS**.

The final main gate passed:
- P9-WU7 source abuse-protection QA;
- P10-WU3A runtime-context regression;
- P13-WU6 activation source QA including duplicate-masthead regression protection;
- P9 DB gate / rollback;
- P11-WU11 QA;
- P12-WU1 → WU7 regression suite;
- build;
- strict typecheck;
- Cloudflare dry-run.

## 4. Cloudflare production evidence

Owner Terminal evidence after syncing product main showed successful deployment of Worker:
`tramnucuoi`

Final Worker Version ID:
`23bacfb3-5ec0-4aa8-88e7-df8008ba1b81`

Build/deploy was performed with:
`VITE_APP_COMMUNITY_AUTH_ENABLED=false`

Therefore the deployed production bundle keeps public My TNC onboarding fail-closed.

## 5. Final live screenshot evidence

Owner supplied final screenshots after the hotfix deployment.

Observed on desktop VI `/cong-dong`:
- one logo in global header;
- one navigation system;
- one VI/EN switch;
- Community hero begins directly below site header;
- no duplicate masthead.

Observed on desktop EN `/en/community`:
- same single-header architecture;
- EN route and content correct;
- no duplicate masthead.

Observed on mobile VI and EN:
- one logo;
- one canonical menu label/control;
- hero spacing no longer includes the removed second header block;
- no duplicate language masthead.

Owner visual evidence therefore resolves the release-blocking regression.

## 6. Production side-effect verification

Final read-only Community-domain snapshot:
- profiles = 0
- participant links = 0
- memories = 0
- reflections = 0
- public reflections = 0
- active contributions = 0
- verified relationships = 0
- claim requests = 1

The one claim request was created at 17:10 ICT with all claim result counts equal to zero.

Supabase API logs show immediately before it:
- an existing authenticated Chrome session refreshed its token;
- the client POSTed `/rest/v1/rpc/claim_my_journey_participations`;
- it then read RLS-scoped Memory / Contribution / Relationship / Reflection context for pilot Journey `19539f36-3ed4-4a22-96b9-c8a9b73c5283`.

Source inspection confirms P13-WU3 `src/components/journeys/journey-relationship-experience.tsx` deliberately performs this RPC for an already-authenticated session on an operational Journey detail page.

The event therefore does not demonstrate accidental activation of Community public sign-in on `/cong-dong` or `/en/community`.

No linked participant, profile or Memory was created.

## 7. Evidence conclusion

- production deployment: PASS
- desktop smoke: PASS
- mobile smoke: PASS
- VI/EN parity: PASS
- duplicate-header hotfix: PASS
- Auth-OFF boundary: PASS
- Owner Review: PASS

**P13-WU6 production evidence: PASS.**
