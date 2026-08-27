# PHASE 8.2 — JOURNEY EXPERIENCE & FIELD STORY

**Status: COMPLETE**
**Content Activation: COMPLETE**
**Owner Review: PASS**
**Backend: external Supabase ref `iwiqprhoohkxvjyxojto` (canonical). Lovable Cloud DB not used.**
**Production deployment: none at phase close.**
**Type: engineering / product archive — not evidence of real community impact.**

Companion documents in this folder (preserved, canonical):

- `JOURNEY_FIELD_STORY_MIGRATION_REVIEW.md` — migrations 0015–0018, data model,
  access model, migration risks.
- `JOURNEY_FIELD_STORY_RLS_TEST_MATRIX.md` — RLS verification matrix.

## Purpose

Turn an operational Journey record into a readable field story: chronological
field updates, an impact presentation for completed journeys, media used as
documented evidence, and links to related editorial Field Journal posts —
bilingual VI/EN, on the canonical external backend.

## Schema delivered (all applied)

| migration | version | status |
| --- | --- | --- |
| `0015_journey_field_story_foundation.sql` | `20260825052429` | applied |
| `0016_journey_media_evidence.sql` | `20260825052443` | applied |
| `0017_journey_field_note_relations.sql` | `20260825052451` | applied |
| `0018_journey_media_authenticated_visibility_hardening.sql` | `20260825062405` | applied |

Key decisions (full detail in the migration review):

- `journey_updates` (`field_note | activity | milestone`) with `content_status`,
  ordering and bilingual `*_en` columns.
- Impact narrative and impact items in **separate** tables
  (`journey_impact_snapshots`, `journey_impact_items`) so anon reads are gated
  strictly on the parent journey being `completed` — a row-level security
  boundary, not a UI choice.
- `journey_media` as a relationship-only table; `media_assets` stays the single
  source of truth for url/alt/caption/credit. Same-journey integrity is
  declarative via a composite FK, so no new `SECURITY DEFINER` RPC was added.
- `journey_field_notes` as a pointer table to `posts` (`kind='journey'`);
  `posts` was not altered.
- `0018` is a forward hardening migration: the authenticated non-staff fallback
  on journey media now also requires the linked `journey_updates` row to be
  `published`, matching anon visibility.

## Application work implemented

- Public: `src/components/journeys/field-updates.tsx`,
  `journey-evidence.tsx`, `journey-impact.tsx`, `related-field-notes.tsx`,
  composed in `journey-detail-page.tsx`, bilingual VI/EN.
- Admin: `src/components/admin/journeys/field-update-manager.tsx`,
  `media-manager.tsx`, `impact-manager.tsx`, `field-note-manager.tsx`,
  with the `Tiếng Việt | English` tab pattern.
- Data access: `src/lib/journeys/{updates,media,impact,field-notes,queries,journeys.server}.ts`.
- Release hardening carried in this phase: durable public media URLs
  (`src/lib/media/stable-url.ts`) used across public journey surfaces — verified
  no signed/expiring URLs in public HTML.

## Content Activation

Content Activation is recorded COMPLETE and Owner Review PASS: the Owner
attached journey media and covers and confirmed the final mapping.

**Media provenance:** the imagery used on the QA/demo journey is
prototype/AI-or-placeholder demo media. It must be labelled demo/prototype
wherever it is surfaced and must never be presented as verified real field
evidence. Replacing it with real Owner-supplied documentary media remains open
work.

## QA journey disposition

The controlled QA journey `hanh-trinh-qa-tram-nu-cuoi` was ultimately moved to
**`archived`**, and verified absent from `/journeys`, `/en/journeys` and the
sitemap.

## Not in scope (unchanged from the phase boundary)

Comments, reactions, followers, chat, gamification, public profiles, social
feed, contributor/host/partner dashboards, public accounts, realtime,
contributions/finance, BI or computed metrics, notification changes, any
conversion between `posts.kind='journey'` and operational journeys, Lovable
Cloud DB, and any deploy or publish.

## Exit

Next phase: **Phase 8.3 — Privacy, Trust & Release Readiness.**
