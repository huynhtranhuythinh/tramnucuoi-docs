# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.1 — LIFECYCLE SCHEMA & SOURCE CONTRACT

Date: 2026-09-22  
Status: **SOURCE CONTRACT IMPLEMENTED / PR OPEN / CI PENDING**  
Production database mutation: **NONE**  
Runtime / feature flag mutation: **NONE**  
Public recruitment: **HOLD**

## 1. Objective

P17-WU2.1 defines the new Journey lifecycle authority without yet changing production behavior.

Canonical separation:

### Journey lifecycle phase

- `draft`
- `upcoming`
- `active`
- `closeout_pending`
- `memory`
- `archived`

### Application state

- `closed`
- `open`
- `paused`

The current legacy `journeys.status` remains in place for compatibility during the WU2 rebase. It is not dropped or mutated as an enum in WU2.1.

## 2. Production truth inspected

Production Supabase project:

`iwiqprhoohkxvjyxojto`

Current legacy Journey status enum:

- draft
- registration_open
- preparing
- completed
- archived

Current submission RLS still authorizes a new application when:

- parent Journey `status='registration_open'`;
- application mode matches;
- replay/contact-window anti-abuse keys are valid;
- all existing anti-injection constraints pass.

Current application/participant workflow also binds positive review/confirmation and capacity confirmation to legacy `registration_open`.

Current post-event dependencies bind several operations to legacy `completed` / `preparing`, including:

- closeout review;
- closeout completion gate;
- impact verification/public visibility;
- reflection opening;
- public Journey visibility;
- source/UI registration and attendance helpers.

Therefore lifecycle separation is not a two-column rename. The downstream dependency ledger is explicit and must be migrated before production cutover.

## 3. Data type decision

WU2.1 uses:

- `lifecycle_phase text + CHECK`
- `application_state text + CHECK`

instead of new PostgreSQL enums.

Reason:

- Phase 17 is still recomposing Journey operations;
- the value set is controlled by database constraints;
- text + CHECK keeps future additive changes/rollback simpler;
- there is no truth/safety advantage from introducing another enum at this stage;
- the legacy enum remains untouched for compatibility.

## 4. Conservative bootstrap contract

Existing Journey rows map only from their explicit legacy state.

No dates are consulted.

| Legacy status | New lifecycle phase |
| --- | --- |
| draft | draft |
| registration_open | upcoming |
| preparing | active |
| completed | closeout_pending |
| archived | archived |

Important:

**legacy `completed` does not bootstrap directly to `memory`.**

Memory is a stronger Phase 17 state that must remain downstream of manual Closeout authority.

For every existing Journey:

`application_state = closed`

This is intentionally fail-closed.

A past `registration_open` Journey therefore does not remain recruitable merely because of legacy status.

## 5. Lifecycle / application-state invariant

Database-level invariant:

> `open` or `paused` application state is valid only while `lifecycle_phase='upcoming'`.

All other phases require:

`application_state='closed'`

This is enforced by a CHECK constraint independently from UI behavior.

## 6. Manual transition contract

Allowed phase transitions:

- `draft → upcoming`
- `draft → archived`
- `upcoming → draft`
- `upcoming → active`
- `upcoming → archived`
- `active → closeout_pending`
- `closeout_pending → active`
- `closeout_pending → memory`
- `memory → archived`

Same-state writes are harmless.

Not allowed by the contract include:

- `draft → memory`
- `upcoming → memory`
- `active → memory`
- `memory → active`
- `archived → upcoming`

State mutation authority:

- ordinary editor may create the default `draft / closed` record;
- lifecycle/application-state changes are Admin-authoritative;
- non-default lifecycle/application state at insert requires Admin authority.

There is **no date-derived transition trigger**.

## 7. Phase capability contract

### New applications

Allowed only when:

`lifecycle_phase='upcoming' AND application_state='open'`

### Existing application workflow

Closing/pausing new submissions must not strand already-submitted applications.

Positive review/confirmation is conceptually allowed while:

- upcoming;
- active.

The application window controls **new submissions**, not the existence of already-submitted operational work.

### Attendance

The new phase contract recognizes attendance/evidence correction during:

- active;
- closeout_pending;
- memory.

The existing separate event-date authority remains mandatory and unchanged:

- attendance cannot be recorded before Journey start date;
- NULL / 0 / >0 semantics remain intact.

### Closeout

Closeout review belongs to:

- closeout_pending.

### Memory / Reflection / public impact

Memory/publication capability belongs to:

- memory.

`closeout_pending` must not be treated as Memory merely because the event date passed.

## 8. Source implementation

Product branch:

`p17-wu2-1-lifecycle-schema-source-contract`

PR:

`#67 — P17-WU2.1: Journey lifecycle schema and source contract`

Base:

`f78fabb329ab994510f9325ce644a30386b38673`

New source contract:

`src/lib/journeys/lifecycle.ts`

It defines:

- lifecycle/application constants and types;
- conservative legacy bootstrap;
- state compatibility rules;
- transition matrix;
- new-application predicate;
- existing-application progress predicate;
- attendance phase predicate;
- closeout predicate;
- Memory publication predicate;
- temporary legacy-status projection for downstream migration.

## 9. Database contract fixture

Source:

`database/contracts/p17_wu2_1_journey_lifecycle_contract.sql`

This file is intentionally marked:

**NON-PRODUCTION CONTRACT FIXTURE — NOT A MIGRATION**

It defines and tests the intended additive database contract without writing production migration history.

It:

- adds lifecycle/application-state columns;
- adds CHECK constraints;
- bootstraps legacy state conservatively;
- bootstraps all existing application state closed;
- adds Admin-authoritative transition guard;
- keeps legacy status intact;
- performs no date-derived state mutation.

Because the current execution environment does not provide a working Supabase CLI, WU2.1 does **not invent a production migration filename**.

At the explicit production gate, the canonical migration must be generated through the Supabase migration workflow, reconciled against this contract, tested, advisor-reviewed, and only then considered for production.

## 10. QA

Source QA:

`scripts/p17-wu2-1-lifecycle-contract-qa.ts`

Ephemeral PostgreSQL QA:

`scripts/p17-wu2-1-lifecycle-schema-qa.sql`

The QA verifies:

- exact lifecycle/application value sets;
- conservative legacy mapping;
- all existing application state closes;
- phase/application-state combination constraint;
- Admin lifecycle authority;
- editor default draft creation remains possible;
- invalid transitions fail;
- explicit close before upcoming→active;
- dates do not mutate lifecycle;
- legacy enum is not altered/dropped;
- legacy status column remains;
- no direct Memory bootstrap.

Both QA gates are wired into generic CI while preserving all inherited P9–P16 gates.

## 11. Downstream dependency ledger

The production cutover must not happen until the following legacy-status couplings are migrated.

### WU2.2 / WU2.3

- public Journey read visibility;
- public Journey index/detail source queries;
- public status grouping and labels;
- application CTA/open-state logic.

### WU2.5

- Journey admin state controls;
- application submit RLS;
- application workflow guard;
- participant/capacity confirmation guard;
- registration cutoff gate;
- capacity mutation authority.

### Closeout / Memory compatibility

Before production cutover, lifecycle-aware equivalents must replace legacy checks in:

- closeout review PASS guard;
- Journey closeout transition gate;
- impact verification;
- public impact policies;
- Journey reflection opening;
- attendance UX gating.

The underlying evidence models themselves are not rewritten.

## 12. Explicit non-scope / invariants preserved

WU2.1 does not change:

- application status workflow values;
- participant status;
- attendance evidence columns or semantics;
- verified-email claim;
- Memory eligibility calculation;
- Reflection source/publication separation;
- shared-experience graph;
- social identity/consent;
- Donation/resources;
- Department/Team staffing;
- routes;
- public navigation;
- production flags;
- Cloudflare runtime.

## 13. Production gate

No production DDL is authorized by WU2.1 source completion alone.

Before any production migration:

1. WU2 downstream source has moved off legacy lifecycle authority;
2. a canonical migration is generated by the Supabase migration workflow;
3. migration SQL is reconciled against this WU2.1 contract;
4. ephemeral DB QA PASS;
5. full inherited CI PASS;
6. current production truth is re-read;
7. Security/Performance Advisors are reviewed;
8. Owner approves the production migration sequence.

Public recruitment remains **HOLD**.
