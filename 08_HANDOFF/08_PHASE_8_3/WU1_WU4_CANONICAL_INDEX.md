# PHASE 8.3 — WU1–WU4 CANONICAL INDEX

Date: 2026-08-27  
Status: CANONICAL SUMMARY / OWNER-APPROVED SEQUENCING  
Product reference branch: `huynhtranhuythinh/tramnucuoi@develop`  
Baseline product commit: `fbc5ed25fef61a5c1f2720839445b78b2fc180ae`

This file is the docs-repository summary of WU1–WU4. Detailed implementation files remain in the product repository because they describe code and migration artifacts.

## WU1 — Privacy & Trust Architecture Audit

Status: **OWNER APPROVED / COMPLETE**

Approved Owner gate:

`APPROVE WU1 AUDIT & SCOPE — authorize canonical media reconciliation, then proceed to WU2.`

Key historical outputs:

- data and provider inventory;
- risk/gap matrix;
- missing legal/trust components;
- Phase 8.3 scope and explicit non-goals;
- no production code change as part of the audit.

Important supersession note:

WU1 captured a point-in-time discrepancy where the six media records still resolved through Supabase. That finding is historical. The newer infrastructure canon/evidence records the completed Bunny reconciliation and Bunny as canonical for new media uploads.

A dedicated WU1 technical file was not found in the product repository. The WU1 audit result was preserved in Owner/CTO conversation history and is summarized here.

## WU2 — Privacy Policy VI/EN

Status: **OWNER APPROVED / SOURCE PREPARED / NOT ACTIVATED**

Approved Owner gate:

`APPROVE WU2 POLICY & RELEASE SEQUENCING — keep migration 0023 unapplied until Final Deploy Gate, then proceed to WU3.`

Product record:

- `docs/PHASE_8_3_WU2_PRIVACY_POLICY.md`

Product migration:

- `db/migrations/0023_phase_8_3_privacy_policy.sql`

Activation state:

- prepared in `develop`;
- not applied to canonical Supabase;
- not deployed to production;
- held until Final Deploy Gate.

Current-provider note:

Any historical provider wording in the WU2 technical record must be interpreted through the newer `canon/INFRA_CANON_2026-08-27.md`, which records Bunny reconciliation and current infrastructure truth.

## WU3 — Website Use & Content Rights

Status: **OWNER APPROVED / SOURCE PREPARED / NOT ACTIVATED**

Approved Owner gate:

`APPROVE WU3 NOTICE & RELEASE SEQUENCING — keep migration 0024 unapplied until Final Deploy Gate, then proceed to WU4.`

Product record:

- `docs/PHASE_8_3_WU3_WEBSITE_USE_NOTICE.md`

Product migration:

- `db/migrations/0024_phase_8_3_website_use_notice.sql`

Decision:

- compact bilingual Website Use & Content Notice;
- no SaaS-style Terms of Use;
- footer legal route, not primary navigation;
- no acceptance checkbox, modal or cookie banner.

Activation state:

- prepared in `develop`;
- not applied to canonical Supabase;
- not deployed to production;
- held until Final Deploy Gate.

## WU4 — Media Consent & Field Documentation Trust

Status: **OWNER APPROVED / SOURCE COMPLETE / NOT ACTIVATED**

Approved Owner gate:

`APPROVE WU4 MEDIA TRUST MODEL & RELEASE SEQUENCING — keep migration 0025 unapplied until Final Deploy Gate, then proceed to WU5.`

Product record:

- `docs/PHASE_8_3_WU4_MEDIA_TRUST.md`

Product migration:

- `db/migrations/0025_phase_8_3_editorial_trust_workflow.sql`

Approved model includes:

- trust states for media and stories;
- staff-only review records;
- consent, rights, minors and vulnerable-person review;
- restricted/archive/takedown workflow;
- public-read and CMS selection gates;
- pre-migration source compatibility;
- no automatic byte deletion.

Activation state:

- implementation complete in `develop`;
- Owner approved;
- migration not applied;
- no backfill or staff review cycle started;
- no public trust enforcement activated;
- held until Final Deploy Gate.

## Sequencing lock

The following migrations remain intentionally unapplied:

1. `0023_phase_8_3_privacy_policy.sql`
2. `0024_phase_8_3_website_use_notice.sql`
3. `0025_phase_8_3_editorial_trust_workflow.sql`

Documentation approval does not authorize database activation. Activation requires the release sequence documented in the current Phase 8.3 handoff and an explicit Owner gate.

## Next work unit

WU5 = **Forms & Consent UX**.

Source scope from the Owner's master brief:

- audit contact, collaboration/partner and newsletter forms if they exist;
- minimize collected fields;
- use a consent checkbox only when actually required;
- link the Privacy Policy;
- make success/error states clear and accessible;
- avoid dark patterns.

WU5 starts with audit evidence. It must not build forms, newsletter infrastructure, marketing consent or a cookie banner merely because they appear in the work-unit title.
