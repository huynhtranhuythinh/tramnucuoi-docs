# TRẠM NỤ CƯỜI — PHASE 12 / P12-WU6
# COMMUNITY ROLES & HOST NETWORK

Date: 2026-08-31  
Status: **COMPLETE / PASS**

## 1. Purpose

P12-WU6 restores the original **One Identity — Multiple Roles** model without turning Community relationships into CMS authorization.

Canonical product loop remains:

**People → Journey → Experience → Memory → Community Relationship → Contribution → Impact Network**

The original architecture said:
- every person starts as Explorer;
- roles are added through participation, contribution and trust;
- one identity may accumulate multiple roles;
- **role does not equal permission**.

WU6 operationalizes that principle.

## 2. Canonical role model after WU6

### Explorer
Base Community relationship for an authenticated Community account.

No database role assignment is needed merely to be an Explorer.

### Participant
**Derived source truth**, not manually assigned.

A person is treated as a real Participant relationship only when their personal Journey Memory is evidence-backed and `memory_eligible = true`, which in turn requires verified attendance semantics.

Registration/confirmation alone does not create this role.

### Contributor
**Derived source truth**, not manually assigned.

A person has a Contributor relationship when they have at least one active verified row in `community_contributions`.

Attendance does not automatically become contribution.

### Host
A staff-verified personal relationship with a specific Journey.

Host may be scheduled before Journey day. `status='verified'` means the source record is valid; `starts_on` / `ends_on` determine whether the assignment is scheduled, current or historical.

A Host does **not** receive CMS Admin/Editor permissions.

### Partner representative
A staff-verified personal relationship with a real Project.

WU6 intentionally uses the term `partner_representative` because the Community identity belongs to a person.

It does **not** create or assert an organization/CSR partner master record. Organization-level Partner entities belong to P12-WU7 Impact Network.

### Admin / Editor
Remain CMS authorization only through the existing `user_roles` + `app_role` model.

Production `app_role` after WU6 is still exactly:
- `admin`
- `editor`

No Community role was added to this enum.

## 3. Key architecture correction

P12-WU5 was intentionally participation-first: Contribution History required an active verified `community_participant_links` relationship.

WU6 generalizes the identity anchor.

A verified Community identity may now originate from either:
1. an active verified participant identity link; or
2. a staff-verified Host / Partner-representative assignment.

This prevents the system from forcing a local Host or Partner representative to fabricate a past Journey participation merely to become a recognized Community identity.

A self-created profile by itself is **not** enough to receive verified Contribution History.

Canonical helper:
`private.tnc_has_verified_community_identity(uuid)`

It is:
- `SECURITY INVOKER`;
- `search_path=''`;
- executable by `authenticated` for RLS/helper evaluation;
- not executable by `anon`.

## 4. Data model

### `public.community_relationship_assignments`

Stores only relationship types that require explicit staff assignment:
- `host`
- `partner_representative`

Core fields:
- `user_id`
- `relationship_type`
- `journey_id`
- `project_id`
- `starts_on`
- `ends_on`
- `verification_method`
- `status`
- timestamps

Canonical rules:
- Host requires a real Journey;
- Host Project is derived from the Journey canonical Project;
- a Host cannot invent a Project edge for a Journey with no canonical Project;
- Partner representative requires a real Project;
- optional Partner Journey scope must belong to the same canonical Project;
- impossible date ranges are rejected;
- future scheduling is allowed because a Host assignment may legitimately be planned before Journey day;
- exact duplicate verified assignments are blocked;
- verified source facts are immutable;
- correction = `verified -> revoked` + replacement record;
- no authenticated DELETE.

### `public.community_relationship_audit_events`

Admin-only immutable actor audit for:
- relationship verification;
- relationship revocation.

Staff actor UUIDs are deliberately kept out of the member-readable assignment row.

## 5. Community profile boundary

WU6 reuses the existing `profiles` table as the minimal Community account directory.

`profiles.full_name` is a **self-provided display label**, not legal identity proof.

A Host/Partner representative may enter the TNC network without prior participation, but their account must first activate its own Community Profile.

WU6 also hardened the existing profile access contract:
- anon table SELECT grant removed;
- authenticated DELETE grant removed;
- authenticated keeps SELECT / INSERT / UPDATE under RLS;
- own-profile policies now use init-plan-safe `(select auth.uid())` form;
- admin retains profile read through the existing admin role check.

No production profile was seeded.

## 6. Member experience

New component:
`src/components/community/community-roles-panel.tsx`

Mounted on:
- `/cong-dong`
- `/en/community`

The signed-in member sees a consolidated relationship picture:
- Explorer — base relationship;
- Participant — derived from Memory eligibility;
- Contributor — derived from active verified contributions;
- Host — verified assignments;
- Partner representative — verified assignments.

The surface explicitly states:
> Community roles do not grant website administration.

Members may set/update their own Community display name.

No self-assignment of Host or Partner representative exists.

## 7. Admin experience

New route:
`/admin/community-roles`

Admin can:
- select an activated Community Profile;
- verify Host assignment to a Journey;
- verify Partner-representative relationship to a Project;
- optionally narrow Partner representative to a Journey in the same Project;
- set start/end dates;
- choose verification basis;
- revoke an incorrect relationship record.

Admin cannot use this screen to manually assign Participant or Contributor.

The database remains authoritative for Journey/Project graph consistency.

## 8. Quality gates

Product PR:
- **#25 — P12-WU6 Community Roles & Host Network**

PR head tested:
`e2428f308bd65d497a3f580300c068ad35540ef5`

PR CI:
- **#131 PASS**

Covered:
- P9 security regressions;
- P10 Cloudflare runtime regression;
- P11-WU11 capacity/cutoff QA;
- P12-WU1 identity QA;
- P12-WU2 claim QA;
- P12-WU3 Memory QA;
- P12-WU4 Reflection QA;
- P12-WU5 Contribution QA;
- dedicated P12-WU6 Community Roles/Host QA;
- build;
- typecheck;
- Cloudflare dry-run.

Dedicated WU6 database QA proves:
- direct Host entry without fake participant link;
- no Community relationship creates `user_roles` CMS authorization;
- Host canonical Project derivation;
- mismatched/invented Journey→Project edges rejected;
- Partner representative Project scope enforced;
- no assignment before Community Profile activation;
- own-only member relationship visibility;
- member cannot see staff actor audit;
- editor/member cannot author verified assignments;
- Host relationship is a valid WU5 Contribution identity anchor;
- profile alone is not a verified Contribution identity anchor;
- existing participant identity anchor still works;
- immutable source facts + revoke-only correction;
- date-period semantics;
- fail-closed grants/helper permissions.

Product merge SHA:
`a0467fabcac57a7b1ee9853bef51dd103e3b3b30`

Post-merge main CI:
- **#132 PASS**

No Cloudflare production deployment occurred in WU6.

## 9. Production migration

Supabase project:
`iwiqprhoohkxvjyxojto`

Migration:
`20260831024125 p12_wu6_community_roles_host_network`

Migration applied successfully after PR CI + post-merge main CI PASS.

## 10. Production postflight

Production remains fact-clean:
- profiles = **0**;
- Community participant links = **0**;
- Memories = **0**;
- Contributions = **0**;
- Reflections = **0**;
- Community relationship assignments = **0**;
- Community relationship audit events = **0**.

No Host, Partner representative, Participant, Contributor or profile was fabricated/seeded.

Pilot remains unchanged:
- status = `registration_open`;
- capacity = **30**;
- confirmed participant rows = **1**;
- confirmed people = **1**;
- attendance-resolved rows = **0**.

`pg_graphql` remains **OFF**.

## 11. Security postflight

`community_relationship_assignments`:
- RLS ON;
- anon SELECT grant = false;
- authenticated DELETE grant = false;
- own/admin SELECT policy;
- admin-only INSERT/UPDATE policies.

`community_relationship_audit_events`:
- RLS ON;
- authenticated has no INSERT/UPDATE/DELETE;
- admin-only SELECT.

Relationship guard:
- `SECURITY INVOKER`;
- `search_path=''`;
- no direct anon/authenticated EXECUTE.

Audit helper:
- private `SECURITY DEFINER` only because it writes sealed audit actor records;
- `search_path=''`;
- no direct anon/authenticated EXECUTE.

Verified-identity helper:
- `SECURITY INVOKER`;
- `search_path=''`;
- authenticated EXECUTE only as required by policy/guard evaluation;
- anon EXECUTE false.

Profiles:
- anon SELECT grant false;
- authenticated DELETE grant false.

`app_role` remains only `admin/editor`.

Security Advisor:
- no new WU6 security lint;
- existing project warning remains `Leaked Password Protection Disabled`.

Remediation reference:
https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

## 12. Performance / platform compatibility

No new WU6 unindexed-FK lint was introduced.

WU6 relationship indexes are reported unused immediately after creation because both new tables contain 0 rows. This is expected and is not a reason to remove lookup/FK-covering indexes before traffic exists.

WU6 explicitly grants required Data API privileges instead of relying on Supabase public-schema default privileges. This is compatible with Supabase's 2026 Data API exposure change requiring explicit table grants for new public entities.

## 13. Activation discipline

Community Auth remains operationally gated while Email is OFF.

WU6 creates infrastructure and UI only. It does not claim that a real Host/Partner network already exists.

Real roles must emerge from real operations:
- Participant after verified attendance;
- Contributor after verified contribution;
- Host after staff verification/assignment;
- Partner representative after staff verification against a real Project relationship.

P11-WU6 real pilot remains ACTIVE and authoritative for real-world facts.

## 14. WU6 closeout decision

**P12-WU6 — COMPLETE / PASS**

The Community OS now supports:

**One person**
→ Explorer by default
→ Participant through real attendance
→ Contributor through verified contribution
→ Host through verified Journey assignment
→ Partner representative through verified Project relationship

without conflating any of those Community relationships with CMS Admin/Editor permissions.

## 15. Next gate

Next planned product work:

**P12-WU7 — Impact Network**

WU7 may connect verified:
- People;
- Journeys;
- Projects;
- Memories;
- Contributions;
- Host relationships;
- Partner relationships;
- evidence;

into credible impact/data storytelling for CSR, funds and institutional partners.

WU7 must not turn the platform into a compliance/reporting system detached from the Community OS loop, and it must not fabricate partner organizations, impact metrics or pilot facts.
