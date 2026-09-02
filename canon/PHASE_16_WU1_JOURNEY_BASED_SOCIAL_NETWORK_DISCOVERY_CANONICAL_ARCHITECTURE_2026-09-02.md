# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU1 — JOURNEY-BASED SOCIAL NETWORK PRODUCT DISCOVERY & CANONICAL ARCHITECTURE

Date: 2026-09-02
Status: **COMPLETE / PASS**
Owner decision: **CANONICAL PRODUCT THESIS APPROVED**

## 1. PURPOSE

P16-WU1 establishes the canonical product and architecture direction for evolving TRẠM NỤ CƯỜI from a living digital community experience into a **Journey-Based Social Network** without weakening the truth, privacy, attendance, Memory, Reflection, Contribution or relationship boundaries established through Phases 14 and 15.

This work unit is discovery and canonical architecture only.

No production mutation, attendance mutation, Memory fabrication, Reflection fabrication or inferred social relationship was performed.

## 2. OWNER CANONICAL PRODUCT DECISION

The following is the canonical Phase 16 product thesis:

> **TRẠM NỤ CƯỜI is a Journey-Based Social Network.**
>
> **Journey — not Post and not Person — is the primary social object.**
>
> **Digital relationships may begin around preparation for a real Journey, but claims of shared real-world experience arise only from appropriate evidence.**
>
> **TNC social features exist to extend meaningful real-world relationships, not maximize time-on-platform, popularity or content volume.**
>
> **Public visibility is always a separate consent/publication decision from private operational truth.**

This thesis governs all subsequent P16 product, schema, UX, privacy and moderation decisions.

## 3. P15 CANONICAL EVOLUTION

Phase 15 correctly established that Community was not to become a generic social network.

P16 intentionally evolves that boundary as follows:

> **Community is not a generic social network. TRẠM NỤ CƯỜI may become a Journey-Based Social Network.**

This is a deliberate canonical evolution, not an implicit rollback of Phase 15.

The following Phase 15 protections remain authoritative:

- no generic social/news feed;
- no global public member directory derived from private participant data;
- no follower-count or popularity model;
- no leaderboard, points, badges or vanity impact counters;
- no inferred attendance;
- no fabricated Community activity;
- no fabricated impact;
- no automatic publication of private Community truth.

P16 may introduce Journey-scoped social capabilities only when they are justified by the Journey relationship model and privacy architecture.

## 4. IMMUTABLE TRUTH RULES CARRIED FORWARD

The following distinctions remain non-negotiable:

- registration != attendance;
- confirmed registration != attendance;
- attendance NULL = unresolved;
- attendance 0 = verified no-show;
- attendance > 0 = verified attended;
- participant claim != attendance;
- participant claim != Memory eligibility;
- account != participant;
- account != attendance;
- relationship role != CMS permission;
- Memory only from real evidence;
- Reflection remains evidence-gated;
- explicit attendance truth outranks date-derived lifecycle presentation;
- no fake Community activity;
- no fake impact;
- no inferred attendance presented as fact.

P16 adds a new separation rule:

> **Social visibility is not operational truth.**

A person may hide, withdraw or block social visibility without rewriting historical attendance or other private operational evidence.

## 5. DISCOVERY SUMMARY

Weighted architecture assessment from WU1:

**Journey-Based Social Network readiness: approximately 60%.**

The strongest existing foundations are:

- Journey lifecycle and truth architecture;
- verified participant linkage;
- attendance evidence separation;
- My TNC private relationship archive;
- evidence-backed Journey Memory;
- private Reflection plus moderated public publication;
- Contribution and verified relationship records;
- Journey updates, documentary media and field-note structures;
- Auth, MFA and RLS foundations;
- bilingual VI/EN and mobile-first P15 experience system.

The largest missing social-native capabilities are:

- social exposure consent;
- Journey-scoped presence;
- Journey Community Room membership/visibility;
- shared-experience graph;
- social graph privacy;
- blocking/reporting;
- moderation for member-generated interactions;
- notification infrastructure;
- Journey-scoped interaction model;
- vulnerable-community/minor protection architecture.

No existing production tables were found for generic follow/friend/comment/reaction/message/feed/notification/block/report/room/social-stream models. This is considered beneficial because P16 can design a Journey-native social layer without inheriting a generic social-network schema.

## 6. CANONICAL PRODUCT MODEL

The canonical social loop is:

**Journey → People → Shared Context → Real Experience → Memory → Reflection → Relationship → Reconnection → Next Journey**

The corresponding retention loop is:

**Discover → Join → Prepare Together → Meet → Experience → Document → Memory → Reflect → Relationship → Reconnect → Discover Next Journey → Go Again**

Preparation may create a legitimate digital relationship context.

Only appropriate evidence may create claims of shared real-world experience.

## 7. CANONICAL ARCHITECTURE

P16 adopts a five-layer model.

### Layer 1 — Truth Ledger

Existing authoritative domain truth remains the source of record for:

- Journey;
- registration/application;
- participant;
- attendance;
- Memory;
- Reflection;
- Contribution;
- verified relationships;
- documentary evidence.

Social mechanics must not mutate these facts indirectly.

### Layer 2 — Social Consent Projection

P16 must introduce a separate consent-controlled social presentation layer rather than exposing operational tables directly.

Candidate concepts include:

- social identity settings;
- profile discoverability preferences;
- Journey presence;
- Journey-only/public/private visibility;
- per-object sharing consent;
- withdrawal/revocation state.

Operational `profiles` and `journey_participants` must not become a public member directory.

### Layer 3 — Journey Community Room

**APPROVED IN PRINCIPLE.**

One Journey may have one durable digital Community Room spanning:

**BEFORE → DURING → AFTER**

The Room is the digital home of one real Journey.

Before the Journey it may support preparation, organizer updates, questions and consented participant presence.

During the Journey it may support contextual field/documentary updates.

After the Journey it may become a shared memory space connected to attendance evidence, Memories, Reflections, Contributions, relationship continuity, anniversary/revisit and future relevant Journeys.

Critical rule:

> **Journey Room membership != attendance.**

### Layer 4 — Shared-Experience / Encounter Graph

P16 does not adopt Friend or Follow as the primary social graph.

The canonical graph is based on real shared Journey context.

The architecture must distinguish:

1. private factual encounter truth derived from evidence-backed shared attendance; and
2. user-facing Journey Connection / Người đồng hành visibility subject to consent and privacy rules.

A future user-facing statement such as:

> `Bạn và Minh đã cùng đi 3 Journey.`

may only exist when all underlying shared-Journey facts are appropriately evidence-backed and the presentation satisfies privacy/consent requirements.

This is shared-history provenance, not a popularity metric.

### Layer 5 — Journey Stream

P16 may introduce a feed-like surface only as a **typed Journey event stream**.

The stream is not a generic status feed.

Candidate event types include:

- Journey opened;
- preparation update;
- organizer update;
- field update;
- approved documentary publication;
- consented Memory share;
- published Reflection;
- meaningful verified Contribution;
- Journey anniversary;
- relevant next Journey.

Stream items should reference authoritative source objects rather than duplicate them as independent generic posts wherever feasible.

Canonical principle:

> **Feed = the flow of real Journeys, not the flow of status updates.**

## 8. INTERACTION MODEL — WU1 DECISION

P16 does not automatically adopt common social-network interactions.

Initial assessment:

- Journey question: viable;
- reply: viable;
- appreciation: viable;
- save: possible;
- share: possible only with visibility enforcement;
- mention: deferred;
- generic Like: not prioritized;
- Friend: rejected as primary model;
- Follow: rejected as primary model;
- private messaging: deferred / not MVP;
- generic user status posts: rejected;
- infinite global feed: rejected.

Every interaction must justify how it improves Journey quality, relationship continuity or meaningful community formation.

## 9. PRIVACY / CONSENT CANON

P16 social visibility must be stricter than standard social media.

Minimum visibility model:

- Private;
- Journey-only;
- Public.

Visibility must be evaluated per object or capability rather than through one broad `public profile` switch.

Examples of independently controlled visibility include:

- profile discoverability;
- Journey presence;
- Memory sharing;
- Reflection publication;
- media/photo/video visibility;
- relationship visibility;
- social graph visibility.

Public visibility is always separate from operational truth.

## 10. SAFETY / MODERATION CANON

Safety cannot wait until late P16 after interaction features are already active.

Before member-generated social interaction is piloted, architecture must support at minimum:

- blocking;
- reporting;
- moderation state;
- visibility withdrawal;
- audit trail;
- anti-harassment boundaries;
- vulnerable-community protection;
- minor/guardian policy where applicable;
- no precise live-location exposure by default;
- no automatic public social graph.

## 11. NETWORK EFFECT THESIS

The relevant TNC network effect is not generic content volume.

The candidate network effect is:

**more real Journeys → more verified shared experiences → richer trusted relationship graph → richer Memories/documentary context → more reconnection → more repeat Journeys → stronger community continuity**

This is an **experience-graph network effect**.

It may provide long-term value for NGO / CSR / community building through privacy-safe understanding of:

- repeat participation;
- community continuity;
- cross-Journey connections;
- volunteer retention;
- project/community relationships;
- durable local networks.

It must not become an individual impact score or popularity rank.

## 12. MINIMUM VIABLE SOCIAL NETWORK

The first MVP should not include a global feed, DM system, Friend/Follow model or generic posting system.

P16 MVP-0 is:

1. Journey Community Room;
2. opt-in Journey Presence;
3. organizer / Journey Updates stream;
4. consent-scoped social identity;
5. evidence-driven AFTER transition;
6. no shared-experience graph edge before real attendance evidence;
7. visibility withdrawal / blocking / reporting / moderation foundation.

This MVP tests one critical hypothesis:

> **Can one real Journey become a durable digital relationship space without corrupting truth or privacy?**

## 13. REAL JOURNEY 2026-09-11 BOUNDARY

The Journey scheduled for 2026-09-11 remains the real evidence lane.

At WU1 discovery time, production showed:

- Journey status: registration_open;
- one application row;
- one participant row;
- attendance unresolved;
- zero verified attended rows;
- zero verified no-show rows.

Therefore P16 must not claim that the current participant has attended, formed an attended Memory, formed a shared-experience connection or generated impact.

The 2026-09-11 Journey is suitable for piloting Journey Room architecture and BEFORE-stage social preparation, but not for pre-declaring network effect or verified shared attendance.

P14-WU5 remains open independently.

## 14. P16 WORK UNIT SEQUENCING

Canonical sequencing after WU1:

### P16-WU2
**Social Identity, Consent & Safety Foundation**

### P16-WU3
**Journey Community Room & Typed Journey Stream**

### P16-WU4
**Journey Presence & Shared-Experience Graph**

### P16-WU5
**Interaction v1 — Journey Question / Reply / Appreciation**

### P16-WU6
**Notifications & Return Loop**

### P16-WU7
**Memory / Reflection / Contribution Social Continuity**

### P16-WU8
**Advanced Moderation, Blocking, Reporting & Vulnerable-Community Hardening**

### P16-WU9
**Mobile / VI-EN / Cross-Surface / Privacy QA**

### P16-WU10
**Real Pilot Evidence & Owner Gate**

Sequencing may be refined only through an explicit later canonical decision.

## 15. WU1 FINAL DECISION

- Journey-Based Social Network thesis: **APPROVED**
- Journey as primary social object: **APPROVED**
- Journey Community Room: **APPROVED IN PRINCIPLE**
- Shared-experience graph: **APPROVED IN PRINCIPLE — evidence + consent gated**
- My TNC private relationship archive: **RETAIN**
- P14/P15 truth architecture: **RETAIN / IMMUTABLE**
- generic global feed: **REJECTED FOR MVP**
- Friend / Follow as primary graph: **REJECTED**
- generic status-post architecture: **REJECTED**
- public visibility separate from private operational truth: **REQUIRED**

## 16. CLOSEOUT

**P16-WU1 — JOURNEY-BASED SOCIAL NETWORK PRODUCT DISCOVERY & CANONICAL ARCHITECTURE = COMPLETE / PASS.**

Next work unit:

**P16-WU2 — SOCIAL IDENTITY, CONSENT & SAFETY FOUNDATION.**
