# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 12 → PHASE 13 HANDOFF

Date: 2026-08-31
Status: **PHASE 12 CLOSED / PHASE 13 READY TO START**

## 1. Handoff declaration

Phase 12 is closed at P12-WU7.

Canonical Phase 12 result:

**LIVING COMMUNITY OS FOUNDATION — COMPLETE / PASS**

Phase 13 may now begin from the verified domain foundation instead of inventing a new product direction.

## 2. Frozen Phase 12 foundation

The following semantics are now architectural invariants unless a future migration explicitly changes them with evidence:

1. Community identity is not CMS authorization.
2. Confirmed registration is not attendance.
3. Identity linkage is not attendance.
4. Attendance `NULL` is unresolved.
5. Attendance `0` is verified no-show.
6. Attendance `>0` is verified attended and may become Memory eligible.
7. Memory is not inferred from registration.
8. Reflection requires real Journey/Memory context and moderation before public publication.
9. Attendance is not Contribution.
10. Contribution preserves explicit context and units.
11. Community roles are relationship facts, not permissions.
12. Host/Partner representative relationship does not grant `admin` or `editor`.
13. Organization existence does not assert partnership.
14. Verified impact requires valid provenance.
15. Unlike units are never blindly aggregated into one Impact Score.
16. `legacy_public` impact remains compatibility content, not provenance-backed WU7 verified truth.

## 3. Canonical HEADs at handoff

Product main:
- `75f9511de8442fcd632429b21cfc56fb727aed7b`

Docs main at Phase 12 closeout:
- `0206d603266f504e3bd504d68f28b432c4639a6a`

Production Supabase latest Phase 12 migration:
- `20260831053522 p12_wu7_impact_network`

## 4. Production fact state at handoff

The Community OS foundation is structurally present but not populated with fabricated facts.

At Phase 12 closeout:
- profiles = 0;
- Community participant links = 0;
- Memories = 0;
- Reflections = 0;
- Contributions = 0;
- Community relationship assignments = 0;
- Partner Organizations = 0;
- verified Partner relationships = 0;
- Partner representative/Organization bridges = 0;
- impact provenance links = 0;
- WU7 verified impact items = 0;
- WU7 verified impact snapshots = 0.

## 5. Parallel operational lane

P11-WU6 — LIVE PILOT OPERATIONS remains ACTIVE for the real Journey on 2026-09-11.

Phase 13 must not block, rewrite or pre-empt the pilot.

Phase 13 UI may be built against empty/unresolved states, but must never manufacture:
- attendance;
- Memory;
- Reflection;
- Contribution;
- Host/Partner assignment;
- Organization relationship;
- documentary evidence;
- verified impact.

## 6. Runtime guards carried into Phase 13

Until explicitly changed by a separately reviewed activation work unit:
- Email: OFF
- Turnstile: OFF
- Community Auth public activation: GATED
- Community routes: non-promoted / noindex where currently gated
- `pg_graphql`: OFF
- CMS roles: `admin | editor` only

Phase 13 visual work must not silently activate these systems.

## 7. Product direction handed to Phase 13

Phase 12 built the trustworthy graph.

Phase 13 must make that graph understandable and emotionally useful to people.

Target transition:

**Foundation / panels / technical account surface**

→

**Living Community Experience**

The UX should help a person understand:
- who they are in the Trạm community;
- which Journeys they are connected to;
- what Memories are genuinely theirs;
- what they contributed;
- what roles/relationships have been verified;
- what happened before, during and after a Journey;
- how those relationships connect to Projects and credible impact storytelling.

## 8. Explicit non-goals for Phase 13

Do not turn the Community OS into:
- a Facebook-like feed;
- follower/following mechanics;
- public popularity scores;
- engagement streaks;
- generic private chat;
- gamified badges detached from real-world facts;
- a CRM dashboard for community members;
- an ESG reporting dashboard as the main product experience.

## 9. Recommended Phase 13 sequence

### P13-WU1 — Community Experience Architecture
Information architecture, experience hierarchy, route/surface ownership, signed-out/signed-in states, empty-state semantics and visual system direction.

### P13-WU2 — Personal Community Home / My TNC
Turn the current stacked technical Community page into a coherent personal home that reflects verified Journey, Memory, Contribution and role relationships.

### P13-WU3 — Journey Community Experience
Design Before / During / After Journey experience without conflating registration, attendance and Memory.

### P13-WU4 — Community People & Relationship UI
Represent Host / Contributor / Partner representative / participant relationships with privacy-safe, evidence-backed semantics.

### P13-WU5 — Living Community Surface
Create the broader Community home as an editorial/living ecosystem surface, not a generic feed.

### P13-WU6 — Public Activation & Polish
Responsive, bilingual, accessibility, editorial motion, SEO/noindex decisions, final Auth/email activation gate and production readiness.

## 10. Phase 13 entry gate

Phase 13 may start because:
- Phase 12 WU1→WU7 are closed;
- final product main CI passed;
- production postflight passed;
- canonical final closeout exists;
- Phase 12 evidence pack exists;
- no Phase 12 data fabrication is required to design empty and pending states.

## 11. Handoff declaration

**PHASE 12: CLOSED / COMPLETE / PASS**

**PHASE 13: AUTHORIZED TO START**

**FIRST WORK UNIT: P13-WU1 — COMMUNITY EXPERIENCE ARCHITECTURE**
