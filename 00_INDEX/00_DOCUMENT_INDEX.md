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

## Other Document Sets

- `01_PRODUCT_VISION/` — project rules, content guideline, future roadmap
- `02_PLATFORM/` — CMS structure, Community OS blueprint, Journey platform spec, identity/role model, journey lifecycle, visual direction
- `03_OPERATIONS/` — operating model, journey operations guide, community governance
- `04_ROADMAP/` — community feature roadmap, Journey MVP product spec
- `05_DATABASE_DESIGN/` — Journey database architecture, technical & Supabase plans
- `06_AUDIT/` — codebase and system audits
- `07_JOURNEY_MVP/` — Journey MVP spec, operations, engineering, QA/release, implementation
