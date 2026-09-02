# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU4 — JOURNEY PRESENCE & SHARED-EXPERIENCE GRAPH

Date: 2026-09-02  
Status: **SOURCE / ARCHITECTURE PREPARED — PR #53 QA PENDING**  
Production database mutation: **NONE IN WU4 SO FAR**  
Production Shared-Experience Graph UX: **OFF**

## 1. PURPOSE

P16-WU4 introduces the evidence and consent boundary required to move from temporary Journey digital presence toward durable relationship continuity after a real Journey.

The work unit does not create a Friend graph, follower graph or generic people network.

The governing distinction is:

> **Journey Presence is a digital visibility choice. Shared Experience is a real-world claim and requires attendance evidence.**

A person may join a Journey Community Room before the Journey occurs. That fact alone can never create a claim that two people met, attended or went together.

## 2. GOVERNING CANON

> **TRẠM NỤ CƯỜI is a Journey-Based Social Network.**
>
> **Journey — not Post and not Person — is the primary social object.**
>
> **Digital relationships may begin around preparation for a real Journey, but claims of shared real-world experience arise only from appropriate evidence.**
>
> **Public/social visibility is always separate from private operational truth.**

WU4 adds these immutable equations:

- `Journey Presence != attendance`
- `Journey Presence != shared experience`
- `same Journey Room != went together`
- `registration != attendance`
- `confirmed registration != attendance`
- `Journey date elapsed != attendance`
- `shared-experience evidence != relationship visibility consent`
- `social visibility != operational truth`

## 3. TWO-LAYER MODEL

WU4 deliberately separates evidence from visibility.

### 3.1 Evidence layer

`journey_shared_experience_edges`

This is a system-derived social projection of already-existing operational Journey truth.

A verified pair edge may exist only when both endpoints are known social identities whose accounts are linked through active participant identity links to confirmed Journey participant rows with verified positive attendance.

Required attendance condition for each endpoint:

- `attended_party_size > 0`
- `attendance_recorded_at IS NOT NULL`
- `attendance_recorded_by IS NOT NULL`

The edge table stores social UUIDs and Journey scope only. It deliberately excludes:

- auth user UUIDs;
- participant IDs;
- application IDs;
- attendance actor UUIDs;
- registration PII;
- Memory IDs.

Clients cannot create, update or delete evidence edges.

### 3.2 Visibility / consent layer

`journey_relationship_consents`

This is a user-controlled per-Journey social visibility choice.

It does not create evidence.

A user may enable relationship visibility only after the user's own verified positive attendance exists for that Journey.

Initial scopes:

- `private`
- `journey_only`

A social shared-experience connection becomes visible only when both people have active `journey_only` relationship consent.

## 4. MUTUAL-CONSENT RULE

A verified evidence edge is not automatically socially visible.

Visibility requires all of the following:

1. verified evidence edge for the same Journey;
2. endpoint A has an enabled social identity;
3. endpoint B has an enabled social identity;
4. A has active `journey_only` relationship consent;
5. B has active `journey_only` relationship consent;
6. no block exists between A and B;
7. neither identity is hidden/suspended by social moderation.

Therefore:

> **Evidence may exist privately while social relationship visibility remains off.**

This preserves the rule that operational truth and publication/social consent are separate decisions.

## 5. EVIDENCE SOURCE

Canonical evidence path:

`social_identity -> community_participant_links(active) -> journey_participants(confirmed + verified positive attendance)`

The graph projection MUST NOT derive from:

- `journey_social_presences`;
- Journey registration/application;
- confirmed registration by itself;
- account creation;
- profile existence;
- Journey date/lifecycle;
- Memory existence alone;
- documentary media alone;
- reflection publication alone;
- Contribution records;
- role assignment alone.

## 6. PARTY-SIZE / UNKNOWN-PERSON RULE

`attended_party_size` is an attendance count, not a people identity list.

If a participant record says two people attended but only one known account/social identity is linked to that participant record, WU4 creates only the known graph node.

WU4 MUST NOT fabricate unnamed guests or infer identities from party size.

A later social identity may enter the graph only when a valid existing account-to-participant link proves which known person that identity represents.

## 7. EDGE LIFECYCLE

Evidence edge states:

- `verified`
- `revoked`

### Verified

Both endpoints currently satisfy the attendance truth contract.

### Revoked

A later operational correction means the pair no longer satisfies that contract.

The edge is not silently deleted. Revocation preserves historical/provenance awareness while preventing a false current shared-experience claim.

If attendance is later re-verified appropriately, the edge may return to `verified`.

Social block/withdrawal/moderation does NOT revoke an evidence edge because those are visibility choices, not corrections to operational attendance truth.

## 8. JOURNEY PRESENCE CONTINUITY

WU3 Journey Presence is intentionally temporary digital context.

A person may leave a Journey Room after the Journey.

That withdrawal does not erase verified shared experience.

Therefore WU4 extends WU2 identity-card visibility so that two people may continue seeing the permitted social identity card after Journey Presence is withdrawn only when:

- a verified shared-experience edge exists; and
- mutual relationship visibility consent exists; and
- block/moderation does not suppress it.

This is the first evidence-backed continuity bridge in the P16 retention loop.

## 9. BLOCK / REPORT / MODERATION

WU2 safety primitives remain authoritative.

### Block

Block suppresses the social graph in both directions.

It does not modify:

- participant rows;
- attendance;
- Memory;
- Reflection;
- Contribution;
- evidence edge state.

### Report

A visible graph connection can be reported through the existing reporter-private WU2 report model.

### Moderation

Identity hide/suspension suppresses graph visibility.

Moderation cannot grant consent or create/rewrite attendance evidence.

## 10. RELATIONSHIP CONSENT AUDIT

WU4 extends the existing `social_consent_events` vocabulary with:

- `journey_relationship_enabled`
- `journey_relationship_visibility_changed`
- `journey_relationship_withdrawn`

These events are social consent history only.

They are NOT attendance evidence.

Clients cannot write consent audit rows directly.

## 11. BROWSER / DATA-API BOUNDARY

The WU4 browser UX reads only governed social projections:

- own `social_identities` source;
- own `journey_relationship_consents`;
- RLS-filtered `journey_shared_experience_edges`;
- allowed `social_identity_cards`;
- WU2 `social_blocks` / `social_reports`.

The browser does not calculate shared experience by reading:

- `journey_participants`;
- `community_participant_links`;
- applications;
- Memory;
- operational profiles.

Evidence derivation remains server/database-side.

## 12. RLS / PRIVILEGE CONTRACT

Both new public tables MUST have RLS enabled.

Anonymous access: none.

### `journey_relationship_consents`

Authenticated user:

- SELECT own row;
- INSERT own row only after verified positive attendance;
- UPDATE own row;
- cannot create consent for another person.

Admin may inspect consent state but may not grant another user's consent.

### `journey_shared_experience_edges`

Authenticated clients:

- SELECT only through mutual-consent/safety RLS;
- no INSERT;
- no UPDATE;
- no DELETE.

System/service functions maintain evidence projection.

## 13. UI CONTRACT

Canonical VI concept:

**TRẢI NGHIỆM CHUNG ĐÃ XÁC MINH**

A connection must communicate both dimensions:

- real Journey experience has appropriate evidence;
- social visibility is mutual consent.

Allowed wording after evidence + consent:

- `Trải nghiệm chung trong Journey đã xác minh`
- `Verified shared Journey experience`

Forbidden without evidence:

- `Đã cùng đi`
- `Đã tham dự cùng nhau`
- `Went together`
- `Attended together`

Journey Presence copy continues to explicitly state that digital presence is not attendance proof.

## 14. TRUTHFUL EMPTY STATE

An empty graph does NOT mean no one participated.

It may mean:

- attendance is unresolved;
- fewer than two known social identities have verified attendance;
- relationship visibility is not mutually consented;
- an identity is disabled;
- a block/moderation control suppresses visibility.

WU4 must never fill this empty state with synthetic people or synthetic relationships.

## 15. REAL JOURNEY — 2026-09-11

Journey id:

`19539f36-3ed4-4a22-96b9-c8a9b73c5283`

At WU4 implementation time:

- participant rows: 1;
- attendance unresolved: 1;
- verified attended people: 0;
- WU2 social identities: 0;
- Journey social presences: 0.

Therefore the only truthful WU4 result now is:

- shared-experience edge count: 0;
- visible shared-experience graph: empty/off.

WU4 architecture and QA can be completed before 2026-09-11, but no actual “went together” edge may be asserted until real attendance evidence exists.

P14-WU5 remains the independent real-Journey evidence lane.

## 16. RELEASE GATE

WU4 adds a third fail-closed feature flag:

`VITE_APP_SHARED_EXPERIENCE_GRAPH_ENABLED`

The graph requires all three layers:

1. Community Auth enabled;
2. Journey Community Room enabled;
3. Shared-Experience Graph enabled.

Source merge therefore does not automatically activate the graph.

Prepared release commands keep the graph independently controllable from the Room.

## 17. SOURCE ARTIFACTS

Product branch:

`p16-wu4-journey-presence-shared-experience-graph`

Base `main` SHA:

`2683e937accde7b8c1e22acca2fd87ff3ed736f2`

Pull request:

`#53 — P16-WU4: Journey Presence and shared-experience graph`

Prepared artifacts:

- `db/migrations/0046_p16_wu4_journey_presence_shared_experience_graph.sql`
- `db/rollbacks/p16_wu4_journey_presence_shared_experience_graph.sql`
- `scripts/p16-wu4-db-qa.sql`
- `scripts/p16-wu4-shared-experience-graph-qa.ts`
- `src/lib/journeys/shared-experience-activation.ts`
- `src/components/journeys/journey-shared-experience-graph.tsx`
- `src/components/journeys/journey-detail-page.tsx`
- `.github/workflows/ci.yml`
- `package.json`

## 18. PRODUCTION MUTATION GATE

No P16-WU4 DDL has been applied to canonical Supabase at this status.

Migration `0046` requires an explicit production migration gate after source/ephemeral QA passes.

Before production migration:

- capture baseline Journey/social truth;
- run advisors;
- apply only reviewed migration SQL;
- verify new table RLS/grants;
- verify real 2026-09-11 Journey has zero evidence edges while attendance is unresolved;
- verify participant/attendance/Memory truth unchanged;
- run Security and Performance Advisors again.

Shared-Experience Graph UX remains OFF until production DB verification passes and a later activation gate is explicitly approved.

## 19. NEXT CLOSEOUT CONDITIONS

WU4 source/architecture may close only after:

1. dedicated WU4 source QA PASS;
2. WU4 ephemeral DB migration/behavior/rollback QA PASS;
3. inherited P9–P16-WU3 regression QA PASS;
4. build PASS;
5. typecheck PASS;
6. Cloudflare dry-run PASS;
7. PR merged to `main`;
8. post-merge `main` CI PASS.

Production DB activation remains a separate explicit gate if it has not yet been approved/applied at source closeout.

Next product work after WU4 architecture/production foundation is validated:

**P16-WU5 — INTERACTION V1: JOURNEY QUESTION / REPLY / APPRECIATION**

WU5 must use Journey-scoped identity/safety boundaries and must not introduce generic posting or popularity mechanics.
