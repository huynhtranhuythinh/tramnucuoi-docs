# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.6
# ADMIN JOURNEY COMMUNITY CONTROLS

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Compose Journey Community operations into the existing Journey Control Center without creating a generic social administration suite.

WU6.6 reuses the authority already proven in WU6.1 and WU6.5.

No new database schema is introduced.

## 2. Admin policy controls

Admin can explicitly manage, per Journey:

- Publishing Window enabled/disabled;
- Publishing opens/closes timestamps;
- Interaction Window enabled/disabled;
- Interaction opens timestamp;
- optional Interaction closes timestamp;
- Visitor interaction policy;
- Post publication mode:
  - `participants_only`
  - `review_required`
  - `participant_choice`

There is no lifecycle-driven automatic open/close.

There is no hardcoded ±N day rule.

Disabled windows are persisted fail-closed.

## 3. Publication review queue

Journey Control Center presents:

- Posts waiting for public review;
- currently public Posts.

Review actions are visibility-only:

- approve public;
- reject public;
- unpublish.

The Admin review UI does not provide participant Post body editing.

Database authority remains WU6.5:

- global Admin; or
- current confirmed Journey BTC for publication review.

Global Editor is not Journey Community policy authority and is not silently promoted to BTC authority.

## 4. Compare-and-set review behavior

Review mutations use the expected prior publication state.

Examples:

- approve/reject requires `public_review_pending`;
- unpublish requires `public`.

If another actor already changed the Post, the UI reports a stale-state failure instead of blindly overwriting the newer truth.

## 5. Sensitive Journey protection

The Admin module explicitly supports forcing:

`participants_only`

for sensitive Journeys.

The product does not infer child/vulnerability status automatically.

Participation consent does not imply public image/content consent.

## 6. Source-first fail-closed behavior

Before WU6.Final production cutover:

- missing WU6 production table/RPC is treated as capability unavailable;
- UI shows a production-not-cutover notice;
- no fake policy row is created;
- no social window opens;
- no feature flag is changed.

## 7. Source files

New:

- `src/lib/journeys/community-admin.ts`
- `src/components/admin/journeys/journey-community-admin-manager.tsx`
- `scripts/p17-wu6-6-admin-community-controls-qa.ts`

Integrated:

- `src/components/admin/journeys/journey-control-center.tsx`
- `.github/workflows/ci.yml`
- `.github/workflows/p16-wu10b.yml`

## 8. QA invariants

WU6.6 QA protects:

- Admin-only policy management;
- Editor receives no Community control surface;
- no delete path;
- no attendance/Memory/shared-experience mutation;
- publication review cannot rewrite participant content;
- explicit Social Windows and publication mode;
- disabling Interaction Window also fails closed Visitor interaction in draft;
- no lifecycle auto-open;
- WU6.1 Admin policy authority remains canonical;
- WU6.5 BTC/Admin publication authority remains canonical.

## 9. Product PR / source truth

Product PR:

`#109 — P17-WU6.6: Admin Journey Community controls`

Final exact head:

`375eee4c02792a6a639728e12ea699b34cd221c8`

Merged product main:

`ac5aae039e183c1424eddc443a86e43e00e271c2`

## 10. Exact-head verification

Final exact-head runs:

- generic CI `36117674989`: **SUCCESS**
- dedicated gate `36117674780`: **SUCCESS**
- Cloudflare staging branch build: **SUCCESS**

Verified:

- WU6.6 Admin Community controls QA;
- all inherited WU6.1–WU6.5 source/UI/DB gates;
- inherited regression suite;
- build;
- typecheck;
- Cloudflare dry-run.

## 11. Post-merge verification

Product main:

`ac5aae039e183c1424eddc443a86e43e00e271c2`

Post-merge:

- main CI `36117861066`: **SUCCESS**
- Cloudflare production build: **SUCCESS**
- Cloudflare staging build: **SUCCESS**

## 12. Production state

No WU6.6 Supabase migration was applied.

No production Community v2 activation occurred.

Recruitment remains:

**HOLD / CLOSED**

## 13. Final decision

# **P17-WU6.6 — COMPLETE / CLOSED / PASS**

Proceed directly to:

# **P17-WU6.7 — NOTIFICATION / SAFETY / SHARED-JOURNEY COMPATIBILITY**
