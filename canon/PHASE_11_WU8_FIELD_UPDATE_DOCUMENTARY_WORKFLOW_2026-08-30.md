# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 11 — REAL JOURNEY OPERATIONS
# P11-WU8 — FIELD UPDATE & DOCUMENTARY WORKFLOW

Date: 2026-08-30  
Owner: TRẠM NỤ CƯỜI  
CTO / Product Architect: ChatGPT  
Builder: Lovable

## STATUS

**P11-WU8: COMPLETE / PASS**

This work unit prepares the real Journey pilot for factual Field Updates and documentary media operations before the event. It does not create synthetic field records, invent capture times, infer consent/rights, fabricate impact, change the live Journey status, or mutate production data.

## PILOT BASELINE AT CLOSE

- Journey ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- Slug: `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`
- Event date: `2026-09-11`
- Current Journey status: `registration_open`
- Capacity: `30`
- Applications: `1`
- Confirmed people: `1`
- Accepted pending people: `0`
- Field Updates: `0`
- Journey media relations: `1`
- Existing relation passes the current asset public/documentation/trust gate: `1`
- Existing relation missing `captured_at`: `1`
- Existing relation missing `evidence_category`: `1`

The missing capture time and category are intentionally left unresolved. They must only be entered from a real documentary record; WU8 never fills them by inference.

## CORE DOCUMENTARY INVARIANTS

1. A Field Update is a factual timeline record of something that actually happened.
2. A Field Update is **not** an impact claim.
3. `documentation` means real field material; it does **not** mean verified impact.
4. Registration, confirmation, attendance, activity, output and impact are separate concepts.
5. Do not infer time, location, identity, consent, rights, outcome or impact.
6. Draft records may remain incomplete.
7. No auto-publish, auto-classification, auto-fill or auto-approval is allowed.
8. The canonical Media Library remains the only place to edit media metadata, trust, consent/rights review and capture time.
9. Sensitive consent/right documents are not to be uploaded merely to satisfy a UI field; use the existing trust-review model and approved offline references/processes.

## FIELD UPDATE PUBLISH CONTRACT

A draft Field Update may be incomplete and may be saved for later work.

A Field Update may be saved as `published` only when all of the following factual fields exist:

- Vietnamese title;
- `happened_at` — real date/time of the event, handled in Vietnam time (UTC+7);
- Vietnamese body describing what actually occurred.

Location remains optional. Staff must not invent a location when it is unknown or uncertain.

The contract is enforced twice:

1. Admin form readiness UI blocks the publish action and shows concrete Vietnamese blockers.
2. `upsertJourneyUpdate()` defensively reuses the same shared readiness contract so a direct/bypassed admin call cannot publish an incomplete record.

Existing `published_at`, RLS, localization and CRUD semantics are unchanged.

## FIELD UPDATE ADMIN READINESS

The Journey admin now shows a compact operational summary for the selected Journey:

- draft count;
- published count;
- previously published records that do not meet the new documentary completeness contract.

Historical incomplete published rows, if any, are surfaced for correction but never auto-filled or silently rewritten.

## PUBLIC ELIGIBILITY VS DOCUMENTARY COMPLETENESS

WU8 deliberately separates two concepts.

### A. Public eligibility

A Journey media relation is considered public-eligible by the admin readiness mirror only when the existing public path conditions are satisfied:

1. parent Journey is in a public Journey state defined by `PUBLIC_JOURNEY_STATUSES`;
2. media asset is readable by the admin workflow;
3. `media_assets.is_public = true`;
4. `evidence_status = documentation`;
5. editorial trust passes the existing compatibility gate `legacyPublicFallback(asset)`;
6. if the relation is scoped to a Field Update, that linked update exists and is `published`.

This is a readiness mirror only. WU8 does not modify RLS or the public renderer.

### B. Documentary complete

A relation is documentary-complete only when it is already public-eligible **and**:

- `captured_at` is present from a real record;
- `evidence_category` is explicitly selected.

Therefore:

`documentary_complete = public_eligible + captured_at + evidence_category`

Missing documentary fields are operational gaps, not invitations to invent values.

## RELATION-LEVEL BLOCKERS

Admin Journey Media now surfaces concrete reasons when a relation is not ready, including:

- parent Journey not public;
- asset unavailable/unreadable;
- asset private;
- asset not classified as `documentation`;
- editorial trust not compatible with public use;
- linked Field Update missing or still draft;
- missing `captured_at`;
- missing evidence category.

The selected-asset form also shows read-only context:

- public/private state;
- evidence classification;
- editorial trust label;
- capture-time presence;
- linked Field Update and its draft/published state.

The Journey relation screen does not edit those canonical Media Library attributes.

## JOURNEY-LEVEL DOCUMENTARY SUMMARY

For every Journey, the Media manager now reports:

- total media relations;
- public-eligible relations;
- documentary-complete relations;
- incomplete relations.

These are derived from current records only. No record is manufactured to improve the counts.

## EVENT-DAY OPERATOR FLOW

For the real field event, staff should use this sequence:

1. Observe an actual event/activity before writing it as fact.
2. Create a Field Update as `draft`.
3. Enter the real Vietnam date/time in `happened_at`.
4. Write the Vietnamese title/body based on the actual observation.
5. Leave location blank if uncertain rather than guessing.
6. Upload documentary media into the canonical Media Library.
7. Set `evidence_status=documentation` only for genuine real-world material.
8. Enter `captured_at` only when the real capture time is known from the record/source.
9. Complete the applicable trust/consent/rights review in the Media Library.
10. Make an asset public only when its actual review state permits public use.
11. Link the asset to the correct Journey and, when appropriate, the correct Field Update.
12. Select an evidence category based on what the material actually documents.
13. Review the blockers/readiness summary.
14. Publish the Field Update only after its factual publish contract is complete.
15. Do not create or publish impact metrics merely because an activity or photograph exists.

## IMPACT SEPARATION

Evidence collected under WU8 may later support impact review, but WU8 itself does not convert documentary material into impact.

Examples:

- a confirmed participant is not proof of attendance;
- a photograph of an activity is not proof of an outcome;
- an output delivered is not automatically a verified long-term impact;
- a Field Update is not a final impact statement.

Verified impact belongs to the later post-event evidence/review workflow.

## SOURCE IMPLEMENTATION

Canonical product `main` changes for WU8:

1. `src/lib/journeys/updates.ts`
   - shared `fieldUpdatePublishBlockers()` contract;
   - publish-readiness helper.

2. `src/components/admin/journeys/field-update-manager.tsx`
   - factual Field Update guidance;
   - draft/published readiness summary;
   - visible publish blockers;
   - fail-closed publish UI.

3. `src/components/admin/journeys/media-manager.tsx`
   - mirrors parent Journey/public/documentation/trust/update eligibility;
   - separates public eligibility from documentary completeness;
   - relation-level blockers;
   - journey-level readiness counts;
   - read-only selected-asset documentary/trust context.

4. `src/lib/journeys/admin-queries.ts`
   - defensive Field Update publish precondition using the shared contract;
   - also restores the missing P11-WU7 `confirmApplication()` defensive accepted/confirmed precondition to canonical `main` (canonical parity repair identified during WU8 preflight).

5. `src/routes/_authenticated/admin.journeys.tsx`
   - passes parent Journey status into `JourneyMediaManager` while preserving WU7 capacity and existing activation-gate behavior.

Canonical product commits:

- `f7122ede86f088416a703957402c189ed6fc6124`
- `e374e32e7308eb6f6ed0428ce72cfbcc9a752f4a`
- `aa48e0c7d4a8305b9058386f40c8bd18765b6627`
- `b5f74a77c5edb7df83d9829eba283ced029e6dad`
- `0f42d53fe5fde9c3567f799ca1e2b24b8258081f`

Canonical product HEAD after WU8 source sync:

`0f42d53fe5fde9c3567f799ca1e2b24b8258081f`

Diff from WU7 canonical HEAD `4518eaf65518a1a470b232bbfcc544bbf8a3808a` contains exactly the five files listed above.

Generated Supabase types remain unchanged in canonical `main`; `PostgrestVersion` is still `14.17`.

## QA

Builder QA against the final intended WU8 implementation:

- `bun run typecheck`: PASS
- `bun run build`: PASS

CTO diff review additionally confirmed:

- no migration file changed;
- no generated Supabase type change entered canonical `main`;
- no public renderer/RLS rule was changed;
- no Email/Turnstile/Cloudflare/runtime configuration was changed;
- canonical `main` is ahead of WU7 by five commits and only five expected files.

## PRODUCTION MUTATION STATEMENT

P11-WU8 did not intentionally perform production mutation of:

- Journey status/content;
- applications/participants;
- Field Updates;
- Journey media relations;
- Media Library metadata;
- trust/consent/rights records;
- impact records;
- Supabase schema/migrations;
- Email configuration;
- Turnstile configuration;
- Cloudflare runtime/deployment configuration.

Production was queried read-only for readiness verification only.

## CLOSE-OUT DECISION

**P11-WU8 = COMPLETE / PASS.**

The system is now operationally prepared to capture factual Field Updates and documentary media during the real Journey while clearly preventing the most important editorial failure modes: premature publication, invented metadata, trust-blind public readiness and documentary-to-impact conflation.
