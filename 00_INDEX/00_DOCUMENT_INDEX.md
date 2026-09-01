# TNC Documentation Index

## Purpose

Canonical reading order for TRẠM NỤ CƯỜI Web App and future Community & Journey OS development.

`tnc-docs` is the canonical project-document archive, intended to be synced to
the Owner's local `~/dev/tnc-docs/`.

## Reading Order

1. TRAM_NU_CUOI_PROJECT_RULES.md
2. TNC_OPERATING_MODEL.md
3. COMMUNITY_OS_BLUEPRINT.md
4. JOURNEY_PLATFORM_SPEC.md
5. USER_IDENTITY_ROLE_MODEL.md
6. JOURNEY_LIFECYCLE.md
7. JOURNEY_OPERATIONS_GUIDE.md
8. COMMUNITY_GOVERNANCE.md
9. CMS_STRUCTURE.md
10. BACKEND.md
11. VISUAL_DIRECTION.md
12. CONTENT_GUIDELINE.md

All future development must respect these documents.

## Phase Archive Rule (permanent)

- `tnc-docs/08_HANDOFF/` is the **container** for Phase 8. It groups the phase documents; it never replaces or merges them.
- Every numbered sub-phase keeps its own directory inside the container.
- Never collapse or merge sub-phase documents into one combined phase document.
- Closed phase folders are immutable historical records; only factual corrections or clearly marked addenda may be added.
- The active phase document may be updated freely until phase close.
- Every phase document must state its status explicitly. Future or pending checks must never be recorded as passed.

## Phase 8 Archive — `08_HANDOFF/`

Handoff index: `08_HANDOFF/00_PHASE_8_HANDOFF_INDEX.md`

### `08_HANDOFF/08_PHASE_8_1/` — Journey Rehearsal & Operations Simulation (COMPLETE)
- `PHASE_8_1_JOURNEY_REHEARSAL_AND_OPERATIONS_SIMULATION.md`

### `08_HANDOFF/08_PHASE_8_2/` — Journey Experience & Field Story (COMPLETE)
- `PHASE_8_2_JOURNEY_EXPERIENCE_AND_FIELD_STORY.md`
- `JOURNEY_FIELD_STORY_MIGRATION_REVIEW.md`
- `JOURNEY_FIELD_STORY_RLS_TEST_MATRIX.md`

### `08_HANDOFF/08_PHASE_8_3/` — Privacy, Trust & Release Readiness
- `PHASE_8_3_PRIVACY_TRUST_RELEASE_READINESS.md`

## Phase 12 — Living Community OS Foundation — COMPLETE / PASS

Canonical chain:
1. `canon/PHASE_12_COMMUNITY_OS_CONTINUATION_PLAN_2026-08-31.md`
2. `canon/PHASE_12_WU1_COMMUNITY_IDENTITY_JOURNEY_LINK_2026-08-31.md`
3. `canon/PHASE_12_WU2_COMMUNITY_ONBOARDING_PARTICIPANT_CLAIM_2026-08-31.md`
4. `canon/PHASE_12_WU3_MY_JOURNEY_MEMORY_2026-08-31.md`
5. `canon/PHASE_12_WU4_JOURNEY_NATIVE_COMMUNITY_INTERACTION_2026-08-31.md`
6. `canon/PHASE_12_WU5_CONTRIBUTION_HISTORY_2026-08-31.md`
7. `canon/PHASE_12_WU6_COMMUNITY_ROLES_HOST_NETWORK_2026-08-31.md`
8. `canon/PHASE_12_WU7_IMPACT_NETWORK_CLOSEOUT_2026-08-31.md`
9. `canon/PHASE_12_FINAL_CLOSEOUT_2026-08-31.md`
10. `evidence/PHASE_12_LIVING_COMMUNITY_OS_EVIDENCE_2026-08-31.md`
11. `handoff/PHASE_12_TO_PHASE_13_HANDOFF_2026-08-31.md`

Phase 12 final product HEAD:
`75f9511de8442fcd632429b21cfc56fb727aed7b`

## Phase 13 — Community Experience & Living UI — COMPLETE / PASS

Canonical chain:
1. `canon/PHASE_13_COMMUNITY_EXPERIENCE_KICKOFF_2026-08-31.md`
2. `canon/PHASE_13_WU1_COMMUNITY_EXPERIENCE_ARCHITECTURE_2026-08-31.md`
3. `canon/PHASE_13_WU2_PERSONAL_COMMUNITY_HOME_2026-08-31.md`
4. `evidence/PHASE_13_WU2_MY_TNC_EVIDENCE_2026-08-31.md`
5. `canon/PHASE_13_WU3_JOURNEY_COMMUNITY_EXPERIENCE_2026-08-31.md`
6. `evidence/PHASE_13_WU3_JOURNEY_EXPERIENCE_EVIDENCE_2026-08-31.md`
7. `canon/PHASE_13_WU4_COMMUNITY_PEOPLE_RELATIONSHIP_UI_2026-08-31.md`
8. `evidence/PHASE_13_WU4_COMMUNITY_RELATIONSHIP_UI_EVIDENCE_2026-08-31.md`
9. `canon/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_2026-08-31.md`
10. `evidence/PHASE_13_WU5_LIVING_COMMUNITY_SURFACE_EVIDENCE_2026-08-31.md`
11. `canon/PHASE_13_WU6_PUBLIC_ACTIVATION_POLISH_2026-08-31.md`
12. `evidence/PHASE_13_WU6_PUBLIC_ACTIVATION_POLISH_EVIDENCE_2026-08-31.md`
13. `canon/PHASE_13_WU6_PRODUCTION_CLOSEOUT_ADDENDUM_2026-08-31.md`
14. `evidence/PHASE_13_WU6_PRODUCTION_HOTFIX_OWNER_REVIEW_EVIDENCE_2026-08-31.md`
15. `canon/PHASE_13_FINAL_CLOSEOUT_2026-08-31.md`

Status:
- P13-WU1 — Community Experience Architecture: **COMPLETE / PASS**
- P13-WU2 — Personal Community Home / My TNC: **COMPLETE / PASS**
- P13-WU3 — Journey Community Experience: **COMPLETE / PASS**
- P13-WU4 — Community People & Relationship UI: **COMPLETE / PASS**
- P13-WU5 — Living Community Surface: **COMPLETE / PASS**
- P13-WU6 — Public Activation & Polish: **COMPLETE / PASS**

Final Phase 13 product main:
`b8c0fd597bbe411bee3165e5741471ea443c529e`

Final Cloudflare Worker Version ID:
`23bacfb3-5ec0-4aa8-88e7-df8008ba1b81`

WU6 final production evidence:
- PR #32 duplicate-header hotfix merged;
- PR CI #150 PASS;
- post-merge main CI #151 PASS;
- production desktop VI/EN visual smoke PASS;
- production mobile VI/EN visual smoke PASS;
- Owner Review PASS.

Final activation split:
- public Living Community: **ON**;
- My TNC public Magic Link/Auth: **OFF / fail-closed** via `VITE_APP_COMMUNITY_AUTH_ENABLED=false`;
- Email: OFF;
- Turnstile: OFF;
- no public people directory;
- no fake Community activity.

The earlier WU6 implementation record/evidence documented the pre-deploy PENDING state. The WU6 production closeout addendum and final Phase 13 closeout supersede that pending status.

Phase 13 is closed without waiting for the 2026-09-11 pilot. The P11 live-pilot operational lane remains independently authoritative.

## Phase 14 — Real Community Activation & Member Lifecycle

### P14-WU1 — Auth & Email Production Readiness — COMPLETE / PASS

Canonical chain:
1. `canon/PHASE_14_WU1_AUTH_EMAIL_PRODUCTION_READINESS_2026-08-31.md`
2. `evidence/PHASE_14_WU1_AUTH_EMAIL_PRODUCTION_EVIDENCE_2026-08-31.md`

WU1 result:
- production Site URL verified;
- exact `/auth`, `/cong-dong`, `/en/community` redirect allowlist verified;
- dedicated Resend SMTP configured for Supabase Auth;
- production Magic Link delivery PASS;
- desktop token/session/logout/reuse behavior PASS;
- VI/EN redirect acceptance PASS;
- resend throttle PASS;
- mobile browser Magic Link flow PASS;
- bilingual Magic Link template applied;
- no Community truth facts manufactured by Auth testing.

### P14-WU2 — Controlled My TNC Activation — COMPLETE / PASS

Canonical chain:
1. `canon/PHASE_14_WU2_CONTROLLED_MY_TNC_ACTIVATION_2026-08-31.md`
2. `evidence/PHASE_14_WU2_PRODUCTION_ACTIVATION_EVIDENCE_2026-08-31.md`

WU2 result:
- My TNC production Auth activated behind controlled/reversible release;
- real Magic Link sign-in, signed-in My TNC, logout and one-time-link reuse rejection PASS;
- no participant/Memory/Contribution/Reflection/relationship facts manufactured;
- pre-existing CMS admin role remained independent;
- product main: `4582e8e6866714711631549ac4ed51cfb2d0c10d`;
- Worker Version: `a47535cc-b6af-4b92-90d2-e6917f8051a4`.

### P14-WU3 — Real Participant Claim & Identity Reconciliation — COMPLETE / PASS

Canonical chain:
1. `canon/PHASE_14_WU3_REAL_PARTICIPANT_CLAIM_IDENTITY_RECONCILIATION_2026-08-31.md`
2. `evidence/PHASE_14_WU3_REAL_PARTICIPANT_CLAIM_IDENTITY_RECONCILIATION_EVIDENCE_2026-08-31.md`

WU3 result:
- live authenticated claim path exercised with the real verified Community account;
- five real claim requests returned zero eligible / zero linked;
- verified account email did not evidence-match the confirmed 2026-09-11 pilot registration;
- reconciliation therefore correctly preserved the account and participant as separate identities;
- no admin/cosmetic link was manufactured;
- pilot attendance remains unresolved;
- no Memory, Contribution, Reflection, Community relationship or CMS permission drift;
- claim RPC/RLS fail-closed security model verified in production;
- no product code, migration, RLS or deploy change required;
- product main remains `4582e8e6866714711631549ac4ed51cfb2d0c10d`;
- Worker Version remains `a47535cc-b6af-4b92-90d2-e6917f8051a4`.

### P14-WU4 — Real My TNC Member Experience — COMPLETE / PASS

Canonical chain:
1. `canon/PHASE_14_WU4_REAL_MY_TNC_MEMBER_EXPERIENCE_2026-09-01.md`
2. `evidence/PHASE_14_WU4_REAL_MY_TNC_MEMBER_EXPERIENCE_EVIDENCE_2026-09-01.md`
3. `canon/PHASE_14_WU4A_ACCOUNT_AUTH_LIFECYCLE_ADDENDUM_2026-09-01.md`

WU4 result:
- real verified My TNC account validated end to end on production;
- truthful no-Journey lane PASS;
- display name save/reload and fresh-session persistence PASS;
- desktop VI/EN PASS;
- mobile VI/EN PASS;
- logout/reload/re-login PASS;
- source audit found and fixed a My TNC own-data privacy boundary for accounts with broader CMS staff access;
- product PR #34 merged;
- PR CI #156 PASS; post-main CI #157 PASS;
- product main at original WU4 close: `6c36d0e9035ec583ca9b3bd67300d4d3c20f1b9d`;
- Worker Version at original WU4 close: `c010f061-3ff2-4ffe-93af-ccd6263ae392`;
- no database migration or RLS mutation;
- VI/EN hydration flicker and repeated claim audit rows are recorded as non-blocking follow-up work.

WU4A auth-lifecycle addendum result:
- account signup + email confirmation PASS;
- unconfirmed password login correctly blocked;
- password login PASS;
- Magic Link remains available for existing accounts only;
- password recovery and reset PASS;
- MFA/TOTP AAL2 challenge PASS for recovery, password login and Magic Link when required;
- explicit Check Email signup-success UX PASS;
- desktop EN PASS;
- mobile VI/EN auth/signup layout PASS;
- final product main: `272fd07d4b5697a22ace650e0e8b87943f1b4276`;
- post-main CI #172 PASS;
- final Worker Version: `d6d564c9-12d0-46c5-b9db-edbe0768e1cd`;
- no database migration or RLS mutation.

WU4A truth postflight:
- a real QA account using the exact pilot-registration email evidence-matched the confirmed participant and created one active verified-email participant link;
- attendance remains unresolved (`attended_party_size = NULL`, no attendance timestamp);
- corresponding Memory row remains `attendance_state = unresolved` and `memory_eligible = false`;
- no active Contribution, Reflection or verified Community relationship was created;
- therefore account/claim truth remains distinct from attendance and Memory eligibility.

Non-blocking debt:
- normalize all Supabase Auth email templates to consistent VI/EN branded copy;
- reduce repeated idempotent claim-request audit noise;
- QA signup accounts may be cleaned up only through an explicit controlled action.

### P14-WU5A — Attendance Date Authority Hardening — COMPLETE / PASS

Canonical record:
- `canon/PHASE_14_WU5_POST_JOURNEY_READINESS_2026-09-01.md`

WU5A result:
- Admin UI and PostgreSQL independently block attendance before the Journey start date;
- Vietnam calendar (`Asia/Ho_Chi_Minh`) is authoritative;
- status alone cannot manufacture attendance;
- product WU5A release baseline: `fbe8f0d85f8b28b13760b1f84307342d6c2d9fc0`;
- Worker Version: `1f31cd53-be12-4075-9e01-cbb58d0fedf5`;
- post-main CI #174 PASS;
- production runtime verification PASS;
- pilot attendance remained unresolved.

### P14-WU5 — Post-Journey Memory & Reflection Operations — PRE-EVENT READY

Phase-level pre-closeout chain:
1. `00_INDEX/PHASE_14_PRE_CLOSEOUT_INDEX_2026-09-01.md`
2. `canon/PHASE_14_CURRENT_CANON_2026-09-01.md`
3. `evidence/PHASE_14_PRE_EVENT_EVIDENCE_2026-09-01.md`
4. `handoff/PHASE_14_POST_JOURNEY_CLOSEOUT_HANDOFF_2026-09-01.md`
5. `04_ROADMAP/PHASE_14_TO_PHASE_15_SEQUENCING_2026-09-01.md`

Current Phase 14 status:

**PRE-EVENT READY / ENGINEERING COMPLETE / FINAL CLOSEOUT PENDING REAL EVENT EVIDENCE.**

Pilot Journey:
- ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- date: `2026-09-11`
- attendance: unresolved
- Memory: unresolved / not eligible
- Reflection count: 0

Final P14-WU5 / Phase 14 closeout must use real post-Journey attendance, Memory and Reflection evidence. It must not be simulated.

Final documents expected after the real event:
- `canon/PHASE_14_FINAL_CLOSEOUT_2026-09-11.md`
- `evidence/PHASE_14_FINAL_EVIDENCE_2026-09-11.md`

Phase 15 product-experience work may proceed in parallel, but it does not close or supersede this Phase 14 real-event evidence lane.

## Other Document Sets

- `01_PRODUCT_VISION/` — project rules, content guideline, future roadmap
- `02_PLATFORM/` — CMS structure, Community OS blueprint, Journey platform spec, identity/role model, journey lifecycle, visual direction
- `03_OPERATIONS/` — operating model, journey operations guide, community governance
- `04_ROADMAP/` — community feature roadmap, Journey MVP product spec, Phase 14→15 sequencing
- `05_DATABASE_DESIGN/` — Journey database architecture, technical & Supabase plans
- `06_AUDIT/` — codebase and system audits
- `07_JOURNEY_MVP/` — Journey MVP spec, operations, engineering, QA/release, implementation
