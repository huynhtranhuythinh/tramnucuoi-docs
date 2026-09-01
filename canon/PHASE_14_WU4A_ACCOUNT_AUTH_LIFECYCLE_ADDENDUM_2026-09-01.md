# PHASE 14 — WU4A ACCOUNT AUTH LIFECYCLE ADDENDUM

Date: 2026-09-01
Status: **COMPLETE / PASS**
Parent: P14-WU4 — Real My TNC Member Experience
Next canonical work unit: **P14-WU5 — Post-Journey Memory & Reflection Operations**

## Purpose

WU4A is a post-WU4 production addendum that upgrades My TNC from a Magic-Link-only entry experience to an account-first authentication lifecycle while preserving the Community truth model.

This addendum does not reopen WU4 truth semantics and does not infer attendance, Memory eligibility, contribution, reflection or Community relationship from authentication.

## Production result

My TNC now supports:

- Create account with display name + email + password.
- Email confirmation before password sign-in is accepted.
- Email + password sign-in.
- Optional one-time Magic Link sign-in for an existing account only.
- Forgot-password email recovery.
- Password reset.
- MFA/TOTP AAL2 challenge during password recovery when the account has verified MFA.
- MFA/TOTP AAL2 challenge during normal password or Magic Link sign-in when required.
- Explicit signup success state that replaces the form with a clear “Check your email” panel and prevents accidental duplicate signup submissions.
- Localized VI/EN application validation for short passwords and unconfirmed email state.

## Security corrections completed

Production runtime testing exposed and closed three auth-specific defects:

1. Recovery-session/profile hydration race
   - Recovery Auth session was valid but an immediate profile read could transiently return 401.
   - Recovery is now an Auth-only lane until the recovery session is established.

2. MFA/AAL2 password-update requirement
   - A verified-MFA account cannot update password while remaining at AAL1.
   - Recovery now performs TOTP challenge/verify and requires AAL2 before password update.

3. Normal login MFA and auth callback deadlock
   - Password and Magic Link sign-in now gate on AAL and require TOTP when `aal1 -> aal2` is required.
   - Async Supabase work is deferred outside the synchronous `onAuthStateChange` callback to avoid the documented client deadlock pattern.

No database migration and no RLS mutation were required for WU4A.

## Production validation

Owner-validated production flows:

- Password recovery email -> recovery session -> MFA -> password update -> logout -> sign in with new password -> MFA -> My TNC: **PASS**.
- Password sign-in -> MFA -> My TNC: **PASS**.
- Magic Link -> MFA -> My TNC: **PASS**.
- New account signup -> explicit Check Email state -> unconfirmed password login blocked -> confirmation link -> My TNC: **PASS**.
- EN desktop account/signup/signed-in experience: **PASS**.
- Mobile VI account/login layout: **PASS**.
- Mobile EN account/login/signup layout: **PASS**.

Final product main:
`272fd07d4b5697a22ace650e0e8b87943f1b4276`

Final post-main CI:
- run #172: **PASS**

Final production Cloudflare Worker Version:
`d6d564c9-12d0-46c5-b9db-edbe0768e1cd`

## Community truth postflight

Read-only production postflight after WU4A:

- Auth users: 4
- Profiles: 4
- Profiles with display name: 4
- Active participant links: 1
- Community claim requests: 91
- Claim rows with a non-zero claim result: 6
- Community Journey Memory rows: 1
- Eligible Memories: 0
- Active Contributions: 0
- Reflections: 0
- Verified Community relationships: 0
- CMS roles: 1

The single active participant link belongs to the account using the exact email `demenkids@gmail.com`, which evidence-matched the confirmed pilot participant record through `verified_email_claim`.

That linked pilot Journey remains:

- status: `registration_open`
- start date: `2026-09-11`
- participant status: `confirmed`
- attended party size: `NULL`
- attendance recorded at: `NULL`

The corresponding Memory-state row is intentionally non-eligible:

- `attendance_state = unresolved`
- `attended_party_size = NULL`
- `memory_eligible = false`
- `attendance_recorded_at = NULL`

Therefore WU4A preserves the canonical rule:

**verified account / participant claim != attendance != eligible Memory**

The Gmail alias QA accounts and the original WU4 account did not acquire participant links or eligible Memories.

## Non-blocking follow-up debt

1. Auth email templates are not yet consistently bilingual VI/EN and on-brand across all template types.
   - Magic Link has branded Vietnamese copy.
   - Recovery and signup confirmation were observed with English-only templates during WU4A runtime testing.
   - A dedicated Auth Email Template normalization round remains required.

2. `community_claim_requests` continues to accumulate repeated idempotent rows because claim reconciliation runs on repeated auth/session hydration. This is audit noise, not a Community truth violation, but should be optimized later.

3. Production contains QA signup accounts created during real WU4A validation. Cleanup must be an explicit controlled action; do not delete them implicitly because Auth-user deletion/session revocation has security and referential implications.

## Decision

**P14-WU4A — COMPLETE / PASS.**

The account lifecycle is production-proven on desktop/mobile and VI/EN, including password, Magic Link, email confirmation, recovery and MFA. Community truth remains evidence-bound and pilot attendance remains unresolved.

Proceed to **P14-WU5 — Post-Journey Memory & Reflection Operations**. WU5 may begin preflight/readiness work before the 2026-09-11 pilot, but final post-Journey closeout must wait for real attendance truth.