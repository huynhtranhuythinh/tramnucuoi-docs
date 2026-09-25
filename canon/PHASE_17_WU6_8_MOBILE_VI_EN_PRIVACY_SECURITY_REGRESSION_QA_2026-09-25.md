# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.8
# MOBILE / VI-EN / PRIVACY / SECURITY REGRESSION QA

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Run the final source-first Journey Community v2 regression gate before production cutover.

WU6.8 verifies that the complete WU6.1–WU6.7 composition still preserves:

- mobile-first behavior;
- VI / EN parity;
- fail-closed runtime activation;
- Journey-scoped social boundaries;
- RLS / least privilege;
- public/private projection separation;
- Admin / Editor authority separation;
- cross-Journey isolation;
- safety without identity-UUID leakage;
- no mutation of attendance, Memory, Shared Journey evidence, Impact or recruitment truth.

WU6.8 adds QA only. It does not add a new product feature.

## 2. Source regression gate

New source QA:

`scripts/p17-wu6-8-mobile-vi-en-privacy-security-qa.ts`

The gate verifies:

### Fail-closed activation

Journey Community v2 requires:

- Community Auth enabled;
- Social Safety Hardening enabled;
- `VITE_APP_JOURNEY_COMMUNITY_V2_ENABLED=true`.

Missing/blank/non-true v2 activation remains disabled.

### Journey-only composition

Journey Community v2 remains composed inside the canonical Journey detail page.

WU6.8 does not add a global free-form composer.

### RLS and least privilege

Source QA asserts RLS on the WU6-owned source tables and inherited P16 safety tables.

Anon users do not receive direct SELECT on Journey Community source tables.

Public-safe projections continue through narrow RPC wrappers.

### Public projection privacy

Public Journey feed remains restricted to:

- active Posts;
- explicit `publication_state = public`;
- governed readable content.

The public projection does not include participant provenance, attendance, Memory, moderation reason or Report identity data.

Public conversation remains restricted to:

`visibility_scope = public_context`

Historical participant-only discussion is not silently published.

### Private media

The Storage bucket remains private:

`journey-community-private`

Object keys remain Journey/Post based and do not contain auth user UUID.

Public signed media remains short-lived.

### Safety

Client Report / Block actions continue to use Post/Interaction IDs only.

The server derives target social identity.

Global Editor does not inherit Journey Community Admin authority.

Admin Safety queue does not expose reporter identity and does not interpret priority as guilt.

### Operational truth isolation

WU6 source contains no write path to:

- attendance;
- Memory;
- Journey Reflection;
- Shared Journey edges;
- recruitment OPEN state.

## 3. VI / EN regression

Canonical Journey routes remain paired:

- VI: `/hanh-trinh/:slug`
- EN: `/en/journeys/:slug`

Notification routes remain paired:

- VI: `/cong-dong/thong-bao`
- EN: `/en/community/notifications`

Both notification routes remain:

`noindex, nofollow`

Core Journey Community vocabulary exists in both locales for:

- Publishing Window;
- Interaction Window;
- public Journey Community;
- Comment / Reply;
- Report / Block;
- publication / unpublish;
- “Lan tỏa” / Share;
- Notifications.

## 4. Mobile regression

WU6.8 locks mobile-first layout behavior for:

- Journey Community shell;
- composer;
- participant/public Post cards;
- Comment / Reply;
- Report / Block forms;
- Notification preferences/cards;
- Admin Community controls.

Verified source invariants include:

- narrow-screen stacking;
- responsive grid fan-out only at breakpoints;
- wrapping metadata/actions;
- full-width form controls;
- touch-target treatment;
- minimum-height select / checkbox rows;
- responsive nested conversation indentation.

## 5. PostgreSQL regression gate

New DB QA:

`scripts/p17-wu6-8-privacy-security-db-qa.sql`

It reuses the full WU6.1–WU6.7 ephemeral chain and then adds final catalog/enforcement checks.

### Catalog checks

Verified:

- RLS enabled on WU6/P16 social tables;
- anon has no direct Journey Community source-table SELECT;
- authenticated clients cannot read private moderation audit tables;
- public RPC wrappers are not SECURITY DEFINER;
- public feed/conversation RPCs remain executable to anon;
- Report / Block RPCs are not executable by anon.

### Global Editor isolation

A synthetic global Editor was unable to:

- read Admin-only Journey Community policy source rows;
- mutate Journey Community policy.

The public-safe Journey Community policy projection remained readable.

### Cross-Journey isolation

Fixture:

- Author participates in Journey 1 and Journey 2;
- Peer participates only in Journey 1;
- Author creates a participant-only Post in Journey 2.

Verified:

- Author can read own Journey 2 Post;
- Journey 1 Peer cannot read it;
- Journey 1 Peer cannot Report it;
- Journey 1 Peer cannot Comment on it;
- anon public projection does not expose it.

### Final truth checks

After the entire WU6.8 DB regression chain:

- attendance fields remain unresolved;
- Shared Journey edge count remains 0;
- all Journey application states remain CLOSED.

## 6. Product source truth

Product PR:

`#112 — P17-WU6.8: Mobile VI-EN privacy security regression QA`

Exact head:

`b92d1c2e94335480e5d4bac97fc127b1bcbbf595`

Merged product main:

`8aa7dad6a596f5ee7a5ef5a1e4958e273e185b9b`

Key source:

- `scripts/p17-wu6-8-mobile-vi-en-privacy-security-qa.ts`
- `scripts/p17-wu6-8-privacy-security-db-qa.sql`
- `.github/workflows/ci.yml`
- `.github/workflows/p16-wu10b.yml`

## 7. Exact-head evidence

Exact-head runs:

- generic CI `36130675002`: **SUCCESS**
- dedicated gate `36130675027`: **SUCCESS**

Verified PASS:

- WU6.8 mobile/VI-EN/privacy/security source QA;
- WU6.8 PostgreSQL privacy/security regression QA;
- inherited WU6.1–WU6.7 gates;
- build;
- typecheck;
- Cloudflare production config dry-run.

## 8. Post-merge evidence

Product main:

`8aa7dad6a596f5ee7a5ef5a1e4958e273e185b9b`

- main CI `36130850946`: **SUCCESS**
- Cloudflare production build: **SUCCESS**
- Cloudflare staging build: **SUCCESS**

## 9. Production preflight note

Read-only production preflight after WU6.8 confirmed:

- no `journey_community_*` production tables exist yet;
- P16 social tables exist;
- all inspected P16 social data counts are 0;
- latest production migration remains P17-WU5.Final.

Therefore WU6.Final does not require social-data backfill from existing production rows.

This preflight did not mutate production.

## 10. Production state

No WU6 Supabase production migration was applied in WU6.8.

Journey Community v2 remains fail-closed until WU6.Final.

Recruitment remains:

**HOLD / CLOSED**

## 11. Final decision

# **P17-WU6.8 — COMPLETE / CLOSED / PASS**

Proceed directly to:

# **P17-WU6.Final — PRODUCTION CUTOVER & CANONICAL CLOSEOUT**
