# TRẠM NỤ CƯỜI — PHASE 8.3 FINAL RELEASE RECORD

Date: 2026-08-28  
Phase: 8.3 — Privacy, Trust & Release Readiness  
Canonical status: RELEASE READY / OWNER FINAL LAUNCH GATE PENDING  
WU9 status: COMPLETE / PASS

## 1. Canonical decision

Phase 8.3 implementation and Release Readiness QA are complete.

The production website is technically ready for public launch under the constraints in this record. Public launch does NOT implicitly activate Journey registrations, transactional email, analytics, Turnstile, or any other held feature.

The next governance step is the Owner Final Launch Gate.

## 2. Canonical product source

Product repo: `huynhtranhuythinh/tramnucuoi`

Production source:

- branch: `main`
- commit: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

Development source:

- branch: `develop`
- commit: `03512ac8189ba9cbb77b9a02afcb37d037cc75ee`
- tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

Interpretation:

> `main` and `develop` have different ancestry but are source-equivalent at the Phase 8.3 final snapshot because both point to the same tree.

Production releases must continue to use deliberate reconciliation rather than blind broad merges when histories diverge.

## 3. Canonical production topology

- Public domain: `https://tramnucuoi.com`
- Edge/runtime: Cloudflare Worker `tramnucuoi`
- Canonical database/Auth/RLS: Supabase `iwiqprhoohkxvjyxojto`
- New media upload provider when active: Bunny
- Bunny CDN: `https://media.tramnucuoi.com`
- Existing six historical media rows: Supabase-backed per-row provider metadata
- Human/privacy contact: `info@tramnucuoi.com`

Staging is optional for the current pre-launch single-owner operating phase. CI + controlled source release remains mandatory.

## 4. Privacy and website-use canon

Production migrations applied:

- `0023_phase_8_3_privacy_policy.sql`
  - Supabase migration version `20260828102538`
- `0024_phase_8_3_website_use_notice.sql`
  - Supabase migration version `20260828102624`
- `0025_phase_8_3_editorial_trust_workflow.sql`
  - Supabase migration version `20260828102705`

Public legal routes:

- `/chinh-sach-bao-mat`
- `/en/privacy`
- `/su-dung-website`
- `/en/website-use`

Privacy operating posture:

- no analytics/tracking;
- no marketing pixels;
- no cookie banner because there is no non-essential tracking system in current scope;
- official privacy/contact email: `info@tramnucuoi.com`;
- AI/sample imagery must never be represented as real documentary evidence;
- children/vulnerable-person documentation requires higher review;
- rights requests include correction, anonymisation, restriction and takedown review;
- normal unused Journey application retention is no later than 12 months after closure/archive, subject to the approved exceptions.

## 5. Editorial/media trust canon

Trust states are active:

- `unreviewed`
- `legacy_public`
- `approved`
- `restricted`
- `archived`
- `takedown`

Current compatibility state:

- 6 media assets exist;
- all 6 current public historical assets are publishable;
- the 6 historical public assets are `legacy_public`, not `approved`;
- 10 posts exist;
- all 10 current published posts are publishable;
- the 10 historical published posts are `legacy_public`, not `approved`;
- 16 editorial trust review rows exist;
- no review/consent was auto-approved by migration.

New media upload behavior:

- current production new-upload path was live-tested through Bunny;
- new QA asset defaulted to `is_public=false`;
- new QA asset defaulted to `trust_status=unreviewed`;
- public action was blocked until trust review became publishable;
- oversize image rejection was verified;
- QA asset was deleted and media count returned to 6.

No physical migration of historical media is required merely to normalize provider history.

## 6. Admin security canon

Production Admin MFA enforcement is ON:

- `ADMIN_MFA_REQUIRED=true`
- one staff/admin account;
- one verified TOTP factor;
- Owner sign-out/sign-in challenge verified;
- invalid TOTP rejected;
- valid TOTP restored access;
- post-enforcement Admin session verified at AAL2;
- no lockout observed.

`/auth` and `/admin` are explicitly noindex/nofollow/noarchive on production.

MFA is authentication assurance only; authorization remains `user_roles` + RLS.

## 7. Transactional email canon

Resend infrastructure and business flow are verified.

Verified controlled production E2E:

- applicant receipt;
- admin notification;
- Reply-To to applicant;
- participant confirmation;
- duplicate participant prevention.

Current production posture:

> `EMAIL_DELIVERY_ENABLED=false`

Production transactional email is therefore fail-closed and remains OFF until a separately approved Journey activation gate.

## 8. Journey registration canon

Current database state:

- Journey applications: 0
- no real Journey is currently open for registration

Current public registration controls include:

- server-side schema validation;
- RLS;
- honeypot field;
- field/party-size limits;
- save-before-email semantics.

Final Phase 8.3 decision:

> A real Journey MUST NOT be moved to `registration_open` until server-side registration rate limiting is implemented and verified.

Turnstile/CAPTCHA remains OFF. Introducing it later would reopen the relevant privacy/tooling decision.

## 9. Retention canon

Current operating model is manual because the system is single-owner and application volume is zero.

Required operation:

- perform a retention review monthly;
- review applications again when a Journey closes or archives;
- delete or anonymise unused/rejected applications when no longer needed;
- normally complete deletion/anonymisation no later than 12 months after Journey closure/archive;
- keep confirmed participant records longer only when there is a continuing operational, safety, accountability or legal purpose.

Historical QA application residue was deleted during WU9. Final Journey application count is 0.

## 10. Security headers and indexing

Production verification passed for:

- HSTS
- CSP Report-Only
- Permissions-Policy
- Referrer-Policy
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`

Production public routes remain indexable.

Production `/auth` and `/admin` are noindex/nofollow/noarchive.

Staging/preview retains global noindex behavior.

## 11. Current canonical data snapshot

| Item | State |
| --- | ---: |
| Media assets | 6 |
| Publishable media | 6 |
| Posts | 10 |
| Publishable posts | 10 |
| Editorial trust reviews | 16 |
| Journey applications | 0 |
| Verified TOTP factors | 1 |

## 12. Held features / explicit activation gates

The following are intentionally NOT active merely because Phase 8.3 is release-ready:

- Production transactional email → OFF.
- Real Journey registration → none open.
- Analytics/tracking → OFF.
- Turnstile/CAPTCHA → OFF.
- Enforcing CSP → not activated; current policy is Report-Only.

Before the first real Journey is opened:

1. implement server-side registration rate limiting;
2. verify the control;
3. explicitly approve Journey activation;
4. separately approve production transactional email if that Journey should send email.

## 13. Residual non-blocking debt

Recorded but not classified as a Phase 8.3 release blocker:

- GitHub `main` and `develop` branch protection remains OFF.
- Supabase leaked-password protection is not enabled under the observed project posture; mandatory TOTP is the compensating Admin control.
- CSP remains Report-Only.

These items must remain visible in future security reviews.

## 14. Release decision

**Phase 8.3 implementation: COMPLETE.**  
**WU9 Release Readiness QA: COMPLETE / PASS.**  
**Production technical readiness: PASS.**  
**Public launch: OWNER FINAL LAUNCH GATE PENDING.**

The Final Launch Gate may publish the current website without opening Journey registration or enabling transactional email.
