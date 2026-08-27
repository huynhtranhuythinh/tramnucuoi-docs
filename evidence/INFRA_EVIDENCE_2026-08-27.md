# TRẠM NỤ CƯỜI — INFRASTRUCTURE EVIDENCE RECORD

Date: 2026-08-27
Status: EVIDENCE / VERIFIED DURING OWNER SESSION
Scope: INFRA 8 Bunny, INFRA 9 Resend, and Cloudflare runtime persistence.

## 1. Evidence standard

This record captures the observed evidence used to mark infrastructure work PASS. It distinguishes verified runtime behavior from configuration intent.

## 2. Bunny — staging evidence

Observed and Owner-confirmed:

1. Bunny Storage Zone `tnc-media` created in Singapore.
2. Pull Zone `tnc-media` connected to the Storage Zone.
3. Custom hostname `media.tramnucuoi.com` configured and SSL verified.
4. Manual Bunny delivery test succeeded at `https://media.tramnucuoi.com/...`.
5. Staging Worker runtime was configured with Bunny provider variables and secret.
6. CMS staging upload created a Bunny-backed asset whose public URL used `media.tramnucuoi.com`.
7. CMS delete removed the Bunny object.
8. Bunny Storage file manager showed the deleted test object no longer present.

Conclusion: **Bunny staging upload / delivery / delete PASS**.

## 3. Bunny — existing media reconciliation evidence

The initial migration mechanism was intentionally abandoned after Owner review because a six-asset manual replacement was simpler and lower-risk for the small dataset.

Owner completed:

1. Re-uploaded 6 canonical images as new Bunny-backed media assets.
2. Copied/recreated required metadata for the replacement assets.
3. Relinked Journey/media references to the new assets.
4. Confirmed the Journey presentation was using the replacement images.
5. Deleted the 6 obsolete Supabase media assets after relinking.
6. Removed the temporary `CHUYỂN SANG BUNNY` migration UI after completion.

Conclusion: **Existing canonical media reconciliation COMPLETE / PASS**.

## 4. Bunny — production evidence

Production release was deliberately narrow to avoid unrelated Phase 8.3 feature leakage.

Git evidence:

- PR #2: `INFRA: enable Bunny media provider in production`
- PR #2 contained only Bunny/media provider files.
- Merge commit: `1554c6ec22c89f655c0454e0d05a21590056a87c`

Observed production runtime evidence:

1. Production Worker `tramnucuoi` was configured with Bunny runtime variables.
2. `MEDIA_ACTIVE_PROVIDER=bunny` was active.
3. Owner uploaded a test image directly through `https://tramnucuoi.com/admin/media`.
4. The uploaded image resolved from `https://media.tramnucuoi.com/library/...` rather than Supabase Storage.
5. Owner deleted that same test asset through production CMS.
6. Reopening the old Bunny URL returned **404**.

Conclusion: **Bunny production upload / delivery / delete PASS**.

## 5. Resend — DNS and domain evidence

Observed in Resend and Cloudflare:

1. Transactional domain: `notify.tramnucuoi.com`.
2. Region: Tokyo (`ap-northeast-1`).
3. DKIM record verified.
4. SPF TXT verified.
5. MX return-path record verified.
6. Resend domain status displayed **Verified**.
7. Sending enabled.
8. Receiving remained disabled by design.

Conclusion: **Resend domain verification PASS**.

## 6. Resend — staging application send evidence

Staging runtime configuration included:

- `RESEND_API_KEY` as Cloudflare Secret
- `EMAIL_FROM=Trạm Nụ Cười <hello@notify.tramnucuoi.com>`
- `EMAIL_REPLY_TO=info@tramnucuoi.com`

Observed application behavior:

1. Admin staging page exposed a staging-only email test action.
2. Initial provider rejection correctly surfaced a safe diagnostic: Resend reported the domain was not yet verified.
3. After domain verification, staging test send succeeded.
4. Gmail received the real message.
5. Subject observed: `TNC Resend staging test`.
6. From observed: `Trạm Nụ Cười <hello@notify.tramnucuoi.com>`.
7. Test body identified the staging environment and Resend provider.

Conclusion: **Resend staging transactional send/receive PASS**.

## 7. Resend — production configuration evidence

Production Worker runtime was configured with:

- `RESEND_API_KEY` as Secret
- `EMAIL_FROM=Trạm Nụ Cười <hello@notify.tramnucuoi.com>`
- `EMAIL_REPLY_TO=info@tramnucuoi.com`
- `EMAIL_ADMIN_TO=info@tramnucuoi.com`

Owner initially set `EMAIL_ADMIN_TO` to a personal Gmail address, then explicitly corrected it to the canonical organizational mailbox `info@tramnucuoi.com` because this variable is for internal transactional notifications.

Conclusion: **Resend production configuration COMPLETE / PASS**.

Note: a real production business-flow transactional email smoke test was not used as the closing evidence in this session. It remains part of the Final Deploy / Release Gate unless separately executed.

## 8. Cloudflare runtime variable persistence incident

Observed incident:

1. Dashboard-managed Text runtime variables were repeatedly disappearing after code deployments.
2. Cloudflare Secrets such as Bunny and Resend API keys remained present.
3. Inspection of production `package.json` showed Wrangler deploy commands without `--keep-vars`.
4. Cloudflare Worker Build settings also showed an incorrect arrangement: deploy logic was placed in Build command and the Dashboard Deploy command invoked Wrangler directly without `--keep-vars`.

Root cause:

> Wrangler deployments without `--keep-vars` replaced Dashboard-managed runtime Text variables.

Remediation evidence:

- PR #3: `INFRA: preserve Cloudflare runtime vars on deploy`
- PR #3 changed only `package.json`.
- Merge commit: `3a0916d9573ba1b0f4432d97494ec9f6324f6ae6`
- Canonical production Build command changed to `bun run cf:build`.
- Canonical production Deploy command changed to `bun run cf:prod:deploy`.
- Production branch remained `main`.
- `Builds for non-production branches` was disabled on production Worker.
- Runtime Text vars were then re-entered and remained available for the successful Bunny production test.

Conclusion: **Cloudflare runtime variable persistence FIXED / PASS**.

## 9. Canonical production runtime evidence at session close

Expected and Owner-confirmed production runtime entries:

Secrets:

- `BUNNY_STORAGE_ACCESS_KEY`
- `RESEND_API_KEY`

Text:

- `BUNNY_CDN_BASE_URL=https://media.tramnucuoi.com`
- `BUNNY_STORAGE_HOSTNAME=sg.storage.bunnycdn.com`
- `BUNNY_STORAGE_ZONE=tnc-media`
- `MEDIA_ACTIVE_PROVIDER=bunny`
- `EMAIL_ADMIN_TO=info@tramnucuoi.com`
- `EMAIL_FROM=Trạm Nụ Cười <hello@notify.tramnucuoi.com>`
- `EMAIL_REPLY_TO=info@tramnucuoi.com`

No secret values are recorded in this document.

## 10. Final evidence verdict

- INFRA 8 — Bunny Staging: **COMPLETE / PASS**
- INFRA 8 — Bunny Production: **COMPLETE / PASS**
- INFRA 9 — Resend Domain/DNS: **COMPLETE / PASS**
- INFRA 9 — Resend Staging: **COMPLETE / PASS**
- INFRA 9 — Resend Production Configuration: **COMPLETE / PASS**
- Cloudflare Runtime Var Persistence: **FIXED / PASS**

The above verdict is based on observed Owner tests, Cloudflare/Resend/Bunny dashboard evidence, and GitHub release evidence captured during the 2026-08-27 infrastructure session.
