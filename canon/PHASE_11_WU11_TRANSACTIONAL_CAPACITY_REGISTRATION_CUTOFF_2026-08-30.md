# TRẠM NỤ CƯỜI — WEBSITE 2026
## PHASE 11 — REAL JOURNEY OPERATIONS
## P11-WU11 — TRANSACTIONAL CAPACITY & REGISTRATION CUTOFF HARDENING

**Date:** 2026-08-30  
**Owner:** Owner  
**CTO / Product Architect:** ChatGPT  
**Builder:** Lovable  
**Status:** **COMPLETE / PASS**

---

## 1. Objective

Close the remaining WU7 concurrency risk before the real Journey event without changing pilot state and without fabricating production registrations, attendance, evidence, or impact.

WU11 makes the following operational rules database-authoritative:

1. Journey capacity is measured in **people (`party_size`)**, not participant-row count.
2. Two concurrent confirmations for the same Journey cannot consume the same remaining capacity.
3. A confirmed participant and its application confirmation must commit together.
4. Confirmation retry/replay remains idempotent.
5. Capacity cannot be lowered below the already-confirmed roster.
6. Active/non-draft Journey capacity changes are admin-only scope decisions.
7. `registration_open -> preparing` is an intentional admin cutoff, never an automatic date transition.
8. An accepted-but-not-confirmed application liability must be resolved before cutoff.
9. After cutoff, no new positive application progression or participant confirmation is allowed.
10. WU9 attendance and WU10 evidence/impact semantics remain unchanged.

Canonical invariant remains:

`Registration != Confirmation != Attendance != Evidence != Impact`

---

## 2. Pre-mutation production audit

Production project:

- Supabase project ref: `iwiqprhoohkxvjyxojto`
- Pilot Journey: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- Pilot event date: `2026-09-11`

Pre-WU11 findings:

- pilot status: `registration_open`
- capacity: `30`
- confirmed participant rows: `1`
- confirmed people: `1`
- accepted pending rows: `0`
- accepted pending people: `0`
- application/participant semantic drift: `0`
- over-capacity Journeys: `0`

Architecture gap confirmed:

- existing admin UI checked projected capacity before confirmation;
- `confirmApplication()` performed participant insert and application status update as two client writes;
- production had no database trigger serializing confirmations per Journey;
- therefore two administrators could theoretically race on the final capacity slot.

No pilot row was mutated during audit.

---

## 3. Canonical implementation

### Product migration

`db/migrations/0031_p11_wu11_transactional_capacity_cutoff.sql`

### 3.1 Application workflow guard

Trigger:

`journey_applications_workflow_guard`

Function:

`private.tnc_assert_application_workflow()`

Enforces legal positive workflow transitions:

- `submitted -> reviewing`
- `reviewing -> accepted`
- `accepted -> confirmed`

Rejection remains available from the pre-confirmation workflow states.

Additional rules:

- positive progression is allowed only while parent Journey is `registration_open`;
- application capacity-bearing identity fields become immutable after participant creation;
- an application cannot enter `confirmed` unless a matching confirmed participant snapshot already exists in the same transaction;
- same-status writes remain idempotent for compatibility with the currently deployed client.

### 3.2 Transactional participant capacity guard

Trigger:

`journey_participants_capacity_guard`

Function:

`private.tnc_assert_participant_capacity()`

On a real confirmation it:

1. locks the application row;
2. validates application state and exact snapshot parity;
3. preserves duplicate-retry behavior through the existing `UNIQUE(application_id)` / SQLSTATE `23505` path;
4. locks the parent Journey row with `FOR UPDATE`;
5. verifies Journey status is still `registration_open`;
6. recomputes confirmed people from committed participant truth;
7. rejects a confirmation that would exceed `capacity`.

The parent-row lock serializes confirmations for the same Journey, removing the WU7 last-slot race condition.

Attendance-only updates remain outside this capacity trigger path.

### 3.3 Atomic application confirmation synchronization

Trigger:

`journey_participants_sync_application_confirmation`

Function:

`private.tnc_sync_application_confirmation()`

A successful confirmed participant insert/update synchronizes the corresponding application to `confirmed` **inside the same database transaction**.

This means the existing two-client-write implementation is now protected even before a future UI/runtime refactor: participant insertion is the authoritative capacity-bearing operation, and the database commits application confirmation atomically with it.

### 3.4 Capacity floor and authority

Trigger:

`journeys_capacity_guard`

Function:

`private.tnc_assert_journey_capacity()`

Rules:

- capacity may never be set below the current confirmed-person roster;
- draft planning remains compatible with existing editor permissions;
- once a Journey is non-draft, an actual capacity change requires an authenticated admin;
- unchanged capacity values remain harmless so existing editor forms can submit unrelated edits safely.

### 3.5 Registration cutoff gate

Trigger:

`journeys_registration_cutoff_gate`

Function:

`private.tnc_assert_journey_cutoff()`

Rules:

- an open Journey may leave `registration_open` only through `preparing`;
- cutoff is authenticated-admin-only;
- confirmed roster may not exceed capacity;
- there must be no `accepted` application still awaiting final confirmation;
- the gate does **not** automatically close registration based on date or capacity.

Owner/operator remains responsible for deciding when the real pilot should enter `preparing`.

---

## 4. Security model

All WU11 trigger functions are:

- in the `private` schema;
- `SECURITY INVOKER` (`prosecdef = false`);
- fixed to an empty `search_path`;
- not executable directly by `anon`;
- not executable directly by `authenticated`;
- reached only through their table triggers under existing table grants/RLS.

WU11 adds no service-role bypass and no new public RPC surface.

Existing RLS policies remain authoritative.

`pg_graphql` remains **OFF**.

Security Advisor after migration:

- no new WU11 security lint;
- existing warning remains: **Leaked Password Protection Disabled**.

Reference remediation:

`https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection`

---

## 5. QA evidence

### 5.1 Dedicated ephemeral DB QA

Added:

`scripts/p11-wu11-db-qa.sql`

CI now runs WU11 database QA against a dedicated ephemeral database named `p11_wu11` on the GitHub-hosted PostgreSQL service. It never connects to production.

Coverage includes:

- illegal application workflow bypass rejected;
- exact-capacity confirmation accepted;
- participant insertion atomically confirms application;
- duplicate confirmation retry resolves through `23505`, not a false capacity failure;
- a distinct confirmation above capacity is rejected;
- failed over-capacity confirmation leaves no partial application/participant state;
- capacity cannot be lowered below confirmed roster;
- cutoff blocked while accepted liability remains;
- cutoff blocked for editor/non-admin;
- valid admin cutoff succeeds after accepted liability is resolved;
- positive workflow progression is blocked after cutoff;
- rejection remains possible after cutoff;
- unchanged capacity remains compatible with existing editor forms;
- actual active Journey capacity change is admin-only;
- final application/participant semantic drift remains zero.

Initial CI run #94 failed at the newly introduced WU11 QA gate before any production mutation. The WU11 QA harness was then isolated into its own fresh ephemeral database to prevent cross-test schema contamination from the destructive P9 database test.

Final branch CI:

- PR #17
- head: `85db12aa9bded6dd5739baea3b4f42e818ec38c8`
- CI run #95: **PASS**
- WU11 DB QA: PASS
- P9-WU7 source QA: PASS
- P10-WU3A runtime-context regression QA: PASS
- P9-WU7 DB gate/rollback QA: PASS
- typecheck: PASS
- build: PASS
- Cloudflare dry-run: PASS

Post-merge main CI:

- product main: `aff8000bc59ed28b973fc06e205b574743e2fa89`
- CI run #96: **PASS**

No Cloudflare deployment was performed by WU11.

---

## 6. Production migration

Applied successfully to canonical Supabase production:

- migration name: `p11_wu11_transactional_capacity_cutoff`
- migration version: `20260830164328`

Verified production triggers:

- `journey_applications_workflow_guard`
- `journey_participants_capacity_guard`
- `journey_participants_sync_application_confirmation`
- `journeys_capacity_guard`
- `journeys_registration_cutoff_gate`

No Journey status, registration, participant, attendance, Field Update, media evidence, or impact row was intentionally created or changed by the migration.

---

## 7. Post-migration pilot verification

Pilot Journey remains:

- status: `registration_open`
- capacity: `30`
- confirmed participant rows: `1`
- confirmed people: `1`
- accepted pending rows: `0`
- accepted pending people: `0`
- semantic drift rows: `0`
- over-capacity Journeys: `0`
- `pg_graphql`: OFF

Attendance remains **unresolved**, not zero-attendance:

- no attendance observation has yet been recorded for the real event;
- no verified no-show has been fabricated.

The real event remains scheduled for `2026-09-11`.

---

## 8. Operational consequence

The live pilot can continue accepting/reviewing/confirming legitimate registrations while `registration_open`.

Confirmation safety is now enforced twice:

1. operator/UI projection for usability;
2. database transaction serialization for authority.

If the final slot is contested by concurrent admin confirmations, only a confirmation that still fits the authoritative capacity can commit.

When Owner/operator intentionally closes registration:

1. resolve every `accepted` application to confirmed or rejected;
2. verify confirmed people do not exceed capacity;
3. transition `registration_open -> preparing` as admin;
4. after cutoff, do not attempt new positive application progression or new confirmations.

WU11 does **not** auto-transition the pilot.

---

## 9. Residual risks / non-goals

1. WU11 does not fabricate a production race test. Transaction semantics were validated in isolated DB QA and production catalog/postflight checks; no fake production registrations were created.
2. The existing Supabase Auth leaked-password-protection warning remains outside WU11 scope.
3. Existing performance-advisor warnings remain outside WU11 scope; WU11 added no index or RLS policy regression.
4. Email remains OFF.
5. Turnstile remains OFF.
6. `pg_graphql` remains OFF.
7. WU11 does not automatically close registration at capacity or by date; cutoff remains an Owner/operator lifecycle decision.
8. WU11 does not replace WU9 attendance truth or WU10 post-event evidence/impact closeout.

---

## 10. Phase 11 gate after WU11

**P11-WU11 — COMPLETE / PASS.**

**P11-WU6 — LIVE PILOT OPERATIONS remains ACTIVE.**

There is no need to wait until `2026-09-11` to perform unrelated safe readiness work, but the system must not fabricate attendance/evidence/impact or prematurely complete the Journey.

The next operational lifecycle gate for this pilot remains:

`registration_open -> preparing`

only when Owner/operator intentionally closes registration and the WU11 cutoff gate passes.

After the real event, WU9 attendance capture and WU10 post-event evidence/impact closeout become the authoritative workflows before any `preparing -> completed` transition.
