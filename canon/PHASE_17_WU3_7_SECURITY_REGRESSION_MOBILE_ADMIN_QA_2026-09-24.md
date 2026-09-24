# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.7 — SECURITY / REGRESSION / MOBILE ADMIN QA

Date: 2026-09-24  
Status: **COMPLETE / PASS — SECURITY / REGRESSION / MOBILE ADMIN QA CLOSED**

## 1. Objective

Verify and harden the P17-WU3 Event Management foundation before WU3.Final production DDL.

WU3.7 owns verification/hardening only:

- RLS / grants;
- PII leakage boundaries;
- lifecycle / application / attendance / Memory regressions;
- recruitment invariants;
- mobile Admin composition;
- inherited source / DB / build / typecheck / Cloudflare gates.

WU3.7 does not create or deploy the four WU3 Event Management tables.

## 2. Starting truth

Product base:

`b47033de09b5cb642bc45100d32720e474e0c969`

Parent WU3 state:

WU3.0 through WU3.6 COMPLETE / PASS.

Production before WU3.7:

- Event Management tables: 0/4;
- non-closed application windows: 0;
- Journeys: 5;
- recruitment: HOLD / CLOSED.

## 3. Supabase security baseline

Current Supabase documentation / platform guidance used by this audit:

- exposed-schema tables must use RLS;
- privileges must be explicit and least-privilege;
- authorization must not depend on user-editable metadata;
- security-definer objects in exposed API surfaces require special scrutiny;
- security-invoker views/functions are preferred where privileged execution is not required.

The WU3 Event Management source contract already follows this direction:

- explicit grants;
- RLS enabled;
- Admin-only policies via `private.has_role('admin'::public.app_role)`;
- no authorization from `raw_user_meta_data` / `user_metadata`;
- no service-role secret in browser source.

## 4. Production sensitive-table audit

Production audit confirmed RLS is enabled for:

- `journey_applications`;
- `journey_participants`;
- `journey_closeout_reviews`.

Sensitive read/update/delete policies remain Admin-gated.

Legacy table privileges for `anon` / `authenticated` exist on some tables, but row visibility remains controlled by RLS policy. In particular:

- `journey_applications` has public/signed-in INSERT policies for protected registration;
- there is no anonymous SELECT policy exposing application rows;
- operational SELECT/UPDATE/DELETE remains Admin-only.

## 5. Security advisor finding

Supabase security advisor found:

`authenticated_security_definer_function_executable`

for:

`public.tnc_reveal_volunteer_identity_document(uuid)`

The function was:

- `SECURITY DEFINER`;
- callable by authenticated users;
- self-authorized with `auth.uid()` + `private.has_role('admin')`;
- `search_path=''`;
- public/anon execute revoked.

The original design was functionally Admin-only, but the advisor correctly identified that privileged implementation lived in the exposed `public` API schema.

WU3.7 hardened this rather than documenting an exception.

## 6. Identity reveal RPC hardening

Added:

`database/migrations/0056_p17_wu3_7_identity_reveal_rpc_hardening.sql`

and rollback:

`database/rollbacks/p17_wu3_7_identity_reveal_rpc_hardening.sql`

New architecture:

### Public API wrapper

`public.tnc_reveal_volunteer_identity_document(uuid)`

is now:

- `SECURITY INVOKER`;
- stable API name for existing frontend RPC call;
- executable by authenticated only;
- public / anon execute revoked.

### Privileged helper

`private.tnc_reveal_volunteer_identity_document(uuid)`

is:

- outside exposed public schema;
- `SECURITY DEFINER`;
- `search_path=''`;
- self-authorized:
  - `auth.uid() is not null`;
  - `private.has_role('admin')`;
- public / anon execute revoked.

The frontend contract remains unchanged:

`src/lib/journeys/volunteer-admin.ts`

still calls:

`rpc("tnc_reveal_volunteer_identity_document", ...)`

No full identity number was added to ordinary application projections.

## 7. Production hardening verification

Migration 0056 was applied only after:

- source merged to main;
- exact-head generic CI PASS;
- dedicated P16-WU10B PASS.

Production migration record:

`p17_wu3_7_identity_reveal_rpc_hardening`

Production function verification:

### private helper
- schema: `private`;
- SECURITY DEFINER: true;
- anon execute: false;
- authenticated execute: true;
- Admin guard remains inside function.

### public wrapper
- schema: `public`;
- SECURITY DEFINER: false;
- anon execute: false;
- authenticated execute: true.

Post-cutover Supabase security advisor no longer reports:

`authenticated_security_definer_function_executable`.

The remaining security advisor warning is:

`auth_leaked_password_protection`

This is an account/Auth configuration warning that predates and is independent of WU3 Event Management. It is not caused by WU3.7 and does not weaken the WU3 operational table contract.

## 8. Mobile Admin audit

Static responsive audit covered:

- Journey Control Center;
- Existing Journey Admin composition;
- Department / Team;
- Runbook;
- Official Update;
- Lifecycle controls;
- application / volunteer workspace;
- Field Update;
- media;
- impact;
- Field Journal;
- Closeout;
- Admin shell.

The major forms already use mobile-first single-column layouts with multi-column expansion at `sm/md/lg`.

Two narrow-screen layout issues were found and fixed:

### Control Center metrics

Before:
- forced two-column metrics at the narrowest viewport.

After:
- one column by default;
- two columns from `sm`.

### Closeout metrics

Before:
- several metric groups forced 2–3 columns on the narrowest viewport.

After:
- one column by default;
- three columns from `sm`.

No new responsive navigation system or mobile-specific business logic was introduced.

## 9. Regression invariants verified

WU3.7 confirms:

- Journey Role != Department != Team != Task != Skill;
- account != participant;
- application != participant;
- participant != attendance;
- attendance NULL remains unresolved truth;
- Memory remains gated by Closeout authority;
- date does not become lifecycle authority;
- Application OPEN remains behind protected activation;
- Editor != Journey-scoped BTC authority;
- P16 `preferred_team` / `assigned_team` are not reinterpreted as WU3 structure;
- Field Update remains distinct from Official Update;
- Official Update remains distinct from Community;
- Impact is not inferred from applications / participants / attendance;
- no Event Management table is created by WU3.7;
- no production recruitment activation occurs.

## 10. WU3.7 QA

Added:

`scripts/p17-wu3-7-security-regression-mobile-qa.ts`

The aggregate gate verifies:

- WU3 RLS/grant contract;
- no user-editable metadata authorization;
- identity reveal RPC architecture;
- explicit rollback;
- frontend RPC compatibility;
- deliberate identity reveal UI;
- Admin-only restricted operations;
- lifecycle/application authority;
- Memory closeout gate;
- no attendance fabrication;
- no legacy team reinterpretation;
- mobile-first Control Center / Closeout metrics;
- responsive WU3 forms;
- WU3.7 migration cannot create/alter the four Event Management tables;
- WU3.7 migration cannot open application windows or alter lifecycle phase.

P16-WU10B QA was also extended so the existing volunteer security suite now proves migration 0056 behavior.

## 11. Exact-head evidence

Branch:

`p17-wu3-7-security-regression-mobile-qa`

PR:

`#84 — P17-WU3.7: Security regression and mobile Admin QA`

Final PR head:

`e618311a37ddd0f6fd71f84ce9e9b164fd10f596`

Exact-head gates:

- generic CI `36000781333`: **SUCCESS**
- dedicated P16-WU10B gate `36000781342`: **SUCCESS**
- P17-WU3.7 aggregate QA: **PASS**
- P16-WU10B source/privacy QA: **PASS**
- P16-WU10B ephemeral Vault/RLS QA: **PASS**
- WU3.1 schema contract QA: **PASS**
- WU3.2–WU3.6 inherited source QA: **PASS**
- all inherited ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 12. Merge / post-merge evidence

PR #84 was squash-merged.

Product main:

`5a31cab2378a63bed6e541cbf8dcfe3a4396c208`

Post-merge main CI:

- run `36000958750`;
- exact head `5a31cab2378a63bed6e541cbf8dcfe3a4396c208`;
- conclusion: **SUCCESS**.

## 13. Production invariants after WU3.7

Production Supabase:

`iwiqprhoohkxvjyxojto`

Verified after migration 0056:

- `journey_departments`: absent;
- `journey_teams`: absent;
- `journey_runbook_items`: absent;
- `journey_official_updates`: absent;
- Event Management tables: **0/4**;
- non-closed application windows: **0**;
- Journeys: **5**;
- recruitment: **HOLD / CLOSED**.

No Event Management production DDL was deployed in WU3.7.

## 14. Decision

**P17-WU3.7 — COMPLETE / PASS.**

WU3 source / security / regression / mobile readiness is now closed.

Next:

**P17-WU3.Final — PRODUCTION FOUNDATION CUTOVER & CANONICAL CLOSEOUT**

Production Event Management DDL remains a separate final gate.
