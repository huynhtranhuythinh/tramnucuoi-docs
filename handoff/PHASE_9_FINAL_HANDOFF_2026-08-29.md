# TRẠM NỤ CƯỜI — PHASE 9 FINAL HANDOFF

Date: 2026-08-29  
Phase: 9 — Journey Registration Abuse Protection & Safe Activation

## Current status

- P9-WU1: COMPLETE / PASS
- P9-WU2: COMPLETE / PASS
- P9-WU3: COMPLETE / PASS
- P9-WU4: COMPLETE / PASS
- P9-WU5: COMPLETE / PASS
- P9-WU6: COMPLETE / PASS
- P9-WU7: COMPLETE / PASS
- P9-WU8: COMPLETE / PASS

## Canonical source

Product repo: `huynhtranhuythinh/tramnucuoi`

Production:

- branch `main`
- commit `7c3f139e0d8660052c988461d11abd4ed52e08b6`

Phase 9 development:

- branch `develop`
- commit `c072296bb209ecfba04d4bdc89dddb57543fe275`

`main` and `develop` are diverged. Do not perform a blind merge.

## Canonical production runtime

- domain: `https://tramnucuoi.com`
- Cloudflare Worker: `tramnucuoi`
- Supabase: `iwiqprhoohkxvjyxojto`
- production email: OFF
- Turnstile: OFF
- all real Journey registrations: CLOSED
- Journey applications: 0

## Prepared but unapplied migrations

Strict order:

1. `0026_phase_9_journey_registration_gate.sql`
2. `0027_phase_9_journey_replay_dedupe.sql`
3. `0028_phase_9_journey_activation_gate.sql`

Production currently has none of these applied.

## Phase 9 protection contract

Prepared and QA-verified controls:

- Cloudflare hard actor/Journey rate limits;
- privacy-preserving transient HMAC actor identity;
- gated Supabase registration REST path;
- replay and short-window contact dedupe;
- email idempotency and privacy-safe operational logs;
- aggregate-only abuse telemetry;
- conditional Turnstile challenge;
- server-side Siteverify with action/hostname validation and timeout;
- 32 KiB pre-parser registration body ceiling;
- admin-only controlled `draft -> registration_open` transition;
- database witness `p9-wu7-v1`;
- dependency-safe rollback.

## WU7 QA evidence

GitHub Actions run `33226764293`: PASS

Included:

- source abuse QA;
- ephemeral PostgreSQL destructive gate QA;
- alternate API path bypass QA;
- rollback QA;
- typecheck;
- build;
- Cloudflare dry-run.

## Production invariant at closeout

Read-only Supabase snapshot:

- `pgrst.db_pre_request = NULL`
- applications = 0
- open Journeys = 0
- replay column absent
- activation witness absent
- `pg_graphql` disabled

Therefore production stayed unchanged throughout Phase 9.

## Future production protection gate

Before the first real Journey can open:

1. intentionally reconcile the exact Phase 9 release into `main`;
2. configure required Cloudflare Phase 9 secrets/vars;
3. keep Turnstile explicitly OFF unless separately approved;
4. deploy only from approved `main` with `--keep-vars`;
5. apply 0026 and configure/verify the registration gate;
6. apply 0027 and verify dedupe + REST-path RLS;
7. apply 0028 and configure/verify the activation gate;
8. verify witness `p9-wu7-v1` and all production probes with real Journeys still closed;
9. obtain a separate Owner gate for the selected real Journey;
10. enable production email only under a separate Owner gate.

## Rollback rule

Close every Journey first.

Then rollback database protection strictly:

`0028 -> 0027 -> 0026`

Never remove registration protection while a real Journey remains open.

## Current Owner decision

Phase 9 engineering and QA are closed.

Production protection deployment remains **HOLD**.

This handoff does not authorize:

- merging/releasing Phase 9 to production;
- applying 0026/0027/0028;
- enabling Turnstile;
- enabling production email;
- opening any real Journey.

## Next recommended gate

`APPROVE PHASE 9 PRODUCTION PROTECTION ACTIVATION — authorize coordinated reconciliation of the approved Phase 9 source into main, application of migrations 0026/0027/0028, required Supabase gate digests/PostgREST reload, Cloudflare rate-limit/analytics bindings and server-only secrets, and production deployment from the approved main SHA; keep Turnstile OFF, email OFF, and all real Journeys closed until post-deploy protection verification PASS.`
