# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 12 — LIVING COMMUNITY OS FOUNDATION
# CANONICAL EVIDENCE PACK

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Purpose

This file is the Phase 12 evidence index. It does not replace the individual canonical work-unit records. It provides one auditable chain from source → CI → production migration → postflight → closeout.

Canonical Phase 12 product graph:

**Identity → Journey → Memory → Reflection → Contribution → Community Roles → Host Network → Partner / Impact Network**

Domain-level invariant:

**Person ↔ Journey ↔ Project ↔ Memory ↔ Contribution ↔ Community Relationship ↔ Impact Network**

Phase 12 deliberately did not fabricate real-world facts to demonstrate the software.

## 2. Repository evidence

Product repository:
- `huynhtranhuythinh/tramnucuoi`
- canonical branch for Phase 12: `main`
- final Phase 12 product HEAD: `75f9511de8442fcd632429b21cfc56fb727aed7b`

Canonical documentation repository:
- `huynhtranhuythinh/tramnucuoi-docs`
- Phase 12 final closeout merge before this evidence addendum: `0206d603266f504e3bd504d68f28b432c4639a6a`

Production Supabase:
- project ref: `iwiqprhoohkxvjyxojto`

Production website:
- `https://tramnucuoi.com`

## 3. Work-unit evidence ledger

### P12-WU1 — Community Identity & Journey Link Foundation

Status: **COMPLETE / PASS**

Product evidence:
- PR #18
- merge SHA: `e48262879be0c7d4448dbfceb2c69147f982e64f`
- migration source: `db/migrations/0032_p12_wu1_community_identity_foundation.sql`

Production migration:
- `20260831004142 p12_wu1_community_identity_foundation`

Canonical record:
- `canon/PHASE_12_WU1_COMMUNITY_IDENTITY_JOURNEY_LINK_2026-08-31.md`

Trust evidence:
- identity link remains additive and separate from operational participant snapshot;
- no historical `journey_participants.user_id` rewrite;
- zero fake identity links in production at closeout.

### P12-WU2 — Community Account Onboarding & Participant Claim

Status: **IMPLEMENTATION COMPLETE / PASS — ACTIVATION GATED**

Product evidence:
- initial PR #19
- security hotfix PR #20
- final WU2 product SHA: `f648238f37cbf695330d7a0b74ba6f5e432a82b1`
- final claim RPC architecture uses `SECURITY INVOKER` at public boundary.

Production migrations:
- `20260831005435 p12_wu2_verified_email_participant_claim`
- `20260831005907 p12_wu2_claim_security_invoker_hotfix`

Canonical record:
- `canon/PHASE_12_WU2_COMMUNITY_ONBOARDING_PARTICIPANT_CLAIM_2026-08-31.md`

Activation evidence:
- Community account implementation exists;
- public activation remains gated while canonical Email delivery is OFF;
- no production navigation promotion or Cloudflare activation was claimed.

### P12-WU3 — My Journey Memory

Status: **COMPLETE / PASS — UX ACTIVATION INHERITS WU2 EMAIL GATE**

Product evidence:
- primary PR #21
- FK/index follow-up PR #22
- final WU3 product SHA: `e32fedfda6bcba0773f7e21bccd0409aadb59215`

Production migrations:
- `20260831012010 p12_wu3_my_journey_memory_projection`
- `20260831012223 p12_wu3_memory_fk_indexes`

Canonical record:
- `canon/PHASE_12_WU3_MY_JOURNEY_MEMORY_2026-08-31.md`

Attendance semantics proven and preserved:
- `NULL` = unresolved;
- `0` = verified no-show;
- `>0` = verified attended / Memory eligible.

Production Memory rows remained zero because real attendance had not occurred.

### P12-WU4 — Journey-native Community Interaction

Status: **COMPLETE / PASS**

Product main checkpoint:
- `b94692c544d5703f7052971ac818b69c5e1e1eb8`

Production migration:
- `20260831014537 p12_wu4_journey_reflections_foundation`

Canonical record:
- `canon/PHASE_12_WU4_JOURNEY_NATIVE_COMMUNITY_INTERACTION_2026-08-31.md`

Trust evidence:
- Reflection requires evidence-backed attended Memory and completed Journey;
- moderation precedes public publication;
- generic feed/follower/chat primitives were not introduced;
- production Reflection rows remained zero.

### P12-WU5 — Contribution History

Status: **COMPLETE / PASS**

Product evidence:
- PR #24
- product main checkpoint: `ec525c889c8e931f7fe2ab3ab50c91a53f7633aa`

Production migration:
- `20260831020939 p12_wu5_contribution_history`

Canonical record:
- `canon/PHASE_12_WU5_CONTRIBUTION_HISTORY_2026-08-31.md`

Trust evidence:
- attendance is not contribution;
- future promise is not contribution history;
- quantities preserve explicit units;
- unlike units are not blindly aggregated;
- verified facts use revoke → replacement correction;
- production Contribution rows remained zero.

### P12-WU6 — Community Roles & Host Network

Status: **COMPLETE / PASS**

Product evidence:
- PR #25
- product main checkpoint: `a0467fabcac57a7b1ee9853bef51dd103e3b3b30`

Production migration:
- `20260831024125 p12_wu6_community_roles_host_network`

Canonical record:
- `canon/PHASE_12_WU6_COMMUNITY_ROLES_HOST_NETWORK_2026-08-31.md`

Canonical role model:
- Explorer;
- Participant;
- Contributor;
- Host;
- Partner representative.

Authorization invariant:

> **Community role does not equal CMS permission.**

`app_role` remains exactly `admin | editor`.

### P12-WU7 — Impact Network

Status: **COMPLETE / PASS**

Product evidence:
- PR #26
- final product main SHA: `75f9511de8442fcd632429b21cfc56fb727aed7b`
- migration source: `db/migrations/0040_p12_wu7_impact_network.sql`
- dedicated DB QA: `scripts/p12-wu7-db-qa-v2.sql`

Production migration:
- `20260831053522 p12_wu7_impact_network`

Canonical record:
- `canon/PHASE_12_WU7_IMPACT_NETWORK_CLOSEOUT_2026-08-31.md`

Trust evidence:
- Organization existence does not assert partnership/funding/endorsement/impact;
- Organization ↔ Project relationship requires explicit verification;
- Partner representative remains a personal relationship and never creates CMS permission;
- verified impact requires currently valid provenance;
- provenance sources are constrained to recorded attendance, active verified Contribution, reviewed documentary media, or verified Partner relationship;
- revoking provenance fails closed by demoting stale verified impact to `needs_evidence`;
- `legacy_public` remains compatibility content and is not relabeled as WU7-verified impact;
- no Impact Score or cross-unit blind aggregation was introduced.

## 4. Final CI evidence

Post-merge product main CI run #137 on SHA:
- `75f9511de8442fcd632429b21cfc56fb727aed7b`

Result: **PASS**

Verified gates:
- Phase 9 abuse/protection QA: PASS
- Phase 10 runtime-context regression QA: PASS
- Phase 11 transactional capacity/cutoff QA: PASS
- P12-WU1 DB QA: PASS
- P12-WU2 DB QA: PASS
- P12-WU3 DB QA: PASS
- P12-WU4 DB QA: PASS
- P12-WU5 DB QA: PASS
- P12-WU6 DB QA: PASS
- P12-WU7 DB QA: PASS
- build: PASS
- typecheck: PASS
- Cloudflare dry-run: PASS

No Cloudflare production deployment was required for WU7.

## 5. Final production postflight evidence

Production remained fact-clean after WU7 for Community OS facts that had not happened in the real world:

- Community Profiles: `0`
- Community participant links: `0`
- Memories: `0`
- Reflections: `0`
- Contributions: `0`
- Community relationship assignments: `0`
- Community relationship audit events: `0`
- Partner Organizations: `0`
- Organization/Project relationships: `0`
- Partner representative/Organization bridges: `0`
- Impact provenance links: `0`

Existing impact compatibility state:
- `legacy_public` Journey impact items: `4`
- WU7 verified Journey impact items: `0`
- `legacy_public` Journey impact snapshots: `1`
- WU7 verified Journey impact snapshots: `0`

`pg_graphql`: OFF.

## 6. Pilot continuity evidence

P11-WU6 — LIVE PILOT OPERATIONS remains authoritative and ACTIVE.

Pilot Journey:
- ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- title: `Trạm Cơm Chay Yêu Thương — Đổi Nụ Cười · Mùng 1 Tháng 8`
- event date: `2026-09-11`
- status: `registration_open`
- capacity: `30`
- confirmed rows: `1`
- confirmed people: `1`
- attendance: unresolved

Phase 12 did not pre-create attendance, Memory, Reflection, Contribution, Host/Partner facts, provenance or verified impact for this Journey.

## 7. Security / advisor evidence

Closeout runtime guards:
- Email: OFF
- Turnstile: OFF
- Community Auth public activation: GATED
- `pg_graphql`: OFF
- CMS authorization: `admin | editor` only

Security Advisor after WU7:
- no WU7-specific new security lint;
- existing project-level warning remains `Leaked Password Protection Disabled`.

Performance Advisor after WU7:
- no WU7 unindexed-FK regression;
- new WU7 indexes may appear as `unused_index` while new tables contain zero rows; this is accepted as expected foundation state and is not evidence that the indexes should be removed.

## 8. Canonical document chain

Read in this order for Phase 12:

1. `canon/PHASE_12_COMMUNITY_OS_CONTINUATION_PLAN_2026-08-31.md`
2. `canon/PHASE_12_WU1_COMMUNITY_IDENTITY_JOURNEY_LINK_2026-08-31.md`
3. `canon/PHASE_12_WU2_COMMUNITY_ONBOARDING_PARTICIPANT_CLAIM_2026-08-31.md`
4. `canon/PHASE_12_WU3_MY_JOURNEY_MEMORY_2026-08-31.md`
5. `canon/PHASE_12_WU4_JOURNEY_NATIVE_COMMUNITY_INTERACTION_2026-08-31.md`
6. `canon/PHASE_12_WU5_CONTRIBUTION_HISTORY_2026-08-31.md`
7. `canon/PHASE_12_WU6_COMMUNITY_ROLES_HOST_NETWORK_2026-08-31.md`
8. `canon/PHASE_12_WU7_IMPACT_NETWORK_CLOSEOUT_2026-08-31.md`
9. `canon/PHASE_12_FINAL_CLOSEOUT_2026-08-31.md`
10. this evidence pack.

## 9. Final evidence declaration

**P12-WU1 → P12-WU7: EVIDENCE CHAIN COMPLETE**

**PHASE 12 — LIVING COMMUNITY OS FOUNDATION: COMPLETE / PASS**

**NO PHASE 12 FACT FABRICATION DETECTED AT CLOSEOUT**

**P11-WU6 LIVE PILOT OPERATIONS: ACTIVE**
