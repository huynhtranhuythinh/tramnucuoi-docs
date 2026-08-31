# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 / P13-WU5
# LIVING COMMUNITY SURFACE

Date: 2026-08-31  
Status: **COMPLETE / PASS**

## 1. Objective

P13-WU5 implements the Phase 13 Layer D experience defined in WU1: a wider **Living Community Surface** that makes the ecosystem feel alive through public, trusted Journey activity without turning TRẠM NỤ CƯỜI into a generic social feed.

Canonical experience progression after WU5:

**Public Story World → My TNC → Journey Relationship Experience → Private Relationship Map → Living Community Surface**

The surface must answer:

> “Trạm đang sống qua những Journey, tư liệu và lời kể thật nào?”

without answering a different question that the data does not support, such as:

> “Ai đang ở trong cộng đồng?”

unless an explicit public-identity publication contract exists.

## 2. Source / product implementation

Product repository:
`huynhtranhuythinh/tramnucuoi`

Product PR:
- **#30 — P13-WU5 Living Community Surface**

Merged product main SHA:
`029d444a32529b23cc0171309e8bc81ae9792957`

Source added:
- `src/lib/community/living-community.server.ts`
- `src/lib/community/living-community.functions.ts`
- `src/components/community/living-community-surface.tsx`

Routes updated:
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`

No database migration was created in WU5.

## 3. Canonical public data boundary

WU5 composes only already-public source truth available through the existing anon/RLS publication boundary.

Allowed sources:
1. public operational Journeys;
2. published Journey Field Updates;
3. identity-minimized `journey_reflection_publications`.

The WU5 public reader does **not** read:
- `profiles`;
- applicant names/email/phone;
- participant operational PII;
- private `journey_reflections` source rows;
- Community relationship assignments for public naming;
- attendance records as a public people list;
- moderator identities;
- CMS actor identities.

RLS remains authoritative. WU5 did not create an alternate public bypass.

## 4. Public Reflection contract

P12-WU4 already established:

**Verified Person → verified attendance → evidence-backed Memory → completed Journey → Reflection → staff moderation → identity-minimized public publication**

WU5 consumes only the final publication projection:
`public.journey_reflection_publications`.

That projection contains only:
- `reflection_id`;
- `journey_id`;
- `body`;
- `locale`;
- publication timestamps.

It excludes:
- auth user UUID;
- Memory ID;
- email / phone;
- moderator UUID;
- internal moderation data.

WU5 does not attempt to reconstruct author identity from another table.

When no publication exists, the UI shows a truthful empty state. It does not generate, seed or editorially invent participant quotes.

## 5. Community identity publication boundary

P13-WU4 established:

**Verified internal relationship ≠ authorization to publish identity publicly.**

WU5 preserves this rule.

Therefore the Living Community Surface does not publish a person merely because that person is internally known as:
- Participant;
- Contributor;
- Host;
- Partner Representative.

No public people directory was created.

No follower/following graph, rank, badge economy, reaction count or engagement score was created.

A future public Host / Contributor / Partner identity surface requires a separate explicit publication/consent contract and is not implied by WU5.

## 6. Living Community information architecture

### 6.1 Editorial hero

Introduces Living Community as a shared ecosystem view rather than a feed.

Links back to:
- operational Journeys;
- Field Journal.

### 6.2 Now / Next Journey

Focus selection:
1. a public `preparing` Journey when one exists;
2. otherwise a public `registration_open` Journey;
3. otherwise a truthful no-current-Journey empty state.

This is an operational Journey focus only.

It does not infer:
- who is attending;
- attendance;
- Memory;
- Community membership.

### 6.3 Journey ecosystem

Shows public operational Journeys using existing trusted Journey visibility and media-cover rules.

Draft/archived Journeys remain outside the public projection.

No stock image is inserted when a trusted public cover is unavailable.

### 6.4 From the field

Shows published `journey_updates` in an editorial timeline.

The timeline is Journey-contextual, not an infinite generic feed.

It preserves:
- Journey association;
- recorded date;
- localized title/body/location when available;
- link back to the source Journey.

### 6.5 What remains / public Reflections

Shows only moderated rows already present in `journey_reflection_publications`.

The surface communicates that the publication is identity-minimized.

When none exist, it explicitly says no approved public Reflection exists yet.

### 6.6 Publication principle band

The closing trust statement makes the product rule visible:

**Public does not mean exposing people.**

Internal relationship verification and public identity publication remain separate decisions.

## 7. Bilingual behavior

The same source architecture is shared by:
- `/cong-dong`
- `/en/community`

Journey Field Updates use the existing VI/EN model:
- English preferred when present on EN surface;
- Vietnamese canonical fallback when English content is absent.

Reflection text is not machine-translated or rewritten.

Each published Reflection retains its source language and the UI labels the source locale. Requested-locale publications are prioritized before other-language publications while preserving recency.

This avoids inventing a translated participant voice that was never submitted or approved.

## 8. Production audit before implementation

Production Supabase project:
`iwiqprhoohkxvjyxojto`

Read-only audit observed:
- public Journeys total: **4**;
- registration-open Journeys: **1**;
- preparing Journeys: **0**;
- completed Journeys: **1**;
- published Journey Updates: **5**;
- published Field Journal posts: **10**;
- Journey media links: **11**;
- public documentary media assets: **5**;
- public Reflection publications: **0**;
- active Community Contributions: **0**;
- verified Community relationship assignments: **0**;
- verified Impact items: **0**;
- verified Impact snapshots: **0**.

The resulting WU5 UX intentionally has real Journey/Field activity and truthful empty states for Community-generated facts that do not yet exist.

## 9. Security / privacy verification

Production policy inspection confirmed:
- `journey_reflection_publications` has public SELECT for `anon, authenticated`;
- private `journey_reflections` does not have anon SELECT;
- published Journey Updates have public SELECT only under the existing public-Journey/status condition;
- public Journeys remain limited to public lifecycle states.

WU5 changed none of these policies.

No new grant, RLS policy, function privilege or SECURITY DEFINER path was added.

## 10. Quality gates

Product PR head:
`1b4bcff7bb5e5b886aa4ded9b3e766de2e284fa4`

PR CI:
- **#146 — PASS**

Post-merge main CI:
- **#147 — PASS**

Both gates include:
- P9 abuse/security regressions;
- P10 Cloudflare runtime regression;
- P11 transactional capacity/cutoff QA;
- P12-WU1 identity QA;
- P12-WU2 verified-email claim QA;
- P12-WU3 Memory QA;
- P12-WU4 Reflection QA;
- P12-WU5 Contribution QA;
- P12-WU6 Community Roles QA;
- P12-WU7 Impact Network QA;
- build;
- strict typecheck;
- Cloudflare dry-run.

## 11. Runtime / production mutation boundary

WU5 performed no:
- database migration;
- Supabase schema mutation;
- production data seed;
- fake Reflection;
- fake Contribution;
- fake relationship;
- fake impact record;
- Cloudflare production deployment;
- Email activation;
- Turnstile activation;
- Community Auth public activation;
- primary-navigation promotion.

The code is merged and source-ready, but public Community activation remains a separate WU6 gate.

## 12. Pilot continuity

P11 live pilot remains operationally authoritative.

The pilot truth is not changed by WU5:
- Journey date: 2026-09-11;
- status remains operationally managed outside WU5;
- confirmed registration is not attendance;
- WU5 does not create attendance, Memory, Reflection or Contribution to make the Community surface appear populated.

Real Community-generated content must emerge from real pilot operations and the Phase 12 verification chain.

## 13. Product outcome

After WU5, TRẠM NỤ CƯỜI now has two complementary Community views in source:

### Personal
**My TNC / Relationship Map**

Answers:
> “Tôi đã đi cùng Trạm như thế nào?”

### Shared
**Living Community Surface**

Answers:
> “Những Journey và câu chuyện thật nào đang làm Trạm sống?”

The shared surface remains editorial, source-backed and privacy-safe rather than social-feed-driven.

## 14. Closeout decision

**P13-WU5 — LIVING COMMUNITY SURFACE: COMPLETE / PASS**

Next work unit:

**P13-WU6 — PUBLIC ACTIVATION & POLISH**

WU6 must review activation readiness rather than automatically switch every gate on. It should cover responsive/a11y/bilingual polish, Email/Auth delivery readiness, navigation/SEO/noindex decisions, production deployment gate and Owner Review while preserving all Phase 12/13 truth and privacy contracts.
