# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 14 — REAL COMMUNITY ACTIVATION & MEMBER LIFECYCLE
# P14-WU1 — AUTH & EMAIL PRODUCTION READINESS

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

Verify production Auth onboarding and email delivery before any public My TNC activation.

This work unit did not enable public Community Auth. The required production gate remains:

`VITE_APP_COMMUNITY_AUTH_ENABLED=false`

## 2. Canonical production baseline

Product main remained unchanged during WU1:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

Docs baseline before this WU:
`e00e55cd5c82e7a5f85fe1e602fdf532760dae27`

Cloudflare Worker remained unchanged:
`23bacfb3-5ec0-4aa8-88e7-df8008ba1b81`

Supabase project:
`iwiqprhoohkxvjyxojto`

No DB migration was applied.
No production source deploy was performed.
No fake Community data was created.

## 3. Production Auth configuration verified

### Site URL

PASS:
`https://tramnucuoi.com`

### Redirect allowlist

PASS with exact production URLs:
- `https://tramnucuoi.com/auth`
- `https://tramnucuoi.com/cong-dong`
- `https://tramnucuoi.com/en/community`

### Email provider

PASS:
- Email provider enabled.
- Secure email change enabled.
- Email OTP / Magic Link expiration = 3600 seconds.
- Email OTP length = 8 digits.

### Custom SMTP

PASS.

Provider: Resend SMTP

Canonical settings:
- sender name: `Trạm Nụ Cười`
- sender email: `hello@notify.tramnucuoi.com`
- host: `smtp.resend.com`
- port: `465`
- username: `resend`
- minimum interval per user: `60` seconds
- credential: dedicated Resend API key scoped to Sending access and `notify.tramnucuoi.com`

The API key itself is intentionally not stored in canonical docs.

### Rate limits

PASS for controlled production activation:
- email sends: 30/hour
- token refreshes: 150 requests / 5 min
- token verifications: 30 requests / 5 min
- sign-ups/sign-ins: 30 requests / 5 min
- anonymous users: 30 requests/hour
- IP address forwarding: OFF

## 4. Magic Link template

The default English-only Magic Link template was remediated to a bilingual VI/EN template using Supabase template variables and redirect-aware rendering.

Canonical subject:
`Trạm Nụ Cười — Liên kết đăng nhập / Sign-in link`

The template continues to use:
`{{ .ConfirmationURL }}`

and branches copy based on `{{ .RedirectTo }}` so `/en/community` receives English copy and other Community flow receives Vietnamese copy.

## 5. Controlled runtime verification

Production verification used the Owner's real account and real email delivery. No fake account was seeded.

### SMTP / delivery

PASS:
- `/otp` returned HTTP 200.
- Email arrived successfully.
- sender was `Trạm Nụ Cười <hello@notify.tramnucuoi.com>`.

### Token verification and session

PASS:
- Magic Link `/verify` completed with HTTP 303.
- Supabase Auth recorded `login_method=implicit`.
- subsequent `/user` requests returned 200.

### Logout

PASS:
- `/logout` returned 204.

### One-time link semantics

PASS:
- reuse of the same Magic Link was rejected as invalid/expired.
- production UI displayed the expected expired/used-link state.

### VI callback

PASS:
- `/otp` accepted `https://tramnucuoi.com/cong-dong` as the production redirect.

### EN callback

PASS:
- `/otp` accepted `https://tramnucuoi.com/en/community` as the production redirect.

### Retry / resend protection

PASS:
- first request returned 200.
- immediate retry returned 429.
- error code: `over_email_send_rate_limit`.
- server requested approximately 55 seconds before retry.

### Mobile browser flow

PASS:
- Magic Link opened from mobile email client/browser.
- production Auth log recorded successful `/verify` 303.
- Auth recorded login.
- subsequent `/user` requests returned 200 from a different mobile IP/device context.

## 6. Source review

Current Community Auth source preserves bilingual redirect behavior:
- VI → `/cong-dong`
- EN → `/en/community`

Current client also preserves:
- session persistence;
- auto refresh;
- auth-token detection in URL;
- explicit sign-out;
- expired/invalid-link UX;
- participant claim after verified sign-in.

The Community UI intentionally uses `shouldCreateUser: true` for verified Community account onboarding.

This does not violate the truth model:
- account creation does not create attendance;
- account creation does not create Memory;
- account creation does not create Contribution;
- participant claim only links confirmed Journey participation that matches the verified email;
- an unmatched real email remains a verified Community account with zero linked Journey history.

## 7. Security review

Supabase Security Advisor has one known warning:

`Leaked Password Protection Disabled`

This is not a blocker for the passwordless Magic Link Community flow and is a Pro-plan feature in the current project configuration.

No new Community-specific security advisory was introduced in WU1.

## 8. Production truth postflight

After all WU1 runtime tests, substantive Community personal-history tables remained clean:
- profiles: 0
- participant links: 0
- Memories: 0
- Contributions: 0
- Reflections: 0
- relationship assignments: 0

This confirms WU1 Auth testing did not manufacture Community facts.

## 9. Activation state after WU1

Still OFF / gated:
- public My TNC Auth onboarding
- `VITE_APP_COMMUNITY_AUTH_ENABLED=true`
- Turnstile

Custom Supabase Auth SMTP is now configured and operational, but this does not itself expose the My TNC UI while the product build gate remains false.

## 10. Repository hygiene

Product repo:
- branch: `main`
- product HEAD unchanged: `b8c0fd597bbe411bee3165e5741471ea443c529e`
- no product PR in WU1
- no source change
- no migration
- no Worker deploy

Docs repo:
- branch: `p14-wu1-auth-email-readiness`
- documentation-only closeout and evidence update

## 11. Declaration

**P14-WU1 — AUTH & EMAIL PRODUCTION READINESS: COMPLETE / PASS**

Production Auth infrastructure is now ready for controlled My TNC activation.

NEXT:
**P14-WU2 — CONTROLLED MY TNC ACTIVATION**

WU2 must preserve an explicit rollback path and must not create fake profiles, roles, attendance, Memories, Contributions or Reflections for UI population.
