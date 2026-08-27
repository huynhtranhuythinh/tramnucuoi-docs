# Phase 8 Handoff Index

Container for all Phase 8 sub-phase archives. Each sub-phase keeps its own
folder and its own canonical document; nothing is merged.

Backend for all Phase 8 work: external Supabase ref `iwiqprhoohkxvjyxojto`
(canonical). Lovable Cloud DB not used. No production deployment as of the
latest phase record.

## Sub-phases

| sub-phase | folder | status | canonical document |
| --- | --- | --- | --- |
| 8.1 — Journey Rehearsal & Operations Simulation | `08_PHASE_8_1/` | COMPLETE | `PHASE_8_1_JOURNEY_REHEARSAL_AND_OPERATIONS_SIMULATION.md` |
| 8.2 — Journey Experience & Field Story | `08_PHASE_8_2/` | COMPLETE | `PHASE_8_2_JOURNEY_EXPERIENCE_AND_FIELD_STORY.md` |
| 8.3 — Privacy, Trust & Release Readiness | `08_PHASE_8_3/` | IN PROGRESS | `PHASE_8_3_PRIVACY_TRUST_RELEASE_READINESS.md` |

Supporting documents in `08_PHASE_8_2/`:

- `JOURNEY_FIELD_STORY_MIGRATION_REVIEW.md` — migrations 0015–0018 review
- `JOURNEY_FIELD_STORY_RLS_TEST_MATRIX.md` — RLS test matrix

## Rule

- Every future Phase 8.x gets its **own subfolder here** (`08_PHASE_8_4/`,
  `08_PHASE_8_5/`, …). Never skipped, never merged, never overwritten.
- Completed sub-phase folders are immutable historical records; only factual
  corrections or clearly marked addenda may be added.
- The active sub-phase document may be updated until phase close.
- Every phase document must state its status explicitly (`IN PROGRESS`,
  `COMPLETE`, …). Pending checks must never be recorded as passed.
