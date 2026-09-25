# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.2
# ORIGINAL JOURNEY POST + PUBLISHING AUTHORIZATION FOUNDATION

Date: 2026-09-25  
Status: **COMPLETE / PASS — SOURCE FOUNDATION MERGED; NO PRODUCTION DB CUTOVER**

## 1. Objective

Introduce the canonical original Journey Post object and prove that publishing authorization is derived from confirmed Journey participation plus the Journey Publishing Window — not from Journey Presence and not from attendance.

## 2. Product source truth

Repository:

`huynhtranhuythinh/tramnucuoi`

PR:

`#105 — P17-WU6.2: Original Journey Post publishing foundation`

Final PR exact head:

`25aa2c4679806c445bb36092f7e6ceb467743405`

Squash-merged product `main` SHA:

`fa8d07ecbb220f1eb21acd811970fd03be36ba62`

Source contract:

`database/contracts/p17_wu6_2_journey_community_posts.sql`

Source QA:

`scripts/p17-wu6-2-journey-post-source-qa.ts`

Ephemeral PostgreSQL QA:

`scripts/p17-wu6-2-journey-post-db-qa.sql`

## 3. Canonical post object

WU6.2 introduces the source contract for:

`public.journey_community_posts`

Core provenance:

- Journey ID;
- author social identity;
- author participant record;
- content;
- locale;
- social lifecycle state;
- publication lifecycle state;
- timestamps.

The composite participant/Journey foreign key guarantees that the stored participant provenance belongs to the same Journey as the post.

`author_participant_id` means:

> this confirmed participant record authorized the post at creation time.

It does **not** mean:

- attended;
- verified attended;
- shared real-world experience;
- Memory eligible;
- impact contributor.

## 4. Publishing authority

Canonical publishing predicate requires:

1. authenticated account;
2. owned social identity;
3. social identity enabled;
4. no active social interaction/suspension restriction;
5. active verified participant link;
6. linked Journey participant remains `confirmed`;
7. exact Journey matches the participant provenance;
8. current time is inside the configured WU6.1 Publishing Window.

Explicitly excluded:

- Journey Presence;
- attendance fields;
- Memory;
- Shared Journey edge;
- application window state.

Therefore a confirmed participant may contribute socially before the real-world Journey date without the platform falsely claiming real attendance.

## 5. Privacy-first publication

New posts are server-forced to:

- `state = active`;
- `publication_state = participants_only`;
- no public publication request timestamp;
- no public-published timestamp;
- no public-unpublished timestamp.

A client cannot bypass this by inserting `public`, `moderated`, or publication timestamps.

Public publication workflow remains deferred to WU6.5.

## 6. WU6.2 read model

WU6.2 intentionally remains participant-only.

Readable by:

- the post author;
- Admin;
- another currently confirmed participant of the same Journey, subject to social blocking/moderation visibility.

Not readable by:

- anon;
- an authenticated outsider;
- a participant from another Journey.

Public read is not introduced in WU6.2.

## 7. User content control

WU6.2 source guard supports:

- creating a privacy-first text post;
- editing the body of an own active post while current publishing authority remains valid;
- withdrawing an own active post.

WU6.2 intentionally does not support:

- restoring a withdrawn post;
- public publication transition;
- moderation transition;
- participant media attachments.

Media/feed/composer extension belongs to WU6.3.

## 8. Critical DB proof

The WU6.2 ephemeral PostgreSQL fixture intentionally does **not create a Journey Presence table**.

An eligible participant still successfully creates an original Journey Post.

This proves at the database-contract level:

`Journey Presence != publishing authority`

The eligible participant fixture also has:

- `attended_party_size = NULL`;
- `attendance_recorded_at = NULL`;
- `attendance_recorded_by = NULL`.

Post creation succeeds while those values remain unchanged.

This proves:

`approved/confirmed participation != attendance`

and:

`social publishing != attendance evidence`.

## 9. Negative authorization proof

Dedicated DB QA proves denial for:

- author attempting to use another participant's provenance;
- confirmed participant outside Publishing Window;
- revoked participant link;
- authenticated outsider;
- moderation-restricted social identity;
- disabled social identity;
- peer attempting to edit another person's post;
- owner attempting to jump directly into public publication state;
- anon attempting direct table read.

## 10. Corrective QA history

Initial DB QA failed because the authenticated participant fixture attempted a direct read of `journey_participants` to inspect the attendance sentinel.

That denial was correct: the participant should not receive direct operational-table access.

The fixture was corrected by keeping attendance verification in the fixture-owner/global sentinel section.

No product authorization, RLS, trigger or privacy contract was weakened.

## 11. Exact-head verification

Final PR head:

`25aa2c4679806c445bb36092f7e6ceb467743405`

Final PR checks:

- dedicated P16-WU10B inherited gate: **SUCCESS**
- generic CI: **SUCCESS**
- WU6.2 source QA: **PASS**
- WU6.2 PostgreSQL DB QA: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

Post-merge `main`:

`fa8d07ecbb220f1eb21acd811970fd03be36ba62`

Post-merge checks:

- main verify: **SUCCESS**
- staging Worker build pipeline: **SUCCESS**
- production Worker build pipeline: **SUCCESS**

## 12. Production boundary

WU6.2 applies no Supabase production migration.

Production migration ledger therefore remains unchanged from WU5.Final.

The Community policy table and Journey Post table remain source contracts until WU6.Final.

No fake post rows are seeded.

No social feature flag is enabled.

## 13. Final decision

# **P17-WU6.2 — COMPLETE / CLOSED / PASS**

Next:

# **P17-WU6.3 — JOURNEY COMMUNITY FEED & PARTICIPANT COMPOSER**
