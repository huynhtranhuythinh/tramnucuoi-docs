# TRẠM NỤ CƯỜI — P13-WU4 EVIDENCE
# COMMUNITY PEOPLE & RELATIONSHIP UI

Date: 2026-08-31
Status: **PASS**

## Evidence purpose

This record provides the auditable evidence chain for P13-WU4 from canonical trust model → source implementation → PR validation → merge → post-merge validation.

## 1. Source baseline

WU4 started from product `main`:

`75706af5b2dfa5e9b01b34150aa2e440406640e4`

Inherited trust sources:
- P12-WU2 verified-email participant claim;
- P12-WU3 evidence-backed Journey Memory;
- P12-WU5 verified Contribution History;
- P12-WU6 Community Roles & Host Network;
- P13-WU1 privacy/publication boundary;
- P13-WU2 My TNC personal home;
- P13-WU3 Before / During / After Journey experience.

## 2. Architecture audit finding

Existing member RLS allowed personal relationship facts to be shown to the owner account, but there was no canonical publication/consent contract authorizing a public people directory.

Therefore WU4 selected:

**private, person-centered Relationship Map**

and explicitly rejected:
- public member directory;
- other-member profile lookup;
- public relationship publication by implication;
- follower/following graph;
- ranking/score mechanics.

## 3. Product source evidence

Branch:
`p13-wu4-community-people-relationships`

New component:
`src/components/community/community-relationship-map.tsx`

Route integrations:
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`

Final PR head:
`0e6ed272e480c8419c031669fb8176c428e92ab6`

Diff against WU3 main:
- 3 commits ahead;
- 3 files changed;
- no migration file;
- no database source change;
- no Worker runtime change.

## 4. Relationship-source evidence

Relationship Map behavior is constrained to existing sources:

### Explorer
- authenticated Community account only;
- base relationship;
- creates no attendance/Memory/Contribution/permission fact.

### Participant
- `community_journey_memories`;
- requires `memory_eligible = true`;
- requires `attendance_state = 'attended'`;
- registration/confirmation alone is insufficient.

### Contributor
- `community_contributions`;
- only `status = 'active'` rows are loaded;
- no cross-unit impact total is generated.

### Host / Partner representative
- `community_relationship_assignments`;
- only `status = 'verified'` rows are loaded;
- assignment dates remain scheduled/current/historical;
- timezone context = `Asia/Ho_Chi_Minh`.

### Person identity
- own `profiles.full_name` only;
- self-provided display label, not legal identity proof;
- no email rendering in Relationship Map;
- no auth UUID rendering;
- no other-profile lookup.

## 5. Security / privacy evidence

WU4 introduced:
- no table;
- no view;
- no migration;
- no new RPC;
- no new RLS policy;
- no grant expansion;
- no SECURITY DEFINER function;
- no anonymous relationship read.

The component relies on existing RLS and returns `null` for signed-out visitors.

The existing `claim_my_journey_participations()` RPC is reused before loading relationship facts; WU4 does not alter its security model.

Canonical publication rule preserved:

**verified relationship existence ≠ permission to publish the person's identity**

## 6. Product PR evidence

Product PR:
**#29 — P13-WU4 Community People & Relationship UI**

PR CI:
**#144 — PASS**

Validated successfully:
- P9-WU7 source abuse-protection QA;
- P10-WU3A Cloudflare runtime-context QA;
- P9 ephemeral DB/rollback QA;
- P11-WU11 capacity/cutoff QA;
- P12-WU1 Community identity QA;
- P12-WU2 verified-email claim QA;
- P12-WU3 Memory QA;
- P12-WU4 Reflection QA;
- P12-WU5 Contribution QA;
- P12-WU6 Roles/Host QA;
- P12-WU7 Impact/provenance QA;
- production build;
- strict TypeScript typecheck;
- Cloudflare dry-run.

No CI workaround, strictness reduction or trust-semantic weakening was needed.

## 7. Merge / main evidence

Product PR #29 merged successfully.

Product `main` after WU4:

`cb465399f4860e4dfa842e2008e62547dbce8fde`

Post-merge main CI:
**#145 — PASS**

The same full regression/build/typecheck/Cloudflare dry-run chain passed on merged `main`.

## 8. Production mutation evidence

WU4 is source-only.

No:
- Supabase production migration;
- production SQL mutation;
- Cloudflare production deployment;
- fake profile;
- fake Memory;
- fake Contribution;
- fake Host/Partner assignment;
- fake impact fact.

Runtime gates remain unchanged:
- Email OFF;
- Turnstile OFF;
- Community Auth public activation GATED;
- Community route noindex/non-promoted;
- `pg_graphql` OFF;
- CMS roles exactly `admin | editor`.

## 9. Pilot continuity

P11-WU6 live pilot remains authoritative for real-world facts.

WU4 does not infer attendance from registration and does not change pilot records.

## 10. Evidence conclusion

**P13-WU4 — PASS**

The delivered UI makes one person's verified Community relationships legible without broadening data access, creating public identity exposure or inventing a social-network model.

Product main evidence SHA:
`cb465399f4860e4dfa842e2008e62547dbce8fde`

Next gate:
**P13-WU5 — Living Community Surface**.