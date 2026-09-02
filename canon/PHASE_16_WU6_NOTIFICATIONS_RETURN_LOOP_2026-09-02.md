# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU6 — NOTIFICATIONS & RETURN LOOP

Date: 2026-09-02
Status: **COMPLETE / PASS — SOURCE, ARCHITECTURE & PRODUCTION DATABASE FOUNDATION**
Production database mutation in WU6: **CONTROLLED / VERIFIED PASS**
Social notification UX: **OFF**
Social email / push delivery: **OFF / NOT IMPLEMENTED IN WU6 v1**
Cloudflare Worker deploy in WU6: **NONE**

## Product decision

WU6 adds a deliberately calm, Journey-scoped return loop. The notification system exists only to return a person to a meaningful Journey conversation that already happened digitally.

WU6 v1 supports exactly two events:

1. **Question Reply**
2. **Interaction Appreciation**

It does not add a generic engagement inbox, global social feed, Friend/Follow mechanics, direct messages, popularity ranking, trending, public reaction counts, global unread-count pressure, online presence or push-notification growth loops.

Canonical rule:

> Notification activity is a private social projection of truthful digital interaction. It never creates or implies attendance, shared real-world experience, Memory, relationship proof, Contribution or impact.

## Existing email infrastructure discovery

The repository already contains transactional-email plumbing from earlier phases, including:

- `src/lib/notifications/email.server.ts`
- `src/lib/notifications/journey-emails.server.ts`

That infrastructure remains separately gated by `EMAIL_DELIVERY_ENABLED` and fails closed when delivery is disabled.

WU6 does **not** build a second email provider and does **not** send social email or push notifications. Future external social delivery may reuse the existing server-side infrastructure only under a separate explicit release decision.

## Notification data model

### `public.social_notification_preferences`

One optional row per social identity.

WU6 v1 preferences:

- `in_app_replies`
- `in_app_appreciations`

Absence of a preference row means calm in-app defaults are enabled. There are no email addresses, push tokens or external-channel settings in the WU6 v1 schema.

### `public.social_notifications`

Private recipient projection with:

- recipient social identity
- actor social identity
- Journey
- notification type
- source interaction
- optional source Appreciation
- stable dedupe key
- lifecycle state `active | withdrawn`
- private read timestamp
- timestamps

The browser cannot create notification rows.

## Server-derived event projection

Notifications are projected only after accepted WU5 source events.

### Question Reply

A notification is created after an accepted `reply` interaction when:

- the parent is a Question in the same Journey
- actor and recipient are different people
- recipient has not disabled Reply notifications

The notification source is the accepted Reply row; no attendance or operational Journey table is consulted.

### Appreciation

One notification is created for the lifetime of an Appreciation row.

Withdrawal changes the existing notification projection to withdrawn. Re-enabling the same Appreciation reactivates that row instead of manufacturing a new notification. The read timestamp is not reset by toggle behavior.

This prevents Appreciation toggling from becoming a notification-spam mechanic.

## Privacy and visibility

A notification is visible only to its recipient and only while the relevant social context remains safe.

The governed read helper requires:

- recipient owns the current social identity
- notification remains active
- actor and recipient remain distinct
- no block between actor and recipient
- both social identities remain enabled
- both people have active `journey_only` Journey Presence in the same Journey
- identity/presence is not hidden or socially suspended
- underlying interaction remains active
- for a Reply, the parent Question also remains active
- for Appreciation, the source Appreciation remains active and matches actor/source

Blocking or social hiding suppresses notification visibility without deleting or rewriting historical operational truth.

## Client mutation boundary

Authenticated client privileges are intentionally narrow.

`social_notifications`:

- SELECT: yes, RLS governed
- UPDATE: yes, only for recipient mark-as-read
- INSERT: no
- DELETE: no

`social_notification_preferences`:

- SELECT / INSERT / UPDATE for the owning identity
- DELETE: no requirement in v1

The client cannot alter notification actor, recipient, Journey, source, dedupe key or lifecycle state.

Server-derived lifecycle updates are kept separate from recipient mark-as-read mutation.

## Source lifecycle synchronization

WU6 withdraws notification projections when their source becomes socially unavailable:

- Reply source interaction withdrawn/moderated
- Appreciation target interaction withdrawn/moderated
- Appreciation withdrawn
- parent Question withdrawn/moderated

This affects only the notification/social projection. It never changes Journey attendance, evidence, Memory or other operational history.

## Activation gate

Environment flag:

`VITE_APP_SOCIAL_NOTIFICATIONS_V1_ENABLED`

`socialNotificationsV1Enabled()` first requires `journeyInteractionV1Enabled()`.

Therefore effective activation requires the already fail-closed chain:

1. Community Auth enabled
2. Journey Community Room enabled
3. Journey Interaction v1 enabled
4. Social Notifications v1 enabled

Production Social Notifications UX remains **OFF**. No Cloudflare Worker deployment occurred during the WU6 production database migration sequence.

## UI / bilingual experience

Private in-app notification center:

- VI: `/cong-dong/thong-bao`
- EN: `/en/community/notifications`

Both routes are `noindex, nofollow`.

The screen provides:

- private Reply/Appreciation notifications
- actor name from governed social identity projection
- Journey title and return link
- per-item NEW / SEEN state
- mark as seen
- in-app Reply/Appreciation preferences

The screen deliberately does not provide a global notification count/bell as an engagement-pressure mechanic.

VI/EN copy explicitly states that notifications do not prove attendance, shared real-world experience, Memory, Contribution, relationship or impact, and that social email/push is not enabled in WU6 v1.

## Canonical source / merge evidence

Product production branch: `main`

WU6 product main SHA:

`7e83f9f71ec3787fd94f578542fb7356350a3e68`

Main tree SHA:

`be8843af522f2968c770210b5816a0bd211c021e`

Canonical source migrations:

- `db/migrations/0049_p16_wu6_social_notifications_return_loop.sql`
  - blob SHA: `310f5a0b4fcb62a481012d759651096a544fbd50`
- `db/migrations/0050_p16_wu6_notification_lifecycle_hardening.sql`
  - blob SHA: `4b4261fbf7aa8a51719c4c8fcff5dabce95b294d`

Associated rollback artifacts:

- `db/rollbacks/p16_wu6_social_notifications_return_loop.sql`
- `db/rollbacks/p16_wu6_notification_lifecycle_hardening.sql`

Feature branch:

`p16-wu6-notifications-return-loop`

PR:

`#55 — P16-WU6: Notifications & Return Loop`

Final PR head:

`a10bf113574c026c4a7318322cf77097c0cf0f72`

PR CI #225 / workflow run `33639587063`: **SUCCESS**

PR #55 squash-merged to product `main` as the SHA above.

Post-merge main CI #226 / workflow run `33639812252`: **SUCCESS** on exact merged SHA.

Passed gates include:

- inherited source QA
- P16-WU6 source contract QA
- inherited ephemeral database QA
- P16-WU6 PostgreSQL 17 migration / behavior / rollback QA
- application build
- TypeScript typecheck
- Cloudflare deployment-config dry-run

No CI step connects to canonical Supabase production.

## WU6 source QA evidence

Ephemeral PostgreSQL 17 QA verifies:

- RLS enabled on both WU6 public tables
- anon has no notification access
- authenticated cannot INSERT or DELETE notifications
- recipient can read governed notifications
- recipient can only mark an active notification as seen
- a user without Journey Presence cannot see notifications
- block suppresses visibility without deleting historical source rows
- accepted Reply derives one recipient notification
- accepted Appreciation derives one recipient notification
- self-notification is prohibited
- Appreciation withdrawal/re-enable reuses one notification row
- Reply preference disables future Reply notifications
- parent Question withdrawal suppresses its Reply notification
- source interaction withdrawal suppresses related Appreciation notification
- WU6 rollback removes WU6 notification foundation while preserving WU5 interaction foundation
- paired rollback can then remove WU5 cleanly

### Source QA corrections before merge

1. CI #223 failed before DB QA because a source assertion depended on exact comment formatting. The assertion was rewritten to verify the actual dedupe/lifecycle contract.
2. Source review found that privileged lifecycle synchronization must stay separate from browser mark-as-read mutation and that Reply validity must depend on the parent Question remaining active. Migration 0050 hardens both cases.
3. CI #224 passed WU6 behavior then failed only during rollback due dependency order. Rollback was corrected to remove external triggers, then WU6 tables/policies, then helpers, then the carried performance index. No `CASCADE` is used.
4. CI #225 then passed the full migration / behavior / rollback gate.

# PRODUCTION DATABASE VERIFICATION

## Owner approval

Owner explicitly approved:

> APPROVE P16-WU6 DIRECT PRODUCTION MIGRATION — apply 0049/0050 under controlled QA, preserve all Journey truth, keep Social Notifications v1 UX OFF and keep social email/push OFF until production verification PASS.

## Preflight production truth

Production Supabase project:

`iwiqprhoohkxvjyxojto`

Before WU6 mutation, the production migration ledger already contained WU5 dependencies:

- `20260902124423` — `0047_p16_wu5_journey_interaction_v1`
- `20260902124444` — `0048_p16_wu5_interaction_moderation_actor_privacy`

WU6 tables did not yet exist.

Pre-migration social baseline:

- social identities: 0
- Journey social presences: 0
- relationship consents: 0
- shared-experience edges: 0
- Journey interactions: 0
- Appreciations: 0

Pilot Journey:

`19539f36-3ed4-4a22-96b9-c8a9b73c5283`

Pre-migration pilot truth:

- participants: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended: 0
- Reflections: 0
- Reflection publications: 0
- Contributions: 0

No WU6 migration was allowed to reinterpret any of these facts.

## Production migration execution

The exact canonical source migrations from product `main` were applied in order:

1. `20260902141428` — `0049_p16_wu6_social_notifications_return_loop`
2. `20260902141453` — `0050_p16_wu6_notification_lifecycle_hardening`

Both controlled production migration operations returned success.

## Final production RLS and grants

Both WU6 public tables have RLS enabled.

### `public.social_notification_preferences`

Anon:

- SELECT: false
- INSERT: false
- UPDATE: false
- DELETE: false

Authenticated:

- SELECT: true
- INSERT: true
- UPDATE: true
- DELETE: false

Verified policies:

- `Social notification preferences own insert`
- `Social notification preferences own read`
- `Social notification preferences own update`

Ownership is governed through `private.tnc_social_identity_owned(...)`. UPDATE has both `USING` and `WITH CHECK`.

### `public.social_notifications`

Anon:

- SELECT: false
- INSERT: false
- UPDATE: false
- DELETE: false

Authenticated:

- SELECT: true
- INSERT: false
- UPDATE: true
- DELETE: false

Verified policies:

- `Social notifications recipient read`
- `Social notifications recipient update`

Both use `private.tnc_can_view_social_notification(id)`. UPDATE has both `USING` and `WITH CHECK`.

Browser clients therefore cannot manufacture or hard-delete notification records.

## Production helper security

WU6 private helper functions were verified with owner `postgres` and explicit empty `search_path`.

Final direct-execute boundary:

- `tnc_can_view_social_notification(uuid)` — SECURITY DEFINER; authenticated execute allowed; anon denied
- `tnc_guard_social_notification_preference()` — SECURITY INVOKER; no direct anon/auth execute
- `tnc_guard_social_notification_read()` — SECURITY INVOKER; no direct anon/auth execute
- `tnc_project_reply_notification()` — SECURITY DEFINER; no direct anon/auth execute
- `tnc_project_appreciation_notification()` — SECURITY DEFINER; no direct anon/auth execute
- `tnc_social_notification_category_enabled(uuid,text)` — SECURITY DEFINER; no direct anon/auth execute
- `tnc_sync_notification_source_state()` — SECURITY DEFINER; no direct anon/auth execute

This preserves the distinction between recipient read-state mutation and server-owned projection lifecycle.

## Production triggers

Direct PostgreSQL catalog verification confirms the following WU6 triggers exist and are enabled:

- `journey_interactions_reply_notification`
- `journey_appreciations_notification`
- `journey_interactions_notification_state_sync`
- `social_notification_preferences_guard`
- `social_notifications_read_guard`

A prior `information_schema.triggers` query returned no rows because of metadata visibility behavior; direct `pg_trigger` verification is the canonical evidence and confirmed all five triggers.

## Production indexes

Verified WU6 indexes include:

- `social_notifications_recipient_created_idx`
- `social_notifications_actor_journey_idx`
- `social_notifications_source_interaction_idx`
- `social_notifications_source_appreciation_idx`
- unique dedupe-key index

WU6 also applied the WU5 performance follow-up index:

`private.journey_interaction_moderation_events_interaction_idx`

The previous WU5 Performance Advisor finding for the moderation-event foreign key is therefore resolved.

## Zero-fabrication / Truth Ledger verification after migration

Immediately after 0049 + 0050:

- notification preferences: 0
- social notifications: 0
- social identities: 0
- Journey social presences: 0
- relationship consents: 0
- shared-experience edges: 0
- Journey interactions: 0
- Appreciations: 0

Pilot Journey remained exactly:

- participants: 1
- attendance unresolved: 1
- verified no-show: 0
- verified attended: 0
- Reflections: 0
- Reflection publications: 0
- Contributions: 0

Therefore WU6 database activation did not fabricate social activity, attendance, real-world evidence, Memory, relationship proof, Contribution or impact.

## Security Advisor after production migration

No WU6-created Security Advisor finding was introduced.

The only Security Advisor warning remains the pre-existing project-level Auth warning:

`auth_leaked_password_protection` — Leaked Password Protection Disabled.

That setting is outside WU6 scope and was not silently changed.

## Performance Advisor after production migration

The prior WU5-specific unindexed foreign-key finding on:

`private.journey_interaction_moderation_events.interaction_id`

is resolved by 0049.

Post-WU6 Performance Advisor reports one new WU6-specific non-blocking INFO:

`public.social_notifications.journey_id` has no standalone covering index for its foreign key.

The existing composite `social_notifications_actor_journey_idx(actor_social_identity_id, journey_id, ...)` does not satisfy the advisor because `journey_id` is not its leading column.

This is a **performance INFO**, not a correctness, privacy or security failure. No production migration 0051 was created or applied because Owner authorization covered 0049/0050 only.

WU6 indexes are also reported as unused while the tables contain zero rows and the feature remains OFF; that is expected at this release stage and is not a removal signal.

Recommended future handling before or with activation:

- add a narrow source migration for a standalone `social_notifications(journey_id)` index;
- run source/ephemeral DB QA and CI;
- apply it only under a separate production approval.

## Production release boundary after verification

After successful database verification:

- Social Notifications v1 UX: **OFF**
- `VITE_APP_SOCIAL_NOTIFICATIONS_V1_ENABLED`: not activated by this migration sequence
- social email delivery: **OFF / not implemented in WU6 v1**
- social push delivery: **OFF / not implemented in WU6 v1**
- Cloudflare Worker deploy: **NONE**

Production currently contains only the verified database foundation. No public or private social notification experience has been activated by this closeout.

## Truth-boundary conclusion

WU6 introduces no new source of operational Journey truth.

Canonical:

> Notification projection != source interaction.
>
> Source interaction != attendance.
>
> Notification activity != attendance, shared experience, Memory, relationship proof, Contribution or impact.

The Return Loop is intentionally social and navigational, not evidentiary.

## Final closeout

P16-WU6 is now:

**COMPLETE / PASS — SOURCE, ARCHITECTURE & PRODUCTION DATABASE FOUNDATION**

The production database foundation is verified. UX activation and any external social delivery remain separate future decisions.

## Next product work

P16-WU7 — Memory / Reflection / Contribution Social Continuity.

WU7 must preserve the evidence gates already established in P12–P16 and must not infer post-Journey continuity from notification activity alone.
