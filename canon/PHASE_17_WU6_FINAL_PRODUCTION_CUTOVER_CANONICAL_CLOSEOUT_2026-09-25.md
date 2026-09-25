# TRẠM NỤ CƯỜI — PHASE 17 / P17-WU6.Final
# JOURNEY COMMUNITY V2 — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-25

Status: COMPLETE / CLOSED / PASS

## 1. Scope

P17-WU6.Final closes the Journey Community v2 production database cutover and post-cutover Advisor hardening.

This closeout does not activate Journey Community v2 runtime unless every production runtime prerequisite is verified.

Canonical choice at closeout:

**B. Production DB cutover complete, Journey Community v2 runtime remains fail-closed/OFF.**

No recruitment, attendance, Memory, Shared Journey claim, fake social data, or publication content was activated or fabricated.

## 2. Product source evidence

Repository:
`huynhtranhuythinh/tramnucuoi`

Production branch:
`main`

### PR #113 — production cutover

Title:
`P17-WU6.Final: Journey Community v2 production cutover`

Merged main:
`d7e4e08355cf8d185c072867d009c74234a55ee7`

Exact-head before merge:
`6a1c78c3002ac3d56a20fa1139d917ae104ea5f6`

Exact-head runs:
- CI `36136660494` — SUCCESS
- dedicated gate `36136660499` — SUCCESS

Post-merge main CI:
- `36136867866` — SUCCESS

### PR #114 — Advisor hardening

Title:
`P17-WU6.Final: Advisor hardening`

Final PR head:
`61cf9cfced23e996119c2308098d642f790ff360`

Exact-head runs:
- CI `36138043407` — SUCCESS
- dedicated gate `36138043498` — SUCCESS

Merged main:
`42ae483b3bd81b318a793c263c11fc6d3a9f1a8f`

Post-merge main CI:
- `36138235076` — SUCCESS

Final production-main SHA at closeout:
`42ae483b3bd81b318a793c263c11fc6d3a9f1a8f`

## 3. Supabase production migrations

Project:
`iwiqprhoohkxvjyxojto`

### 0060 Journey Community v2 cutover

Source:
`database/migrations/0060_p17_wu6_final_journey_community_v2_cutover.sql`

Production migration ledger:
`20260925124831_p17_wu6_final_journey_community_v2_cutover`

### 0061 Advisor hardening

Source:
`database/migrations/0061_p17_wu6_final_advisor_hardening.sql`

Production migration ledger:
`20260925132548_p17_wu6_final_advisor_hardening`

Apply result:
SUCCESS

## 4. Production schema / RLS / RPC / storage verification

Verified Journey Community source tables:
- `journey_community_policies`
- `journey_community_posts`
- `journey_community_post_media`
- `journey_community_post_appreciations`
- `private.journey_community_post_publication_events`
- `private.journey_community_post_moderation_events`

Verified RPCs:
- `tnc_public_journey_community_feed`
- `tnc_public_journey_post_conversation`
- `tnc_report_journey_post`
- `tnc_block_journey_post_author`
- `tnc_admin_moderate_reported_journey_post`

Verified additive integration fields:
- `journey_interactions.post_id`
- `journey_interactions.visibility_scope`
- `social_notifications.source_post_id`
- `social_reports.target_post_id`

RLS remains enabled on Journey Community source tables.

Storage bucket:
`journey-community-private`

Verified:
- exists
- public = false
- file size limit = 52428800
- object count = 0 at cutover verification

## 5. Advisor hardening verification

0061 added five covering indexes:
- `journey_community_policies_created_by_idx`
- `journey_community_policies_updated_by_idx`
- `journey_community_posts_participant_journey_idx`
- `journey_community_post_publication_events_journey_idx`
- `journey_community_post_publication_events_actor_idx`

Production verification:
- hardening indexes present = 5/5
- consolidated UPDATE policy present = 1
- old duplicate UPDATE policies present = 0

Consolidated policy:
`Journey Community posts owner or publication reviewer update`

Authority remains equivalent:
- owner authority via `private.tnc_social_identity_owned(...)`
- reviewer authority via `private.tnc_can_review_journey_post_publication(...)`

No security standard was reduced.

## 6. Supabase Advisor result

### Security Advisor

No WU6-specific security regression remains.

Pre-existing warning remains:
`Leaked Password Protection Disabled`

This is unrelated to WU6.Final and was not silently modified.

### Performance Advisor

The five WU6-specific unindexed foreign-key findings are no longer present.

The WU6 duplicate permissive UPDATE-policy finding is no longer present.

Historical unrelated performance findings remain outside WU6.Final scope and were not bundled into this closeout.

## 7. Production truth invariants

Post-0061 verification:

- Journey Community policies rows = 0
- Journey Community posts rows = 0
- Journey Community media rows = 0
- Journey Community appreciations rows = 0
- Journey interactions rows = 0
- Social notifications rows = 0
- Social reports rows = 0
- Shared Journey experience edges rows = 0
- non-closed recruitment/application windows = 0

Therefore:
- no fake Community content was seeded;
- no fake participant activity was created;
- recruitment remains CLOSED;
- no Shared Journey evidence was fabricated;
- no attendance truth was created by WU6.Final.

## 8. Runtime activation audit

Canonical activation function:
`src/lib/journeys/community-v2-activation.ts`

Journey Community v2 requires all three build-time conditions:

1. `VITE_APP_COMMUNITY_AUTH_ENABLED=true`
2. `VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED=true`
3. `VITE_APP_JOURNEY_COMMUNITY_V2_ENABLED=true`

Community Auth has historical production activation evidence:
- build activation `VITE_APP_COMMUNITY_AUTH_ENABLED=true`
- Magic Link production flow verified PASS in P14-WU2.

However, canonical production evidence does not prove that:
- `VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED=true`, or
- `VITE_APP_JOURNEY_COMMUNITY_V2_ENABLED=true`

The current tool environment has no Cloudflare Worker variable/configuration mutation capability and cannot safely claim those build-time flags are active.

Therefore runtime is intentionally recorded as:

**FAIL-CLOSED / OFF**

This is a valid P17-WU6.Final closeout state because the database cutover is complete and runtime activation is explicitly separated from database readiness.

## 9. Canonical truths preserved

Journey Community remains Journey-scoped.

It is not:
- a generic global social feed;
- attendance truth;
- Memory truth;
- Shared Journey evidence;
- Impact truth.

Privacy/publication guarantees remain:
- new posts default participant-only;
- public publication requires explicit publication state;
- historical participant-only conversations do not auto-public;
- public feed does not expose participant IDs, auth UUIDs, or moderation identities;
- media remains in a private bucket;
- public media URLs are short-lived signed URLs;
- Report / Block target identity is server-derived;
- Editor is not Community Admin;
- BTC / TNV / Bản địa are Journey Roles, not global account roles.

No engagement ranking.
No follower count.
No public reaction count.
Chronological feed only.

## 10. Final gate

PASS criteria satisfied for actual WU6.Final scope:
- PR #113 merged and verified;
- PR #114 merged and verified;
- exact-head CI PASS;
- post-merge main CI PASS;
- 0060 production migration applied;
- 0061 production migration applied;
- production schema/RLS/RPC/storage verified;
- Advisor hardening verified;
- WU6-specific Advisor regressions removed;
- zero-content production truth preserved;
- recruitment remains CLOSED;
- runtime truth recorded honestly as OFF.

# FINAL STATUS

**P17-WU6.Final — COMPLETE / CLOSED / PASS**

Runtime state:

**Production DB cutover complete, Journey Community v2 runtime remains fail-closed/OFF.**
