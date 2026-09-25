# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU7.1 — RESOURCE NEED SOURCE CONTRACT

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE CONTRACT MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Establish the canonical Journey-scoped Resource Need source contract required by P17-WU7 without applying production DDL.

Canonical truth boundary:

`Need != Pledge != Received != Distributed != Contribution != Impact`

WU7.1 owns only **Resource Need**.

## 2. Product source truth

Product repository:

`huynhtranhuythinh/tramnucuoi`

Base production-main before WU7.1:

`42ae483b3bd81b318a793c263c11fc6d3a9f1a8f`

Branch:

`p17-wu7-1-resource-need-source-contract`

PR:

`#115 — P17-WU7.1: Resource Need source contract`

Final PR exact head:

`aa3641d2e8affc54d0d1d3fd70302bb6eb55c612`

Squash-merged production-main SHA:

`c715f7eda59a3672b438a3f5f64acc81d4b39c18`

Source contract:

`database/contracts/p17_wu7_1_resource_need.sql`

Source QA:

`scripts/p17-wu7-1-resource-need-source-qa.ts`

Ephemeral PostgreSQL QA:

`scripts/p17-wu7-1-resource-need-db-qa.sql`

## 3. Canonical Resource Need model

Defines planned table:

`public.journey_resource_needs`

Resource Need is Journey-scoped and contains:

- Journey;
- category;
- VI / EN title;
- VI / EN description;
- requested quantity;
- VI / EN unit;
- visibility;
- open/closed state;
- optional target date;
- internal operational note;
- sort order;
- created/updated audit fields.

Initial lightweight categories:

- `in_kind`
- `service`
- `transport`
- `food`
- `equipment`
- `venue`
- `other`

No charity-specific commodity taxonomy is hardcoded.

Requested quantity must be positive.

## 4. Truth boundaries

WU7.1 does NOT create or reinterpret:

- Pledge;
- Receipt;
- Distribution;
- cash/payment transaction;
- community contribution;
- impact result;
- participant;
- attendance;
- Memory;
- Shared Journey evidence;
- volunteer application state.

There is no Campaign object.

Cash-support architecture remains external official channel only and is not modeled as a quantity-bearing Resource Need transaction.

## 5. Public/private visibility architecture

Operational source table remains non-public.

Direct source grants:

- authenticated: SELECT / INSERT / UPDATE;
- service_role: full;
- anon: no direct table grant;
- no normal DELETE grant.

RLS source authority:

- Admin read;
- Admin insert;
- Admin update.

Normal hard delete is intentionally absent. Resource Needs are closed rather than casually deleted so later Pledge / Receipt / Distribution truth can retain a valid source reference.

## 6. Audit guard

Private trigger guard:

`private.tnc_guard_journey_resource_need()`

Properties:

- SECURITY INVOKER;
- fixed `search_path = ''`;
- Admin required;
- server-derives `created_by` and `updated_by`;
- immutable:
  - id;
  - Journey identity;
  - created_by;
  - created_at.

Global Editor is not Resource Need operational authority.

## 7. Public-safe lifecycle gate

Private helper:

`private.tnc_journey_resource_support_publicly_visible(uuid)`

Public Resource Need visibility is allowed only when Journey lifecycle is:

- `upcoming`; or
- `active`.

This gate is intentionally independent from volunteer recruitment.

Therefore:

> Resource support may be public while Application Window remains CLOSED.

No dependency on:

`application_state = 'open'`

is introduced.

## 8. Public-safe projection

Public wrapper:

`public.tnc_public_journey_resource_needs(uuid)`

It exposes only:

- Need ID;
- Journey ID;
- category;
- VI / EN title;
- VI / EN description;
- requested quantity;
- VI / EN unit;
- target date;
- sort order.

It requires:

- Need visibility = `public`;
- Need is open;
- Journey lifecycle public-resource gate passes.

It does not expose:

- internal note;
- audit actors;
- donor identity;
- Pledge;
- Receipt;
- Distribution;
- payment/accounting data;
- Impact;
- Attendance;
- Memory.

## 9. Data API / RLS compatibility

WU7.1 uses explicit grants plus RLS rather than relying on Supabase default table exposure.

This is compatible with the current Supabase direction where new public-schema tables require explicit grants for Data API exposure, while RLS remains a separate row-authorization layer.

The source contract therefore keeps exposure deliberate and reviewable.

## 10. Exact-head QA evidence

PR exact head:

`aa3641d2e8affc54d0d1d3fd70302bb6eb55c612`

PR runs:

- generic CI `36144432683` — **SUCCESS**
- dedicated P16-WU10B Volunteer Pilot Gate `36144432650` — **SUCCESS**

Exact-head CI proves:

- WU7.1 source contract QA: PASS;
- WU7.1 ephemeral PostgreSQL DB QA: PASS;
- all inherited source gates: PASS;
- inherited DB regressions: PASS;
- build: PASS;
- typecheck: PASS;
- Cloudflare dry-run: PASS.

WU7.1 DB QA specifically proves:

- Editor cannot manage Resource Needs;
- Admin audit identity is server-derived;
- Resource Need identity/Journey source is immutable;
- invalid zero quantity is rejected;
- public projection exposes only explicit public/open Needs;
- internal Needs remain hidden;
- Draft and Closeout-Pending Journeys do not expose live Resource Needs;
- Upcoming and Active Journeys may expose public Needs;
- Application Window remains CLOSED while public Resource Need visibility works;
- closing a Need hides it without deleting operational history;
- no attendance truth is manufactured;
- Memory sentinel remains unchanged.

## 11. Merge / post-merge verification

PR #115 was squash-merged.

Product main:

`c715f7eda59a3672b438a3f5f64acc81d4b39c18`

Post-merge main CI:

`36144643347` — **SUCCESS**

Post-merge CI again passed:

- WU7.1 source QA;
- WU7.1 DB QA;
- inherited regressions;
- build;
- typecheck;
- Cloudflare dry-run.

## 12. Production database state

WU7.1 performs **no production Supabase migration**.

Production verification after merge:

- `public.journey_resource_needs`: **ABSENT**
- non-closed Application Windows: **0**

Therefore no WU7 production DDL was smuggled into production.

Recruitment remains:

**HOLD / CLOSED**

No fake Resource Need rows exist.

## 13. Decision

# **P17-WU7.1 — COMPLETE / CLOSED / PASS**

Proceed to:

# **P17-WU7.2 — PLEDGE / DONOR INTENT CONTRACT**

WU7.2 may define donor intent and BTC review semantics, but production cutover remains owned by WU7.Final.
