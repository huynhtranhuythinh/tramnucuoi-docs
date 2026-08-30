# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 11 / P11-WU10 — POST-EVENT EVIDENCE & IMPACT CLOSEOUT

**Date:** 2026-08-30  
**Owner:** TRẠM NỤ CƯỜI Owner  
**CTO / Product Architect:** ChatGPT  
**Builder:** Lovable + CTO hardening  
**Status:** COMPLETE / PASS  
**Pilot Journey:** `19539f36-3ed4-4a22-96b9-c8a9b73c5283`  
**Pilot event date:** 2026-09-11  

---

## 1. Canonical principle

WU10 preserves the following semantic separation as an invariant:

`Registration != Confirmation != Attendance != Evidence != Impact`

**No real evidence => no impact claim.**

Registrations, confirmed people, attendance, media counts, Field Update counts, capacity and planned outputs remain operational/evidence facts only. None of them automatically creates or verifies impact.

---

## 2. Architecture audit — pre-WU10

The read-only production and source audit found two material gaps:

1. `journey_impact_items` and `journey_impact_snapshots` were editor-authored records with no independent verification state. Public SELECT was effectively controlled by the parent Journey being `completed`; a staff-entered number could therefore become publicly visible without an explicit evidence-verification state.
2. The Journey lifecycle had the Phase 9 activation gate for entering `registration_open`, but no authoritative database gate for `preparing -> completed` based on post-event reconciliation.

Additional findings:

- Field Update status remains `draft | published`; publication is not treated as impact verification.
- Attendance evidence is canonical on `journey_participants.attended_party_size` from WU9.
- Documentary evidence remains based on `journey_media` + canonical `media_assets`, including `evidence_category`, `evidence_status`, trust state and public visibility rules.
- Production contained one previously completed legacy Journey with 4 impact items and 1 impact snapshot already publicly exposed under the pre-WU10 rule.
- Those historical impact records were not retroactively called verified.

---

## 3. Post-event evidence reconciliation model

### A. Attendance evidence

Canonical source:

`journey_participants.attended_party_size`

Semantics remain:

- `NULL` = attendance unresolved / not recorded
- `0` = verified no-show
- `1..party_size` = actual people present

Confirmed count is never substituted for attendance.

### B. Field evidence

Field Updates remain factual observations. A closeout review requires staff to explicitly attest that Field Updates were reviewed. Any draft Field Update blocks closeout. Zero Field Updates is permitted when staff explicitly reviews that fact; the system does not fabricate a Field Update merely to satisfy closeout.

### C. Documentary evidence

Documentary review uses the Journey-to-media relation plus canonical media asset state.

A linked media relation blocks closeout when it is unclassified/unreviewed or unsafe for its current visibility. Reviewed private/restricted evidence may remain private and still satisfy closeout; documentary closeout is not the same thing as public eligibility.

`captured_at` remains evidence metadata only when known. A genuinely unknown value is a WATCH condition, not a reason to invent a timestamp.

### D. Impact evidence

Each impact item/snapshot now carries an explicit verification state and evidence basis. Impact verification is not inferred from attendance, media count, Field Update count, registration, capacity or any other operational number.

---

## 4. Impact verification model

Migration `0030_p11_wu10_post_event_evidence_impact_closeout.sql` adds to both impact tables:

- `verification_status`
- `verification_source`
- `withheld_reason`
- `verified_at`
- `verified_by`

Allowed states:

- `draft`
- `needs_evidence`
- `verified`
- `withheld`
- `legacy_public`

### Verification rules

- New impact defaults to `draft`.
- Verification is always explicit; no auto-verify path exists.
- `verified` requires a nonblank evidence basis.
- Verification is only allowed after the Journey event end date and while the parent Journey is `preparing` or `completed`.
- `verified_by` and `verified_at` are server-owned and stamped from the authenticated staff identity.
- A client cannot self-assign verifier attribution.
- `withheld` requires a nonblank reason.
- Editing the material content/evidence basis of an already verified claim demotes it to `needs_evidence`; an old verification stamp cannot silently survive a changed claim.
- Moving away from verified clears verifier attribution.
- `legacy_public` cannot be selected for new records or new transitions.

### Legacy compatibility

Only pre-existing impact rows whose parent Journey was already completed at migration time were relabelled `legacy_public`.

Production verification after migration:

- legacy impact items: **4**
- legacy impact snapshot: **1**
- legacy rows with fabricated verifier/source/time: **0**
- new verified impact items: **0**
- new verified impact snapshots: **0**

`legacy_public` means: content that was public before WU10 and has **not** been re-verified under WU10.

---

## 5. Public impact publication gate

Old completed-only public SELECT policies were replaced because PostgreSQL permissive RLS policies are OR-combined.

Public impact now requires:

1. parent Journey is `completed`; and
2. impact row is either:
   - `verified` with evidence source + server verifier attribution, or
   - `legacy_public` compatibility content.

`draft`, `needs_evidence` and `withheld` are never public impact facts.

Public application code also filters impact verification state as defense in depth. Legacy-public content is rendered with a trust note and is never labelled verified.

---

## 6. Admin-only closeout review

New table:

`public.journey_closeout_reviews`

One row per Journey, containing:

- `attendance_gap_note`
- `field_updates_reviewed`
- `documentary_reviewed`
- `final_narrative_reviewed`
- `review_status` = `pending | passed`
- `review_notes`
- `reviewed_by`
- `reviewed_at`
- audit timestamps

RLS is enabled and all CRUD policies are admin-only via the canonical role helper.

No participant PII is stored in this table.

### PASS semantics

PASS is a **human review attestation**, not an impact calculation.

A PASS can only be created after the event end date while the Journey is `preparing` or `completed`, requires all three review attestations, and requires an explicit gap note if confirmed attendance remains unresolved.

Database triggers own reviewer attribution.

If relevant evidence or reviewed narrative changes after PASS, the PASS is invalidated back to `pending` and must be reviewed again.

---

## 7. Authoritative `preparing -> completed` closeout gate

A database trigger now guards entry into `completed`.

Minimum enforced conditions:

- transition must be exactly `preparing -> completed`;
- authenticated admin required;
- event end date must have been reached/passed in `Asia/Ho_Chi_Minh`;
- a current `journey_closeout_reviews.review_status = passed` must exist;
- required manual review attestations must remain true;
- impossible attendance values block;
- unresolved confirmed attendance requires an explicit gap note;
- draft Field Updates block;
- linked documentary evidence must be classified/reviewed and safe for its visibility;
- public media must satisfy documentary/trust rules;
- impact `draft`, `needs_evidence`, or `legacy_public` blocks a new WU10 closeout;
- verified impact must contain evidence basis + server attribution;
- withheld impact must contain a reason;
- zero impact rows is valid when no evidence-backed impact claim is made;
- final public narrative must have been reviewed;
- evidence/narrative mutation after PASS invalidates/stales the review and blocks closeout until re-review.

The Phase 9 Journey Activation Guard remains independent and unchanged.

---

## 8. Admin closeout UX

Admin Journey workflow now includes **ĐÓNG SỔ**.

The closeout summary exposes operational facts without participant PII:

### Attendance

- confirmed people
- attendance recorded rows
- attendance unresolved rows
- actual attended people
- verified no-show rows
- invalid attendance rows

### Field Updates

- total
- draft
- published

### Documentary

- linked
- public eligible
- review complete
- blocked/incomplete
- `captured_at` unknown WATCH count

### Impact

- draft
- needs evidence
- verified
- withheld
- legacy public

### Closeout

- `READY`
- `WATCH`
- `BLOCKED`

Blockers and WATCH items are shown explicitly.

There is no bypass COMPLETE button in the closeout manager. The Journey status editor disables `completed` while the readiness model has blockers, and the database trigger is still authoritative against stale/racing UI state.

---

## 9. Source implementation and QA

### Product repository

Repository:

`huynhtranhuythinh/tramnucuoi`

Canonical PR:

`#16 — P11-WU10 post-event evidence and impact closeout`

PR head QA commit:

`807d093bfd72665b61bb0d97e138fa8bf316b4e9`

Merged canonical `main` HEAD:

`69e5cc57b5c76a4c031aa0e576a5a09e316e5b0e`

### Files changed

1. `db/migrations/0030_p11_wu10_post_event_evidence_impact_closeout.sql`
2. `src/components/admin/journeys/closeout-manager.tsx`
3. `src/components/admin/journeys/impact-manager.tsx`
4. `src/components/admin/journeys/journey-editor.tsx`
5. `src/components/admin/journeys/media-manager.tsx`
6. `src/components/journeys/journey-detail-page.tsx`
7. `src/components/journeys/journey-impact.tsx`
8. `src/lib/i18n/dictionary.ts`
9. `src/lib/journeys/admin-queries.ts`
10. `src/lib/journeys/closeout-admin.ts`
11. `src/lib/journeys/closeout.ts`
12. `src/lib/journeys/documentary.ts`
13. `src/lib/journeys/impact.ts`
14. `src/lib/journeys/journeys.server.ts`
15. `src/lib/journeys/schema-capability.ts`
16. `src/routes/_authenticated/admin.journeys.tsx`

No generated Supabase types, environment config, Cloudflare deployment config, Email settings or Turnstile settings were changed.

### CI result

GitHub Actions CI run 92: **PASS**

- dependency install: PASS
- P9-WU7 source abuse-protection QA: PASS
- P10-WU3A Cloudflare runtime-context regression QA: PASS
- P9-WU7 ephemeral database gate/rollback QA: PASS
- `bun run typecheck`: PASS
- `bun run build`: PASS
- `bun run cf:dry-run`: PASS

The Cloudflare step was dry-run only; no Worker production change was made.

### Builder/CTO execution note

Lovable was used for the initial source prototype. The first large request produced no edit, then a smaller migration request produced a prototype commit. Its history was divergent from canonical `main`, and a later Lovable hardening round could not proceed because workspace credits were exhausted. The CTO therefore did **not** merge the divergent Builder branch. Required WU10 code was selectively ported and hardened on a fresh branch from canonical WU9 `main`, reviewed through PR #16, passed CI, then merged.

---

## 10. Production migration record

Production Supabase project:

`iwiqprhoohkxvjyxojto`

Canonical source migration file:

`db/migrations/0030_p11_wu10_post_event_evidence_impact_closeout.sql`

Applied production migration name:

`p11_wu10_post_event_evidence_impact_closeout`

Production migration version:

`20260830150701`

Apply result: **SUCCESS**

---

## 11. Post-migration production verification

Pilot Journey remained unchanged in lifecycle and real-world data:

- status: `registration_open`
- event date: `2026-09-11`
- capacity: `30`
- confirmed participant rows: `1`
- confirmed people: `1`
- attendance recorded rows: `0`
- attendance unresolved rows: `1`
- actual attended people: `0` operational sum with attendance still unresolved; **not an attendance claim**
- verified no-show rows: `0`
- Field Updates: `0`
- linked Journey media: `1`
- impact items: `0`
- impact snapshots: `0`
- closeout review rows: `0`

No post-event evidence was fabricated to test WU10.

RLS remains enabled on:

- `journey_impact_items`
- `journey_impact_snapshots`
- `journey_closeout_reviews`

Required WU10 verification/closeout triggers are present in production, including the authoritative `journeys_closeout_gate`.

`pg_graphql` remains OFF.

Security advisor after migration shows no new Journey/WU10 security lint. The pre-existing Auth warning `Leaked Password Protection Disabled` remains outside WU10 scope.

Performance advisor contains pre-existing informational/warning items and newly created indexes currently appear as unused immediately after deployment; this is expected before post-event usage and is not a security regression.

Email remains OFF. Turnstile remains OFF. No Cloudflare production mutation was performed.

---

## 12. Post-event operator checklist

After the real event on 2026-09-11, staff must use real evidence only:

1. Close registration and move Journey into the existing pre-event/operational `preparing` state using the approved lifecycle.
2. Record attendance per confirmed participant using actual field evidence:
   - `0` only for a verified no-show;
   - `1..party_size` for actual people present;
   - leave `NULL` when unresolved.
3. Resolve attendance gaps where possible; if a confirmed row remains unresolved, document the real reason in the closeout gap note.
4. Create/publish factual Field Updates only when real observations exist. Never create a placeholder Field Update for closeout.
5. Review linked documentary media:
   - correct Journey linkage;
   - evidence category;
   - documentation status;
   - trust/rights state;
   - public visibility;
   - `captured_at` only when known.
6. Enter impact only for claims supported by evidence.
7. For each impact claim choose the real state:
   - `needs_evidence` when evidence is insufficient;
   - `withheld` when intentionally not public, with reason;
   - `verified` only after reviewing real evidence and recording the evidence basis.
8. Review final public narrative and ensure it does not exceed the evidence.
9. Open **ĐÓNG SỔ**, review blockers/WATCH items and record the human closeout attestations.
10. Mark closeout review PASS only when the real post-event evidence review is complete.
11. Attempt `preparing -> completed` only when readiness has no blocker. The database will re-check the gate.

---

## 13. Anomaly handling

### Attendance unresolved

Do not guess. Keep `attended_party_size = NULL` until resolved. Closeout is only possible with an explicit, truthful attendance gap note and staff PASS.

### Impossible attendance

Any value outside `0..party_size` is invalid and blocks closeout. Correct from real evidence; never normalize by guessing.

### Field Update draft remains

Review it. Either publish it when it is factual/public-ready or keep the Journey uncompleted. Do not publish solely to clear a blocker.

### Documentary material untrusted/unclassified

Review the media asset and relation. Sensitive/restricted material may remain private. Do not make media public merely to satisfy closeout.

### `captured_at` unknown

Leave it unknown. It is a WATCH item, not a reason to fabricate a date/time.

### Impact lacks evidence

Use `needs_evidence` or do not create the impact claim. A Journey may close with zero impact claims when evidence does not support them, but unresolved draft/needs-evidence claims must first be reconciled/removed/withheld based on truth.

### Impact intentionally not public

Use `withheld` and record the reason.

### Verified impact later edited

The system demotes the modified claim to `needs_evidence`. Staff must perform a new real-evidence review before verifying it again.

### Evidence/narrative changed after closeout PASS

PASS is invalidated to `pending`. Re-run the review; stale PASS cannot be used to complete the Journey.

### Legacy public impact

Do not call it WU10-verified. Keep the explicit legacy warning until staff conducts a real review and transitions it to an appropriate current state.

---

## 14. Residual risks / non-goals

1. WU10 intentionally did not create destructive/fake post-event QA rows. The authoritative production gate was installed and schema/invariants were verified without fabricating evidence. Full end-to-end closeout will be exercised with real post-event data after 2026-09-11.
2. The existing Auth leaked-password-protection warning remains outside this WU.
3. Existing performance-advisor items remain outside this WU; newly added FK-support indexes may be reported unused until real closeout usage occurs.
4. WU10 does not enable Email, Turnstile, pg_graphql, or change Cloudflare production configuration.
5. WU10 does not close P11-WU6. Live Pilot Operations remains active until the real event ends and post-event evidence is reconciled.

---

## 15. Next Owner gate

**No Owner action is required before the real event solely for WU10.**

Keep P11-WU6 ACTIVE through the 2026-09-11 pilot. After the event, staff should run the real WU10 closeout checklist with actual attendance, factual Field Updates, documentary evidence and evidence-backed impact claims.

The next Owner gate is the **real post-event closeout review**: verify the closeout summary and final public narrative before allowing the pilot Journey to transition from `preparing` to `completed`.

---

## 16. Final WU10 verdict

**P11-WU10 — COMPLETE / PASS**

Reason:

- Registration, confirmation, attendance, evidence and impact remain semantically separated.
- Impact cannot become verified merely because staff entered a value.
- Public impact is verification-gated.
- Admin has a post-event reconciliation/readiness model with concrete blockers.
- `preparing -> completed` is database fail-closed.
- Stale review is invalidated when relevant evidence/narrative changes.
- Production protections and RLS were preserved.
- No fake attendance, Field Update, media or impact was created.
- Pilot did not prematurely enter `preparing` or `completed`.
- Product source, production migration and canonical documentation are synchronized.
