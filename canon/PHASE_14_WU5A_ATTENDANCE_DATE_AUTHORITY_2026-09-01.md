# PHASE 14 — WU5A
# ATTENDANCE DATE AUTHORITY HARDENING

Date: 2026-09-01
Status: COMPLETE / PASS

## Purpose

Prevent verified Journey attendance evidence from being recorded before the Journey start date while preserving post-event correction and unresolved reset semantics.

## Canonical implementation

Product repository: `huynhtranhuythinh/tramnucuoi`
Canonical product main SHA after P14-WU5A: `fbe8f0d85f8b28b13760b1f84307342d6c2d9fc0`

Commit message:
`Merge P14-WU5A attendance date authority — Harden attendance date authority at the admin UI and database layers; add deterministic source/DB QA and preserve Journey truth semantics.`

Implementation includes:
- `db/migrations/0041_p14_wu5a_attendance_date_authority.sql`
- admin UI date-authority guard using Vietnam calendar date (`Asia/Ho_Chi_Minh`)
- deterministic source QA
- deterministic ephemeral DB QA
- CI wiring for both source and DB gates

## Canonical truth semantics preserved

- registration != attendance
- confirmed registration != attendance
- attendance `NULL` = unresolved
- attendance `0` = verified no-show
- attendance `> 0` = verified attended
- participant claim != attendance
- Memory remains evidence-gated
- Reflection remains evidence-gated

## Date authority contract

Attendance evidence may become recorded only on or after `journeys.start_date` according to the Vietnam calendar date.

Fail-closed cases:
- Journey status does not permit attendance operations
- Journey start date is missing
- current Vietnam date is before Journey start date

Allowed corrections on/after the Journey start date:
- verified no-show (`0`)
- verified attended (`1..party_size`)
- correction between recorded values
- explicit reset of all attendance evidence fields to `NULL` to restore unresolved state

## Database protection

A private trigger function guards updates to attendance evidence on `public.journey_participants` and rejects pre-event writes even if an admin UI path is bypassed.

## QA contract

Repository CI includes:
- `scripts/p14-wu5a-attendance-date-authority-qa.ts`
- `scripts/p14-wu5a-db-qa.sql`

The DB QA verifies:
- pre-event write rejection
- missing-start-date rejection
- event-day verified no-show allowed
- event-day/post-event attended correction allowed
- reset to unresolved allowed
- exactly one date-authority trigger exists

## Production reference at P15 audit

Production website: `https://tramnucuoi.com`
Cloudflare Worker: `tramnucuoi`
Worker Version recorded in the P15 handoff context: `1f31cd53-be12-4075-9e01-cbb58d0fedf5`

No P15 work may weaken or bypass this authority.
