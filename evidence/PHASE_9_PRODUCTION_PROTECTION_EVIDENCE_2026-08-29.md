# TRẠM NỤ CƯỜI — PHASE 9 PRODUCTION PROTECTION EVIDENCE

Date: 2026-08-29
Owner gate: APPROVE PHASE 9 PRODUCTION PROTECTION ACTIVATION

## Final state

**PHASE 9 PRODUCTION PROTECTION: ACTIVE / PASS**

- Product main: `3322e0d35dbb1d97ec0e0ebbea93486dbe813fe0`
- Cloudflare Worker: `tramnucuoi`
- Worker version: `5b7c0d10-3e61-49a0-a702-75a9c5836e5f`
- Supabase project: `iwiqprhoohkxvjyxojto`
- Activation witness: `p9-wu7-v1`
- All real Journeys: CLOSED
- Journey applications: 0
- Email delivery: OFF
- Turnstile: OFF
- pg_graphql: OFF

## Production protections

- 0026 registration pre-request gate active.
- Locked `private.tnc_phase9_security_config` singleton contains SHA-256 digests only.
- Runtime roles have no table privileges on the private digest table.
- Missing registration gate returns 403 `TNC_REGISTRATION_GATE_REQUIRED`.
- Invalid registration gate returns 403 `TNC_REGISTRATION_GATE_INVALID`.
- Positive server gate proceeds to grants/RLS; REST path accepts the verified leading-slash runtime form after normalization.
- 0027 replay and contact-window columns, shape checks and unique constraints active.
- Alternate API/RPC paths remain excluded by normalized canonical REST-path RLS.
- 0028 admin activation trigger active; closing remains gate-free.
- Cloudflare Analytics Engine binding `REGISTRATION_ABUSE_ANALYTICS` active with dataset `tnc_journey_registration_security`.
- Five hard/conditional rate-limit bindings deployed.
- 32 KiB raw-body ceiling and fail-closed limiter behavior passed CI/destructive QA and are present in the deployed approved bundle.

## Release and QA evidence

- CI #47 / run 33248830250: PASS — private singleton config.
- CI #49 / run 33249144375: PASS — PostgREST API-role execution.
- CI #51 / run 33249319967: PASS — request-path normalization.
- CI #53 / run 33249506556: PASS — current PostgREST error envelope.
- CI #55 / run 33250572150: PASS — normalized 0027 RLS path.
- Production domain: HTTP 200.
- Direct Data API missing/invalid gate probes: 403 / 403.
- Final database snapshot: witness true; both digest shapes valid; activation trigger true; replay/contact unique indexes true; db_pre_request true; open Journeys 0; applications 0.

## Compatibility hotfixes recorded

Persistent custom GUC storage was rejected by hosted Supabase with SQLSTATE 42501. It was replaced by the approved locked private singleton digest table. Production verification additionally required API-role EXECUTE on the pre-request function, optional leading-slash normalization for PostgREST request paths, and the current PGRST error DETAIL envelope with an explicit empty headers object.

## Advisor note

No new blocking Phase 9 security advisory was reported. Existing Auth leaked-password protection warning is outside this gate. Performance advisor observations are non-blocking and deferred.

## Owner boundary

This activation does not authorize opening a Journey, enabling email delivery, or enabling Turnstile. The first real Journey requires a separate Owner Gate. Rollback remains strictly `0028 -> 0027 -> 0026` and only while every Journey is CLOSED.
