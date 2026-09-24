# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.6 — VI/EN / MOBILE / REGRESSION QA

Date: 2026-09-24  
Status: **COMPLETE / PASS — RELEASE REGRESSION GATE; NO PRODUCTION MUTATION**

## Objective

Lock the Phase-17 Journey rebase against bilingual route drift, mobile navigation regressions, truth-boundary regressions and accidental recruitment reopening before the production lifecycle cutover.

## Product evidence

Base main:
`4c4691f8de65138d1dad08bfe56e5051ac51a567`

Branch:
`p17-wu2-6-bilingual-mobile-regression-qa`

PR:
`#72 — P17-WU2.6: bilingual mobile release regression QA`

Final PR head:
`eb33c6d2b5c12e3aaf0ba4a02de13b5f127a64aa`

PR generic CI:
- run `35946055673`
- conclusion: **SUCCESS**

Dedicated P16-WU10B Volunteer Pilot Gate:
- run `35946055686`
- conclusion: **SUCCESS**

Squash-merged main:
`3647bf8e55249dcba5f3d9e8d88ff26065f5d95f`

Post-merge main CI:
- run `35946234628`
- conclusion: **SUCCESS**

## Regression gate

Added:
`scripts/p17-wu2-6-release-regression-qa.ts`

The gate verifies:

### VI / EN reciprocity
- every `PAGE_PATHS` pair round-trips VI ↔ EN;
- operational Journey detail remains `/hanh-trinh/:slug ↔ /en/journeys/:slug`;
- Field Journal remains `/nhat-ky/:slug ↔ /en/journal/:slug`;
- Impact remains `/tac-dong ↔ /en/impact`.

### Legacy compatibility
- `/journeys/*` permanently redirects to VI operational Journey;
- `/en/journey/*` permanently redirects to EN Field Journal.

### Mobile navigation
- mobile menu renders the same canonical primary navigation;
- locale switch closes the menu correctly;
- My TNC remains authenticated-only;
- Community does not return as a global primary mental model.

### Recruitment safety
- legacy `registration_open` projects to `upcoming + closed`;
- missing lifecycle columns report capability unavailable and remain closed;
- only explicit `upcoming + open` accepts new applications;
- open state outside upcoming is sanitized closed;
- Admin OPEN remains on the protected activation server path.

### Memory / publication safety
- `closeout_pending` cannot publish Memory;
- Impact remains gated by Memory;
- Social Continuity remains gated by Memory;
- Reflection requires Memory phase plus evidence-backed attendance Memory;
- public Impact navigation reads no private application/participant/Memory/Reflection source.

## Full inherited evidence

The exact PR head and post-merge main passed:
- all inherited P9–P16 source gates;
- all inherited ephemeral DB gates;
- P16-WU10B Vault/privacy gate;
- P17-WU2.1 through WU2.6 gates;
- build;
- typecheck;
- Cloudflare production-config dry-run.

## Non-scope

WU2.6 did not:
- mutate production Supabase;
- reconcile production Journey rows;
- open any application window;
- deploy Cloudflare production runtime;
- change feature flags.

Public recruitment remains **HOLD**.

## Decision

**P17-WU2.6 — COMPLETE / PASS.**

Next:
**P17-WU2.Final — PRODUCTION LIFECYCLE CUTOVER & CLOSEOUT**
