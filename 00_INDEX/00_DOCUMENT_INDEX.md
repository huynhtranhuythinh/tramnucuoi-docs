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

- `tnc-docs/08_HANDOFF/` is the **container** for Phase 8. It groups the phase
  documents; it never replaces or merges them.
- Every numbered sub-phase keeps **its own directory inside the container**:
  `08_HANDOFF/08_PHASE_8_1`, `08_HANDOFF/08_PHASE_8_2`,
  `08_HANDOFF/08_PHASE_8_3`, then `08_PHASE_8_4`, `08_PHASE_8_5`, …
- Never collapse or merge sub-phase documents into one combined phase document.
  No phase folder is ever skipped or overwritten.
- Closed phase folders are **immutable historical records**; only factual
  corrections or clearly marked addenda may be added.
- The active phase document may be updated freely until phase close.
- Every phase document must state its status explicitly: `IN PROGRESS`,
  `COMPLETE`, etc. Future or pending checks must never be recorded as passed.

## Phase 8 Archive — `08_HANDOFF/`

Handoff index: `08_HANDOFF/00_PHASE_8_HANDOFF_INDEX.md`

### `08_HANDOFF/08_PHASE_8_1/` — Journey Rehearsal & Operations Simulation (COMPLETE)

- `PHASE_8_1_JOURNEY_REHEARSAL_AND_OPERATIONS_SIMULATION.md`

### `08_HANDOFF/08_PHASE_8_2/` — Journey Experience & Field Story (COMPLETE)

- `PHASE_8_2_JOURNEY_EXPERIENCE_AND_FIELD_STORY.md` — canonical phase summary / handoff
- `JOURNEY_FIELD_STORY_MIGRATION_REVIEW.md` — migrations 0015–0018 review
- `JOURNEY_FIELD_STORY_RLS_TEST_MATRIX.md` — RLS test matrix

### `08_HANDOFF/08_PHASE_8_3/` — Privacy, Trust & Release Readiness (IN PROGRESS)

- `PHASE_8_3_PRIVACY_TRUST_RELEASE_READINESS.md` — live phase record

## Other Document Sets

- `01_PRODUCT_VISION/` — project rules, content guideline, future roadmap
- `02_PLATFORM/` — CMS structure, Community OS blueprint, Journey platform spec,
  identity/role model, journey lifecycle, visual direction
- `03_OPERATIONS/` — operating model, journey operations guide, community governance
- `04_ROADMAP/` — community feature roadmap, Journey MVP product spec
- `05_DATABASE_DESIGN/` — Journey database architecture, technical & Supabase plans
- `06_AUDIT/` — codebase and system audits
- `07_JOURNEY_MVP/` — Journey MVP spec, operations, engineering, QA/release,
  implementation (see `07_JOURNEY_MVP/JOURNEY_MVP_DOCUMENT_INDEX.md`)
