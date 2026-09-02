# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU6 — NOTIFICATIONS & RETURN LOOP

Date: 2026-09-02
Status: **SOURCE / ARCHITECTURE COMPLETE / PASS — PRODUCTION MIGRATION PENDING OWNER GATE**
Production database mutation in WU6: **NONE**
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
- no DELETE requirement in v1

The client cannot alter notification actor, recipient, Journey, source, dedupe key or lifecycle state.

Server-derived lifecycle updates are kept separate from recipient mark-as-read mutation.

## Source lifecycle synchronization

WU6 withdraws notification projections when their source becomes socially unavailable:

- Reply source interaction withdrawn/moderated
- Appreciation target interaction withdrawn/moderated
- Appreciation withdrawn
- parent Question withdrawn/moderated

This affects only the notification/social projection. It never changes Journey attendance, evidence, Memory or other operational history.

## WU5 performance follow-up

WU5 production verification found one new Performance Advisor INFO:

`private.journey_interaction_moderation_events.interaction_id` lacked a covering index.

WU6 source migration 0049 carries the narrow covering index:

`journey_interaction_moderation_events_interaction_idx`

This is source-only in WU6 and is not yet applied to production.

## Activation gate

Environment flag:

`VITE_APP_SOCIAL_NOTIFICATIONS_V1_ENABLED`

`socialNotificationsV1Enabled()` first requires `journeyInteractionV1Enabled()`.

Therefore effective activation requires the already fail-closed chain:

1. Community Auth enabled
2. Journey Community Room enabled
3. Journey Interaction v1 enabled
4. Social Notifications v1 enabled

Production remains OFF.

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

## Source migrations

Canonical product `main` contains:

- `db/migrations/0049_p16_wu6_social_notifications_return_loop.sql`
  - blob SHA: `310f5a0b4fcb62a481012d759651096a544fbd50`
- `db/migrations/0050_p16_wu6_notification_lifecycle_hardening.sql`
  - blob SHA: `4b4261fbf7aa8a51719c4c8fcff5dabce95b294d`

Associated rollback artifacts:

- `db/rollbacks/p16_wu6_social_notifications_return_loop.sql`
- `db/rollbacks/p16_wu6_notification_lifecycle_hardening.sql`

**Neither 0049 nor 0050 has been applied to production at this source closeout stage.**

## Source branch / PR / merge

Feature branch:

`p16-wu6-notifications-return-loop`

PR:

`#55 — P16-WU6: Notifications & Return Loop`

Final PR head:

`a10bf113574c026c4a7318322cf77097c0cf0f72`

PR CI #225 / workflow run `33639587063`: **SUCCESS**

PR #55 squash-merged to product `main` as:

`7e83f9f71ec3787fd94f578542fb7356350a3e68`

Main tree SHA:

`be8843af522f2968c770210b5816a0bd211c021e`

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

## WU6 database QA evidence

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

## QA findings corrected before closeout

### 1. Source-QA brittleness

CI #223 failed before database QA because one source assertion depended on an exact comment line wrap.

Correction:

The assertion was rewritten to verify the actual dedupe/lifecycle contract instead of comment formatting.

### 2. Lifecycle hardening

Source review identified two important lifecycle cases before final CI:

- privileged internal notification lifecycle updates had to stay distinct from browser mark-as-read updates
- Reply notification validity must depend on the parent Question remaining active

Migration 0050 hardens both cases without loosening client RLS or grants.

### 3. Rollback dependency order

CI #224 passed all WU6 behavior tests, then failed only during rollback because `tnc_can_view_social_notification()` was dropped before RLS policies that depended on it.

Correction:

Rollback now uses dependency-safe ordering:

1. remove external source-table triggers
2. drop WU6 tables, which removes table-owned policies/triggers
3. drop WU6 helper functions
4. drop the carried performance index

No `CASCADE` is used.

CI #225 then passed the complete migration / behavior / rollback gate.

## Truth-boundary conclusion

WU6 introduces no new source of operational Journey truth.

Canonical:

> Notification projection != source interaction.
>
> Source interaction != attendance.
>
> Notification activity != attendance, shared experience, Memory, relationship proof, Contribution or impact.

The Return Loop is therefore intentionally social and navigational, not evidentiary.

## Source / architecture closeout

P16-WU6 is now:

**SOURCE / ARCHITECTURE COMPLETE / PASS**

Production remains unchanged by WU6.

## Remaining production gate

Before production foundation can be declared complete:

1. Owner separately approves production migrations 0049 + 0050
2. verify current production `main` and exact migration blobs
3. re-confirm production migration ledger and Truth Ledger immediately before DDL
4. apply exact 0049 followed by 0050
5. verify RLS, grants, policies, triggers, helper ACLs and WU5 performance index
6. verify zero notifications/preferences are fabricated automatically
7. verify Journey truth counts remain unchanged
8. rerun Security and Performance Advisors
9. keep Social Notification UX OFF
10. do not activate social email/push
11. no Worker deployment unless separately approved

## Recommended production gate wording

> APPROVE P16-WU6 DIRECT PRODUCTION MIGRATION — apply 0049/0050 under controlled QA, preserve all Journey truth, keep Social Notifications v1 UX OFF and keep social email/push OFF until production verification PASS.

## Next product work after production foundation

P16-WU7 — Memory / Reflection / Contribution Social Continuity.

WU7 must preserve the evidence gates already established in P12–P16 and must not infer post-Journey continuity from notification activity alone.
