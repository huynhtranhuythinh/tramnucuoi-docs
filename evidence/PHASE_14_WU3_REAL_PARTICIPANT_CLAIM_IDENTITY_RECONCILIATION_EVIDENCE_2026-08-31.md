# P14-WU3 Real Participant Claim & Identity Reconciliation — Evidence

Date: 2026-08-31
Result: PASS

## Baseline
- Product main: `4582e8e6866714711631549ac4ed51cfb2d0c10d`
- Product CI on baseline: run #155 PASS
- Worker Version: `a47535cc-b6af-4b92-90d2-e6917f8051a4`
- Docs baseline: `d5370d7d3960a0db93b6ce4634fc819a80420d56`

No product code, migration, RLS or deployment change was required in WU3.

## Production identity audit
Read-only production audit established:
- Auth users total: 1
- profiles total: 1
- profiled verified non-anonymous Auth users: 1
- pilot applications: 1
- pilot confirmed applications: 1
- pilot participants: 1
- pilot confirmed participants: 1
- verified Auth-email matches to the confirmed pilot application: 0
- participant links: 0

PII values and UUIDs are intentionally excluded from this evidence file.

## Real claim-path evidence
`community_claim_requests` production audit:
- total real claim requests: 5
- requesting users: 1
- zero-eligible requests: 5
- rows with any non-zero result: 0

All processed requests therefore had:
- eligible = 0
- newly linked = 0
- already linked = 0

The live authenticated claim flow was exercised and correctly produced no link for the current verified account.

## Pilot truth audit
Pilot Journey `19539f36-3ed4-4a22-96b9-c8a9b73c5283`:
- status: `registration_open`
- start/end date: 2026-09-11
- confirmed application: 1
- confirmed participant: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended: 0
- application rows with claim-assigned `user_id`: 0
- participant rows with claim-assigned `user_id`: 0

No registration-to-attendance drift occurred.

## Community truth postflight
- active participant links: 0
- Memories: 0
- Contributions: 0
- Reflections: 0
- Community relationships: 0
- orphan participant links: 0
- orphan user links: 0
- duplicate active participant links: 0
- profiles without Auth users: 0
- CMS roles: one pre-existing `admin` role

No WU3 Community claim action created CMS permission.

## Claim RPC / RLS audit
Production introspection verified:
- `public.claim_my_journey_participations`: SECURITY INVOKER
- anon EXECUTE: false
- authenticated EXECUTE: true
- `private.tnc_process_community_claim_request`: SECURITY DEFINER, empty search path
- direct anon EXECUTE on private trigger function: false
- direct authenticated EXECUTE on private trigger function: false
- claim-request RLS: own-user SELECT + own-user INSERT
- participant-link RLS: own/admin SELECT; admin-governed INSERT/UPDATE

Source migration `0034_p12_wu2_claim_security_invoker_hotfix.sql` matches this production security model.

## Frontend audit
`CommunityExperienceShell` production source:
- calls `claim_my_journey_participations()` after verified session resolution;
- parses claim counts only;
- reloads RLS-scoped personal data after claim;
- has truthful bilingual empty copy when no Journey is verified with the account;
- explicitly states that sign-in/registration does not imply attendance;
- derives Memory/participant state from verified attendance-backed data, not from claim existence alone.

No UI change was required to represent the current zero-eligible state.

## Advisors
Security Advisor:
- no new claim/RLS vulnerability detected;
- one existing project-level warning: `auth_leaked_password_protection` disabled.
- remediation: https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

Performance Advisor:
- pre-existing informational/warning lints across the wider schema;
- no WU3 correctness/security blocker;
- no schema change made in WU3 to avoid scope expansion.

## Reconciliation outcome
The current verified Community account is not evidence-matched to the confirmed pilot participant under the canonical verified-email claim rule. The correct production result is therefore **no participant link**.

This is a successful fail-closed reconciliation, not a missing feature.

If the real pilot participant later authenticates using the same verified email as the confirmed registration, the existing live claim mechanism can create the link idempotently without rewriting attendance truth.

## Final result
PASS — no fake identity, no fake participant link, no fake attendance, no fake Memory/Contribution/Reflection, no permission drift.
