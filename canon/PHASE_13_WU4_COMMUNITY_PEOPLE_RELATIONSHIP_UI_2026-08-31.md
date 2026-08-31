# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 / P13-WU4
# COMMUNITY PEOPLE & RELATIONSHIP UI

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

P13-WU4 makes the Phase 12 Community role model legible as a human relationship experience without turning internal verified relationships into a public people directory.

Canonical relationship model remains:

**One person → Explorer → Participant → Contributor → Host → Partner representative**

The same person may hold several of these relationships at once, but:

**Community relationship ≠ CMS permission**

WU4 is an experience layer over existing trusted facts. It does not create a new authorization model, social graph, public member directory, impact score or synthetic community activity.

## 2. Architecture decision — private relationship map first

The existing Phase 12 security model permits a Community member to read their own relationship facts under RLS. It does not provide a publication/consent contract that authorizes arbitrary public display of another person's Community profile or relationship history.

P13-WU1 also established that existence of an internal Host / Partner / Participant relationship does not itself authorize public identity presentation.

Therefore WU4 deliberately does **not** create:
- a public Community member directory;
- a searchable people page;
- public Participant lists;
- public Contributor lists;
- public Host/Partner profile cards merely because an internal relationship exists;
- follower/following mechanics;
- social graph edges between Community members.

Instead, WU4 adds a signed-in **Relationship Map** to My TNC. The current person is the center of the map, and only their own RLS-scoped verified facts are projected around them.

Public identity publication remains a separate future product/privacy decision for P13-WU5/P13-WU6 or a later explicitly scoped work unit.

## 3. Implemented source surface

New component:

`src/components/community/community-relationship-map.tsx`

Mounted after the existing `CommunityExperienceShell` on:
- `/cong-dong`
- `/en/community`

The existing My TNC experience remains intact. WU4 adds a second, synthesis-oriented layer rather than replacing WU2 modules.

The map uses the established documentary/editorial visual language:
- dark ink field;
- warm/yellow relationship signal;
- generous spacing;
- person-centered hierarchy;
- no dashboard scorecards;
- no gamified badge grid;
- no feed language.

## 4. Relationship derivation rules

### Explorer

Base relationship for an authenticated Community account.

It does not imply:
- attendance;
- Memory;
- Contribution;
- Host assignment;
- Partner representation;
- CMS access.

### Participant

Derived only when an owned Journey Memory is evidence-backed:
- `memory_eligible = true`; and
- `attendance_state = 'attended'`.

Registration or confirmed participation alone never creates Participant presentation.

### Contributor

Derived only from active verified rows in `community_contributions`.

Contribution context links back to the real Journey and/or Project where available.

Different contribution units remain separate; WU4 introduces no aggregate score.

### Host

Read only from verified `community_relationship_assignments` rows with:
- `relationship_type = 'host'`.

Journey context is shown where available.

Assignment period remains explicit:
- scheduled;
- current;
- verified history.

Date-state evaluation uses `Asia/Ho_Chi_Minh` context.

### Partner representative

Read only from verified `community_relationship_assignments` rows with:
- `relationship_type = 'partner_representative'`.

Project context is shown, with optional Journey scope where the existing record carries it.

The UI explicitly avoids converting this internal personal relationship into a claim of public organizational authority.

## 5. Person / profile boundary

The center of the Relationship Map uses the signed-in account's own `profiles.full_name` when present.

Canonical semantics remain:
- `full_name` is a self-provided Community display label;
- it is not legal identity verification;
- it is not automatically public;
- WU4 does not expose the signed-in account's email in the new map;
- WU4 never displays auth UUIDs;
- WU4 does not query or display other members' profile names.

The map contains explicit privacy copy explaining that it is private by default.

## 6. Data access / security boundary

WU4 reuses the existing Phase 12 model and introduces no database migration.

The component reads, for the current authenticated account under existing RLS:
- `profiles` — own profile only;
- `community_journey_memories` — own Memory projection;
- `community_contributions` — own active Contribution history;
- `community_relationship_assignments` — own verified Host/Partner assignments.

It then resolves related Journey / Project titles and slugs for context.

It also calls the existing idempotent `claim_my_journey_participations()` RPC before loading the map, preserving the WU2 verified-email participant-claim flow.

WU4 adds:
- no new table;
- no new view;
- no new RPC;
- no new RLS policy;
- no grant expansion;
- no SECURITY DEFINER code;
- no new public Data API surface.

Signed-out visitors receive no Relationship Map and no new Community sign-in promotion from this component while public Auth activation remains gated.

## 7. Truthful empty states

Production may legitimately contain no verified relationship facts for an account.

WU4 treats absence as a first-class state:
- Explorer remains the base authenticated relationship;
- Participant says there is no verified source yet when no attended Memory exists;
- Contributor says there is no verified source yet when no active Contribution exists;
- Host and Partner representative remain absent/inactive until staff-verified assignments exist.

The UI does not suggest self-assignment and does not fabricate examples in production.

## 8. Bilingual parity

The same component architecture serves VI and EN.

Both routes preserve identical truth semantics for:
- privacy-by-default;
- Participant derivation;
- Contributor derivation;
- Host/Partner verification;
- assignment period;
- role-versus-permission separation;
- no-source states.

## 9. Product source / GitHub evidence

Product branch:
`p13-wu4-community-people-relationships`

Base product main:
`75706af5b2dfa5e9b01b34150aa2e440406640e4`

Product PR:
**#29 — P13-WU4 Community People & Relationship UI**

PR head:
`0e6ed272e480c8419c031669fb8176c428e92ab6`

Changed files:
- `src/components/community/community-relationship-map.tsx`
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`

PR CI:
**#144 — PASS**

Product merge SHA:
`cb465399f4860e4dfa842e2008e62547dbce8fde`

Post-merge main CI:
**#145 — PASS**

Full gate covered:
- P9-WU7 source abuse-protection regression;
- P10-WU3A Cloudflare runtime-context regression;
- P9 ephemeral database / rollback QA;
- P11-WU11 capacity/cutoff QA;
- P12-WU1 identity QA;
- P12-WU2 claim QA;
- P12-WU3 Memory QA;
- P12-WU4 Reflection QA;
- P12-WU5 Contribution QA;
- P12-WU6 Community Roles / Host Network QA;
- P12-WU7 Impact Network / provenance QA;
- build;
- strict typecheck;
- Cloudflare dry-run.

## 10. Production / activation boundary

WU4 made no production database mutation and required no migration.

WU4 also made no Cloudflare production deployment.

Preserved runtime gates:
- Email OFF;
- Turnstile OFF;
- Community Auth public activation GATED;
- Community remains non-promoted / noindex;
- `pg_graphql` OFF;
- CMS role enum remains exactly `admin | editor`.

P11-WU6 live pilot remains operationally authoritative. WU4 does not seed or alter pilot attendance, Memory, Contribution, relationship or impact facts.

## 11. Publication boundary carried forward

A verified internal relationship answers:

> “Does Trạm have a trustworthy source for this person's relationship?”

It does **not** automatically answer:

> “May Trạm publish this person's identity and relationship publicly?”

WU5/WU6 must preserve that distinction.

Any wider public Living Community surface should prefer:
- public Journey/Project facts;
- already-public moderated Reflections;
- approved documentary evidence;
- non-identifying relationship narratives;

unless a separate, explicit identity-publication rule authorizes named people.

## 12. Closeout decision

**P13-WU4 — COMMUNITY PEOPLE & RELATIONSHIP UI: COMPLETE / PASS**

The Community experience can now express a person's real relationship graph in My TNC without inventing a social graph or weakening privacy/security boundaries.

Next work unit:

**P13-WU5 — LIVING COMMUNITY SURFACE**

WU5 should reveal the wider living ecosystem editorially through verified activity and relationships while remaining evidence-backed, privacy-safe and non-feed-like.