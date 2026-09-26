# ADMIN V3 — PRODUCTION REALITY CHECK

**Date:** 2026-09-26  
**Scope:** Local/Lovable V3 ↔ GitHub main ↔ production-equivalent build ↔ Cloudflare production

## Result

### 1. Lovable/develop V3 vs GitHub main

- `src/components/admin/shell.tsx`: identical between `develop` and `main`.
- The Admin V3 route/components were present on `main`.
- The divergence was isolated to `src/styles.css`.

### 2. GitHub main vs production-equivalent build

A diagnostic build was executed using the same P17 feature flags as `scripts/p17-wu11-pilot-activate.sh`.

Before hotfix, the build produced:
- `styles-DD9WwKXX.css`
- `admin.index-B7y-sXrO.js`
- `admin.journeys-DentBSBu.js`
- `shell-DI3PfAjA.js`

These hashes matched the assets shown in the Owner's Cloudflare deploy terminal.

Conclusion:
**Local deployment source/artifact and GitHub main were aligned. This was not a stale-cache, wrong-branch, or dirty-local-build issue.**

### 3. Root cause

Production `main` CSS was missing the canonical V3 theme mappings present on `develop`, including:
- sidebar color contract;
- sidebar foreground/accent/border/ring mappings;
- primary/secondary/muted/accent/popover/border/input/ring/destructive semantic tokens.

The prior controlled Admin transplant had copied Admin-specific variables/utilities but did not carry the full UI theme contract required by `src/components/ui/sidebar.tsx`.

This caused utilities such as:
- `bg-sidebar`
- `text-sidebar-foreground`
- `bg-sidebar-accent`
- `border-sidebar-border`

to render incorrectly in production, producing the white sidebar seen in production while Lovable preview rendered the canonical dark navy V3 sidebar.

A literal `\\n` had also been introduced into the CSS declaration block during the earlier transplant and was repaired.

### 4. Hotfix

PR #135 restored only the missing canonical V3 theme contract in `src/styles.css`.

No DB, runtime, business logic, or public Journey code was changed.

Verification before merge:
- production-equivalent P17 build: PASS
- Admin V3 theme contract grep proof: PASS
- TypeScript: PASS
- generated CSS changed from `styles-DD9WwKXX.css` to `styles-B1AFQkM2.css`

Hotfix merged to `main`:
`ac84824d51ec23292fccb1394bfe1f22ee068edc`

## Status

- Source reality check: PASS
- Root cause: CONFIRMED
- Hotfix source merge: PASS
- Cloudflare redeploy after hotfix: PENDING
- Production visual smoke QA after redeploy: PENDING
