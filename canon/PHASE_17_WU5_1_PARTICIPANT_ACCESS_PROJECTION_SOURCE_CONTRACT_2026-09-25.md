# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU5.1 — PARTICIPANT ACCESS / PROJECTION SOURCE CONTRACT

Date: 2026-09-25  
Status: **COMPLETE / PASS — SOURCE CONTRACT READY; PRODUCTION RPCs NOT YET CUT OVER**

## 1. Objective

Lock the participant-facing access authority and safe database projection contract for P17-WU5 without broadening WU3/WU4 operational source-table access.

## 2. Starting truth

WU5.0 architecture is canonical.

Product base:

`a47c099c4b5f1af6c162e75a0f16c5059222a7cb`

Production recruitment remained HOLD / CLOSED.

## 3. Access authority

WU5 participant access is authorized only when:

- caller is authenticated;
- an active `community_participant_links` row belongs to `auth.uid()`;
- the linked `journey_participants` row belongs to the requested Journey;
- participant remains `status = confirmed`.

Account/CMS role is not participant authority.

A stale identity link does not preserve access after participant withdrawal.

P12 Memory/attendance projection is not operational workspace authority.

## 4. Source contract

Added:

`database/contracts/p17_wu5_1_participant_workspace_projection.sql`

Source-first only. No production migration.

Canonical public APIs:

- `public.tnc_my_journey_workspaces()`;
- `public.tnc_my_journey_plan(uuid)`;
- `public.tnc_my_journey_official_updates(uuid)`.

Privileged implementation remains in non-exposed `private` functions with fixed empty search path and self-authorization.

Public wrappers remain SECURITY INVOKER.

Execution is revoked from public/anon and granted only to authenticated/service_role.

No new direct participant grants are added to WU3/WU4 operational source tables.

## 5. Workspace projection

The workspace returns caller-owned confirmed Journey participation plus optional CURRENT P17 assignment only.

Current assignment:

`ended_at IS NULL`

A confirmed participant with no current assignment is a valid workspace row with null:

- Journey Role;
- Department;
- Team.

The projection excludes application PII, review metadata, assignment note/actor/history, attendance and Memory/Reflection/Impact truth.

## 6. Plan projection

`tnc_my_journey_plan(journey_id)` exposes:

- Journey-wide Runbook items to any eligible participant;
- Department items only to matching current Department;
- Team items only to matching current Team.

Unassigned participants receive Journey-wide items only.

Source `journey_runbook_items` remains Admin-controlled.

## 7. Official Update projection

`tnc_my_journey_official_updates(journey_id)` delivers only:

- `status = published`;
- currently effective rows;
- audience-matched content.

Audience matching:

- all participants;
- current Journey Role;
- current Department;
- current Team.

Draft, future-effective and audience-mismatched rows remain hidden.

Official Update remains distinct from Community.

## 8. QA

Added:

- `scripts/p17-wu5-1-participant-workspace-projection-qa.ts`;
- `scripts/p17-wu5-1-participant-workspace-projection-schema-qa.sql`.

CI coverage proves:

- cross-user isolation;
- withdrawn participant loses workspace;
- unlinked authenticated user gets no participant data;
- current assignment only;
- valid unassigned workspace;
- Plan Journey/Department/Team scoping;
- Official Update role/Department/Team audience scoping;
- draft and future-effective exclusion;
- anonymous wrapper execution denied;
- direct participant source-table reads remain closed;
- recruitment remains closed;
- no attendance or Memory mutation.

## 9. Exact-head evidence

PR:

`#94 — P17-WU5.1: Participant workspace projection contract`

Exact PR head:

`ecb1cfaab6a252796b386eef5a5cecfdbba85b01`

Exact-head gates:

- generic CI `36100096269`: SUCCESS;
- dedicated P16-WU10B gate `36100096311`: SUCCESS;
- WU5.1 source QA: PASS;
- WU5.1 ephemeral DB QA: PASS;
- inherited gates: PASS;
- build: PASS;
- typecheck: PASS;
- Cloudflare dry-run: PASS.

## 10. Merge / post-merge evidence

PR #94 squash-merged.

Product main:

`f5f62069abfd8c8af75915a4c8bd715c31a823bf`

Post-merge main CI:

- run `36100208621`;
- exact head `f5f62069abfd8c8af75915a4c8bd715c31a823bf`;
- conclusion: SUCCESS.

Cloudflare Git integration builds for main and staging also completed successfully.

## 11. Production state

No WU5 projection migration has been applied to production.

WU5.1 is source-contract only.

Application Windows remain CLOSED.

No production participant, assignment, attendance, Memory, Reflection or Impact row was mutated.

## 12. Decision

# **P17-WU5.1 — COMPLETE / PASS**

Proceed:

# **P17-WU5.2 — MY JOURNEY ENTRY / JOURNEY WORKSPACE SHELL**
