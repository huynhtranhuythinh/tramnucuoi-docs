# TRẠM NỤ CƯỜI — INFRASTRUCTURE CANON

Date: 2026-08-27
Status: CANONICAL
Scope: Production/staging infrastructure baseline after INFRA 8 Bunny and INFRA 9 Resend work.

## 1. Canonical ownership and release model

- Owner: TRẠM NỤ CƯỜI Owner
- CTO / Product Architect: ChatGPT
- Builder: Lovable
- Canonical repository: `huynhtranhuythinh/tramnucuoi`
- `develop` = staging/development integration branch
- `main` = production-approved branch
- Production Worker: `tramnucuoi`
- Staging Worker: `tramnucuoi-staging`
- Production URL: `https://tramnucuoi.com`
- Staging URL: `https://tramnucuoi-staging.huynhtranhuythinh.workers.dev`

Canonical release flow:

`Lovable -> GitHub develop -> Cloudflare staging -> QA -> approved release to main -> Cloudflare production`

Production Worker MUST NOT build non-production branches.

## 2. Canonical backend

- External Supabase project ref: `iwiqprhoohkxvjyxojto`
- Supabase remains canonical for DB, Auth, RLS, CMS metadata and application data.
- Lovable Cloud DB is not the canonical production database.

## 3. Canonical media architecture

Bunny is the canonical provider for NEW CMS media uploads.

- Storage Zone: `tnc-media`
- Pull Zone: `tnc-media`
- Region: Singapore
- CDN hostname: `https://media.tramnucuoi.com`
- Bunny access credentials are server-only Cloudflare runtime secrets.
- Existing asset metadata remains in Supabase `media_assets`.
- Per-asset provider metadata remains authoritative for asset resolution.

Production runtime configuration:

- `MEDIA_ACTIVE_PROVIDER=bunny`
- `BUNNY_CDN_BASE_URL=https://media.tramnucuoi.com`
- `BUNNY_STORAGE_HOSTNAME=sg.storage.bunnycdn.com`
- `BUNNY_STORAGE_ZONE=tnc-media`
- `BUNNY_STORAGE_ACCESS_KEY` = Cloudflare Secret

Operational rule:

> Never delete a source binary until the replacement asset is verified in the application and its references are confirmed.

INFRA 8 production state: **COMPLETE / PASS**.

## 4. Canonical transactional email architecture

Human mailbox and transactional email are intentionally separated.

Human/public mailbox:

- `info@tramnucuoi.com`
- Existing human mailbox provider remains independent from Resend.

Transactional sender:

- Resend domain: `notify.tramnucuoi.com`
- Region: Tokyo (`ap-northeast-1`)
- Sending: enabled
- Receiving: disabled
- Sender: `Trạm Nụ Cười <hello@notify.tramnucuoi.com>`
- Reply-To: `info@tramnucuoi.com`
- Internal notification recipient: `info@tramnucuoi.com`

Production runtime configuration:

- `RESEND_API_KEY` = Cloudflare Secret
- `EMAIL_FROM=Trạm Nụ Cười <hello@notify.tramnucuoi.com>`
- `EMAIL_REPLY_TO=info@tramnucuoi.com`
- `EMAIL_ADMIN_TO=info@tramnucuoi.com`

Resend DNS state:

- DKIM: verified
- SPF: verified
- MX return-path: verified
- Domain: verified

INFRA 9 staging transactional send test: **COMPLETE / PASS**.
INFRA 9 production configuration: **COMPLETE / PASS**.
Production business-flow email smoke test remains part of the Final Deploy / Release Gate unless explicitly executed earlier.

## 5. Cloudflare runtime configuration persistence

Cloudflare Dashboard runtime variables and secrets are canonical runtime configuration.

Golden rule:

> **Never deploy a Cloudflare Worker without `--keep-vars`.**

Reason: Wrangler deployments without `--keep-vars` can replace Dashboard-managed Text runtime variables, while secrets may remain, creating partial configuration drift.

Canonical production Build settings:

- Build command: `bun run cf:build`
- Deploy command: `bun run cf:prod:deploy`
- Root directory: `/`
- Production branch: `main`
- Builds for non-production branches: disabled

Canonical package scripts include `--keep-vars` for staging and production Wrangler deploy commands.

Cloudflare runtime var persistence state: **FIXED / PASS**.

## 6. Runtime variable names — production

Expected production runtime entries:

Secrets:

- `BUNNY_STORAGE_ACCESS_KEY`
- `RESEND_API_KEY`

Text:

- `BUNNY_CDN_BASE_URL`
- `BUNNY_STORAGE_HOSTNAME`
- `BUNNY_STORAGE_ZONE`
- `MEDIA_ACTIVE_PROVIDER`
- `EMAIL_ADMIN_TO`
- `EMAIL_FROM`
- `EMAIL_REPLY_TO`

Secrets MUST NOT be stored in Git, copied into documentation, or exposed in client bundles.

## 7. DNS and mail safety

Cloudflare is authoritative DNS for `tramnucuoi.com`.

Human mail records remain DNS-only and must not be proxied.

Do not replace or rewrite the existing human-mail SPF/DKIM/MX records when modifying Resend. Resend uses the dedicated transactional subdomain architecture.

DNSSEC, if enabled, must be managed through Cloudflare authoritative DNS and the registrar/registry DS workflow; do not restore obsolete DNSSEC settings from the former authoritative DNS provider.

## 8. Production release safeguards

- No automatic broad `develop -> main` merge for infrastructure-only releases.
- Infrastructure changes may use narrow PRs from `main` when needed to avoid unrelated feature leakage.
- Bunny production code was released as a narrow infrastructure change.
- Cloudflare `--keep-vars` persistence fix was released as a separate narrow infrastructure change.
- Builder must never be treated as an authority to auto-release production.
- Owner Gate remains required for production changes with material product or data impact.

## 9. Current infrastructure status

- Cloudflare production runtime: PASS
- Cloudflare staging runtime: PASS
- GitHub canonical release model: PASS
- External Supabase canonical backend: PASS
- Bunny staging: PASS
- Bunny production: PASS
- Resend domain verification: PASS
- Resend staging send/receive test: PASS
- Resend production configuration: PASS
- Runtime var persistence: PASS

This document supersedes temporary chat assumptions for the infrastructure items above until explicitly amended by a newer canonical record.
