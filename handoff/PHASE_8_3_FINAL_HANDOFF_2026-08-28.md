# TRẠM NỤ CƯỜI — PHASE 8.3 FINAL HANDOFF

Date: 2026-08-28  
Status: CURRENT CONTINUATION PACKAGE  
Phase: 8.3 — Privacy, Trust & Release Readiness  
Next gate: FINAL LAUNCH GATE / WU10

## 1. Read these first

Canonical Phase 8.3 release record:

- `canon/PHASE_8_3_FINAL_RELEASE_RECORD_2026-08-28.md`

WU9 evidence:

- `evidence/WU9_RELEASE_READINESS_EVIDENCE_2026-08-28.md`

Media runtime:

- `canon/MEDIA_RUNTIME_CANON_2026-08-28.md`
- `evidence/MEDIA_RUNTIME_RECONCILIATION_2026-08-28.md`

This handoff supersedes `handoff/PHASE_8_3_HANDOFF_2026-08-27.md` for current continuation purposes. The older file remains historical evidence.

## 2. Current source

Product repo: `huynhtranhuythinh/tramnucuoi`

- production `main`: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- develop: `03512ac8189ba9cbb77b9a02afcb37d037cc75ee`
- both source trees: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

Do not assume equal source trees means branches may be blindly merged in the future. Continue deliberate reconciliation when ancestry diverges.

## 3. Phase status

- Phase 8.2 Journey Experience & Field Story: COMPLETE.
- Phase 8.3 WU1–WU9: COMPLETE / PASS.
- Privacy/Website Use migrations 0023–0025: APPLIED.
- Production source: DEPLOYED and QA-verified.
- Final public launch decision: PENDING OWNER GATE.

## 4. Current production posture

- Production: `https://tramnucuoi.com`
- Cloudflare Worker: `tramnucuoi`
- Supabase: `iwiqprhoohkxvjyxojto`
- Admin MFA enforcement: ON
- Owner TOTP: verified
- Production email: OFF / fail-closed
- Analytics/tracking: OFF
- Turnstile/CAPTCHA: OFF
- Real Journey registration: none open
- CSP: Report-Only
- Staging: optional for current single-owner pre-launch phase

## 5. Current database snapshot

- media assets: 6
- publishable media: 6
- posts: 10
- publishable posts: 10
- editorial trust reviews: 16
- Journey applications: 0
- verified TOTP factors: 1

Historical public media/posts are compatibility-classified `legacy_public`, not `approved`.

## 6. Final launch constraints

The website may be publicly launched with the current content and security posture.

Do NOT implicitly activate the following at launch:

- production transactional email;
- Journey `registration_open`;
- analytics;
- Turnstile/CAPTCHA.

Before the first real Journey registration opens:

1. implement server-side registration rate limiting;
2. QA it;
3. obtain explicit Journey activation approval;
4. separately approve production email activation if required.

## 7. Monthly operation after launch

Owner should perform a monthly retention review:

- review Journey applications;
- remove/anonymise unused or rejected records when no longer needed;
- review again when each Journey closes/archives;
- keep within the approved Privacy Policy retention window.

## 8. Known non-blocking debt

- GitHub branch protection: OFF.
- Supabase leaked-password protection: not enabled in observed posture; Admin TOTP is mandatory compensating control.
- CSP: Report-Only.

Do not silently mark these closed in a future phase.

## 9. Immediate next action

Run the **Final Launch Gate / WU10**.

The Final Launch Gate should decide only what is actually being launched. The safe default is:

- launch the public website;
- keep email OFF;
- keep Journey registration closed;
- keep analytics/Turnstile OFF;
- retain MFA enforcement ON.
