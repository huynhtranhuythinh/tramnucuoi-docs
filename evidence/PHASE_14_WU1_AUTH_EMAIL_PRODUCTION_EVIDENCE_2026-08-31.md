# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 14 / P14-WU1
# AUTH & EMAIL PRODUCTION EVIDENCE

Date: 2026-08-31
Status: **PASS**

## Evidence summary

This evidence record supports `canon/PHASE_14_WU1_AUTH_EMAIL_PRODUCTION_READINESS_2026-08-31.md`.

### Dashboard evidence confirmed by Owner

1. Authentication → URL Configuration
   - Site URL = `https://tramnucuoi.com`
   - Redirect URLs:
     - `https://tramnucuoi.com/auth`
     - `https://tramnucuoi.com/cong-dong`
     - `https://tramnucuoi.com/en/community`

2. Authentication → Emails → SMTP Settings
   - custom SMTP enabled
   - sender = `Trạm Nụ Cười <hello@notify.tramnucuoi.com>`
   - host = `smtp.resend.com`
   - port = `465`
   - username = `resend`
   - dedicated Resend sending credential created and stored only in Supabase

3. Authentication → Sign In / Providers → Email
   - email provider enabled
   - secure email change enabled
   - OTP/Magic Link expiry = 3600 seconds
   - OTP length = 8 digits

4. Authentication → Rate Limits
   - sending emails = 30/hour
   - token refreshes = 150/5 min
   - token verifications = 30/5 min
   - sign-ups/sign-ins = 30/5 min
   - anonymous users = 30/hour
   - IP forwarding = OFF

5. Authentication → Emails → Magic link or OTP
   - template updated to bilingual VI/EN copy
   - subject = `Trạm Nụ Cười — Liên kết đăng nhập / Sign-in link`
   - secure link remains `{{ .ConfirmationURL }}`

## Runtime evidence

### Delivery test

Controlled request to `/auth/v1/otp` returned:
- HTTP 200
- response `{}`

Owner received the production email from:
`Trạm Nụ Cười <hello@notify.tramnucuoi.com>`

### Desktop token/session test

Production Auth logs recorded:
- `/verify` → 303
- `login_method=implicit`
- repeated `/user` → 200

Logout test recorded:
- `/logout` → 204

A later attempt to reuse the same one-time link was rejected with the expected invalid/expired result.

### VI redirect test

Production Auth log recorded `/otp` → 200 with redirect/referer:
`https://tramnucuoi.com/cong-dong`

### EN redirect test

Production Auth log recorded `/otp` → 200 with redirect/referer:
`https://tramnucuoi.com/en/community`

### Retry protection test

Controlled immediate resend produced:
- first request → 200
- immediate retry → 429
- `error_code=over_email_send_rate_limit`
- message requested retry after approximately 55 seconds

### Mobile test

Owner opened the latest Magic Link from mobile email/browser.

Production Auth logs recorded from a different network/device context:
- `/verify` → 303
- Auth login event
- multiple `/user` → 200

Result: mobile browser Auth lifecycle PASS.

## Security evidence

Supabase Security Advisor after WU1:
- one existing warning: `Leaked Password Protection Disabled`
- no new Community/Auth security regression

The Community activation remains passwordless Magic Link based.

## Truth-safety postflight

Final production counts after WU1 Auth testing:
- profiles = 0
- participant links = 0
- Memories = 0
- Contributions = 0
- Reflections = 0
- relationship assignments = 0

No fake Community fact was produced by the tests.

## Repository evidence

Product main remained:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

No product commit, migration or Worker deploy occurred in P14-WU1.

Docs work was performed on:
`p14-wu1-auth-email-readiness`

## Final result

**P14-WU1 evidence result: PASS**

Auth/Email production readiness is proven sufficiently to enter controlled My TNC activation in P14-WU2 while retaining the explicit fail-closed product gate until WU2 deployment.
