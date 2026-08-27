# TRẠM NỤ CƯỜI — CROSS-REPOSITORY BASELINE EVIDENCE

Date: 2026-08-27  
Status: EVIDENCE / READ-ONLY AUDIT  
Scope: GitHub baseline after Owner local sync of product and documentation repositories.

## 1. Repositories observed

### Product

- GitHub: `huynhtranhuythinh/tramnucuoi`
- Visibility observed: private
- Default branch observed: `develop`
- Branches observed:
  - `develop`
  - `main`
  - `infra9-bunny-resend-prod`
  - `infra-keep-vars`
- `develop` HEAD observed: `fbc5ed25fef61a5c1f2720839445b78b2fc180ae`
- `main` HEAD observed: `6a3a4767aaa9566fc15f3f04236850782dfcec7b`

### CTO / project docs

- GitHub: `huynhtranhuythinh/tramnucuoi-docs`
- Visibility observed: public
- Default and only branch observed: `main`
- `main` HEAD observed: `50dc4af0f45737013d9351787e35e7465ff441fc`

The Owner reported the matching local working copies as:

- `~/dev/tramnucuoi`
- `~/dev/tramnucuoi-docs`

The ChatGPT environment cannot inspect the Owner's Mac filesystem or local `git status`; only GitHub remote state was independently read.

## 2. Product branch divergence

GitHub comparison `main...develop` reported:

- status: `diverged`
- `develop` ahead of `main`: 118 commits
- `develop` behind `main`: 10 commits
- merge base: `4e45b7f7cabb7395304ba04d8f5b7ad585aa7ab0`

Release implication:

> Final Deploy Gate must not use an unreviewed broad `develop -> main` merge. A deliberate reconciliation or narrow release branch/PR is required.

## 3. Documentation repository structure observed

The docs repository contains the intended canonical classes:

- `canon/`
- `evidence/`
- `handoff/`
- numbered domain/history directories
- `08_HANDOFF/`

Current observations:

- `canon/INFRA_CANON_2026-08-27.md` exists.
- `evidence/INFRA_EVIDENCE_2026-08-27.md` exists.
- `handoff/` contains only `.gitkeep`; no current continuation package exists.
- `08_HANDOFF/08_PHASE_8_3/` contains one older Phase 8.3 record.
- `.DS_Store` files are tracked at repository root and in nested folders.

## 4. Canon/handoff consistency finding

The latest infrastructure canon/evidence records:

- Bunny as canonical for new CMS media uploads;
- successful reconciliation of the six historical media assets;
- Resend production runtime configuration complete;
- Cloudflare runtime-variable persistence fixed;
- a production business-flow email smoke test still reserved for the Final Deploy / Release Gate.

The older Phase 8.3 historical handoff still describes earlier states such as transactional email being paused/unconfigured and pre-Bunny reconciliation assumptions.

Conclusion:

> The latest `canon/` and `evidence/` files are authoritative. The historical Phase 8.3 record must not be used as the current continuation package without a new `handoff/` record.

## 5. WU1–WU4 documentation coverage

### Product repository

- WU2 record exists:
  - `docs/PHASE_8_3_WU2_PRIVACY_POLICY.md`
- WU3 record exists:
  - `docs/PHASE_8_3_WU3_WEBSITE_USE_NOTICE.md`
- WU4 record exists:
  - `docs/PHASE_8_3_WU4_MEDIA_TRUST.md`
- Product Phase 8.3 handoff records WU4 Owner approval.
- No dedicated `PHASE_8_3_WU1...` file was found.

### Documentation repository

- No dedicated WU1–WU4 current records were found.
- The Phase 8.3 archive file predates the latest infrastructure canon and WU4 approval.

Conclusion:

> WU1–WU4 are not yet fully canonicalized in `tramnucuoi-docs`.

## 6. WU5 source

The Owner's Phase 8.3 master brief defines WU5 as **Forms & Consent UX**:

- audit contact form;
- collaboration/partner form;
- newsletter if present;
- checkbox consent only where genuinely required;
- Privacy Policy link;
- success/error messages;
- no dark patterns.

WU5 must begin with a source/runtime audit and must not assume that a contact form, newsletter or marketing consent flow exists.

## 7. GitHub write limitation observed

The ChatGPT GitHub connector successfully read both repositories. A contents-API write to `huynhtranhuythinh/tramnucuoi-docs` returned:

`403 Resource not accessible by integration`

Therefore:

- no remote docs-repo write was made;
- no branch, commit, product source, database or deployment was changed;
- the accompanying package was prepared for safe application from the Owner's local `~/dev/tramnucuoi-docs` working copy.

## 8. Baseline verdict

| Area | Result |
| --- | --- |
| Product GitHub remote identified | PASS |
| Docs GitHub remote identified | PASS |
| Remote branch/HEAD baseline | PASS |
| Current infra canon/evidence found | PASS |
| Current active handoff in docs repo | GAP |
| WU1–WU4 fully canonicalized in docs repo | GAP |
| Product `main` / `develop` safely mergeable without reconciliation | FAIL |
| WU5 scope identifiable from Owner master brief | PASS |
| Remote docs write through current connector | BLOCKED — 403 |
