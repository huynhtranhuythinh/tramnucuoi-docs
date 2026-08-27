# TRẠM NỤ CƯỜI — PHASE 8.3 CURRENT HANDOFF

Date: 2026-08-27  
Status: CURRENT CONTINUATION PACKAGE  
Phase: 8.3 — Privacy, Trust & Release Readiness  
Next work unit: WU5 — Forms & Consent UX

## 1. Roles

- Owner: Jean Huỳnh
- CTO / Product Architect: ChatGPT
- Builder: Lovable

## 2. Canonical repositories

### Product/source

- Local: `~/dev/tramnucuoi`
- GitHub: `huynhtranhuythinh/tramnucuoi`
- Development/staging branch: `develop`
- Production-approved branch: `main`

### CTO/project documentation

- Local: `~/dev/tramnucuoi-docs`
- GitHub: `huynhtranhuythinh/tramnucuoi-docs`
- Canonical branch: `main`

## 3. Remote baseline at this handoff

### Product

- `develop`: `fbc5ed25fef61a5c1f2720839445b78b2fc180ae`
- `main`: `6a3a4767aaa9566fc15f3f04236850782dfcec7b`
- GitHub comparison: diverged; develop ahead 118 and behind 10.

Release rule:

> Do not perform a blind broad merge from `develop` to `main`. Reconcile deliberately or construct a narrow release PR after WU9.

### Documentation

- `main`: `50dc4af0f45737013d9351787e35e7465ff441fc` before this canonicalization package.

## 4. Current infrastructure truth

Use:

- `canon/INFRA_CANON_2026-08-27.md`
- `evidence/INFRA_EVIDENCE_2026-08-27.md`

Current infrastructure recorded there includes:

- Cloudflare staging and production runtime PASS;
- external Supabase `iwiqprhoohkxvjyxojto` canonical;
- Bunny canonical for new CMS media uploads;
- six historical media assets reconciled to Bunny;
- Resend production runtime configuration complete;
- Cloudflare deploy scripts fixed to preserve Dashboard runtime variables with `--keep-vars`.

Transactional email nuance:

- Resend infrastructure and production runtime configuration are complete.
- A real production business-flow email smoke test remains part of the Final Deploy / Release Gate unless separately completed and evidenced.
- Do not use older handoff wording such as “Resend unconfigured” as current truth.

## 5. Product status

- Phase 8.2 complete.
- Content activation complete.
- Owner review pass.
- Website bilingual VI/EN.
- Production domain: `https://tramnucuoi.com`.
- Cloudflare is authoritative edge/runtime.
- Supabase manages DB/Auth/RLS/CMS metadata.
- Bunny manages canonical media delivery for new uploads.
- Human mailbox: `info@tramnucuoi.com`.
- No Final Deploy Gate for the Phase 8.3 product changes yet.

## 6. WU1–WU4 status

| Work unit | Status | Activation |
| --- | --- | --- |
| WU1 Privacy & Trust Architecture Audit | Owner approved / complete | Audit only |
| WU2 Privacy Policy VI/EN | Owner approved / source prepared | migration 0023 unapplied |
| WU3 Website Use & Content Notice | Owner approved / source prepared | migration 0024 unapplied |
| WU4 Media Consent & Field Documentation Trust | Owner approved / source complete | migration 0025 unapplied |

Canonical WU summary:

- `08_HANDOFF/08_PHASE_8_3/WU1_WU4_CANONICAL_INDEX.md`

Detailed product records:

- `tramnucuoi/docs/PHASE_8_3_WU2_PRIVACY_POLICY.md`
- `tramnucuoi/docs/PHASE_8_3_WU3_WEBSITE_USE_NOTICE.md`
- `tramnucuoi/docs/PHASE_8_3_WU4_MEDIA_TRUST.md`

## 7. Migration lock

Keep these unapplied until the approved release sequence:

- `0023_phase_8_3_privacy_policy.sql`
- `0024_phase_8_3_website_use_notice.sql`
- `0025_phase_8_3_editorial_trust_workflow.sql`

No documentation commit, source commit or Builder message applies these migrations.

## 8. WU5 — Forms & Consent UX

### Objective

Audit every current public input surface and normalize data minimization, privacy notice, consent logic, accessible feedback and anti-dark-pattern behavior.

### Required audit targets

- Journey registration form;
- contact surface;
- collaboration/partner surface;
- newsletter/subscription surface if one exists;
- any upload or free-text field exposed publicly;
- form submission success and error states;
- spam/rate-limit protections;
- retention trigger when the related Journey/activity closes.

### Decision rules

- Do not add a consent checkbox for processing necessary to handle a request or registration.
- Use an independent, unchecked checkbox only for optional marketing/newsletter consent if such a feature is actually introduced.
- Media consent belongs to the WU4 field-documentation workflow, not the Journey registration form.
- Link the correct VI/EN Privacy Policy.
- Avoid pre-checked options, bundled consent, coercive copy and misleading success states.
- Do not build a contact form, partner CRM, newsletter system or cookie banner unless the audit proves it is needed and Owner approves the scope.

### Proposed WU5 deliverables

1. Current form/surface inventory.
2. Field-by-field necessity and sensitivity matrix.
3. Consent/legal-basis UX decision per surface.
4. VI/EN copy for notices, success and errors.
5. Spam/rate-limit decision.
6. Retention and deletion trigger mapping.
7. Code changes only where a verified gap exists.
8. QA matrix and Owner Gate.

## 9. Later work units

- WU6 — Cookie / Analytics Decision
- WU7 — Transactional Email Readiness
- WU8 — Security & Trust Hardening
- WU9 — Release Readiness QA
- WU10 — Final Deploy Gate

Because current infrastructure canon records Resend configuration complete, WU7 should verify application/business-flow readiness and release gating rather than repeat provider setup.

## 10. Open release risks

1. Product `main` and `develop` are diverged; release reconciliation is mandatory.
2. Branches are currently unprotected according to the observed GitHub baseline.
3. Leaked Password Protection remains an Owner/security decision unless newer evidence closes it.
4. WU4 migration/backfill and staff trust reviews have not run.
5. Phase 8.3 migrations 0023–0025 remain unapplied.
6. Production business-flow email smoke evidence remains pending unless captured later.
7. Docs repo visibility is currently public; no visibility change is authorized by this handoff.
8. DNSSEC and DMARC decisions must be handled in their appropriate WU/gate with current evidence.

## 11. Immediate next action

Start **PHASE 8.3 / WU5 — Forms & Consent UX** as a read-first audit against the product `develop` branch.

Do not deploy production. Do not apply migrations 0023–0025. Stop at the WU5 Owner Gate after reporting evidence, recommended changes and explicit non-goals.
