# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU2.3 — JOURNEY INDEX / DETAIL COMPOSITION

Date: 2026-09-23  
Status: **COMPLETE / PASS — SOURCE COMPOSITION REBASE; NO PRODUCTION DDL / FLAG ACTIVATION**

## 1. Objective

P17-WU2.3 recomposes the canonical Journey index/detail around the Phase-17 lifecycle model while production still runs on the legacy `journeys.status` schema.

The WU intentionally avoids querying lifecycle columns that do not yet exist in production.

## 2. Product baseline

Base product main:

`cb18d0a3a8c030b347f215d9b8df7f7d6be485eb`

Branch:

`p17-wu2-3-journey-index-detail-composition`

PR:

`#69 — P17-WU2.3: Journey index and detail composition`

Final PR head:

`c817985c00a6af10f1ad2c094fcae1dc294c1ab4`

Squash-merged product main:

`f463302adf98b0d009bbbc1be21e58e4283d129a`

## 3. Compatibility composition model

Until production lifecycle DDL exists, public composition derives a conservative phase from explicit legacy status only:

| Legacy status | Composition phase |
| --- | --- |
| draft | not public |
| registration_open | upcoming |
| preparing | active |
| completed | closeout_pending |
| archived | not public |

Application state in compatibility mode is always:

`closed`

Therefore legacy `registration_open` does **not** cause the public UI to present recruitment as open.

No dates are used to promote a Journey into active, closeout or Memory composition.

## 4. Journey index composition

Canonical public index groups by:

1. `upcoming`
2. `active`
3. `closeout_pending`
4. `memory`

The old public grouping by:

- open for registration;
- preparing;
- completed

is no longer the composition authority.

Dates, location and capacity remain displayed structured facts.

Lifecycle phase drives labels and CTA semantics.

## 5. Journey detail composition

### Upcoming

Priorities:

- objective/story/context;
- dates/location/capacity;
- participation information;
- Project relationship;
- application-window panel.

Current compatibility application state is fail-closed, so the panel states that applications are not yet open.

Volunteer-vs-standard application mode remains authoritative if/when applications are opened later.

### Active

Priorities:

- schedule;
- official Field Updates;
- operational context;
- Journey-scoped Community;
- interaction/social surfaces only under their existing gates.

Active phase does not create attendance.

### Closeout pending

Priorities:

- explicit reconciliation state;
- documentation/evidence;
- Field Notes;
- no premature Memory/Impact claim.

Legacy `completed` now maps here in compatibility mode.

### Memory

Reserved for post-Closeout composition:

- verified result;
- public Impact when allowed;
- Memory;
- Reflection continuity;
- governed social continuity.

Current production rows cannot enter this phase yet because the new lifecycle columns are not applied.

## 6. Impact / Memory fail-closed behavior

WU2.3 tightened the previous implementation in two layers.

### UI layer

- `JourneySocialContinuity` renders only when canonical phase is `memory`;
- `JourneyImpact` renders only in `memory`;
- personal Memory label is suppressed before `memory`;
- Reflection availability is lifecycle-phase aware.

### Server projection layer

The public Journey detail server no longer fetches/serializes Impact merely because legacy status is `completed`.

Impact fetch is gated by canonical Memory publication authority.

Because current production has no lifecycle columns yet, legacy completed rows remain closeout-pending and public Impact is fail-closed.

## 7. Personal Journey context

`JourneyRelationshipExperience` no longer derives the operating phase from dates.

It now receives the canonical lifecycle phase from Journey composition.

Explicit attendance truth still outranks generic phase presentation:

- attended remains attended;
- verified no-show remains no-show;
- unresolved remains unresolved.

Memory/Reflection presentation remains stricter:

- evidence-backed Memory required;
- canonical `memory` phase required.

## 8. Lifecycle guide

The old 3-stage date-relative guide:

- before;
- during;
- after

was replaced by the Phase-17 operating lifecycle:

- upcoming;
- active;
- closeout_pending;
- memory.

The guide explicitly states that dates do not manufacture:

- attendance;
- Closeout;
- Memory.

## 9. Bilingual composition copy

VI/EN copy now distinguishes:

- Upcoming / Sắp diễn ra
- Active / Đang diễn ra
- Reconciling / Đang đối soát
- Memory / Ký ức

Application copy separately distinguishes:

- closed;
- paused;
- open.

Compatibility mode currently renders application state as closed.

## 10. QA

New QA:

`scripts/p17-wu2-3-journey-composition-qa.ts`

It verifies:

- fail-closed legacy projection;
- no date-derived composition authority;
- no direct legacy completed→Memory path;
- lifecycle-aware index grouping;
- lifecycle-aware detail composition;
- active Field Updates priority;
- Impact server projection blocked before Memory;
- Reflection presentation blocked before Memory;
- bilingual phase/application copy.

Generic CI includes the WU2.3 gate.

## 11. Inherited QA reconciliation

P15/P16 inherited tests were updated only where their prior assertion contradicted the new canonical lifecycle model.

Updated:

- P15-WU5 lifecycle experience QA;
- P15-WU7 post-Journey continuity QA;
- P16-WU7 social continuity QA;
- P16-WU10B authoritative application-mode assertion.

The underlying attendance/Memory/privacy/Volunteer invariants remain enforced.

## 12. Verification evidence

### Final PR-head evidence

Exact PR head:

`c817985c00a6af10f1ad2c094fcae1dc294c1ab4`

Generic CI:

- run `35806619027`;
- conclusion: **SUCCESS**.

Dedicated P16-WU10B Volunteer Pilot Gate:

- run `35806619166`;
- conclusion: **SUCCESS**.

PASS includes:

- WU2.3 composition QA;
- all inherited source gates P9–P16;
- all inherited ephemeral DB gates;
- WU10B Vault/privacy QA;
- build;
- typecheck;
- Cloudflare dry-run.

### Post-merge main evidence

Product main:

`f463302adf98b0d009bbbc1be21e58e4283d129a`

Post-merge main CI:

- run `35806736533`;
- exact head `f463302adf98b0d009bbbc1be21e58e4283d129a`;
- conclusion: **SUCCESS**.

## 13. Production boundary verification

Production Supabase project:

`iwiqprhoohkxvjyxojto`

After WU2.3 merge:

- `public.journeys.lifecycle_phase` does not exist;
- `public.journeys.application_state` does not exist.

Production Journey rows remain on legacy statuses.

Therefore WU2.3 did **not** perform lifecycle schema cutover.

## 14. Explicit non-scope

WU2.3 did not change:

- production Supabase schema/data;
- application RLS or backend submission authority;
- Admin lifecycle controls;
- Department/Team staffing;
- top-level public navigation rebase;
- feature flags;
- Cloudflare production deployment;
- public recruitment activation.

Public recruitment remains:

**HOLD**

## 15. WU2.3 decision

**P17-WU2.3 — COMPLETE / PASS.**

Canonical result:

- Journey index/detail now speaks the Phase-17 lifecycle language;
- legacy status is only a conservative compatibility input;
- dates are no longer lifecycle authority for canonical composition;
- recruitment presentation is fail-closed;
- legacy completed no longer means Memory;
- Impact and social continuity are withheld before Memory;
- active Journey composition prioritizes operational information;
- all inherited evidence/privacy invariants remain green.

Next:

**P17-WU2.4 — PUBLIC NAVIGATION REBASE**
