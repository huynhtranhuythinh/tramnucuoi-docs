# TRẠM NỤ CƯỜI — PHASE 12
# COMMUNITY OS CONTINUATION PLAN

Date: 2026-08-31
Status: ACTIVE

## 1. Why Phase 12 exists

Phase 12 restores the original product trajectory after the production-readiness work required to safely operate the first real Journey.

The North Star is not an event-management system and not an impact-reporting system by themselves.

It is a **Living Community Operating System** in which real relationships accumulate through:

**People → Journey → Experience → Memory → Community Relationship → Contribution → Impact Network**

Journey/Event Management is the operational entry point into that loop.

## 2. What we are explicitly not building

TRẠM NỤ CƯỜI is not becoming a generic social network.

Do not prioritize:
- follower counts;
- a generic infinite feed;
- private chat as a platform goal;
- engagement gamification detached from real-world activity;
- social features without a Journey/Memory/Contribution context.

## 3. Parallel relationship with P11-WU6

P11-WU6 — LIVE PILOT OPERATIONS remains ACTIVE for the real Journey on 2026-09-11 and verified post-event closeout.

Phase 12 may progress in parallel only when it does not fabricate or pre-empt pilot facts.

Before the real event we may build identity/account/memory infrastructure, but we must not manufacture:
- attendance;
- no-show facts;
- personal Memories;
- documentary evidence;
- impact claims.

## 4. Canonical Phase 12 sequence

### P12-WU1 — Community Identity & Journey Link Foundation — COMPLETE / PASS
Create a privacy-safe, verified relationship between an authenticated person and an operational Journey participant snapshot.

Key rule: do **not** rewrite `journey_participants.user_id` for historical/anonymous participation. Journey operational truth remains intact; Community identity is a separate verified relationship.

### P12-WU2 — Community Account Onboarding & Participant Claim — NEXT
Create a public community-account path and a separately verified claim flow for existing Journey participants.

Staff `/auth` remains a staff UX. Community accounts receive no CMS role by default.

### P12-WU3 — My Journey Memory
Turn verified participation into a personal Journey archive using real source records: Journey history, verified attendance when available, and published Field Updates/documentary media.

### P12-WU4 — Journey-native Community Interaction
Add constrained reflection/comment/reaction capabilities around real Journey context after identity is proven.

### P12-WU5 — Contribution History
Record meaningful contribution beyond attendance: volunteer time, skills, media, knowledge and resources.

### P12-WU6 — Community Roles & Host Network
Allow one identity to accumulate real participant/contributor/host/partner relationships without conflating those roles with CMS authorization.

### P12-WU7 — Impact Network
Connect verified Journey, Memory, Contribution, Project and Partner records into credible impact/data storytelling for CSR, funds and institutional partners.

## 5. Product gate

Every major feature must strengthen at least one real link in:

**Person ↔ Journey ↔ Project ↔ Memory ↔ Contribution ↔ Community Relationship**

If a feature mainly adds software complexity without strengthening that graph, it should not be built yet.

## 6. P12-WU1 discovery finding

Production audit on 2026-08-31 found that the first pilot's current confirmed participant is not linked to `auth.users` (`journey_participants.user_id` and its application `user_id` are null).

Therefore a simple `auth.uid()`-based “My Memories” implementation would not serve the first real pilot participant.

P12-WU1 first created a verified identity-link layer between a community account and an existing participant snapshot.

The existing `profiles` table remains the base personal profile record. `user_roles` remains staff authorization only (`admin` / `editor`). A community user does not receive a CMS role merely by having a community account.

## 7. P12-WU1 implementation outcome

Product main:
- merge SHA: `e48262879be0c7d4448dbfceb2c69147f982e64f`
- PR #18: `P12-WU1 community identity and Journey link foundation`
- canonical migration: `db/migrations/0032_p12_wu1_community_identity_foundation.sql`
- dedicated QA: `scripts/p12-wu1-db-qa.sql`

Production Supabase:
- migration: `20260831004142 p12_wu1_community_identity_foundation`

Implemented:
- additive `community_participant_links` table;
- no duplicated applicant email/phone/name;
- own-link read via RLS;
- WU1 link creation/revocation admin-only;
- no authenticated DELETE; revocation preserves history;
- one active identity per participant snapshot;
- verified ownership/audit fields immutable after creation;
- DB stamps verifier/revoker actor identity;
- linked participant snapshot cannot be deleted while identity history exists;
- no public self-claim until P12-WU2;
- no Memory creation in WU1;
- no production Journey data mutation.

Quality gates:
- PR CI #102: PASS — P9 QA, P10 runtime QA, P11-WU11 DB QA, P12-WU1 DB QA, typecheck, build, Cloudflare dry-run.
- main CI #103 after merge: PASS with the same gates.
- no Cloudflare production deployment.

Production postflight:
- `community_participant_links` exists, RLS enabled;
- link rows = 0 (no fake identity claim);
- anon SELECT = false;
- authenticated DELETE = false;
- private identity guard is `SECURITY INVOKER`, `search_path=''`, and directly executable by neither anon nor authenticated;
- pilot remains `registration_open`, capacity 30, confirmed rows 1, confirmed people 1;
- current real participant `participant_user_id = NULL` and `application_user_id = NULL` remain unchanged;
- global application/participant semantic drift = 0;
- `pg_graphql` remains OFF.

Advisor result:
- Security Advisor: no new P12-WU1 security lint; existing project warning remains `Leaked Password Protection Disabled`.
- Performance Advisor: existing project warnings remain; the new empty-table lookup index is naturally reported as unused immediately after creation. This is not treated as a reason to remove the index before the feature has traffic.

## 8. Current status

P12 roadmap realignment: COMPLETE.

P12-WU1 — Community Identity & Journey Link Foundation: **COMPLETE / PASS**.

Next product work: **P12-WU2 — Community Account Onboarding & Participant Claim**.

P11-WU6: **ACTIVE**; real pilot attendance, Memory, evidence and impact remain unresolved until real-world operation.
