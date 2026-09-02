# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU3 — JOURNEY COMMUNITY ROOM & TYPED JOURNEY STREAM

Date: 2026-09-02  
Status: **COMPLETE / PASS — SOURCE & ARCHITECTURE**  
Database mutation: **NONE**  
Production Worker deploy: **NONE**  
Production Journey Room UX activation: **OFF / SEPARATE OWNER GATE**

## 1. PURPOSE

P16-WU3 converts the P16 Journey-Based Social Network thesis and the WU2 privacy/safety substrate into the first Journey-scoped social experience.

The primary product object remains the Journey, not a generic post and not a person profile.

WU3 implements:

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

WU3 adds:

> **Typed Stream projection != a new source of truth.**

A Stream item exists only because an already-governed source row has an appropriate publication/evidence boundary and a truthful source timestamp.

## 3. ARCHITECTURE DECISION — NO GENERIC POSTS TABLE

WU3 creates no `social_posts`, generic feed, status-update or user-posting table.

Existing source semantics remain distinct:

- `journey_updates` — operational factual Journey updates;
- Field Notes — editorial publication;
- `journey_media` / `media_assets` — documentary evidence/media relation;
- `journey_reflection_publications` — identity-minimized published Reflection projection.

The Journey Stream is a typed projection over these sources, not a new content truth ledger.

Therefore WU3 does not collapse:

- a staff Field Update into a social post;
- an editorial Field Note into member-generated content;
- a documentary photo into proof of attendance;
- a published Reflection into a generic status update.

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

A Journey Update enters the chronological Stream only when it has a real `happened_at`.

WU3 does not substitute creation/publication timestamps and imply that an event occurred then.

### 4.2 Field Note

A Field Note is an editorial publication. Its Stream placement uses publication time, explicitly not real-world event time.

### 4.3 Documentary Moment

Journey-level media enters the Stream only when:

- `captured_at` exists; and
- the media is not already attached to a Journey Update.

Media without `captured_at` can remain documentary/context material elsewhere but cannot become a fabricated chronological event.

Update-attached media is not duplicated as a second Stream item.

### 4.4 Published Reflection

Only `journey_reflection_publications` enters the WU3 Stream.

Private `journey_reflections` source rows are not read by the Room.

The time basis is publication time, not Journey event time.

## 5. EXPLICIT STREAM EXCLUSIONS

WU3 does not turn any of the following into Stream activity:

- registration/application;
- confirmed registration;
- participant status;
- unresolved/no-show/attended state;
- attendance count;
- Memory or Memory eligibility;
- private Contribution;
- operational relationship assignments;
- social identity activation;
- Journey Presence join/withdraw events;
- block/report/moderation events;
- generic user status posts;
- Friend/Follow activity;
- reaction counters;
- online status;
- live or precise location.

These keep their own truth/privacy semantics.

## 6. JOURNEY COMMUNITY ROOM

The Journey Room is composed into the existing Journey detail experience. It does not replace Story, lifecycle, registration, Field Updates, Evidence, Impact or Field Notes.

The Room contains two distinct concepts:

1. typed Journey Stream;
2. consented people-in-this-Journey-space layer.

There is no global feed in WU3.

## 7. SOCIAL IDENTITY ACTIVATION

Account existence does not create social visibility.

A signed-in person must explicitly enable a social identity with consent version:

`p16-social-consent-v1`

Canonical intent remains:

> Bật hồ sơ cộng đồng để tham gia các không gian Journey. Hồ sơ này không tự động công khai và không xác nhận rằng bạn đã tham dự bất kỳ Journey nào.

> Enable your community identity to take part in Journey spaces. This does not make your account publicly discoverable and does not claim that you attended any Journey.

The Room does not read `profiles` to manufacture a public social person.

## 8. PER-JOURNEY PRESENCE CONSENT

Social identity activation does not automatically publish the person into every Journey.

Joining each Journey space is a separate explicit decision using:

- state: `active`
- visibility: `journey_only`
- consent version: `p16-social-consent-v1`

Canonical CTA:

- VI: **THAM GIA KHÔNG GIAN JOURNEY**
- EN: **JOIN THE JOURNEY SPACE**

Canonical meaning:

> I choose to be visible in this Journey's digital community context.

It does NOT mean:

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

The Room reads WU2 social projections governed by RLS:

- own `social_identities` source;
- `journey_social_presences`;
- `social_identity_cards`.

It does not read operational/private Journey participant or attendance sources to render people.

## 10. SAFETY ENTRY POINTS

Because WU3 begins showing consented people to one another, WU2 safety controls are present before richer member-generated interaction launches.

### Block

Block suppresses governed social visibility/interactions but does not rewrite Journey operational history.

### Report

A user can submit a reporter-private social safety report against another visible Journey social identity/presence. Review remains under the WU2 admin model.

The target cannot read reporter identity/details through the report table.

## 11. WITHDRAWAL / DISABLE CONTRACT

A user can:

- leave a Journey space;
- disable social identity;
- block another person.

Immutable rule:

> **Rút chia sẻ không xóa lịch sử thật của Journey.**

None of these social actions mutates:

- application/registration;
- participant record;
- attendance;
- Memory;
- Reflection source;
- Contribution;
- verified operational relationship truth.

## 12. RELEASE SAFETY

WU3 adds a dedicated fail-closed Room flag:

`VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED`

The Room renders only when BOTH are true:

- `VITE_APP_COMMUNITY_AUTH_ENABLED=true`
- `VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED=true`

This means WU3 source can exist on `main` while production Journey Room UX stays off.

Controlled commands now exist:

- `cf:p16-wu3:activate:dry-run`
- `cf:p16-wu3:activate:deploy`
- `cf:p16-wu3:rollback:dry-run`
- `cf:p16-wu3:rollback:deploy`

Rollback preserves Community Auth while disabling only the Journey Room flag.

No production Worker deployment occurred in WU3 closeout.

## 13. DATABASE / RLS DECISION

WU3 requires no database migration.

It reuses WU2 production social foundation plus existing Journey publication/evidence sources.

No WU3 production data mutation was performed.

RLS remains the client authorization boundary. No service-role key is introduced into browser code.

## 14. REAL JOURNEY 2026-09-11 TRUTH

Journey:

- id: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- date: 2026-09-11
- current lifecycle at WU3 closeout: BEFORE / registration open

Read-only WU3 discovery/post-flight evidence:

- participant count: 1
- attendance unresolved: 1
- verified attended: 0
- published Journey Updates: 0
- Field Notes: 0 at discovery
- Reflection publications: 0
- social identities: 0
- social identity cards: 0
- Journey social presences: 0
- active Journey-only presences: 0
- blocks: 0
- reports: 0
- Journey media relations: 1 at discovery

The one documentary relation had `captured_at = NULL`, so it does not become a chronological Stream item.

Truthful expected Room state before additional real evidence/publication exists:

- BEFORE-stage context;
- quiet/empty Stream;
- no fake people;
- no fake activity;
- no attendance claim;
- no shared-experience claim.

## 15. REALTIME DECISION

Realtime is not enabled in WU3 v1.

No online status, typing indicator, instant reaction, live presence or precise location is required to establish the Journey Room concept.

Future realtime use requires a Journey-specific need plus privacy/safety review.

## 16. WU5 BOUNDARY

WU3 does not introduce member-generated Question / Reply / Appreciation.

Those remain sequenced for:

**P16-WU5 — Interaction v1: Journey Question / Reply / Appreciation**

People visibility and safety therefore precede member-generated interaction.

## 17. SOURCE IMPLEMENTATION / MERGE

Implementation branch:

`p16-wu3-journey-community-room-typed-stream`

Base production `main` SHA:

`f9219b05d1faca9aa6845f1f17db851c1925f25e`

Pull request:

`#52 — P16-WU3: Journey Community Room and typed Journey stream`

PR #52 was squash-merged after full PR CI PASS.

Canonical product `main` SHA after merge:

`2683e937accde7b8c1e22acca2fd87ff3ed736f2`

Primary source artifacts on `main`:

- `src/lib/journeys/community-room.ts`
- `src/lib/journeys/community-room-activation.ts`
- `src/components/journeys/journey-community-room.tsx`
- `src/components/journeys/journey-detail-page.tsx`
- `scripts/p16-wu3-journey-community-room-qa.ts`
- `package.json`
- `.github/workflows/ci.yml`

## 18. AUTOMATED QA CONTRACT

`qa:p16-wu3` verifies:

- Community Auth + Room double activation gate;
- exact typed Stream kinds;
- truthful event/publication/capture time bases;
- no private/operational truth reads by Room UI;
- social identity + Journey Presence consent;
- block/report integration;
- no generic social primitives;
- explicit Journey Presence != attendance copy;
- no-time Field Update excluded;
- no-time media excluded;
- update-attached media not duplicated;
- canonical Field Note routing;
- Reflection chronological behavior;
- deterministic descending Stream ordering.

Inherited CI also executes all existing P9-P15 source and ephemeral DB regression gates, production build, typecheck and Cloudflare dry-run.

## 19. CI EVIDENCE

### PR run #209

- WU3 QA: PASS
- inherited source/DB QA: PASS
- build: PASS
- typecheck: FAIL

The only issue was TypeScript strict env-property access (`TS4111`) for the new Room flag.

The fix changed only the env lookup to bracket access; no product/security behavior changed.

### PR re-run #210

- WU3 QA: PASS
- all inherited source QA: PASS
- all inherited ephemeral database QA: PASS
- build: PASS
- typecheck: PASS
- Cloudflare dry-run: PASS
- result: **SUCCESS**

### Post-merge main run #211

Executed on exact `main` SHA `2683e937accde7b8c1e22acca2fd87ff3ed736f2`.

- WU3 QA: PASS
- all inherited source QA: PASS
- all inherited ephemeral database QA: PASS
- build: PASS
- typecheck: PASS
- Cloudflare dry-run: PASS
- result: **SUCCESS**

The Cloudflare step was dry-run only; no Worker upload/deploy occurred.

## 20. CLOSEOUT DECISION

- Journey is primary social object: **PASS / IMMUTABLE**
- generic posts/feed model introduced: **NO**
- typed Journey Stream: **PASS**
- truthful source timestamps: **PASS**
- quiet empty state rather than fabricated activity: **PASS**
- social identity explicit opt-in: **PASS**
- per-Journey Presence explicit opt-in: **PASS**
- People list != attendee list: **PASS / IMMUTABLE**
- Journey Presence != attendance: **PASS / IMMUTABLE**
- block/report entry points: **PASS**
- no private operational truth reads in Room: **PASS**
- database migration: **NONE**
- production DB mutation: **NONE**
- PR CI: **PASS**
- post-merge `main` CI: **PASS**
- product Source of Truth merged to `main`: **PASS**
- production Journey Room deployment/activation: **OFF / NOT PART OF CLOSEOUT**

# P16-WU3 — COMPLETE / PASS

Next gate:

**P16-WU4 — JOURNEY PRESENCE & SHARED-EXPERIENCE GRAPH**

WU4 MUST derive shared real-world experience only from appropriate attendance evidence. `journey_social_presences` may express consented digital context but can never, by itself, establish that two people actually went on a Journey together.
