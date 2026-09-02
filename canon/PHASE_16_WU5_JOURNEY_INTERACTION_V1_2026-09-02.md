# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU5 — JOURNEY INTERACTION V1

Date: 2026-09-02
Status: **COMPLETE / PASS — SOURCE, ARCHITECTURE & PRODUCTION DATABASE FOUNDATION**
Production database mutation: **CONTROLLED / VERIFIED PASS**
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

Production remains **OFF** after database verification. No Worker deploy occurred in WU5.

## Source migrations

Canonical source:

- `db/migrations/0047_p16_wu5_journey_interaction_v1.sql`
- `db/migrations/0048_p16_wu5_interaction_moderation_actor_privacy.sql`

Associated rollback artifacts:

- `db/rollbacks/p16_wu5_journey_interaction_v1.sql`
- `db/rollbacks/p16_wu5_interaction_moderation_actor_privacy.sql`

## Source QA evidence

Feature branch:

`p16-wu5-journey-interaction-v1`

PR:

`#54 — P16-WU5: Journey Question Reply Appreciation`

Final PR head:

`5809de4122c3969c599a0895c29613cc59c7bff2`

PR CI run #221 / workflow run `33627110672`: **PASS**.

Post-merge `main` CI run #222 / workflow run `33627267685`: **SUCCESS** on exact merged main SHA.

Passed:

- all inherited source QA
- P16-WU5 source contract QA
- all inherited ephemeral database QA
- P16-WU5 ephemeral PostgreSQL migration / behavior / rollback QA
- application build
- TypeScript typecheck
- Cloudflare deployment-config dry-run

### QA findings corrected before merge

1. package lock drift corrected; no dependency drift retained
2. Reply guard retained internal helper ACL boundaries instead of loosening permissions
3. rollback privacy hardening no longer recreates public moderator UUID
4. exact optional TypeScript prop typing corrected

## Source merge

PR #54 squash-merged to product `main`:

`b926d72f89fd516e70b268fd9528efda861e9de1`

Main tree SHA:

`95201148b4c85a6a1051141ba11da2725942efdd`

## Owner production approval

Owner explicitly approved:

> APPROVE P16-WU5 DIRECT PRODUCTION MIGRATION — apply 0047/0048 under controlled QA, preserve all Journey truth, keep Journey Interaction v1 UX OFF until production verification PASS.

## Production migration execution

Production Supabase project:

`iwiqprhoohkxvjyxojto`

Pre-flight confirmed:

- WU4 migration 0046 present
- `journey_interactions` absent
- `journey_interaction_appreciations` absent
- private moderation ledger absent
- no hidden WU5 migration drift

Applied exact canonical migrations in order:

1. `20260902124423` — `0047_p16_wu5_journey_interaction_v1`
2. `20260902124444` — `0048_p16_wu5_interaction_moderation_actor_privacy`

Both migration operations returned success.

## Production verification

### Tables and RLS

Production now contains:

- `public.journey_interactions`
- `public.journey_interaction_appreciations`
- `private.journey_interaction_moderation_events`

RLS:

- `journey_interactions`: ON
- `journey_interaction_appreciations`: ON

### Client privileges

Anon:

- interactions SELECT: false
- interactions INSERT: false
- governed helper EXECUTE: false

Authenticated:

- interactions SELECT: true
- interactions INSERT: true
- interactions UPDATE: true
- interactions DELETE: false
- appreciations SELECT: true
- appreciations INSERT: true
- appreciations UPDATE: true
- appreciations DELETE: false

### Policies verified

Interactions:

- `Journey interactions governed read`
- `Journey interactions active-presence insert`
- `Journey interactions owner or admin update`

Appreciations:

- `Journey appreciations own read`
- `Journey appreciations own insert`
- `Journey appreciations own update`

### Privacy hardening verified

The final public `journey_interactions` columns do **not** include `moderated_by`.

Private moderation ledger access:

- anon SELECT: false
- authenticated SELECT: false
- service_role SELECT: true

Triggers verified present:

- `journey_interactions_guard`
- `journey_interactions_moderation_audit`
- `journey_interaction_appreciations_guard`

`social_reports.target_interaction_id` exists in production.

Authenticated can execute only the governed authorization helpers required by RLS:

- `tnc_can_write_journey_interaction(uuid,uuid)`
- `tnc_can_view_journey_interaction(uuid)`

Anon cannot execute those helpers.

## Zero-fabrication verification

Immediately after production migration:

- Journey interactions: **0**
- Appreciations: **0**
- private moderation events: **0**

No interaction, appreciation, moderation event, attendance, shared-experience edge, Memory, Reflection, Contribution or impact record was fabricated by migration.

## Pilot Journey truth preservation

Pilot Journey:

`19539f36-3ed4-4a22-96b9-c8a9b73c5283`

Pre/post production truth remained identical:

- participants = 1
- attendance unresolved = 1
- verified no-show = 0
- verified attended rows = 0
- verified attended people = 0
- social identities = 0
- Journey presences = 0
- shared-experience edges = 0
- relationship consents = 0
- participant links = 1
- Memories = 1
- Reflections = 0
- Reflection publications = 0
- Contributions = 0
- WU5 interactions = 0
- WU5 appreciations = 0

Therefore WU5 production migration preserved all Journey operational truth.

## Advisor verification

### Security Advisor

No WU5-created security warning.

The only security warning remains the pre-existing project-level:

- `auth_leaked_password_protection` — Leaked Password Protection Disabled

Reference:

https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

### Performance Advisor

Legacy performance findings remain.

WU5-specific findings after migration:

- INFO: unindexed FK on `private.journey_interaction_moderation_events.interaction_id`
- INFO: several new WU5 indexes reported unused because the feature has zero production rows and UX remains OFF

The missing private-audit FK index is a performance follow-up, not a correctness/security regression. It is intentionally **not** silently fixed under the 0047/0048 Owner approval because doing so would require a new production migration outside the approved migration set.

## Final WU5 closeout

P16-WU5 is **COMPLETE / PASS — SOURCE, ARCHITECTURE & PRODUCTION DATABASE FOUNDATION**.

The following remain OFF / not performed:

- Journey Interaction v1 UX activation
- production Worker deploy for WU5
- any real Question / Reply / Appreciation content

Any future UI activation remains a separate release decision.

## Next Phase 16 work unit

**P16-WU6 — Notifications & Return Loop**

WU6 must consume governed interaction/social events without turning them into popularity mechanics, spam, fake urgency, attendance evidence or public relationship proof.
