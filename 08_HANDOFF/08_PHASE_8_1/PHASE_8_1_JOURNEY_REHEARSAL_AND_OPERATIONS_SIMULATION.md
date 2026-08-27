# PHASE 8.1 — JOURNEY REHEARSAL & OPERATIONS SIMULATION

**Status: COMPLETE**
**Type: engineering / product archive — NOT evidence of real community impact.**
**Backend: external Supabase ref `iwiqprhoohkxvjyxojto` (canonical). Lovable Cloud DB not used.**
**Production deployment: none during this phase.**

## Purpose

Rehearse the Journey MVP end-to-end — public discovery, public registration,
admin review workflow, participant handling and Journey visual presentation —
before starting the Journey Experience & Field Story upgrade. The goal was to
prove the operational loop worked on the canonical external backend, not to run
a real field programme.

## Scope of the rehearsal

### Controlled public QA journey

A single controlled, non-real journey was used for the simulation:

- slug: `hanh-trinh-qa-tram-nu-cuoi`
- reachable on both public locales (VI journey route and the `/en/...`
  counterpart) through the existing bilingual route mapping
- treated as QA content from the start; it was later moved to `archived` (see
  Phase 8.2 record) so it would drop out of public lists and the sitemap

All content on that journey is rehearsal content. No partner, location, metric
or outcome recorded on it should be read as verified fact.

### Public registration simulation

The public registration form (`src/components/journeys/registration-form.tsx`)
was exercised against the live external backend: validation, submit, and the
resulting `journey_applications` row. Registration collects only the fields the
form declares (name, email, optional phone, way of joining / participant type,
number of people, optional notes).

### Admin application workflow

The application status model is defined in
`db/migrations/0014_journey_mvp_foundation.sql` as the enum
`submitted → reviewing → accepted | rejected → confirmed`, and driven from
`src/components/admin/journeys/application-manager.tsx`. The rehearsal walked
applications through those transitions in the admin console.

Access model as implemented in 0014: applications are admin-only for read and
review (not editor), and `journey_participants` is admin-only in every
direction with no anon grant.

### Participant handling on confirmation

`public.journey_participants` exists from 0014 with
`status default 'confirmed'` and admin-only RLS in all directions. Participants
are therefore created/managed from the admin side rather than by a public
action.

> Uncertainty made explicit: the repository record for this phase does not
> prove whether participant creation on confirmation was fully automatic or an
> explicit admin step at the time Phase 8.1 closed. Do not cite either as fact
> without re-reading the current admin code.

### Journey cover / visual QA

Multiple read-only visual QA passes were run on the QA journey at 1440 / 768 /
390 across VI and EN, checking hero image, field-update covers, gallery order,
category labels and per-image metadata. Two classes of issue were found and
handled differently:

- **Code defect — fixed:** journey covers were being stored/served as expiring
  signed Storage URLs even though the assets were public. Fixed by introducing
  `src/lib/media/stable-url.ts`, storing durable public URLs from
  `src/components/admin/media-picker.tsx`, and resolving covers from
  `media_assets` as source of truth in `src/lib/journeys/journeys.server.ts`
  (legacy `cover_url` kept only as fallback). Verified: no `/object/sign` or
  `token=` URLs in public HTML.
- **Content/data mismatches — Owner-side:** image ↔ metadata pairings on the QA
  journey (notably EN alt/caption for the first and last gallery images) were
  reported to the Owner as data corrections; they were not code bugs and were
  not fixed by editing records in this phase.

### Transactional email

Email infrastructure was *prepared*, not activated. `src/lib/notifications/`
implements a provider send path that reads `RESEND_API_KEY` / `EMAIL_FROM` at
call time and returns `{ sent: false, reason: "provider_not_configured" }` when
they are absent, logging and continuing without breaking registration.

**Production email remained paused/unconfigured throughout Phase 8.1.** No
transactional mail was live and none should be described as having been sent.

## What Phase 8.1 did NOT do

- No production deployment or publish.
- No real participants, partners or field activity.
- No public accounts / public authentication.
- No comments, reactions, feeds, gamification or finance features.

## Exit

Phase 8.1 closed with the operational Journey loop rehearsed and the media URL
durability defect fixed. Next phase:
**Phase 8.2 — Journey Experience & Field Story (experience upgrade).**
