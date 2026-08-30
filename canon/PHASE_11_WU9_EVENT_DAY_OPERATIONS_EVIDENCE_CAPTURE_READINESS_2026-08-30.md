# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 11 — REAL JOURNEY OPERATIONS
# P11-WU9 — EVENT-DAY OPERATIONS & EVIDENCE CAPTURE READINESS

Date: 2026-08-30  
Owner: TRẠM NỤ CƯỜI  
CTO / Product Architect: ChatGPT  
Builder: Lovable

## STATUS

- **SOURCE READINESS: COMPLETE / PASS**
- **PRODUCTION ATTENDANCE STORAGE: ACTIVE / PASS**
- **SECURITY / RLS PARITY: PASS**
- **ATTENDANCE DATA BACKFILL: NONE / PASS**
- **OVERALL WU9: COMPLETE / PASS**

WU9 establishes event-day operations readiness for the first real Journey pilot while keeping registration, confirmation, attendance, documentary evidence and impact as separate facts.

No production attendance value was fabricated or inferred during activation.

## PILOT

- Journey ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- Slug: `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`
- Event date: `2026-09-11`
- Location: `Trạm Nụ Cười Đà Nẵng`
- Capacity: `30`
- Journey status after WU9 activation: `registration_open`
- Applications: `1`
- Confirmed applications: `1`
- Confirmed party size: `1`
- Recorded attendance rows after activation: `0`

## CANONICAL SEMANTIC SEPARATION

The following facts MUST remain distinct:

1. **Registration** — visitor submitted an application.
2. **Confirmation** — staff accepted and confirmed participation.
3. **Attendance** — people were actually observed as present on event day.
4. **Impact** — verified outcomes supported by post-event evidence.

Therefore:

`registration ≠ confirmation ≠ attendance ≠ impact`

`journey_participants.joined_at` is participant creation / confirmation time and MUST NOT be interpreted as check-in.

## ATTENDANCE EVIDENCE FOUNDATION

Production now contains the additive fields:

- `attended_party_size integer null`
- `attendance_recorded_at timestamptz null`
- `attendance_recorded_by uuid null references auth.users(id) on delete set null`

Semantics:

- `attended_party_size IS NULL` = not yet observed/recorded.
- `attended_party_size = 0` = verified no-show.
- `1..party_size` = verified number of people actually present.

Database constraints:

1. `journey_participants_attended_party_size_check`
   - allows NULL or `0 <= attended_party_size <= party_size`.
2. `journey_participants_attendance_evidence_complete_check`
   - attendance count, recorded timestamp and recorder must be either all NULL or all non-NULL.
3. `journey_participants_attendance_recorded_by_fkey`
   - recorder references `auth.users(id)` with `ON DELETE SET NULL`.

Index:

- `journey_participants_attendance_recorded_by_idx`

Existing participant RLS remains enabled with the same four admin-only policies for SELECT / INSERT / UPDATE / DELETE.

No new anon or editor access was added.

## PRODUCTION MIGRATION

Supabase migration applied successfully:

`p11_wu9_attendance_evidence_foundation`

Canonical source mirror:

`db/migrations/0029_p11_wu9_attendance_evidence_foundation.sql`

Product commit:

`6cf6205644116012587a3f55933a9f1eeb46ea72`

Rollback prepared:

`db/rollbacks/p11_wu9_attendance_evidence_foundation.sql`

Rollback commit:

`839431f36f9f10da0aeba1b18e5378a486755367`

Rollback is destructive to any attendance evidence created later and requires separate Owner approval before use.

## SOURCE IMPLEMENTATION

Canonical product source already provides backward-compatible attendance support:

- `src/lib/journeys/types.ts`
- `src/lib/journeys/admin-queries.ts`
- `src/components/admin/journeys/application-manager.tsx`
- `src/routes/_authenticated/admin.journeys.tsx`

Source canonical HEAD before migration mirror:

`1b84e1ab3b334b873af78c1ba015270665195769`

Migration / rollback canonical HEAD after activation:

`839431f36f9f10da0aeba1b18e5378a486755367`

Generated Supabase type remains canonical `PostgrestVersion: "14.17"`.

Builder QA on event-day source:

- `bun run typecheck`: PASS
- `bun run build`: PASS

## WRITE SAFETY

`recordParticipantAttendance()` is fail-closed and:

- requires authenticated staff identity;
- re-reads participant under admin RLS;
- requires participant persisted status `confirmed`;
- re-reads parent Journey;
- requires persisted Journey status `preparing` or `completed`;
- validates integer attendance against freshly-read `party_size`;
- writes only the three attendance evidence fields;
- update-filter includes participant id, Journey id and `status='confirmed'`;
- rejects a zero-row update caused by concurrent state change.

Attendance cannot be recorded while the real pilot remains `registration_open`.

## EVENT-DAY OPERATING RULES

Before event-day attendance:

1. intentionally close registration under WU7 rules;
2. resolve submitted/reviewing/accepted liability;
3. verify confirmed party size does not exceed capacity;
4. verify replay/dedupe and protection state;
5. freeze the confirmed roster;
6. assign Event Lead / Attendance Operator / Documentary Lead;
7. transition `registration_open → preparing` intentionally.

During the event:

- attendance is recorded from real observation only;
- `0` is deliberate verified no-show, never a default;
- NULL remains unknown/not-yet-recorded;
- group attendance may be lower than confirmed `party_size`;
- Field Updates remain factual timeline records, not impact claims;
- media goes through Media Library, rights/consent/trust and documentary classification;
- no impact value is inferred from attendance, registrations, photos or planned outputs.

## POST-EVENT HANDOFF

After the event:

1. reconcile every confirmed participant/group attendance row;
2. calculate actual attendance only from recorded `attended_party_size`;
3. report registration, confirmation and attendance separately;
4. complete Field Update review;
5. complete documentary metadata / captured time / category / trust review;
6. reconcile evidence with staff records;
7. author verified Impact Snapshot / Impact Items only after evidence exists;
8. move Journey to `completed` only after verified post-event evidence review.

## PRODUCTION VERIFICATION AFTER ACTIVATION

Verified:

- all 3 attendance columns present: PASS
- attendance count constraint present: PASS
- evidence completeness constraint present: PASS
- recorder FK present: PASS
- recorder index present: PASS
- `journey_participants` RLS enabled: PASS
- participant policy count unchanged at 4: PASS
- attendance partial rows: `0`: PASS
- attendance rows recorded: `0`: PASS
- pilot status remains `registration_open`: PASS
- confirmed party size remains `1`: PASS
- open Journey count remains `1`: PASS

No attendance was backfilled into the real participant.

## ADVISOR REVIEW

Post-migration Supabase Security Advisor did not report a new WU9 schema/RLS issue. The existing project-level warning remains:

- Leaked Password Protection disabled.

Post-migration Performance Advisor did not report an unindexed FK for `attendance_recorded_by`; the new supporting index is present. It reports the new index as unused, which is expected immediately before the first real attendance record. Existing unrelated project advisor notices remain outside WU9 scope.

## FINAL DECISION

**P11-WU9 — EVENT-DAY OPERATIONS & EVIDENCE CAPTURE READINESS: COMPLETE / PASS**

The attendance evidence foundation is now production-ready, but real attendance data MUST remain empty until a verified event-day observation is made after the Journey has intentionally moved to `preparing`.
