# TRẠM NỤ CƯỜI — WU10 FINAL LAUNCH EVIDENCE

Date: 2026-08-28
Phase: 8.3 — Privacy, Trust & Release Readiness
Work unit: WU10 — Final Launch Gate
Status: COMPLETE / PASS
Launch status: OFFICIALLY LAUNCHED

## Owner approval

Owner explicitly approved official launch of `https://tramnucuoi.com` on the current approved production `main` release, with:
- `ADMIN_MFA_REQUIRED=true`
- production transactional email disabled
- all Journey registrations closed until server-side rate limiting passes a separate activation gate
- analytics disabled
- Turnstile/CAPTCHA disabled
- CSP kept in Report-Only
- no additional source deploy
- no additional database migration

## Product source frozen at launch

Product repo: `huynhtranhuythinh/tramnucuoi`

- branch: `main`
- commit: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

No new source deploy was performed for WU10.

## Canonical database snapshot at launch

Supabase project: `iwiqprhoohkxvjyxojto`

- media assets: 6
- publishable media: 6
- posts: 10
- publishable posts: 10
- editorial trust reviews: 16
- Journey applications: 0
- Journeys in `registration_open`: 0
- verified TOTP factors: 1

No database write or migration was performed for WU10.

## Runtime posture at official launch

ON:
- public website
- Admin MFA enforcement
- Owner TOTP / AAL2 Admin access

OFF / HELD:
- production transactional email
- real Journey registration
- analytics / behavioral tracking
- Turnstile / CAPTCHA
- CSP enforcement; CSP remains Report-Only

## Journey activation constraint

A real Journey MUST NOT enter `registration_open` until:
1. server-side Journey registration rate limiting is implemented;
2. the rate limit is QA-verified;
3. Owner explicitly approves Journey activation;
4. production transactional email is separately approved if required.

## Retention operation

Current model: monthly manual retention review.
At launch, Journey application count is 0.

## Known non-blocking debt

- GitHub `main` and `develop` branch protection remains OFF.
- Supabase leaked-password protection is not enabled under the observed project posture; mandatory Admin TOTP is the compensating control.
- CSP remains Report-Only.
- Journey server-side rate limiting remains a future activation prerequisite.
- Production transactional email remains disabled until a separate activation gate.

## Conclusion

**WU10 — Final Launch Gate: COMPLETE / PASS.**

**`https://tramnucuoi.com` is officially launched as of 2026-08-28 under the constraints in this record.**

Phase 8.3 no longer has a pending production launch gate.

