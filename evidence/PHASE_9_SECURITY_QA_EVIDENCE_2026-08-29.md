# TRẠM NỤ CƯỜI — PHASE 9 SECURITY QA EVIDENCE

Date: 2026-08-29  
Scope: P9-WU1 through P9-WU7 evidence supporting P9-WU8 closeout

## 1. Source snapshots

Product repo: `huynhtranhuythinh/tramnucuoi`

Production:

- `main`: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

Phase 9 development:

- `develop`: `c072296bb209ecfba04d4bdc89dddb57543fe275`
- tree: `d929aa27f5cf3eedb46489e4641112fa7b9bf694`

Compare state:

- merge base: `03512ac8189ba9cbb77b9a02afcb37d037cc75ee`;
- histories diverged;
- no blind broad merge is safe.

## 2. Phase 9 source delta

The Phase 9 delta includes:

- migrations 0026/0027/0028;
- full Phase 9 rollback SQL;
- destructive CI QA scripts;
- public Journey registration form changes;
- conditional Turnstile component;
- registration and activation server clients;
- request-scoped Cloudflare runtime environment;
- HMAC replay/dedupe logic;
- protected registration server write path;
- privacy-safe email logging;
- abuse telemetry;
- 32 KiB pre-parser body ceiling;
- hard/soft rate limiting;
- server-side Turnstile validation;
- Admin controlled activation;
- Cloudflare Rate Limiting and Analytics Engine bindings.

## 3. Final CI evidence

GitHub Actions run: `33226764293`

Result: **PASS**

Verified steps:

- dependency install: PASS;
- P9-WU7 source abuse-protection QA: PASS;
- P9-WU7 ephemeral PostgreSQL gate + rollback QA: PASS;
- TypeScript typecheck: PASS;
- production build: PASS;
- Cloudflare configuration dry-run: PASS.

The workflow did not deploy a Worker and did not connect to or mutate canonical production Supabase.

## 4. Request-body abuse evidence

Verified:

- only POST `registerForJourney` receives the public body ceiling;
- exactly 32 KiB passes;
- 32 KiB + 1 fails;
- declared oversized body fails;
- compressed registration request fails;
- body measurement occurs before TanStack payload parsing;
- cloned-stream cancellation does not block the 413 decision.

## 5. Duplicate/replay evidence

Verified deterministic behavior:

- same normalized payload in same UTC day -> same replay material;
- changed payload -> changed replay key;
- same normalized email in same 10-minute bucket -> same contact-window key;
- same contact/day -> same notification event identity;
- next bucket rotates the contact key;
- next UTC day rotates replay and notification identity;
- different Journey rotates Journey-scoped material.

Database QA also verified replay/contact uniqueness constraints.

## 6. Data API bypass evidence

Ephemeral PostgreSQL destructive QA verified:

- missing registration gate -> denied;
- invalid registration gate -> denied;
- valid gate -> pre-request check passes while normal RLS remains authoritative;
- alternate path such as `rpc/graphql` -> denied by 0027 REST-path RLS hardening.

Read-only production verification confirmed `pg_graphql` is currently disabled.

## 7. Activation evidence

Ephemeral database QA verified:

- admin without activation gate -> denied;
- editor with valid activation gate -> denied;
- admin + valid activation gate -> permits controlled `draft -> registration_open`;
- closing `registration_open -> draft` does not require the activation gate.

This preserves fail-safe emergency closure.

## 8. Rollback evidence

Canonical rollback file:

`db/rollbacks/phase_9_journey_registration_protection.sql`

Verified:

- rollback refuses to proceed while a Journey is open;
- reverse order `0028 -> 0027 -> 0026` works;
- activation trigger/witness are removed;
- replay/contact constraints and columns are removed;
- registration pre-request gate is removed;
- PostgREST configuration is reset;
- no open Journey remains after QA.

## 9. Turnstile evidence

Source QA verified:

- server-side Siteverify;
- expected action validation;
- hostname validation;
- no `remoteip` field in the verification payload;
- 10-second timeout;
- hard limits execute before soft challenge escalation;
- enabled-but-misconfigured Turnstile fails closed.

Production Turnstile remains OFF.

## 10. Email amplification/privacy evidence

Verified:

- deterministic idempotency identities for application receipt/admin notification/participant confirmation;
- duplicate/replay reuses the same daily notification event identity;
- disabled logs do not emit subject or recipient;
- provider rejection logs do not echo provider text that may contain contact data;
- network failure logs are generic.

Production `EMAIL_DELIVERY_ENABLED=false`.

## 11. Production read-only closeout snapshot

Supabase project: `iwiqprhoohkxvjyxojto`

Observed:

- `pgrst.db_pre_request = NULL`;
- Journey application count = `0`;
- open Journey count = `0`;
- replay column applied = `false`;
- activation guard applied = `false`;
- `pg_graphql` enabled = `false`.

Interpretation:

- 0026/0027/0028 remain unapplied;
- no production registration activation occurred in Phase 9;
- production database stayed unchanged.

## 12. Evidence conclusion

**P9-WU7 Abuse / Rollback QA: COMPLETE / PASS.**

Evidence supports **P9-WU8 Canonical Closeout: COMPLETE / PASS** while Phase 9 production deployment and real Journey activation remain on HOLD pending separate Owner authorization.
