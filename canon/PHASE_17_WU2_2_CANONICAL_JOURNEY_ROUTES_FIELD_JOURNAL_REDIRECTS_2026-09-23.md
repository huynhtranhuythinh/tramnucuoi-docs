# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.2 — CANONICAL JOURNEY ROUTES & FIELD JOURNAL REDIRECTS

Date: 2026-09-23  
Status: **COMPLETE / PASS — SOURCE ROUTE REBASE; NO PRODUCTION DB OR FLAG MUTATION**

## 1. Objective

P17-WU2.2 establishes one unambiguous public route authority for operational Journey while preserving the existing Field Journal as a separate editorial object.

Canonical product rule:

> **Hành Trình / Journey is the primary operational and participation object.**
>
> Field Journal / Nhật ký is editorial documentation and must not compete for the same namespace.

WU2.2 changes routing/source composition only.

It does not apply WU2.1 lifecycle DDL to production.

## 2. Product baseline

Base product main:

`b870525511e346e2f06ed10c0270823c078b7131`

Branch:

`p17-wu2-2-canonical-journey-routes`

PR:

`#68 — P17-WU2.2: canonical Journey routes and Field Journal redirects`

Final PR head:

`3bccc967ef896cad3ff6f70d72ca8ab75988b353`

Squash-merged product main:

`cb18d0a3a8c030b347f215d9b8df7f7d6be485eb`

## 3. Canonical route authority

### Vietnamese operational Journey

Canonical:

- `/hanh-trinh`
- `/hanh-trinh/:slug`

These now render the operational Journey product.

### English operational Journey

Canonical remains:

- `/en/journeys`
- `/en/journeys/:slug`

### Vietnamese Field Journal

Canonical:

- `/nhat-ky`
- `/nhat-ky/:slug`

### English Field Journal

Canonical:

- `/en/journal`
- `/en/journal/:slug`

## 4. Compatibility redirects

Permanent 301 redirects:

- `/journeys` → `/hanh-trinh`
- `/journeys/:slug` → `/hanh-trinh/:slug`
- `/en/journey` → `/en/journal`
- `/en/journey/:slug` → `/en/journal/:slug`

No existing editorial record is deleted or silently repurposed.

## 5. Vietnamese legacy slug collision rule

Historical VI Field Journal detail used the same namespace now assigned to operational Journey:

`/hanh-trinh/:slug`

Canonical resolver behavior is therefore:

1. resolve publicly readable operational Journey first;
2. if operational Journey is readable, it owns the slug;
3. if no public operational Journey is readable, resolve legacy published Field Journal;
4. if a legacy published Field Journal exists, permanently redirect to `/nhat-ky/:slug`;
5. otherwise return the operational Journey not-found state.

Privacy boundary:

The resolver uses the existing public Journey query/RLS visibility.

It does **not** perform a privileged lookup merely to detect a hidden draft/archived Journey.

Therefore unpublished operational Journey existence is not disclosed by route collision logic.

## 6. Locale/canonical metadata contract

`PAGE_PATHS` now separates:

### Operational Journey

- VI: `/hanh-trinh`
- EN: `/en/journeys`

### Field Journal

- VI: `/nhat-ky`
- EN: `/en/journal`

The locale switch and `localizedHead` canonical/hreflang model now map:

- `/hanh-trinh/:slug` ↔ `/en/journeys/:slug`;
- `/nhat-ky/:slug` ↔ `/en/journal/:slug`.

English operational Journey metadata now correctly uses the Vietnamese canonical counterpart `/hanh-trinh`.

## 7. Cross-surface route correction

WU2.2 also corrected operational Journey links outside the public route files.

### Authenticated Journey Social Home

Before:

- VI operational Journey: correct;
- EN operational Journey incorrectly pointed to legacy Field Journal `/en/journey/:slug`.

After:

- VI: `/hanh-trinh/:slug`;
- EN: `/en/journeys/:slug`.

Journey discovery index follows the same authority.

### Community / My TNC continuity

Before:

- VI links used legacy `/journeys/:slug`.

After:

- VI: `/hanh-trinh/:slug`;
- EN: `/en/journeys/:slug`.

Field Journal references continue through the editorial helper and now resolve to:

- `/nhat-ky/:slug`;
- `/en/journal/:slug`.

## 8. Route QA

New QA:

`scripts/p17-wu2-2-route-authority-qa.ts`

It verifies:

- canonical Journey and Field Journal route pairs;
- reciprocal VI/EN locale mapping;
- canonical Journey/detail helpers;
- VI `/hanh-trinh` renders operational Journey;
- VI Field Journal uses `/nhat-ky`;
- EN Field Journal uses `/en/journal`;
- legacy route 301 redirects;
- operational Journey precedence over legacy Field Journal slug fallback;
- canonical/hreflang authority;
- Community/My TNC Journey links.

Generic CI includes the WU2.2 gate.

## 9. Inherited regression QA rebase

The first PR CI attempt exposed expected canonical-test debt.

### Regression 1 — P15-WU8

Old QA still asserted:

- Field Journal: `/hanh-trinh ↔ /en/journey`;
- operational Journey: `/journeys ↔ /en/journeys`.

WU2.2 updated the inherited assertion to the new canonical route model.

### Regression 2 — P16-WU3

Old typed Journey stream QA asserted Field Note href:

`/hanh-trinh/ghi-chep-1`

WU2.2 updated it to:

`/nhat-ky/ghi-chep-1`

No truth behavior changed; only the canonical editorial route changed.

## 10. Verification evidence

### Final PR-head evidence

Exact PR head:

`3bccc967ef896cad3ff6f70d72ca8ab75988b353`

Generic CI:

- run `35802506826`;
- conclusion: **SUCCESS**.

Dedicated P16-WU10B Volunteer Pilot Gate:

- run `35802506775`;
- conclusion: **SUCCESS**.

Generic CI PASS included:

- P15-WU8 bilingual/mobile route regression;
- P16-WU3 Journey Community Room / typed stream;
- P16-WU4–WU10 inherited social/privacy gates;
- P17-WU2 Gate 0;
- P17-WU2.1 lifecycle source contract;
- P17-WU2.2 canonical route authority;
- all inherited ephemeral DB QA;
- build;
- typecheck;
- Cloudflare dry-run.

### Post-merge main evidence

Product main:

`cb18d0a3a8c030b347f215d9b8df7f7d6be485eb`

Post-merge main CI:

- run `35802626064`;
- event: push;
- exact head: `cb18d0a3a8c030b347f215d9b8df7f7d6be485eb`;
- conclusion: **SUCCESS**.

## 11. Explicit non-scope

WU2.2 did not change:

- production Supabase schema;
- Journey production lifecycle columns;
- application state;
- registration RLS;
- attendance truth;
- participant claims;
- Memory eligibility;
- Reflection/publication truth;
- shared-experience graph;
- feature flags;
- Cloudflare deployment;
- public recruitment state.

Public recruitment remains:

**HOLD**

## 12. WU2.2 decision

**P17-WU2.2 — COMPLETE / PASS.**

Canonical result:

- Hành Trình now owns the operational Journey namespace;
- Field Journal has a separate canonical editorial namespace;
- old public links remain recoverable through permanent redirects;
- VI/EN canonical and hreflang relationships are unambiguous;
- Community/My TNC operational Journey links now point to the correct product object;
- inherited QA has been reconciled to the new route canon;
- no production database or runtime activation occurred.

Next:

**P17-WU2.3 — JOURNEY INDEX / DETAIL COMPOSITION**

WU2.3 should recompose the canonical Journey index/detail around lifecycle-aware information architecture and CTA behavior, while production lifecycle DDL remains behind its separate explicit gate.
