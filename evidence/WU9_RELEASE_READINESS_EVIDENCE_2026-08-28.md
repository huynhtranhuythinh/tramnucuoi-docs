# TRẠM NỤ CƯỜI — WU9 RELEASE READINESS EVIDENCE

Date: 2026-08-28  
Phase: 8.3 — Privacy, Trust & Release Readiness  
Work unit: WU9 — Release Readiness QA  
Status: COMPLETE / PASS

## 1. Scope and evidence rule

This record captures the verified WU9 release-readiness work completed on 2026-08-28. It distinguishes source release, database activation, runtime activation, operational decisions and QA cleanup.

Canonical product repo: `huynhtranhuythinh/tramnucuoi`  
Canonical docs repo: `huynhtranhuythinh/tramnucuoi-docs`  
Canonical Supabase project: `iwiqprhoohkxvjyxojto`  
Production: `https://tramnucuoi.com`

## 2. Gate A — CI repair and media-runtime reconciliation

Owner approved WU9 Remediation Gate A.

### CI blocker

Initial GitHub Actions failure came from the repository typecheck command attempting to fetch a non-existent `tsgo` package.

Remediation:

- `package.json` typecheck changed to `tsc --noEmit`.
- CI workflow remained frozen Bun install → typecheck → build.
- No dependency was added for the correction.

Verified develop release-candidate commit:

- `04b08215abbb7cef161bccbaa06bd687297c4091`
- GitHub Actions: SUCCESS.

Result: CI blocker CLOSED.

### Media runtime reconciliation

Fresh canonical Supabase inspection showed six current `media_assets` rows and all six existing rows were Supabase-backed, despite older historical evidence describing a Bunny reconciliation.

Current runtime decision:

- Bunny is canonical for NEW uploads when runtime selection is `bunny` and full Bunny config is present.
- Existing assets resolve from each row's own provider metadata.
- The six current historical assets remain Supabase-backed.
- No migration or deletion is performed merely to match historical evidence.

Canonical references:

- `canon/MEDIA_RUNTIME_CANON_2026-08-28.md`
- `evidence/MEDIA_RUNTIME_RECONCILIATION_2026-08-28.md`

Result: Gate A PASS / CLOSED.

## 3. Gate B2A — controlled production source release

Owner revised the environment strategy during WU9:

- production became the primary Owner QA environment for the pre-launch, single-owner phase;
- staging became optional;
- CI and deliberate release discipline remained mandatory;
- DB migrations, email activation, MFA enforcement and other state-changing actions remained separate gates.

### Source reconciliation

A blind `develop → main` merge was explicitly rejected because the branches had diverged historically.

The release used deliberate reconciliation preserving `main` production lineage while keeping the exact approved `develop` tree. Main CI had to pass before production deploy.

Production source release sequence:

- approved `develop` tree reconciled into `main`;
- GitHub Actions install/typecheck/build PASS;
- source-only Cloudflare Worker deploy using the canonical production command with `--keep-vars`;
- no database migration/write;
- no production email activation;
- no MFA enforcement activation at this gate.

### Production HTTP/security evidence

Verified production behavior:

- Home, Privacy VI/EN, Website Use VI/EN, `/auth` and `/admin` routes responded successfully.
- HSTS present.
- CSP present in Report-Only mode.
- `X-Content-Type-Options: nosniff`.
- `X-Frame-Options: DENY`.
- restrictive Permissions-Policy.
- Referrer-Policy present.

### B2A.1 auth/admin indexing correction

A narrow source-only follow-up added production `X-Robots-Tag: noindex, nofollow, noarchive` to `/auth`, `/auth/*`, `/admin`, `/admin/*` while leaving public production pages indexable.

Verification:

- `/` has no global noindex header.
- `/auth` is noindex/nofollow/noarchive.
- `/admin` is noindex/nofollow/noarchive.

Result: B2A + B2A.1 PASS / CLOSED.

## 4. Gate B2B-1 — Owner TOTP MFA readiness

Owner enrolled TOTP for the sole staff/admin account.

Verified:

- one staff/admin account;
- one verified TOTP factor;
- enrollment session reached AAL2;
- sign-out/sign-in triggered TOTP challenge;
- deliberately wrong TOTP was rejected;
- correct TOTP restored Admin access at AAL2;
- global enforcement remained OFF during this readiness test.

Result: B2B-1 PASS / CLOSED.

## 5. Gate B2B-2 — production transactional-email E2E

Transactional email was activated temporarily only for the controlled test window through the fail-closed runtime flag `EMAIL_DELIVERY_ENABLED`.

Verified business flow:

1. Public Journey registration saved successfully.
2. Applicant receipt arrived directly in the applicant inbox.
3. Admin notification arrived at `info@tramnucuoi.com`.
4. Reply-To from the admin notification resolved to the applicant address.
5. Admin accepted and then confirmed participation.
6. Participant confirmation email arrived successfully.
7. UI removed the confirm action after confirmation.
8. Database contained only one participant for the application.

The backend confirmation path uses a stable application-based idempotency key. No forced production replay was performed merely to provoke a second provider call after UI and data-layer duplicate protection were already verified.

After testing:

- `EMAIL_DELIVERY_ENABLED=false` was restored immediately;
- QA Journey/application/participant records were removed;
- production email returned to fail-closed state.

Result: B2B-2 PASS / CLOSED.

## 6. Journey Admin UX clarification

Owner identified confusing Admin wording between application acceptance and participant confirmation.

Source-only wording correction:

- `CHẤP NHẬN` → `DUYỆT ĐĂNG KÝ`
- accepted state → `ĐÃ DUYỆT ĐĂNG KÝ`
- `XÁC NHẬN THAM GIA` → `CHỐT THAM GIA`
- confirmed state → `ĐÃ CHỐT THAM GIA`

Backend statuses and business logic were unchanged.

Current product source snapshot:

- production `main`: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- `main` tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`
- `develop`: `03512ac8189ba9cbb77b9a02afcb37d037cc75ee`
- `develop` tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

`main` and `develop` therefore have different commit histories but the same final source tree at this evidence snapshot.

## 7. Gate B2B-3 — production migrations 0023–0025

Owner approved sequential activation with stop-on-failure verification.

Applied to canonical Supabase:

| Migration | Supabase migration version | Verification |
| --- | --- | --- |
| `0023_phase_8_3_privacy_policy.sql` | `20260828102538` | VI/EN Privacy content present, 11 sections each, official contact correct |
| `0024_phase_8_3_website_use_notice.sql` | `20260828102624` | VI/EN Website Use content present, visible/published, 9 sections each |
| `0025_phase_8_3_editorial_trust_workflow.sql` | `20260828102705` | schema/RLS/backfill active, existing public content preserved |

Post-0025 verification:

- six existing media assets became `legacy_public` rather than `approved`;
- ten existing published posts became `legacy_public` rather than `approved`;
- 16 `editorial_trust_reviews` rows exist;
- no consent/review row was auto-approved;
- six existing media remained publishable;
- ten existing posts remained publishable;
- no physical media migration or deletion occurred.

Result: B2B-3 PASS / CLOSED.

## 8. Gate B2B-4 — post-migration functional QA

### Journey/RLS

Read-only inspection confirmed migration 0025 did not change Journey application submission policies. The public registration path remains server-schema validated and database-RLS gated.

A new PII test application was intentionally not created after B2B-2 because the same business flow had already passed end-to-end and 0025 did not alter Journey application policies.

### Media allowlist and trusted upload path

The media contract was executed against representative cases.

Allowed formats:

- JPEG/JPG
- PNG
- WebP
- AVIF
- MP4
- WebM
- PDF

Limits:

- images: 15 MiB
- video: 250 MiB
- PDF: 25 MiB

Rejected cases verified:

- SVG/unsupported extension;
- HTML/unsupported extension;
- MIME/extension mismatch;
- image above 15 MiB;
- PDF above 25 MiB.

Live production smoke test:

- >15 MiB JPG was rejected before asset creation;
- valid tiny PNG uploaded through Bunny;
- created row had provider `bunny`, bucket `bunny`, `is_public=false`, `trust_status=unreviewed`;
- public action was blocked while trust status remained unreviewed;
- QA asset was deleted afterward;
- canonical media count returned from 7 to 6;
- no QA media residue remained.

Result: B2B-4 PASS / CLOSED.

## 9. Gate B2B-5 — final security and operational decisions

Owner approved the final decision package.

### MFA enforcement

Decision: ENABLE production Admin MFA enforcement.

Runtime action:

- `ADMIN_MFA_REQUIRED=true` enabled in the production Cloudflare Worker.

Owner verification after activation:

- current session AAL2;
- TOTP verified and configured;
- Admin security page reports global requirement enabled;
- no lockout occurred.

### Public abuse protection

Decision:

- Turnstile/CAPTCHA remains OFF.
- Analytics/tracking remains OFF.
- Current website launch is not blocked because no real Journey is open for registration.
- A real Journey MUST NOT enter `registration_open` until server-side registration rate limiting has been implemented and verified in a separate Journey activation gate.

### Retention operations

Decision:

- monthly manual retention review is sufficient for the current single-owner scale;
- unused/rejected Journey applications should be deleted or anonymised when no longer needed;
- normal unused-application retention should not exceed the Privacy Policy window (normally no later than 12 months after Journey closure/archive);
- confirmed participant records may be kept longer only where an operational, safety, accountability or legal purpose remains.

One historical QA application residue from the archived Journey QA flow was identified and explicitly deleted.

Final cleanup verification:

- QA application residue: 0
- total Journey applications: 0

Result: B2B-5 PASS / CLOSED.

## 10. Final WU9 canonical database snapshot

Verified after cleanup:

| Item | Count / state |
| --- | ---: |
| Media assets | 6 |
| Publishable current media | 6 |
| Posts | 10 |
| Publishable current posts | 10 |
| Editorial trust reviews | 16 |
| Journey applications | 0 |
| Verified TOTP factors | 1 |

Existing six historical media rows remain Supabase-backed per the current media-runtime canon. New verified production uploads route to Bunny when `MEDIA_ACTIVE_PROVIDER=bunny` is active.

## 11. Final WU9 runtime posture

- Production source deployed and QA-verified.
- Production Admin MFA enforcement: ON.
- Production transactional email delivery: OFF / fail-closed.
- Analytics/tracking: OFF.
- Turnstile/CAPTCHA: OFF.
- Real Journey registration: none open.
- Server-side Journey rate limiting: mandatory before any real Journey is opened for registration.
- CSP: Report-Only.
- Staging: optional for the current pre-launch single-owner operating phase.

## 12. Residual non-blocking governance/security debt

The following items were not silently treated as closed by WU9:

1. GitHub `main` and `develop` remain unprotected at the repository level.
2. Supabase leaked-password protection remains unavailable/not enabled under the observed project posture; mandatory Admin TOTP is the current compensating control.
3. CSP remains Report-Only rather than enforcing.
4. Journey registration rate limiting remains a future activation prerequisite.
5. Production transactional email remains disabled until a separately approved Journey activation gate.

These are explicit operational constraints, not hidden launch assumptions.

## 13. WU9 conclusion

**WU9 — Release Readiness QA: COMPLETE / PASS.**

The production website is technically release-ready for public launch under the recorded constraints. Opening real Journey registration and enabling production transactional email remain separate activation decisions, not implicit consequences of WU9 completion.
