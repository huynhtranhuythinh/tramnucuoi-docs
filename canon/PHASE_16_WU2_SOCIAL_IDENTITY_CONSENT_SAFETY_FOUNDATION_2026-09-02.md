# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU2 — SOCIAL IDENTITY, CONSENT & SAFETY FOUNDATION

Date: 2026-09-02
Status: **ARCHITECTURE LOCKED / IMPLEMENTATION PREPARED / STAGING QA PENDING**
Production mutation: **NONE**

## 1. PURPOSE

P16-WU2 establishes the privacy and safety substrate required before Journey Community Room, Journey Stream, shared-experience graph or member-generated interactions can be activated.

The governing P16 thesis remains:

> **TRẠM NỤ CƯỜI is a Journey-Based Social Network.**
>
> **Journey — not Post and not Person — is the primary social object.**
>
> **Digital relationships may begin around preparation for a real Journey, but claims of shared real-world experience arise only from appropriate evidence.**
>
> **TNC social features exist to extend meaningful real-world relationships, not maximize time-on-platform, popularity or content volume.**
>
> **Public visibility is always a separate consent/publication decision from private operational truth.**

WU2 converts those principles into an enforceable social identity, consent, blocking, reporting and moderation architecture.

## 2. CURRENT PRODUCTION FINDINGS

Production was inspected read-only before design.

Existing privacy boundaries remain strong and should NOT be weakened:

- `profiles` is own/admin readable; it is not a public member directory.
- `journey_participants` is operational/admin-only.
- `community_participant_links` separates account identity from operational Journey participant truth.
- `community_journey_memories` remains owner-scoped.
- `journey_reflections` remains private author/staff source.
- `journey_reflection_publications` is a separate identity-minimized public projection.
- existing Community code already keeps participant/attendance/Memory distinctions explicit.
- existing staff MFA foundation is present.

Security Advisor currently reports one account-security warning:

- Supabase leaked-password protection is disabled.

This does not invalidate WU2 architecture, but it should be resolved before broad social/account rollout when plan capability permits.

## 3. WU2 CONSTITUTIONAL RULES

### 3.1 Social identity is not account identity

`auth.users` and `profiles` remain private account/operational identity layers.

A new social identity is an explicit, optional projection chosen by the person.

Account existence MUST NOT automatically create a visible social person.

### 3.2 Journey Presence is not attendance

A person may explicitly appear in a Journey digital context before the real event.

That state means only:

> **I choose to be visible in this Journey's digital community context.**

It MUST NOT mean:

- I attended;
- I completed the Journey;
- I have a Memory;
- I created impact;
- I have a verified shared-experience relationship.

### 3.3 Social visibility is not operational truth

Withdrawing social visibility, blocking another person or being hidden by moderation MUST NOT mutate:

- Journey registration/application;
- participant record;
- attendance evidence;
- Memory eligibility;
- Reflection eligibility/source;
- Contribution evidence;
- verified operational relationship.

### 3.4 Consent can expand visibility; moderation cannot

Only the user may opt into or expand their own social visibility.

Staff moderation may only:

- reduce visibility;
- hide a social projection;
- temporarily suspend social capability;
- later restore the prior user-controlled capability.

Staff moderation MUST NOT grant social consent on behalf of a user.

### 3.5 Fail closed

No social identity or Journey Presence becomes visible by default.

No anonymous public member directory is introduced in WU2.

No public participant presence is introduced in WU2.

## 4. CANONICAL DATA MODEL

Implementation is prepared on product branch:

`p16-wu2-social-identity-consent-safety`

based directly on production `main` SHA:

`d9a67f58ef25f650fe2e378683b6d92fb36a0137`

### 4.1 `social_identities`

Private user-controlled source.

Purpose:

- separate social presentation from `profiles`;
- store optional display name/avatar/short bilingual intro;
- explicit social enable/disable;
- fail-closed default visibility settings;
- record current consent-copy version.

Key rule:

> `social_identities.user_id` is private ownership mapping and never becomes Journey-visible social output.

### 4.2 `social_identity_cards`

Identity-minimized Journey-social projection.

It contains only social presentation fields and deliberately excludes:

- auth UUID;
- email;
- registration PII;
- participant id;
- attendance;
- Memory id;
- Contribution data;
- moderator/staff identifiers.

It is readable only through Journey-context authorization.

It is NOT a global public profile directory.

### 4.3 `journey_social_presences`

One durable user-controlled social-presence choice per Journey.

Canonical scopes for WU2:

- `private`;
- `journey_only`.

`public` participant presence is deliberately not enabled yet.

Presence state:

- `active`;
- `withdrawn`.

There is intentionally no `attended`, `no_show` or `unresolved` state in this table.

Eligibility to create Journey Presence is derived from verified Journey context, such as:

- an active verified participant link to a confirmed Journey participant; or
- a verified Journey host relationship.

Eligibility does not create attendance truth.

### 4.4 `social_consent_events`

System-written append-only ledger for:

- social identity activation/deactivation;
- social profile changes;
- default visibility changes;
- Journey Presence activation;
- Journey Presence visibility change;
- Journey Presence withdrawal.

This ledger is NOT attendance evidence.

### 4.5 `social_blocks`

Private blocker-owned social control.

Block semantics are bidirectional on social discovery/visibility/interactions:

> if A blocks B, A and B should no longer be socially exposed to each other through TNC surfaces governed by this layer.

Block existence/reason is not shown to the blocked person.

Blocking never changes shared operational history.

### 4.6 `social_reports`

Reporter-private / admin-review safety report.

Initial categories:

- harassment;
- privacy;
- impersonation;
- unsafe content;
- spam;
- other.

The reported person must not receive reporter identity/details from this table.

### 4.7 `social_moderation_controls`

Admin-only append-oriented controls:

- hide social identity;
- hide a specific Journey Presence;
- suspend social capability.

Controls preserve actor/time/reason audit and are revoked rather than silently overwritten.

They cannot grant consent or edit operational Journey evidence.

## 5. SOCIAL IDENTIFIER ARCHITECTURE

WU2 introduces a social identity UUID distinct from `auth.users.id`.

This is intentional.

Journey/community clients should work with:

`social_identity_id`

instead of exposing:

`auth.users.id`.

This reduces coupling between social surfaces and the authentication identity and prevents a future Journey Room from becoming a thin public wrapper around private account records.

## 6. RLS / SECURITY CONTRACT

### Social source

`social_identities`

- owner read;
- owner insert;
- owner update;
- admin read for support/safety;
- no anonymous access;
- no ordinary staff grant of another user's consent.

### Social identity card

`social_identity_cards`

- authenticated only;
- own identity may read own card;
- another identity may read only when both identities have valid active Journey-only social presence in the same Journey;
- blocking or moderation suppresses visibility;
- no anonymous read.

### Journey Presence

`journey_social_presences`

- owner may create only with verified Journey context;
- owner may withdraw/change own visibility;
- another member may read only through valid shared Journey social context;
- no anonymous read;
- block/moderation suppresses social visibility.

### Consent events

- owner/admin read;
- system-trigger write only;
- no client update/delete.

### Blocks

- blocker owns create/read/delete;
- blocked person cannot query the block through this table;
- admin may inspect for safety.

### Reports

- reporter may submit/read own reports;
- admin may review/update status;
- target has no read policy.

### Moderation controls

- admin-only read/write;
- no community user access.

## 7. CONSENT UX CONTRACT

### 7.1 Social identity activation

Canonical activation intent:

**VI**

> Bật hồ sơ cộng đồng để tham gia các không gian Journey. Hồ sơ này không tự động công khai và không xác nhận rằng bạn đã tham dự bất kỳ Journey nào.

**EN**

> Enable your community identity to take part in Journey spaces. This does not make your account publicly discoverable and does not claim that you attended any Journey.

Consent version for first implementation should be explicit and versioned, for example:

`p16-social-consent-v1`

### 7.2 Journey Presence opt-in

Preferred Vietnamese CTA:

> **THAM GIA KHÔNG GIAN JOURNEY**

Supporting copy:

> Cho phép những người cũng chọn hiện diện trong Journey này nhìn thấy tên, ảnh đại diện và phần giới thiệu cộng đồng của bạn. Đây là hiện diện trong không gian số của Journey — không phải xác nhận attendance.

Preferred English CTA:

> **JOIN THE JOURNEY SPACE**

Supporting copy:

> Let other people who also choose to be present in this Journey space see your community name, avatar and short introduction. This is digital Journey presence — not proof of attendance.

### 7.3 BEFORE-stage social language

Allowed:

- `Đang chuẩn bị cùng Journey`
- `Có mặt trong không gian Journey`
- `Preparing around this Journey`
- `In this Journey space`

Forbidden before attendance evidence:

- `Đã cùng đi`
- `Đã tham dự`
- `Đồng hành thực tế`
- `Attended together`
- `Went together`

## 8. WITHDRAWAL CONTRACT

A user can:

- leave/withdraw Journey social presence;
- disable social identity;
- block another social identity.

Disabling social identity must automatically withdraw all active Journey Presence rows so a future re-enable cannot silently republish previous presence choices.

Withdrawal does not delete or falsify operational truth.

Canonical explanation:

> **Rút chia sẻ không xóa lịch sử thật của Journey.**

## 9. SAFETY CONTRACT

WU2 intentionally provides safety before member-generated social interaction.

Launch boundaries:

- no DM/private messaging;
- no global public member search;
- no public participant directory;
- no follower/friend system;
- no generic status posting;
- no reaction counters;
- no online status;
- no real-time location;
- no precise live location;
- no anonymous social identity access.

For minors or vulnerable people, WU2 makes no age inference from profile data and creates no public exposure path. Any future age/guardian-specific feature requires an explicit policy and data model rather than inference.

Existing media/privacy consent remains a separate publication decision.

## 10. RELATIONSHIP WITH WU3 / WU4

### WU3 may use WU2 for

- Journey Room participant presence;
- Journey-only identity cards;
- Journey-scoped visibility;
- blocked-person filtering;
- safety reporting entry points.

### WU4 may use WU2 for

- consented Journey Connection visibility;
- evidence-backed shared-experience graph presentation.

WU4 MUST NOT derive a real-world shared-experience edge merely from `journey_social_presences`.

Shared real-world experience still requires appropriate attendance evidence from the Truth Ledger.

## 11. REAL 2026-09-11 JOURNEY PILOT

The real Journey dated 2026-09-11 remains in BEFORE phase at WU2 authoring time.

WU2 may support:

- an eligible participant choosing a social identity;
- opting into the Journey's digital space;
- seeing consented co-presence if another real eligible participant later opts in.

WU2 must NOT pre-create:

- attendance;
- attended Memory;
- shared-experience edge;
- `đã cùng đi` language;
- impact claims.

## 12. IMPLEMENTATION ARTIFACTS PREPARED

Product branch:

`p16-wu2-social-identity-consent-safety`

Prepared migrations:

- `db/migrations/0042_p16_wu2_social_identity_consent_safety.sql`
- `db/migrations/0043_p16_wu2_social_guard_hardening.sql`

Prepared rollback:

- `db/rollbacks/p16_wu2_social_identity_consent_safety.sql`

All remain unapplied to production.

## 13. BRANCH / BUILDER SAFETY FINDING

Repository production truth remains `main`, but GitHub repository default branch is currently `develop`.

At WU2 discovery, `develop` is strongly diverged from `main`.

Therefore:

> **Lovable must not be allowed to implement WU2 blindly against the repository default branch.**

Any Builder implementation must originate from the P16 WU2 branch created directly from production `main` SHA or another explicitly verified equivalent base.

This is a release-safety gate, not a product decision.

## 14. SUPABASE STAGING GATE

No Supabase development branch currently exists for this project.

Supabase reported branch cost at WU2 time:

**USD 0.01344/hour.**

Per operational safety, the prepared DDL should be applied and destructively/non-destructively QA-tested on a temporary Supabase development branch before production.

Creating that branch requires explicit Owner cost approval.

## 15. REQUIRED STAGING QA

Before WU2 can be `COMPLETE / PASS`, verify at minimum:

1. all new public tables have RLS enabled;
2. anon receives zero social identity/presence/report/block access;
3. user A cannot read private source for user B;
4. user A cannot create/update social identity for user B;
5. account/profile existence alone creates no social projection;
6. active social card contains no auth UUID/email/participant/attendance data;
7. ineligible user cannot create Journey Presence;
8. verified confirmed participant may explicitly opt in;
9. Journey Presence does not mutate `journey_participants`;
10. presence creation does not change attendance fields;
11. presence creation does not create Memory eligibility;
12. two users without shared active Journey presence cannot see each other's card;
13. two eligible users with mutual active Journey-only presence can see the permitted cards/presence;
14. block suppresses social visibility both directions;
15. block leaves Journey operational truth unchanged;
16. reported target cannot read reporter/report details;
17. admin can triage reports;
18. admin hide/suspend reduces social visibility;
19. admin moderation cannot enable user consent;
20. disabling social identity withdraws active Journey presences;
21. re-enabling social identity does not silently reactivate old Journey presences;
22. consent events are append-only/client-unwritable;
23. audit timestamps cannot be rewritten by ordinary client update;
24. rollback removes only P16-WU2 additive objects;
25. Security Advisor and Performance Advisor reviewed after DDL;
26. generated TypeScript database types regenerated after staging PASS.

## 16. SECURITY HARDENING FOLLOW-UP

Before broad social launch:

- review/enable leaked-password protection if current Supabase plan supports it;
- retain staff/admin MFA enforcement;
- review Auth CAPTCHA/rate limits before increased public account traffic;
- keep service-role keys out of browser clients;
- re-run Security Advisor after every social DDL change.

## 17. WU2 CURRENT DECISION

- Separate social identity from `profiles`: **APPROVED / IMPLEMENTED IN PREPARED DDL**
- Distinct social UUID from auth UUID: **APPROVED**
- Explicit opt-in social identity: **APPROVED**
- Explicit per-Journey Presence: **APPROVED**
- Journey Presence != attendance: **IMMUTABLE**
- `private` + `journey_only` presence in MVP: **APPROVED**
- public participant presence: **NOT ENABLED**
- global public people directory: **REJECTED**
- user-controlled withdrawal: **APPROVED**
- bidirectional social blocking: **APPROVED**
- reporter-private/admin-reviewed reports: **APPROVED**
- moderation may reduce but not grant visibility: **APPROVED**
- production DDL application: **NOT AUTHORIZED / NOT PERFORMED**
- staging DDL QA: **PENDING OWNER COST GATE**

## 18. STATUS

**P16-WU2 = ARCHITECTURE LOCKED / IMPLEMENTATION PREPARED / STAGING QA PENDING.**

WU3 remains gated until WU2 staging QA demonstrates that social visibility, consent, block/report and moderation behave as designed without weakening Journey truth.
