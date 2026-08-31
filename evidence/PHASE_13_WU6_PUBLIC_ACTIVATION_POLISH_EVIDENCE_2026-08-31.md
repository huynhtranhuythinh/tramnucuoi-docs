# TRẠM NỤ CƯỜI — P13-WU6 EVIDENCE
# PUBLIC ACTIVATION & POLISH

Date: 2026-08-31

## Evidence declaration

This record proves the work completed for P13-WU6 and explicitly separates **merged/source activation readiness** from **Cloudflare production runtime deployment**.

No evidence below should be interpreted as proof that the WU6 bundle has already been deployed to `https://tramnucuoi.com`.

## 1. Product repository evidence

Repository:
`huynhtranhuythinh/tramnucuoi`

Starting product main:
`029d444a32529b23cc0171309e8bc81ae9792957`

Implementation branch:
`p13-wu6-public-activation-polish`

Product PR:
- #31 — `P13-WU6 Public Activation & Polish`

PR head:
`a94fe48b2fa8352662d08336ef3ea1458324398e`

Merged product main:
`1072c11366222847ca931ab392b04862c947cfca`

## 2. Source evidence

Added:
- `src/lib/community/activation.ts`
- `src/components/community/community-access-gate.tsx`
- `scripts/p13-wu6-activation-source-qa.ts`

Modified:
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`
- `src/lib/i18n/locale.ts`
- `src/components/home/site-nav.tsx`
- `src/components/site/site-footer.tsx`
- `src/styles.css`
- `.env.example`
- `.github/workflows/ci.yml`

Source diff from WU5 main to WU6 branch:
- 11 files changed;
- +269 / -44 at PR creation.

## 3. Activation boundary evidence

Environment control:
`VITE_APP_COMMUNITY_AUTH_ENABLED`

Default documented state:
`false`

Fail-closed implementation:
only literal case-normalized `true` activates personal Community Auth.

When OFF, VI/EN Community routes render:
- `PageShell`;
- `CommunityAccessGate`;
- `LivingCommunitySurface`.

When OFF, the routes do not mount:
- `CommunityExperienceShell`;
- `CommunityRelationshipMap`.

This prevents page visits from invoking the private/authenticated Community flow.

## 4. Public route / SEO evidence

Bilingual mapping:
- `/cong-dong`
- `/en/community`

Source now routes language-switch counterpart mapping through `PAGE_PATHS.community`.

Both routes use `localizedHead()`.

The previous route-level hard-coded `robots=noindex` was removed.

The public navigation and footer contain a Community link.

## 5. Accessibility evidence

`src/styles.css` adds a visible keyboard `:focus-visible` outline for:
- anchors;
- buttons;
- inputs;
- textareas;
- selects.

Existing `prefers-reduced-motion` handling remains present.

## 6. Dedicated WU6 source QA

CI step:
`P13-WU6 Community activation source QA`

Script:
`scripts/p13-wu6-activation-source-qa.ts`

The script checks:
- fail-closed env example;
- explicit activation helper contract;
- VI/EN route gate;
- public PageShell/access/Living Community composition;
- absence of hard-coded Community noindex;
- bilingual route mapping;
- nav/footer Community promotion;
- keyboard focus baseline.

Result in PR CI #148:
**PASS**.

Result in post-merge main CI #149:
**PASS**.

## 7. Full CI evidence

### PR CI #148

Commit:
`a94fe48b2fa8352662d08336ef3ea1458324398e`

Conclusion:
**SUCCESS**

Passed sequence:
1. P9-WU7 source abuse-protection QA
2. P10-WU3A Cloudflare runtime-context regression QA
3. P13-WU6 Community activation source QA
4. P9-WU7 ephemeral database gate / rollback QA
5. P11-WU11 transactional capacity / cutoff QA
6. P12-WU1 Community identity QA
7. P12-WU2 participant claim QA
8. P12-WU3 Memory projection QA
9. P12-WU4 Reflection moderation/publication QA
10. P12-WU5 Contribution History QA
11. P12-WU6 Community Roles / Host network QA
12. P12-WU7 Impact Network / provenance QA
13. build
14. strict typecheck
15. Cloudflare dry-run

### Main CI #149

Commit:
`1072c11366222847ca931ab392b04862c947cfca`

Conclusion:
**SUCCESS**

Same gate set passed post-merge.

## 8. Supabase production preflight/postflight evidence

Project:
`iwiqprhoohkxvjyxojto`

Project status:
`ACTIVE_HEALTHY`

Auth snapshot:
- auth users: 1
- email-confirmed auth users: 1

Auth logs show existing authenticated session/token activity from the production site.

This proves existing Auth operation but does not prove the public Magic Link delivery path required for Community onboarding.

### Community fact state after WU6 product merge

Read-only production query returned:
- profiles: 0
- Community participant links: 0
- Community claim requests: 0
- Community Journey Memories: 0
- Journey Reflections: 0
- public Reflection publications: 0
- active Contributions: 0
- verified Community relationship assignments: 0

No production data mutation occurred.

## 9. Auth activation evidence gap

P12-WU2 requires public onboarding readiness to prove:
- Site URL;
- allowed `/cong-dong` redirect;
- allowed `/en/community` redirect;
- Magic Link / OTP delivery;
- production email/SMTP path.

The available evidence in WU6 did not establish all five.

Therefore My TNC Auth remains OFF.

This is a PASS of the fail-closed release policy, not a failed implementation.

## 10. Advisor evidence

Security Advisor:
- no WU6-specific security regression;
- existing warning: `Leaked Password Protection Disabled`.

Performance Advisor:
- existing historical informational/warning items remain;
- WU6 did not mutate schema/RLS and introduced no new database object requiring performance remediation.

## 11. Deployment evidence boundary

Repository package scripts provide:
- `cf:prod:dry-run`
- `cf:prod:deploy`

CI only executes:
- `cf:dry-run`

There is no repository production-deploy workflow.

The current ChatGPT execution environment has no Cloudflare deployment connector. A plugin search for Cloudflare deployment capability returned none.

Direct web retrieval of the production Community URLs was not available in the current environment, so there is no independent live runtime proof of the WU6 source bundle.

Therefore:

**Cloudflare production deploy = NOT EVIDENCED / NOT CLAIMED.**

## 12. Required remaining evidence

To promote WU6 from implementation-pass to complete production-pass, capture:
1. Cloudflare deployment output/version for Worker `tramnucuoi` from product main `1072c113...`;
2. smoke check of `/cong-dong`;
3. smoke check of `/en/community`;
4. desktop/mobile navigation check;
5. language switch check;
6. SEO canonical/hreflang check;
7. confirmation that Magic Link UI remains unavailable while Auth flag is OFF;
8. post-deploy Supabase zero-side-effect check;
9. Owner visual review.

## 13. Evidence result

**P13-WU6 implementation evidence: PASS.**

**P13-WU6 production activation evidence: PENDING.**

Canonical product main ready for deployment:
`1072c11366222847ca931ab392b04862c947cfca`
