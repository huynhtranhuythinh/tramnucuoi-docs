# PHASE 14 — WU3 REAL PARTICIPANT CLAIM & IDENTITY RECONCILIATION

Date: 2026-08-31
Status: COMPLETE / PASS

## Objective
Validate the real production participant-claim lane after My TNC activation and reconcile the current verified Community account against real Journey participation without manufacturing identity, attendance, Memory, Contribution, Reflection, relationship, or CMS permission facts.

## Canonical decision
P14-WU3 passes through the **truthful no-eligible-claim lane**.

The current real verified Community account and the confirmed participant record for the active 2026-09-11 pilot are **not the same verified identity under the evidence currently available to the claim system**. Their normalized verified/registration emails do not match. The system therefore correctly returns zero eligible participation and creates no participant link.

No admin override or cosmetic link was created. A participant link is evidence-backed identity reconciliation, not a UI-filling mechanism.

## Product repository
- Repo: `huynhtranhuythinh/tramnucuoi`
- Starting product main: `4582e8e6866714711631549ac4ed51cfb2d0c10d`
- Ending product main: `4582e8e6866714711631549ac4ed51cfb2d0c10d`
- Product branch / PR: **N/A — no source change required**
- Existing main CI on this exact SHA: run **#155 PASS**
- Database migration: **none**
- RLS/schema mutation: **none**
- Production deploy: **none**
- Worker Version remains: `a47535cc-b6af-4b92-90d2-e6917f8051a4`

The current source already implements the required fail-closed model:
- public `claim_my_journey_participations()` is `SECURITY INVOKER`;
- only authenticated users can execute the wrapper;
- privileged processing is in a private trigger function not executable by `anon` or `authenticated`;
- claim matching requires a verified, non-anonymous Auth email and confirmed application/participant truth;
- the UI loads zero-linked accounts as a truthful no-Journey state rather than manufacturing a relationship.

## Production identity reconciliation
Production audit found:
- Auth users: 1
- Community profiles: 1
- verified non-anonymous profiled accounts: 1
- pilot confirmed applications: 1
- pilot confirmed participants: 1
- verified-account email matches to confirmed pilot application: 0
- active Community participant links: 0

The actual email values and user/participant UUIDs are intentionally omitted from canonical documentation.

### Real claim execution evidence
The production claim audit contains 5 real processed requests from the single real Community account.

All 5 requests returned:
- `eligible_count = 0`
- `newly_linked_count = 0`
- `already_linked_count = 0`

This proves the live claim path executed and failed closed on a real identity mismatch rather than silently attaching an unrelated participant.

## Pilot truth preservation
Pilot Journey:
- Journey ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- event date: 2026-09-11
- production status: `registration_open`
- confirmed application: 1
- confirmed participant: 1
- participant/application operational `user_id` values set by claim: 0
- attendance resolved: 0
- attendance remains unresolved (`NULL`): 1

Therefore:
- confirmed registration remains distinct from attendance;
- participant claim remains distinct from attendance;
- the pre-event pilot has not been converted into an attended Journey;
- no Memory eligibility is inferred.

## Downstream truth postflight
Production remains:
- `community_participant_links`: 0
- `community_journey_memories`: 0
- `community_contributions`: 0
- `journey_reflections`: 0
- `community_relationship_assignments`: 0
- `user_roles`: 1 pre-existing `admin` role

No Community action in WU3 created or changed CMS authorization.

## RLS / security result
PASS.

Verified production properties:
- claim wrapper is `SECURITY INVOKER`;
- `anon` cannot execute the claim wrapper;
- `authenticated` can execute the wrapper;
- private processing function is `SECURITY DEFINER` with empty `search_path` and cannot be directly executed by `anon` or `authenticated`;
- claim-request RLS restricts read/insert to the owning authenticated user;
- participant-link read is own-or-admin; direct insert/update remains admin-governed;
- no orphan or duplicate active participant links exist.

Supabase Security Advisor has no new claim/RLS finding. The only security warning is the pre-existing project-level `auth_leaked_password_protection` warning, unrelated to this passwordless Magic Link claim lane.

Performance Advisor findings are pre-existing/non-blocking and are not expanded into schema changes in this operational work unit.

## Reconciliation rule established
For production Community identity:

1. A verified My TNC account may claim only confirmed participant records whose confirmed registration email matches the account's verified Auth email and whose ownership constraints remain compatible.
2. A zero-match result is a valid, truthful state.
3. Staff/admin must not attach a participant merely because a Community account exists.
4. If a real participant later signs in with the verified email actually used for the confirmed registration, the existing idempotent claim path is the correct reconciliation mechanism.
5. Evidence-backed corrections may use the governed revoke/replacement model; they must never rewrite historical attendance truth.

## Rollback / activation
No WU3 runtime mutation or deploy occurred, so no WU3 rollback is required.

The P14-WU2 My TNC rollback mechanism remains available: controlled rebuild/deploy with `VITE_APP_COMMUNITY_AUTH_ENABLED=false`. Public Living Community remains independent.

## Gate to P14-WU4
WU3 establishes both legitimate WU4 account lanes:
- claimed Journey lane when a real eligible participant claim exists; and
- truthful no-Journey lane when the signed-in account has no eligible confirmed participation.

The current real account is presently in the second lane.

P14-WU4 may therefore validate My TNC against that truthful state without fake Journey/attendance/Memory data.

## Result
**P14-WU3 — COMPLETE / PASS.**

Next: **P14-WU4 — Real My TNC Member Experience**.
