# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU5 — JOURNEY INTERACTION V1

Date: 2026-09-02
Status: **SOURCE / ARCHITECTURE COMPLETE / PASS — PRODUCTION MIGRATION PENDING OWNER GATE**
Production database mutation: **NONE IN WU5 SO FAR**
Journey Interaction v1 UX: **OFF**
Cloudflare Worker deploy: **NONE**

## Product decision

WU5 introduces three deliberately narrow Journey-scoped interaction primitives:

1. **Journey Question**
2. **Journey Reply**
3. **Appreciation**

These exist to help people prepare and communicate around one real Journey. They do not create a generic posting system, global feed, Friend/Follow graph, direct messaging system, popularity ranking, reaction leaderboard or engagement-growth mechanic.

Canonical rule:

> Journey interaction is digital Journey context only. It never creates or implies attendance, shared real-world experience, Memory, Contribution or impact.

## Truth boundary

WU5 does not derive interaction eligibility from:

- `journey_participants`
- `community_participant_links`
- `journey_shared_experience_edges`
- `community_journey_memories`
- Contributions
- impact records
- elapsed Journey date

A Question or Reply may therefore exist **before** a Journey happens.

Write eligibility is based on voluntary social participation only:

- signed-in user owns the social identity
- social identity is enabled
- same-Journey `journey_social_presences` row is `active`
- presence visibility is `journey_only`
- identity/presence is not suppressed by social moderation

This means:

> Journey Presence can authorize digital conversation, but it is not attendance and cannot create shared-experience evidence.

## Data model

### `public.journey_interactions`

Supports only:

- `question`
- `reply`

Core fields:

- Journey ID
- author social identity ID
- interaction type
- parent Question ID for Reply
- body
- locale `vi | en`
- state `active | withdrawn | moderated`
- creation/update timestamps
- withdrawal timestamp
- moderation timestamp / reason code

Source fields are immutable after creation. The author may withdraw an active interaction; an administrator may moderate an interaction with an explicit reason.

The final public interaction projection contains **no moderator/staff auth UUID**.

### `public.journey_interaction_appreciations`

One governed Appreciation state per social identity / interaction pair.

Rules:

- sender must own the social identity
- target interaction must be visible and active
- sender must still have active Journey Presence
- self-Appreciation is prohibited
- sender may withdraw/re-enable their own Appreciation state
- Appreciation rows are private to the sender (and admin) in v1
- no public appreciation count
- no ranking/trending/leaderboard

### Private moderation audit

`private.journey_interaction_moderation_events`

Stores:

- interaction ID
- moderation reason
- moderator auth UUID
- moderation timestamp

This private ledger prevents staff/auth UUIDs from leaking into the public social projection.

## RLS / authorization

Both new public tables have RLS enabled.

Anonymous access: denied.

Authenticated table privileges are intentionally narrow:

- interactions: SELECT / INSERT / UPDATE, no DELETE
- appreciations: SELECT / INSERT / UPDATE, no DELETE

Row visibility and writes remain controlled by ownership / Journey Presence / block / moderation helpers.

Key helpers:

- `private.tnc_can_write_journey_interaction(uuid, uuid)`
- `private.tnc_can_view_journey_interaction(uuid)`

The governed visibility helper incorporates same-Room presence, identity state, block and moderation checks. Low-level block/moderation helpers remain unavailable for direct client execution.

## Reply contract

A Reply must point to:

- an existing active Question
- in the same Journey
- visible to the replying user under current Journey Room safety rules

There is no arbitrary nested thread tree in v1. Replies attach to Questions only.

## Reporting

WU5 extends `social_reports` with `target_interaction_id` so a report can identify the exact Question or Reply.

The existing reporter-private / admin-review model is preserved.

Reported users do not gain access to reporter details merely because an interaction is the report target.

## UI / bilingual UX

Source surface:

`src/components/journeys/journey-interactions.tsx`

VI/EN copy explicitly states that Question / Reply / Appreciation do not prove attendance or shared real-world experience.

The UI provides:

- Ask Question
- Reply
- Withdraw own interaction
- Appreciation for another person's interaction
- Report interaction

The UI does not expose:

- public Appreciation counts
- rankings
- trending
- Friend/Follow
- generic post composer
- global feed
- DMs
- online status
- precise/live location

## Activation gate

Environment flag:

`VITE_APP_JOURNEY_INTERACTION_V1_ENABLED`

`journeyInteractionV1Enabled()` also requires `journeyCommunityRoomEnabled()`.

Therefore WU5 is fail-closed unless Community Auth + Journey Community Room + WU5 Interaction flag are all explicitly enabled.

Production currently remains OFF.

## Source migrations

Canonical source contains:

- `db/migrations/0047_p16_wu5_journey_interaction_v1.sql`
- `db/migrations/0048_p16_wu5_interaction_moderation_actor_privacy.sql`

Associated rollback artifacts:

- `db/rollbacks/p16_wu5_journey_interaction_v1.sql`
- `db/rollbacks/p16_wu5_interaction_moderation_actor_privacy.sql`

**Neither 0047 nor 0048 has been applied to production at this source closeout stage.**

## QA evidence

Feature branch:

`p16-wu5-journey-interaction-v1`

PR:

`#54 — P16-WU5: Journey Question Reply Appreciation`

Final PR head:

`5809de4122c3969c599a0895c29613cc59c7bff2`

PR CI run #221 / workflow run `33627110672` PASS:

- all inherited source QA
- P16-WU5 source contract QA
- all inherited ephemeral database QA
- P16-WU5 ephemeral PostgreSQL migration / behavior / rollback QA
- application build
- TypeScript typecheck
- Cloudflare deployment-config dry-run

Post-merge `main` CI run #222 / workflow run `33627267685` also completed **SUCCESS** on the exact merged main SHA.

### Issues discovered and corrected during QA

1. **Package lock drift** — an accidental dependency/devDependency change caused frozen-lock install failure. Dependencies were restored to canonical main; WU5 retained scripts only.
2. **Reply guard helper ACL** — an invoker trigger initially called a deliberately non-client-executable low-level block helper. Final guard reuses the governed `tnc_can_view_journey_interaction()` authorization helper instead of loosening internal ACLs.
3. **Privacy rollback** — the first hardening rollback attempted to recreate a public moderator UUID field. Final paired rollback intentionally does not re-expose staff identity before deleting WU5 tables.
4. **Exact optional TypeScript typing** — one UI prop was corrected to explicitly accept `undefined` under the repository's exact optional property semantics.

These failures were treated as QA findings; no production WU5 mutation occurred while fixing them.

## Merge

PR #54 was squash-merged to product `main` as:

`b926d72f89fd516e70b268fd9528efda861e9de1`

Main tree SHA:

`95201148b4c85a6a1051141ba11da2725942efdd`

Merge message preserves the release boundary:

> Production migrations remain unapplied pending Owner gate.

## Source / architecture closeout

P16-WU5 source and architecture are now **COMPLETE / PASS**.

The remaining work is a separate production database gate, not additional source discovery or redesign.

## Remaining production gate

1. Owner separately approves production migrations 0047 + 0048
2. re-confirm production Truth Ledger and current migration ledger immediately before DDL
3. apply exact canonical 0047 followed by 0048 under controlled QA
4. verify new tables, RLS, grants, policies and private moderation audit
5. verify anon denied and client cannot DELETE / forge moderation audit
6. verify Journey truth counts unchanged and zero interactions created automatically
7. rerun Security / Performance Advisors
8. keep Interaction UX OFF until a separate release/activation decision

## Next production gate wording

> APPROVE P16-WU5 DIRECT PRODUCTION MIGRATION — apply 0047/0048 under controlled QA, preserve all Journey truth, keep Journey Interaction v1 UX OFF until production verification PASS.
