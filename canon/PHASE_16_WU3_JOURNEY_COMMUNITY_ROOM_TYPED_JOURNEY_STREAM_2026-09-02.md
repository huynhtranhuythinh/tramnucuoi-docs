# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU3 — JOURNEY COMMUNITY ROOM & TYPED JOURNEY STREAM

Date: 2026-09-02  
Status: **SOURCE IMPLEMENTED / PR #52 CI RE-RUN PENDING / PRODUCTION UX OFF**  
Database mutation: **NONE**  
Production social UX activation: **OFF**

## 1. PURPOSE

P16-WU3 converts the P16 Journey-Based Social Network thesis and the WU2 privacy/safety substrate into the first Journey-scoped social experience.

The primary product object remains the Journey, not a generic post and not a person profile.

The work unit introduces:

- a Journey Community Room composed into Journey detail;
- a typed Journey Stream derived from already-governed Journey truth/publication sources;
- explicit social identity activation and per-Journey digital presence consent;
- Journey-scoped people visibility through WU2 RLS;
- block/report entry points before richer member-generated interaction is introduced.

WU3 intentionally does NOT introduce a generic social posting model.

## 2. GOVERNING CANON

> **TRẠM NỤ CƯỜI is a Journey-Based Social Network.**
>
> **Journey — not Post and not Person — is the primary social object.**
>
> **Digital relationships may begin around preparation for a real Journey, but claims of shared real-world experience arise only from appropriate evidence.**
>
> **TNC social features exist to extend meaningful real-world relationships, not maximize time-on-platform, popularity or content volume.**
>
> **Public visibility is always a separate consent/publication decision from private operational truth.**

WU3 adds one further implementation rule:

> **Typed Stream projection != a new source of truth.**

A Stream item exists only because an already-governed source row has an appropriate publication/evidence boundary and a truthful source timestamp.

## 3. ARCHITECTURE DECISION — NO GENERIC POSTS TABLE

WU3 does not create a `social_posts`, generic feed, status-update or user-posting table.

Existing sources already carry distinct semantics:

- `journey_updates` — operational factual Journey updates;
- Field Notes — editorial publication;
- `journey_media` / `media_assets` — documentary evidence/media relation;
- `journey_reflection_publications` — identity-minimized published Reflection projection.

The Journey Stream is therefore a typed projection over these sources rather than a new content truth ledger.

This avoids semantic collapse such as treating:

- a staff Field Update as a social post;
- an editorial Field Note as member-generated content;
- a documentary photo as proof of attendance;
- a published Reflection as a generic status update.

## 4. TYPED JOURNEY STREAM V1

Canonical stream kinds:

1. `journey_update`
2. `field_note`
3. `documentary_media`
4. `reflection_publication`

Canonical time bases:

- `happened_at`
- `published_at`
- `captured_at`

### 4.1 Field Update

A published Journey Update enters the chronological Stream only when it has a real `happened_at`.

WU3 MUST NOT substitute `created_at` or `published_at` and imply that an event happened at that time.

Time label:

- VI: `thời điểm sự việc`
- EN: `event time`

### 4.2 Field Note

A Field Note is an editorial publication.

Its chronological placement uses publication time and MUST NOT be presented as the time the real-world event occurred.

Time label:

- VI: `thời điểm xuất bản`
- EN: `publication time`

### 4.3 Documentary Moment

Journey-level media may enter the Stream only when:

- it has a real `captured_at`; and
- it is not already attached to a Journey Update.

Media without `captured_at` may remain documentary/context material elsewhere on the Journey page but does not become a fabricated chronological event.

Media already attached to a Field Update is not duplicated into a second Stream item.

Time label:

- VI: `thời điểm ghi nhận`
- EN: `capture time`

### 4.4 Published Reflection

Only `journey_reflection_publications` may enter the WU3 Stream.

Private `journey_reflections` source rows are not read by the Room.

The publication layer is already identity-minimized and public under the Reflection trust model.

Its time basis is publication time, not Journey event time.

## 5. EXPLICIT STREAM EXCLUSIONS

WU3 MUST NOT turn any of the following into a Journey Stream item:

- Journey registration/application;
- confirmed registration;
- participant status;
- attendance unresolved / no-show / attended state;
- attendance count;
- Memory state or Memory eligibility;
- private Contribution records;
- operational relationship assignments;
- social identity activation;
- Journey Presence join/withdraw events;
- block/report/moderation events;
- generic user status posts;
- follower/friend activity;
- reaction counters;
- online status;
- live or precise location.

These objects retain their own truth/privacy semantics.

## 6. JOURNEY COMMUNITY ROOM

The Journey Room is composed into the existing Journey detail experience. It does not replace Story, lifecycle, registration, Field Updates, Evidence, Impact or Field Notes.

The Room has two distinct layers:

1. typed Journey Stream;
2. consented people-in-this-Journey-space layer.

The Room is Journey-scoped. There is no global social feed in WU3.

## 7. SOCIAL IDENTITY ACTIVATION

Account existence does not create social visibility.

A signed-in person may explicitly enable a social identity with consent version:

`p16-social-consent-v1`

Canonical activation copy:

### VI

> Bật hồ sơ cộng đồng để tham gia các không gian Journey. Hồ sơ này không tự động công khai và không xác nhận rằng bạn đã tham dự bất kỳ Journey nào.

### EN

> Enable your community identity to take part in Journey spaces. This does not make your account publicly discoverable and does not claim that you attended any Journey.

The Room does not read `profiles` to manufacture a public social person.

## 8. PER-JOURNEY PRESENCE CONSENT

Enabling a social identity does not automatically make the person visible in every Journey.

Joining each Journey space is a separate explicit decision using:

- state: `active`
- visibility: `journey_only`
- consent version: `p16-social-consent-v1`

Canonical CTA:

### VI

**THAM GIA KHÔNG GIAN JOURNEY**

### EN

**JOIN THE JOURNEY SPACE**

Canonical meaning:

> I choose to be visible in this Journey's digital community context.

It MUST NOT mean:

- I attended;
- I completed the Journey;
- I have a Memory;
- I share a verified real-world experience with another visible person.

## 9. PEOPLE LIST CONTRACT

The people list is a consented Journey-scoped digital-presence list.

It is explicitly NOT:

- an attendee list;
- a public participant directory;
- a popularity ranking;
- a follower graph;
- proof of shared real-world experience.

The Room reads only WU2 social projections governed by RLS:

- `social_identities` for the current user's private source;
- `journey_social_presences`;
- `social_identity_cards`.

It does not read operational/private Journey sources to render people.

## 10. SAFETY ENTRY POINTS

Because WU3 begins showing consented people to one another, WU2 safety controls are available from the Room before richer interaction launches.

### Block

A user may block another visible social identity.

Block suppresses governed social visibility/interactions but does not rewrite Journey operational history.

### Report

A user may submit a reporter-private social safety report against another visible Journey social identity/presence.

The report is reviewed through the existing WU2 admin model.

The reported target does not gain access to reporter identity/details through the report table.

## 11. WITHDRAWAL / DISABLE CONTRACT

A user may:

- leave a Journey space;
- disable the social identity;
- block another person.

Canonical invariant:

> **Rút chia sẻ không xóa lịch sử thật của Journey.**

Leaving or disabling social visibility does not mutate:

- application/registration;
- participant record;
- attendance;
- Memory;
- Reflection source;
- Contribution;
- verified relationship truth.

## 12. RELEASE SAFETY

WU3 introduces a dedicated fail-closed Room flag:

`VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED`

The Room is enabled only when BOTH are true:

- `VITE_APP_COMMUNITY_AUTH_ENABLED=true`
- `VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED=true`

Therefore source code may be merged to `main` while Journey Room remains invisible in production until a separate activation decision.

Controlled scripts prepared:

- `cf:p16-wu3:activate:dry-run`
- `cf:p16-wu3:activate:deploy`
- `cf:p16-wu3:rollback:dry-run`
- `cf:p16-wu3:rollback:deploy`

Rollback keeps Community Auth available while setting the Room flag false.

No production deploy is part of source implementation/merge.

## 13. DATABASE / RLS DECISION

WU3 requires no database migration.

It reuses WU2 production social foundation and existing Journey publication/evidence tables.

No production database row was created, updated or deleted by WU3 implementation work.

RLS remains the authorization boundary for client social reads/writes.

No service-role key is introduced into browser code.

## 14. REAL JOURNEY 2026-09-11 SNAPSHOT

Journey:

- id: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- date: 2026-09-11
- status at WU3 implementation time: `registration_open`

Read-only production snapshot during WU3 discovery:

- published Journey Updates: 0
- Field Notes: 0
- Reflection publications: 0
- Contributions: 0
- impact items: 0
- Journey social presences: 0
- active Journey-only presences: 0
- Journey media relations: 1

The one Journey media relation is approved public documentation but has `captured_at = NULL`.

Therefore it MUST NOT become a chronological `documentary_media` Stream item.

Expected truthful WU3 Room state for this real Journey before additional real evidence/publication exists:

- BEFORE-stage Journey context;
- quiet/empty typed Stream;
- no fake people;
- no fake activity;
- no attendance claim;
- no shared-experience claim.

## 15. REALTIME DECISION

Realtime is NOT enabled in WU3 v1.

The product does not need live social presence, online status, typing indicators, instant reactions or live location to establish the Journey Room concept.

Realtime may be considered later only where it serves a clear Journey need and preserves privacy/safety boundaries.

## 16. WU5 BOUNDARY

WU3 does not introduce member-generated Question / Reply / Appreciation interaction.

Those belong to:

**P16-WU5 — Interaction v1: Journey Question / Reply / Appreciation**

This sequencing ensures people visibility and safety boundaries exist before member-generated interaction is activated.

## 17. SOURCE IMPLEMENTATION

Product branch:

`p16-wu3-journey-community-room-typed-stream`

Base production `main` SHA:

`f9219b05d1faca9aa6845f1f17db851c1925f25e`

Pull request:

`#52 — P16-WU3: Journey Community Room and typed Journey stream`

Primary source artifacts:

- `src/lib/journeys/community-room.ts`
- `src/lib/journeys/community-room-activation.ts`
- `src/components/journeys/journey-community-room.tsx`
- `src/components/journeys/journey-detail-page.tsx`
- `scripts/p16-wu3-journey-community-room-qa.ts`
- `package.json`
- `.github/workflows/ci.yml`

## 18. AUTOMATED QA CONTRACT

`qa:p16-wu3` verifies at minimum:

- Community Auth + Room double activation gate;
- exact typed Stream kinds;
- truthful event/publication/capture time bases;
- no private/operational truth source reads by Room UI;
- social identity + Journey Presence consent use;
- block/report integration;
- no generic social primitives;
- explicit Journey Presence != attendance language;
- no-time Field Update excluded;
- no-time media excluded;
- update-attached media not duplicated;
- Field Note canonical routing;
- published Reflection chronological behavior;
- deterministic descending Stream ordering.

CI additionally runs all existing P9-P15 source and ephemeral database regression gates, production build, TypeScript typecheck and Cloudflare dry-run.

## 19. CI HISTORY DURING IMPLEMENTATION

Initial PR #52 CI run #209:

- P16-WU3 dedicated QA: PASS
- existing regression source QA: PASS
- existing ephemeral database QA: PASS
- build: PASS
- typecheck: FAIL

The only compiler failure was strict TypeScript env-property access:

`TS4111` for `VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED`.

The implementation was corrected to bracket access without changing product behavior or architecture.

PR CI re-run #210 is the current closeout gate.

## 20. CLOSEOUT GATE

P16-WU3 may be marked `COMPLETE / PASS` only after:

1. PR #52 WU3 QA PASS;
2. all inherited regression QA PASS;
3. build PASS;
4. typecheck PASS;
5. Cloudflare dry-run PASS;
6. source merged into production `main`;
7. post-merge `main` CI PASS;
8. production Room UX remains OFF unless separately activated by Owner approval.

Next product gate after WU3 source closeout:

**P16-WU4 — JOURNEY PRESENCE & SHARED-EXPERIENCE GRAPH**

WU4 MUST derive shared real-world experience only from appropriate attendance evidence, never from Journey social Presence alone.
