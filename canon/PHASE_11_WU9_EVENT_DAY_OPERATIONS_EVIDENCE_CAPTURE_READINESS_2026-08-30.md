# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 11 — REAL JOURNEY OPERATIONS
# P11-WU9 — EVENT-DAY OPERATIONS & EVIDENCE CAPTURE READINESS

Date: 2026-08-30  
Owner: TRẠM NỤ CƯỜI  
CTO / Product Architect: ChatGPT  
Builder: Lovable

## STATUS

- **SOURCE READINESS: COMPLETE / PASS**
- **PRODUCTION ATTENDANCE STORAGE: HOLD — OWNER GATE REQUIRED**
- **OVERALL WU9: WATCH / READY FOR ATTENDANCE FOUNDATION ACTIVATION**

WU9 prepares the first real Journey pilot for event-day operations while keeping registration, confirmation, attendance, documentary evidence and impact as separate facts.

No production attendance value is fabricated or inferred in this work unit.

## PILOT

- Journey ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- Slug: `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`
- Event date: `2026-09-11`
- Location: `Trạm Nụ Cười Đà Nẵng`
- Capacity: `30`
- Production Journey status at WU9 readiness audit: `registration_open`
- Applications: `1`
- Confirmed application rows: `1`
- Confirmed party size: `1`
- Field Updates: `0`
- Journey media relations: `1`

## CANONICAL SEMANTIC SEPARATION

The following facts MUST NOT be collapsed into one another:

1. **Registration** — a visitor submitted an application.
2. **Confirmation** — staff accepted and confirmed a registration.
3. **Attendance** — people were actually observed as present on event day.
4. **Impact** — verified outcomes or results supported by evidence after the event.

Therefore:

`submitted/accepted/confirmed registration ≠ attendance ≠ impact`

A confirmed participant row is NOT evidence that the person or entire group actually attended.

`journey_participants.joined_at` is the participant-record creation / confirmation time. It is NOT a check-in timestamp and must never be reported as one.

## ATTENDANCE EVIDENCE MODEL

WU9 defines a minimal additive attendance evidence model for `journey_participants`:

- `attended_party_size integer null`
- `attendance_recorded_at timestamptz null`
- `attendance_recorded_by uuid null references auth.users(id) on delete set null`

Semantics:

- `attended_party_size IS NULL` = attendance has not yet been observed/recorded.
- `attended_party_size = 0` = verified no-show for this participant/group row.
- `attended_party_size BETWEEN 1 AND party_size` = verified number of people actually present.

Recommended database constraint for the later migration:

`attended_party_size IS NULL OR attended_party_size BETWEEN 0 AND party_size`

Recommended consistency rule:

- if `attended_party_size IS NULL`, `attendance_recorded_at` should remain null;
- if attendance is recorded, `attendance_recorded_at` must be non-null;
- normal application writes require an authenticated admin recorder even though the foreign key may later become null if that user account is deleted.

Do NOT add an `attended` participant status. Existing participant lifecycle `confirmed / withdrawn` remains separate from attendance evidence.

## SOURCE IMPLEMENTATION

Canonical product source adds backward-compatible event-day attendance support without requiring the production attendance columns to exist yet.

### `src/lib/journeys/types.ts`

- adds optional attendance fields to `JourneyParticipant`;
- preserves the existing compatibility `PARTICIPANT_COLUMNS` selector;
- adds a separate attendance-aware selector;
- attendance writes are permitted by source only for Journey status `preparing` or `completed`.

### `src/lib/journeys/admin-queries.ts`

Adds `listParticipantsWithAttendance()`:

- attempts the attendance-aware columns first;
- falls back to the existing participant query only for recognized missing-column/schema-cache errors;
- reports `attendanceAvailable = false` when the production attendance schema is not yet active;
- does NOT hide unrelated auth, RLS, parse, query or network errors.

Adds defensive `recordParticipantAttendance()`:

- requires the UI-visible Journey state to be attendance-recordable as an early guard;
- requires an authenticated staff identity;
- re-reads the participant row under admin RLS immediately before writing;
- requires persisted participant status `confirmed`;
- re-reads the persisted parent Journey and requires `preparing` or `completed`;
- validates the verified attendance number against the freshly-read `party_size`;
- writes only `attended_party_size`, `attendance_recorded_at`, and `attendance_recorded_by`;
- filters the update by participant id, Journey id and `status='confirmed'`;
- rejects a zero-row update so concurrent state changes fail closed.

It never changes participant status, registration status, party size, Field Updates, media or impact.

### `src/components/admin/journeys/application-manager.tsx`

Preserves all WU7 registration and capacity behavior and adds a separate admin-only Event-Day Attendance panel.

The panel shows:

- confirmed participant/group rows;
- rows with attendance recorded;
- verified attended people;
- rows still awaiting attendance observation;
- verified no-show rows.

Staff semantics are explicit:

- null = not yet recorded;
- zero = deliberately verified no-show;
- a confirmed row may represent multiple people;
- actual attendance count must be entered from observation;
- no attendance value is automatically filled.

An explicit `ĐỦ CẢ NHÓM` action may populate the form with the confirmed group size, but staff still must deliberately save it. This is not automatic attendance.

Corrections are allowed by entering and saving a new verified count.

If the production schema does not yet contain attendance columns, the panel explains that attendance storage is not activated while the existing registration workflow continues normally.

### `src/routes/_authenticated/admin.journeys.tsx`

Passes the parent Journey status to the event-day attendance workflow. Activation Gate behavior remains unchanged.

## CANONICAL SOURCE COMMITS

WU8 source baseline:

`0f42d53fe5fde9c3567f799ca1e2b24b8258081f`

WU9 canonical source commits:

- `40808342dedafb88476df3cad664e7f932909e3c`
- `243edcd3cbef49452ffc118bf9755b8513b883fe`
- `a3359c041820c7593988fccafb14e59ad4fcc2f5`
- `1b84e1ab3b334b873af78c1ba015270665195769`

Canonical product HEAD after WU9 source sync:

`1b84e1ab3b334b873af78c1ba015270665195769`

CTO diff verification from WU8 baseline to WU9 HEAD contains exactly four intended files:

1. `src/lib/journeys/types.ts`
2. `src/lib/journeys/admin-queries.ts`
3. `src/components/admin/journeys/application-manager.tsx`
4. `src/routes/_authenticated/admin.journeys.tsx`

No migration, public renderer, runtime, environment, Cloudflare or generated Supabase type file is included.

Canonical generated Supabase type remains `PostgrestVersion: "14.17"`; the Builder-side auto-regenerated `14.5` value was explicitly excluded.

Builder QA on the final intended source implementation:

- `bun run typecheck`: PASS
- `bun run build`: PASS

## EVENT-DAY OPERATING ROLES

Role assignments are functional, not named-person assignments. Owner may assign actual staff later.

### Event Lead

- owns overall go/no-go and event status;
- confirms registration has intentionally closed before event-day check-in;
- ensures Journey transition to `preparing` follows the WU7 gate;
- resolves operational anomalies.

### Attendance Operator

- works from the final confirmed roster;
- records actual attended party size per confirmed participant/group;
- never defaults confirmed party size into attendance;
- explicitly records zero only after verifying a no-show;
- resolves all remaining null attendance rows before attendance totals are treated as complete.

When near capacity or during intensive check-in, use one attendance operator as the authoritative recorder to minimize conflicting edits.

### Documentary Lead

- captures only real Field Updates and documentary material;
- records time/location only when known;
- routes media through Media Library;
- handles consent, rights and editorial trust before public use;
- never uploads consent forms, identity documents or sensitive originals into public-addressable website storage;
- keeps uncertain media private/unreviewed/restricted until resolved.

## PRE-EVENT → EVENT-DAY GATE

Do NOT begin attendance recording while the Journey is `registration_open`.

Before event-day check-in:

1. intentionally close registration under the WU7 operational cutoff rule;
2. verify application status counts and confirmed `party_size` total;
3. resolve accepted-pending liability;
4. verify there is no over-capacity condition;
5. verify duplicate/replay integrity;
6. confirm this is still the only registration-open Journey before closure;
7. preserve Phase 9 protection essentials;
8. retain/freeze the confirmed participant roster;
9. confirm logistics and staff roles;
10. transition `registration_open → preparing` intentionally.

Only after the Journey is `preparing` should event-day attendance recording become operationally writable.

## EVENT-DAY CHECKLIST

### Attendance

- Record actual attendance per confirmed participant/group.
- Use `0` only for an observed/verified no-show.
- Leave null when not yet known.
- Do not infer attendance from registration confirmation, message replies, media appearance or `joined_at`.
- Correct mistakes by recording a new verified count.

### Field Updates

- Create from real observations only.
- Published update requires VI title, actual `happened_at`, and factual VI body under WU8.
- Location remains optional if not certain.
- Field Update is a factual timeline record, not an impact claim.

### Documentary media

- Upload/curate through Media Library.
- Mark `documentation` only for real field material.
- Enter `captured_at` only when known.
- Assign evidence category only from actual context.
- Complete consent/rights/trust review before public publication.
- Keep uncertain/sensitive media non-public.

### Impact

Do NOT create final impact totals during the event merely from:

- confirmed registration count;
- attendance count;
- number of photos;
- number of Field Updates;
- planned outputs.

Those are operational/evidence facts, not automatically impact.

## POST-EVENT HANDOFF

After the event:

1. resolve every confirmed participant/group attendance row to a verified number or document why evidence is incomplete;
2. calculate attendance only from recorded `attended_party_size` values;
3. report registrations, confirmed capacity and actual attendance as separate metrics;
4. complete Field Update review and publication readiness;
5. complete documentary media metadata, category and trust review;
6. reconcile documentary evidence with staff/event records;
7. only then author verified Impact Snapshot / Impact Items;
8. final review must confirm impact claims are supported;
9. only after verified post-event evidence may the Journey become `completed`.

## ANOMALY PROTOCOL

The Phase 11 operating safety rule remains:

1. CLOSE registration first if registration is still open and an anomaly requires containment.
2. Keep Email OFF.
3. Keep Turnstile OFF unless separately approved.
4. Preserve privacy-safe evidence.
5. Diagnose.
6. Stop further mutation if protection state is uncertain.

Event-day evidence uncertainty is handled by leaving data unknown/private, not by guessing values.

## PRODUCTION MUTATION STATEMENT

WU9 source/readiness work did NOT intentionally perform:

- production Journey status mutation;
- application or participant mutation;
- attendance data mutation;
- Supabase schema migration;
- Email enablement;
- Turnstile enablement;
- Cloudflare/runtime/deploy mutation;
- public Journey rendering change;
- Field Update fabrication;
- media metadata fabrication;
- impact data creation.

Production currently has no attendance evidence columns, so attendance persistence is intentionally HOLD.

## OWNER GATE

Next recommended Owner authorization:

**APPROVE P11-WU9 ATTENDANCE EVIDENCE FOUNDATION**

After approval, CTO may create/review/apply the minimal additive production migration for the three attendance fields, preserve existing admin-only RLS, verify security and capability reads, and leave all attendance values null until real event-day observations are recorded.

No fake/test attendance should be inserted into the real pilot production row.