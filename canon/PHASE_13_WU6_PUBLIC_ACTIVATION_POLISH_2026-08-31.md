# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 / P13-WU6
# PUBLIC ACTIVATION & POLISH

Date: 2026-08-31  
Status: **IMPLEMENTATION COMPLETE / PASS — PRODUCTION DEPLOY + OWNER REVIEW PENDING**

## 1. Objective

P13-WU6 is the final Phase 13 activation gate.

Its purpose is not to turn every Community capability ON at once. It must decide which layer is safe to expose publicly now, preserve the Phase 12 truth/privacy model, harden the user-facing experience, and refuse to claim production activation without runtime evidence.

WU6 therefore separates two different products that previously shared the same route:

1. **Living Community — public editorial layer**
   - safe to index and navigate when it reads only already-public trusted sources;
   - does not require a Community account;
   - does not expose private people/relationship facts.

2. **My TNC — personal authenticated layer**
   - requires production Supabase Auth Site URL, redirect allowlist and Magic Link delivery to be proven;
   - remains activation-gated until those checks pass.

Canonical activation principle:

> **Public Community storytelling may open before public Community account onboarding.**

This avoids blocking the entire Community experience on email infrastructure while preserving the P12-WU2 onboarding gate.

## 2. Product implementation

Product repository:
`huynhtranhuythinh/tramnucuoi`

Product PR:
- #31 — `P13-WU6 Public Activation & Polish`

Merged product main SHA:
`1072c11366222847ca931ab392b04862c947cfca`

### 2.1 Explicit Auth activation switch

New source:
`src/lib/community/activation.ts`

Environment key:
`VITE_APP_COMMUNITY_AUTH_ENABLED`

The helper is fail-closed:
- literal `true` → personal Community Auth may mount;
- missing → OFF;
- blank → OFF;
- `false` → OFF;
- malformed/other values → OFF.

`.env.example` now documents:
`VITE_APP_COMMUNITY_AUTH_ENABLED=false`

This is a deliberate release control, not a cosmetic UI flag.

### 2.2 What happens while Auth is OFF

For:
- `/cong-dong`
- `/en/community`

WU6 renders:
- normal public `PageShell`;
- public site navigation;
- bilingual Community entry / activation explanation;
- `LivingCommunitySurface`;
- public footer.

It does **not mount**:
- `CommunityExperienceShell`;
- `CommunityRelationshipMap`.

Therefore the public page cannot accidentally:
- send a Magic Link;
- create a Supabase Auth user;
- create a Community profile;
- invoke `claim_my_journey_participations()`;
- create a participant link;
- create a Memory;
- expose a private Relationship Map.

The route explicitly explains that Living Community is open while My TNC sign-in is not yet public.

### 2.3 Future Auth activation path

The existing My TNC implementation is preserved.

If and only if production Auth readiness is later proved and the build explicitly sets:
`VITE_APP_COMMUNITY_AUTH_ENABLED=true`

the same stable bilingual routes can mount:
- `CommunityExperienceShell`;
- `CommunityRelationshipMap`;
- `LivingCommunitySurface`.

No second `/my-tnc` route is introduced, preserving the P13-WU1 route architecture.

## 3. Public navigation and SEO

WU6 promotes Community in:
- desktop public navigation;
- mobile menu;
- public footer.

Bilingual route mapping is now canonical:
- VI: `/cong-dong`
- EN: `/en/community`

`pathForLocale()` therefore keeps the language switch on the same Community page rather than producing an invalid translated path.

Community routes no longer hard-code `robots=noindex`.

They now use the shared `localizedHead()` contract and therefore receive:
- localized title/description;
- canonical URL;
- `hreflang=vi`;
- `hreflang=en`;
- `hreflang=x-default`;
- Open Graph metadata;
- social image fallback from the official Trạm Nụ Cười logo.

This SEO activation applies to the **public editorial Community layer**, not to publication of private people data.

## 4. UX / accessibility polish

WU6 adds a site-wide keyboard focus baseline for:
- links;
- buttons;
- inputs;
- textareas;
- selects.

The focus treatment uses the canonical yellow signal color and remains visible across light/dark editorial surfaces.

Existing reduced-motion behavior remains in force:
- smooth scrolling disabled for reduced motion;
- transitions/animations minimized;
- reveal/mask motion removed;
- botanical line animation disabled.

The Community public entry is responsive using the existing editorial breakpoints and avoids dashboard/social-feed chrome.

## 5. Supabase Auth activation review

Supabase project:
`iwiqprhoohkxvjyxojto`

Project health at WU6 review:
- ACTIVE_HEALTHY;
- hosted Supabase project;
- one existing Auth user;
- one email-confirmed Auth user.

Recent Auth logs demonstrate that the existing authenticated account can maintain/refresh sessions from `https://tramnucuoi.com/`.

That evidence is **not sufficient** to prove public Community Magic Link readiness.

Per the canonical P12-WU2 activation gate, public My TNC onboarding still requires proof of:
1. production Site URL;
2. allowed redirect URL for `/cong-dong`;
3. allowed redirect URL for `/en/community`;
4. production Magic Link / OTP email delivery;
5. production email/SMTP configuration appropriate for the onboarding flow.

Those conditions were not all independently proven in WU6.

Therefore:

**`VITE_APP_COMMUNITY_AUTH_ENABLED` remains OFF.**

No test user or fake participant was created merely to populate the UI.

## 6. Production Community fact state

Read-only production postflight after product merge:
- profiles: 0
- Community participant links: 0
- Community claim requests: 0
- Community Journey Memories: 0
- Journey Reflections: 0
- public Reflection publications: 0
- active Contributions: 0
- verified Community relationship assignments: 0

Thus WU6 introduced no Community person/history facts.

P11-WU6 live-pilot truth remains authoritative and independent.

## 7. Database / security boundary

WU6 is source-only.

It introduced:
- no database migration;
- no RLS policy change;
- no SQL function change;
- no role vocabulary change;
- no Supabase production mutation.

Existing guards remain:
- Email operational activation: OFF;
- Turnstile: OFF;
- public My TNC Auth: OFF;
- `pg_graphql`: OFF;
- CMS roles: `admin | editor` only;
- Community relationship role ≠ CMS permission.

## 8. Advisor review

### Security Advisor

No WU6-specific security regression was introduced.

Existing warning remains:
- `Leaked Password Protection Disabled`.

WU6 does not change password/Auth policy because public My TNC onboarding remains OFF and changing unrelated Auth policy would be scope expansion.

### Performance Advisor

Existing project performance advisories remain, including historical unindexed-FK / RLS-initplan / unused-index notices.

WU6 introduces no schema or query-policy mutation and does not claim to resolve those unrelated advisories.

## 9. CI evidence

### PR CI

CI run #148: **PASS**.

Passed:
- P9-WU7 source abuse-protection QA;
- P10-WU3A Cloudflare runtime-context regression;
- new P13-WU6 Community activation source QA;
- P9 DB gate / rollback QA;
- P11-WU11 QA;
- P12-WU1 → WU7 database regression suite;
- build;
- strict TypeScript typecheck;
- Cloudflare dry-run.

### Post-merge main CI

CI run #149: **PASS** on:
`1072c11366222847ca931ab392b04862c947cfca`

The same gate set passed after merge.

### WU6 source contract QA

New QA:
`scripts/p13-wu6-activation-source-qa.ts`

It asserts, among other things:
- Auth env flag defaults false in `.env.example`;
- activation helper requires literal `true`;
- VI/EN routes branch on the Auth gate;
- gated public routes use `PageShell` + `CommunityAccessGate` + `LivingCommunitySurface`;
- Community routes do not hard-code `noindex`;
- bilingual Community route mapping exists;
- Community is present in public nav/footer;
- keyboard `focus-visible` baseline exists.

The QA is now a permanent CI step.

## 10. Production deployment status

**Cloudflare production deployment was NOT performed or claimed in this WU6 execution.**

Evidence boundary:
- repository CI has dry-run only;
- no repository deploy workflow exists;
- the available ChatGPT environment has no Cloudflare deploy connector;
- direct runtime retrieval of `tramnucuoi.com` was not available in the current tool environment, so no live-version inference is permitted.

Therefore source readiness and production runtime activation are recorded separately.

### Canonical production deployment sequence

From the canonical local product repository:

```bash
cd ~/dev/tramnucuoi
git switch main
git pull --ff-only
bun install --frozen-lockfile
```

Before build/deploy, keep public My TNC Auth fail-closed:

```bash
unset VITE_APP_COMMUNITY_AUTH_ENABLED
```

or explicitly use:

```bash
export VITE_APP_COMMUNITY_AUTH_ENABLED=false
```

Then:

```bash
bun run cf:prod:dry-run
bun run cf:prod:deploy
```

The dry-run performs the production build before the deploy command consumes `dist/server/wrangler.json`.

After deploy, production smoke QA must verify:
- `/cong-dong` returns the new public Community surface;
- `/en/community` returns the EN surface;
- Community appears in desktop/mobile navigation and footer;
- VI ↔ EN Community language switch is correct;
- canonical/hreflang metadata is present;
- no Magic Link form is available while Auth flag is OFF;
- no new Community DB rows appear merely from page visits;
- existing Journey registration/pilot behavior is unchanged.

## 11. Owner Review gate

Owner Review remains pending until the merged WU6 source is deployed to the canonical Cloudflare production Worker and visually reviewed on real desktop/mobile production routes.

Required review focus:
- emotional/editorial quality;
- mobile spacing and hierarchy;
- public Community copy;
- clarity that Living Community is open while My TNC sign-in remains gated;
- navigation balance;
- no accidental “social network” feel.

## 12. Phase status decision

P13-WU6 has passed **implementation, architecture, privacy, source QA and CI**.

It has **not yet passed production deploy + live smoke + Owner Review**.

Therefore the correct declaration is:

**P13-WU6 — PUBLIC ACTIVATION & POLISH: IMPLEMENTATION COMPLETE / PASS — PRODUCTION DEPLOY + OWNER REVIEW PENDING**

And:

**PHASE 13 remains ACTIVE until that final production gate is evidenced.**

This is intentionally stricter than declaring Phase 13 complete from source code alone.
