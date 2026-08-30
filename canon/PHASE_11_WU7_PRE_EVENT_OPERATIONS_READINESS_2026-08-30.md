# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 11 — REAL JOURNEY OPERATIONS
# P11-WU7 — PRE-EVENT OPERATIONS READINESS

Date: 2026-08-30  
Owner: TRẠM NỤ CƯỜI  
CTO / Product Architect: ChatGPT  
Builder: Lovable

## STATUS

**P11-WU7: COMPLETE / PASS**

This work unit prepares the first real Journey pilot for safe pre-event operations without changing the live Journey status, enabling Email, enabling Turnstile, fabricating impact, or marking the Journey completed.

## PILOT

- Journey ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- Slug: `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`
- Event date: `2026-09-11`
- Capacity: `30`
- Current operational state at WU7 start: `registration_open`

## WU7 INVARIANTS

- Only the pilot Journey may remain `registration_open` unless Owner separately approves another real Journey.
- Manual staff review is mandatory.
- No auto-accept.
- No auto-confirm.
- Email remains OFF unless separately approved.
- Turnstile remains OFF unless separately approved.
- No impact number may be fabricated.
- `completed` is forbidden before post-event evidence is verified.
- Khe Chữ impact completion is outside this work unit.

## CAPACITY OPERATIONS

Capacity is measured in **people**, using `party_size`, not by counting application or participant rows.

Admin operational view now presents:

- total applications;
- confirmed people;
- Journey capacity;
- accepted-but-not-yet-confirmed people;
- remaining capacity;
- capacity risk state.

### Capacity thresholds

- `< 70%` confirmed: **NORMAL / BÌNH THƯỜNG**
- `>= 70% and < 90%`: **WATCH / CẦN THEO DÕI**
- `>= 90% and < 100%`: **NEAR CAPACITY / GẦN ĐỦ CHỖ**
- `>= 100%`: **FULL / DỪNG NHẬN ĐĂNG KÝ**

For the 30-person pilot these correspond operationally to:

- 0–20 confirmed people: NORMAL
- 21–26: WATCH
- 27–29: NEAR CAPACITY
- 30: FULL / stop registration

Accepted-but-not-yet-confirmed `party_size` is treated as **soft capacity liability** and must be reviewed before confirming more applications.

## MANUAL APPLICATION WORKFLOW

Staff UI is constrained to the intended sequence:

`submitted → reviewing → accepted → confirmed`

Allowed actions:

- `submitted`: XEM XÉT or TỪ CHỐI
- `reviewing`: DUYỆT ĐĂNG KÝ or TỪ CHỐI
- `accepted`: CHỐT THAM GIA or TỪ CHỐI
- `rejected`: no accept/confirm shortcut
- `confirmed`: no workflow mutation action

No automatic workflow transitions were introduced.

## OVER-CAPACITY OPERATOR GUARD

Before showing CHỐT THAM GIA, Admin calculates:

`projected_confirmed_party_size = confirmed_party_size + application.party_size`

Rules:

- projected `< capacity`: confirmation allowed;
- projected `= capacity`: confirmation allowed;
- projected `> capacity`: confirmation action is blocked in the Admin UI and the operator is shown the reason.

The Journey status is never changed automatically by this guard.

### Residual concurrency note

WU7 intentionally makes no production database migration. The capacity check is therefore an operator/UI guard, not a transactional database capacity lock. Two admins confirming different applications concurrently could theoretically race against the same displayed capacity snapshot.

Operational mitigation for this pilot:

- only one staff member should perform final confirmation at a time when near capacity;
- refresh/reload the application state immediately before final confirmations in WATCH/NEAR CAPACITY;
- at 30 confirmed people, close registration before any further review/confirmation mutation.

A transactional database-level capacity guard may be introduced later as a separately reviewed hardening change if required.

## REGISTRATION-OPEN → PREPARING GATE

Do **not** transition merely because the calendar reaches a fixed date. Transition to `preparing` only when registration is intentionally closed for operational reasons.

Acceptable triggers include:

1. capacity reaches 30 confirmed people;
2. Owner explicitly closes registration;
3. staff declares the operational registration cutoff before the event;
4. anomaly response requires closing registration.

Before changing to `preparing`, verify:

- pilot is the only open Journey;
- application status counts are known;
- confirmed `party_size` total is known;
- accepted pending liability is known;
- no over-capacity state exists;
- duplicate/replay integrity is clean;
- suspicious applications are resolved or held;
- Phase 9 protection essentials remain intact;
- Email state has not been changed without approval;
- Turnstile state has not been changed without approval;
- staff has a confirmed participant roster;
- event logistics/contact instructions are ready.

After the transition, public registration must no longer accept new applications.

## PRE-EVENT CHECKLIST

### Before operational cutoff

- Review new applications manually.
- Confirm identity/contact plausibility without collecting unnecessary data.
- Review duplicate/replay signals.
- Track confirmed party size, not row count.
- Track accepted pending party size.
- Watch for sudden registration bursts or repeated suspicious patterns.
- Keep only this Journey open.

### At WATCH / NEAR CAPACITY

- Re-read the latest application and participant counts before each final confirmation.
- Resolve accepted applications deliberately; do not create a confirmation backlog larger than remaining capacity.
- Do not increase capacity casually to fit excess registrations; Owner decision is required for material scope change.

### Emergency close

If anomaly or protection uncertainty occurs:

1. CLOSE registration first.
2. Keep Email OFF.
3. Keep Turnstile OFF unless separately approved.
4. Preserve privacy-safe evidence.
5. Diagnose.
6. Stop further mutation if protection state is uncertain.

## EVENT-DAY READINESS

On or immediately before `2026-09-11`:

- confirm Journey is `preparing` once registration has intentionally closed;
- retain the final confirmed roster;
- capture attendance as verified operational evidence, not assumed registration attendance;
- create Field Updates only from real observations;
- upload documentary media only with applicable rights/consent/trust handling;
- link evidence to the correct Journey;
- do not create final impact metrics during the event unless evidence is already verifiable.

## POST-EVENT HANDOFF

After the event, the next evidence workflow may include:

- Field Updates;
- documentary media;
- evidence classification;
- impact snapshot;
- verified impact items/metrics;
- final review;
- only then `completed`.

Registration totals and attendance/impact totals must remain distinct. A confirmed registration is not automatically proof of attendance or impact.

## SOURCE IMPLEMENTATION

Canonical product `main` changes for WU7:

1. `src/components/admin/journeys/application-manager.tsx`
   - party-size capacity accounting;
   - accepted pending liability;
   - capacity risk display;
   - strict manual action sequence;
   - projected over-capacity confirmation guard.

2. `src/routes/_authenticated/admin.journeys.tsx`
   - passes Journey `capacity` into `ApplicationManager`.

3. `src/lib/journeys/admin-queries.ts`
   - defensive `confirmApplication()` precondition: only `accepted` or already-`confirmed` applications can enter the confirmation path;
   - preserves idempotency for already-confirmed rows.

Initial canonical source commits:

- `186a29a2dd1db6d2fabb66914d91380c49c4b880`
- `4518eaf65518a1a470b232bbfcc544bbf8a3808a`

### Canonical parity repair discovered during P11-WU8 preflight

During WU8 preflight, CTO verification found that the Builder implementation of the WU7 defensive `confirmApplication()` precondition had not been copied into canonical product `main`, although the WU7 UI controls were already canonical. This was a source-parity omission, not a production-data failure.

The missing guard was restored to canonical `main` in commit:

- `b5f74a77c5edb7df83d9829eba283ced029e6dad`

That commit also carries the WU8 Field Update defensive publish guard in the same file; the WU7 behavior itself is unchanged from the previously reviewed Builder implementation.

Builder QA on the intended WU7 implementation:

- `bun run typecheck`: PASS
- `bun run build`: PASS

An accidental generated Supabase type-version change produced during Builder work was detected during CTO diff review and explicitly reverted; it was **not** carried into canonical product `main`.

## PRODUCTION MUTATION STATEMENT

WU7 source/readiness work does not require and did not intentionally perform:

- Journey status mutation;
- application/participant mutation;
- Supabase schema migration;
- protection-stack mutation;
- Email enablement;
- Turnstile enablement;
- Cloudflare runtime mutation;
- impact data mutation.

P11-WU6 remains the live operational monitoring workstream while WU7 supplies the pre-event operating controls and transition gate.
