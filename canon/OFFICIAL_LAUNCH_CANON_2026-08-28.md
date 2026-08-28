# TRẠM NỤ CƯỜI — OFFICIAL LAUNCH CANON

Date: 2026-08-28
Canonical status: OFFICIALLY LAUNCHED
Phase 8.3 status: COMPLETE / CLOSED
WU10 status: COMPLETE / PASS

## Canonical launch declaration

`https://tramnucuoi.com` is officially launched.

The launch is a governance/state declaration on top of the already-approved production release. No additional source deploy or database migration was required for WU10.

Launch posture:
- public website ON
- Admin MFA enforcement ON
- production transactional email OFF
- Journey registration CLOSED
- analytics OFF
- Turnstile/CAPTCHA OFF
- CSP Report-Only

## Official launch source baseline

Product repo: `huynhtranhuythinh/tramnucuoi`

- production `main`: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

## Canonical platform state

- Production domain: `https://tramnucuoi.com`
- Cloudflare Worker: `tramnucuoi`
- Supabase: `iwiqprhoohkxvjyxojto`
- Bunny CDN: `https://media.tramnucuoi.com`
- Official contact: `info@tramnucuoi.com`

Launch snapshot:
- media assets: 6
- publishable media: 6
- posts: 10
- publishable posts: 10
- editorial trust reviews: 16
- Journey applications: 0
- open Journey registrations: 0
- verified TOTP factors: 1

## Admin security

`ADMIN_MFA_REQUIRED=true`

Owner TOTP enrollment and AAL2 access are verified.

## Transactional email

`EMAIL_DELIVERY_ENABLED=false`

The transactional business flow has been verified, but production email remains fail-closed until a separate approved activation gate.

## Journey registration

No real Journey is open at launch.

Before a real Journey enters `registration_open`:
1. implement server-side rate limiting;
2. verify it;
3. obtain explicit Owner approval;
4. separately approve production email if needed.

## Privacy/tooling posture

- no analytics
- no marketing pixels
- no Turnstile/CAPTCHA
- no non-essential cookie banner

## Retention

Monthly manual retention review remains canonical at current scale.

## Carried non-blocking debt

- GitHub branch protection OFF
- Supabase leaked-password protection not enabled in observed posture
- CSP Report-Only

## Phase closure

**Phase 8.3 — Privacy, Trust & Release Readiness: COMPLETE / CLOSED.**
**WU9 — Release Readiness QA: COMPLETE / PASS.**
**WU10 — Final Launch Gate: COMPLETE / PASS.**
**Website status: OFFICIALLY LAUNCHED.**

