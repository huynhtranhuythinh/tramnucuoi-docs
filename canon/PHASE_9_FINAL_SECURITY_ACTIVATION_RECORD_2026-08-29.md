# TRẠM NỤ CƯỜI — PHASE 9 FINAL SECURITY & ACTIVATION RECORD

Date: 2026-08-29  
Phase: 9 — Journey Registration Abuse Protection & Safe Activation  
Canonical status: COMPLETE / PASS — PRODUCTION ACTIVATION HOLD

## 1. Canonical decision

Phase 9 is complete at architecture, implementation and QA level.

The Journey registration system now has a verified protection model covering:

- direct Data API bypass;
- server-side rate limiting;
- duplicate/replay submission;
- notification amplification;
- privacy-preserving abuse telemetry;
- conditional Turnstile escalation;
- controlled admin activation;
- request-body abuse;
- emergency rollback.

This closeout does **not** authorize production deployment, application of Phase 9 migrations, Turnstile activation, transactional email activation, or opening any real Journey.

## 2. Canonical source snapshot

Product repo: `huynhtranhuythinh/tramnucuoi`

Production source:

- branch: `main`
- commit: `7c3f139e0d8660052c988461d11abd4ed52e08b6`
- tree: `0385d9a708a1473fd6e1fbe25650362abbc3b0f1`

Phase 9 development source:

- branch: `develop`
- commit: `c072296bb209ecfba04d4bdc89dddb57543fe275`
- tree: `d929aa27f5cf3eedb46489e4641112fa7b9bf694`

`main` and `develop` are diverged. Do **not** blindly merge `develop -> main`. The Phase 9 release must be reconciled intentionally.

## 3. Work-unit closure

| Work Unit | Result |
| --- | --- |
| P9-WU1 — Current attack surface audit | COMPLETE / PASS |
| P9-WU2 — Server-side rate limit architecture & registration gate | COMPLETE / PASS |
| P9-WU3 — Duplicate/replay protection & email amplification control | COMPLETE / PASS |
| P9-WU4 — Privacy-preserving abuse metadata | COMPLETE / PASS |
| P9-WU5 — Conditional challenge / Turnstile escalation | COMPLETE / PASS |
| P9-WU6 — Safe Journey activation gate | COMPLETE / PASS |
| P9-WU7 — Abuse / rollback QA | COMPLETE / PASS |
| P9-WU8 — Canonical closeout | COMPLETE / PASS |

## 4. Prepared database contract — NOT APPLIED

Canonical production migration order:

1. `0026_phase_9_journey_registration_gate.sql`
2. `0027_phase_9_journey_replay_dedupe.sql`
3. `0028_phase_9_journey_activation_gate.sql`

All three remain **UNAPPLIED in production**.

### 0026

Adds the registration pre-request gate. Canonical REST inserts into `journey_applications` require the server-only registration gate before normal grants and RLS are evaluated.

### 0027

Adds opaque HMAC replay/contact-window keys and uniqueness constraints. WU7 hardening additionally requires the public INSERT policy to be reached through the canonical REST method/path (`POST`, `journey_applications`), closing future alternate API paths such as GraphQL/RPC mutation bypass.

### 0028

Adds the controlled Journey activation trigger and non-secret database witness:

`public.tnc_journey_activation_guard_version()` -> `p9-wu7-v1`

Opening registration requires admin authorization plus the server-side activation gate. Closing registration remains gate-free so emergency closure is always simpler than opening.

## 5. Canonical runtime protection

Trusted path:

`Browser -> Cloudflare/TanStack server function -> gated Supabase REST insert -> grants/RLS`

No service-role key is introduced into the public registration flow.

Hard rate limits:

- actor + Journey burst: `6 / 10 seconds`
- actor + Journey sustained: `24 / 60 seconds`
- Journey flood: `120 / 60 seconds`

Conditional challenge thresholds when Turnstile is explicitly enabled and ready:

- `4 / 10 seconds`
- `12 / 60 seconds`

Hard rate limits remain authoritative after challenge success.

`registerForJourney` also has a 32 KiB raw request ceiling before TanStack payload parsing. Oversize/compressed registration requests fail closed with HTTP 413.

## 6. Duplicate/replay and email amplification canon

Opaque HMAC material is derived with `REGISTRATION_DEDUPE_KEY_SECRET`:

- exact replay: normalized payload + Journey + UTC day;
- contact window: normalized email + Journey + fixed 10-minute bucket;
- notification identity: normalized email + Journey + UTC day.

Duplicate/replay conflicts return generic success and reuse the same notification event identity.

Production email remains OFF.

## 7. Privacy-preserving telemetry canon

No applicant-abuse profile table is created.

Custom abuse telemetry must not contain raw IP, actor HMAC, replay/contact HMAC, name, email, phone, notes, User-Agent, request body, fingerprint or visitor ID.

Telemetry is best-effort only and never part of the security-critical path.

## 8. Turnstile canon

Prepared Turnstile mode is conditional, not universal.

Server verification requires Siteverify success, action `journey_registration`, hostname match and a 10-second timeout. TNC does not send `remoteip`.

Production Turnstile remains OFF.

## 9. Safe activation canon

A Journey must not become `registration_open` through an ordinary CMS status mutation.

The controlled path verifies runtime readiness, Phase 9 database witness, registration-gate negative/positive probes, draft state, admin role and activation gate.

## 10. Rollback canon

Canonical rollback:

`db/rollbacks/phase_9_journey_registration_protection.sql`

All Journeys must be closed before database protection is removed.

Strict rollback order:

`0028 -> 0027 -> 0026`

## 11. Production closeout state

Read-only Supabase verification on project `iwiqprhoohkxvjyxojto`:

| Item | State |
| --- | --- |
| `pgrst.db_pre_request` | `NULL` |
| Journey applications | `0` |
| `registration_open` Journeys | `0` |
| replay column applied | `false` |
| activation guard applied | `false` |
| `pg_graphql` enabled | `false` |

Therefore production remained unchanged throughout Phase 9.

## 12. Future production activation sequence

While every real Journey remains closed:

1. intentionally reconcile exact Phase 9 source into `main`;
2. configure required Cloudflare runtime secrets/vars;
3. keep `TURNSTILE_ENABLED=false` unless separately approved;
4. deploy only from approved `main` with `--keep-vars`;
5. apply `0026` and configure/verify registration-gate digest;
6. apply `0027` and verify dedupe + REST-path RLS;
7. apply `0028` and configure/verify activation-gate digest;
8. verify witness `p9-wu7-v1` and production probes while all real Journeys stay closed;
9. obtain a separate Owner gate before opening the first real Journey;
10. enable production email only under a separate Owner gate.

## 13. Residual non-blocking items

- fixed 10-minute contact windows are buckets, not rolling windows;
- production traffic has not exercised Phase 9 controls because production stayed unchanged;
- Turnstile challenge path is not live-tested in production;
- Analytics Engine telemetry is source-prepared but not deployed;
- `main` and `develop` remain diverged and branch protection remains OFF;
- CSP remains Report-Only from the prior production posture.

## 14. Final decision

**Phase 9 implementation: COMPLETE / PASS.**  
**P9-WU8 Canonical Closeout: COMPLETE / PASS.**  
**Production mutation during Phase 9: NONE.**  
**Phase 9 production deployment/activation: HOLD pending separate Owner gate.**  
**Turnstile: OFF.**  
**Transactional email: OFF.**  
**All real Journeys: CLOSED.**
