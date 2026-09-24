# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU3.6 — EXISTING JOURNEY ADMIN RECOMPOSITION

Date: 2026-09-24  
Status: **COMPLETE / PASS — EXISTING JOURNEY ADMIN TOOLS RECOMPOSED INTO CONTROL CENTER**

## 1. Objective

Recompose the existing Journey Admin tools into one selected Journey Control Center without rewriting their source truth, authorization model or business semantics.

Canonical operating direction:

> /admin/journeys is the Journey Portfolio.  
> Existing Journey operations belong inside that Journey's Control Center.

Public recruitment remains **HOLD / CLOSED**.

## 2. Starting truth

Product base:

`9df3676d85edb612ca691cba85f93b0cac9f13fe`

WU3.0 through WU3.5 were already COMPLETE / PASS.

The dedicated Control Center route already existed:

`/admin/journeys/:journeyId`

Before WU3.6, however, `/admin/journeys` still opened multiple operational managers directly below the full Journey list.

## 3. Fragmentation found

The Journey Portfolio still exposed independent existing-Journey actions for:

- lifecycle/application controls;
- Application / Volunteer Recruitment;
- Field Updates;
- Media;
- Impact;
- Closeout;
- related Field Journal;
- Journey content edit;
- Journey delete.

This meant the product had a canonical Journey Control Center route but retained the legacy fragmented operating experience in parallel.

WU3.6 removes that parallel operating surface.

## 4. Portfolio after recomposition

Updated:

`src/routes/_authenticated/admin.journeys.tsx`

The route now serves only as a Journey Portfolio:

- list Journeys;
- show lifecycle/application state as read-only context;
- create a new Journey draft;
- enter the selected Journey Control Center.

For existing Journeys, the primary operating action is:

`CONTROL CENTER`

The Portfolio no longer directly mounts:

- JourneyLifecycleControls;
- ApplicationManager;
- VolunteerRecruitmentWorkspace;
- FieldUpdateManager;
- JourneyMediaManager;
- JourneyFieldNoteManager;
- JourneyImpactManager;
- JourneyCloseoutManager.

It also no longer surfaces the destructive Journey delete action.

New Journey creation remains allowed, but the form is explicitly content/metadata only. It does not control lifecycle phase or application authority.

## 5. Existing Journey composition layer

Added:

`src/components/admin/journeys/journey-existing-admin-manager.tsx`

This is deliberately a **composition-only layer**.

It reuses the existing managers:

- `JourneyEditor`;
- `ApplicationManager`;
- `VolunteerRecruitmentWorkspace`;
- `FieldUpdateManager`;
- `JourneyMediaManager`;
- `JourneyFieldNoteManager`;
- `JourneyImpactManager`;
- `JourneyCloseoutManager`.

The composition component introduces no direct Supabase table query and no new source-of-truth model.

Existing manager code continues to own its own:

- queries;
- mutations;
- RLS behavior;
- validation;
- publication rules;
- evidence semantics;
- closeout semantics.

## 6. Control Center recomposition

Updated:

`src/components/admin/journeys/journey-control-center.tsx`

The selected Journey now hosts:

`<JourneyExistingAdminManager journey={journey} isAdmin={isAdmin} />`

alongside the WU3.2-WU3.5 areas.

Updated Control Center map in:

`src/lib/journeys/control-center.ts`

The following existing-foundation areas are now active:

- `Thiết lập — WU3.6`;
- `Người & đăng ký — WU3.6 / WU4`;
- `Điểm danh & Tư liệu — WU3.6 / WU8`;
- `Đóng sổ & Kết quả — WU3.6 / WU9`.

This marks current composition authority only. Later WUs still own deeper redesign/extensions for their domains.

## 7. Authority boundaries

### Lifecycle and Application

WU2 lifecycle/application authority remains canonical.

The Control Center continues to reuse:

`JourneyLifecycleControls`

The protected Application OPEN path remains:

`openJourneyApplicationWindowFromAdmin(...)`

The Portfolio does not reimplement lifecycle/application writes.

### People & Applications

Application / participant / attendance operational data remains restricted.

The WU3.6 composition layer exposes People & Applications only when:

`isAdmin === true`

For volunteer Journeys, the existing:

`VolunteerRecruitmentWorkspace`

is reused.

For standard Journeys, the existing:

`ApplicationManager`

is reused.

Existing `application_mode` remains the selector.

WU3.6 does not reinterpret legacy:

- `preferred_team`;
- `assigned_team`.

### Closeout

Existing Closeout review remains Admin-only.

Global Editor is not inferred as Journey BTC authority.

## 8. Existing truth models preserved

WU3.6 deliberately reuses rather than rewrites:

### Journey Setup
Existing `JourneyEditor` and `upsertJourney`.

`upsertJourney` continues to strip:

- `lifecycle_phase`;
- `application_state`;
- legacy `status`;

from generic content edits.

### Field Updates
Existing Field Update truth remains factual field observation / public publication content.

It is **not** merged with WU3.5 Official Update.

Official Update remains operational communication.

### Media
Existing Journey Media relation truth is unchanged.

### Field Journal
Existing Journey ↔ Field Journal relation truth is unchanged.

### Impact
Existing hand-authored / verification-aware impact truth is unchanged.

WU3.6 does not infer impact from applications, participants, attendance or media.

### Closeout
Existing closeout review remains a human attestation and does not become an impact calculation or automatic lifecycle transition.

## 9. No duplicate source truth

The WU3.6 composition component does not:

- call `.from(...)`;
- instantiate the backend client;
- create SQL;
- create tables;
- mutate production schema.

Its purpose is product composition only.

## 10. Security / privacy protection

Restricted tools are gated in the composition layer:

- People & Applications — Admin-only;
- Closeout — Admin-only.

Existing RLS remains final database authority.

WU3.6 does not expand participant data exposure and does not infer Journey BTC authority from the global Editor role.

## 11. Inherited WU2.5 gate reconciliation

The first PR run exposed a stale inherited WU2.5 assertion.

Old assertion required lifecycle/application controls to remain physically inside:

`/admin/journeys`

That no longer represented canonical architecture after WU3.2 created the dedicated per-Journey Control Center.

Failure:

`P17-WU2.5 Admin lifecycle QA failed: Admin Journey route missing: <JourneyLifecycleControls`

This was a stale location assertion, not a lifecycle regression.

The WU2.5 gate was repaired semantically so it now protects:

- Portfolio remains lifecycle-aware and links to the dedicated Control Center;
- lifecycle/application write authority lives in the selected Journey route;
- `JourneyLifecycleControls` remains the canonical UI authority surface;
- Application OPEN still uses the protected activation path;
- recomposed application managers receive canonical lifecycle phase;
- legacy registration activation remains absent.

No lifecycle/security invariant was weakened.

## 12. WU3.6 QA

Added:

`scripts/p17-wu3-6-existing-admin-recomposition-qa.ts`

and wired into:

`.github/workflows/ci.yml`

The gate locks:

- Portfolio → Control Center architecture;
- removal of legacy inline existing-Journey operations from Portfolio;
- new Journey creation remains content-only;
- reuse of all existing Journey managers;
- no duplicate DB/source truth in composition layer;
- Admin-only People / Closeout;
- Editor != Journey BTC;
- existing standard/volunteer application manager selection;
- no legacy team reinterpretation;
- active WU3.6 Control Center map areas;
- WU2 lifecycle/application authority retention;
- protected Application OPEN path;
- no production migration smuggling.

## 13. Exact-head CI evidence

Branch:

`p17-wu3-6-existing-admin-recomposition`

PR:

`#83 — P17-WU3.6: Existing Journey Admin recomposition`

Final PR head:

`6e470c7e5a3663f3a964a22fd41e79d61aed9744`

Exact-head gates:

- generic CI `35997471137`: **SUCCESS**
- P16-WU10B Volunteer Pilot Gate `35997471158`: **SUCCESS**
- repaired P17-WU2.5 lifecycle gate: **PASS**
- P17-WU3.6 Existing Journey Admin recomposition QA: **PASS**
- P17-WU3.1–WU3.5 inherited source/schema QA: **PASS**
- all inherited source gates: **PASS**
- all inherited ephemeral DB regressions: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

## 14. Merge / post-merge evidence

PR #83 was squash-merged.

Product main:

`b47033de09b5cb642bc45100d32720e474e0c969`

Post-merge main CI:

- run `35997651998`;
- exact head `b47033de09b5cb642bc45100d32720e474e0c969`;
- conclusion: **SUCCESS**.

## 15. Production verification

Production Supabase:

`iwiqprhoohkxvjyxojto`

Post-merge read-only verification:

- `journey_departments`: absent;
- `journey_teams`: absent;
- `journey_runbook_items`: absent;
- `journey_official_updates`: absent;
- WU3 Event Management production tables: **0/4**;
- non-closed application windows: **0**;
- Journeys: **5**.

No production mutation was performed by WU3.6.

## 16. Decision

**P17-WU3.6 — COMPLETE / PASS.**

Next:

**P17-WU3.7 — SECURITY / REGRESSION / MOBILE ADMIN QA**

Public recruitment remains:

**HOLD / CLOSED**
