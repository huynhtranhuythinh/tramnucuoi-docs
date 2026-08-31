# TRẠM NỤ CƯỜI — PHASE 12 / P12-WU5
# CONTRIBUTION HISTORY

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

P12-WU5 adds a credible personal Contribution History to the Community OS.

The purpose is not to create points, badges, a leaderboard, a self-declared social profile, or a synthetic impact score. The purpose is to preserve verified facts about how a real person contributed to a real Journey and/or Project.

Canonical relationship strengthened by this WU:

**Person ↔ Journey / Project ↔ Contribution**

Contribution remains distinct from:
- registration;
- confirmation;
- attendance;
- Memory;
- Reflection;
- institutional impact claims.

Attendance never automatically becomes a contribution record.

## 2. Product outcome

### Community member surface

Source:
- `src/components/community/community-contributions-panel.tsx`
- integrated into `/cong-dong`
- integrated into `/en/community`

A signed-in member may see only Contribution History belonging to the authenticated identity while that identity still has an active verified Community participant link.

The bilingual history presents:
- contribution type;
- date;
- VI/EN title and description;
- optional quantity + unit;
- verification basis;
- Journey context when present;
- Project context when present;
- active/revoked record state.

The UI explicitly states that unlike units are not combined into one impact number.

There is no member self-declaration workflow in WU5.

### Admin verification surface

Source:
- `src/routes/_authenticated/admin.contributions.tsx`
- `/admin/contributions`
- Admin navigation label: `Đóng góp`

Only an admin may create or revoke verified Contribution History.

The admin console supports:
- selecting a verified Community identity;
- Journey or Project context;
- contribution type;
- occurrence date;
- bilingual title/description;
- optional quantitative value with unit;
- verification method;
- approved documentary evidence when applicable.

Editors are deliberately not authorized to create/revoke Contribution History in WU5.

## 3. Database model

Canonical migration:

`db/migrations/0038_p12_wu5_contribution_history.sql`

Production migration:

`20260831020939 p12_wu5_contribution_history`

### `public.community_contributions`

Contribution types:
- `volunteer_time`
- `skill`
- `media`
- `knowledge`
- `resource`
- `other`

Verification methods:
- `staff_observed`
- `record_review`
- `documentary_evidence`

Lifecycle:
- `active`
- `revoked`

Core rules:
1. A contribution requires at least one real Journey or Project context.
2. The person must have an active verified `community_participant_links` relationship before staff can create a contribution.
3. `occurred_on` cannot be in the future. Future promises are not Contribution History.
4. A quantitative value is optional, but `quantity` and `unit` must either both exist or both be absent.
5. Quantity must be positive.
6. Journey-to-Project graph authority comes from the canonical Journey record.
7. If a Journey has a canonical Project, the Project is derived from that Journey and a mismatched Project is rejected.
8. If a Journey has no canonical Project, WU5 does not permit inventing one on the contribution row.
9. `documentary_evidence` verification requires a media asset with:
   - `evidence_status = documentation`
   - `trust_status = approved`
10. Verified source facts are immutable after creation.
11. Corrections use **revoke old record + create replacement**, never silent rewriting.
12. A revoked record cannot be reactivated.

### Identity-revocation privacy rule

Community SELECT fails closed if all verified identity links for that auth identity are later revoked/corrected.

This prevents a wrongly claimed account from continuing to read Contribution History merely because historical rows still carry its old `user_id`.

Admin retains the source for audit and correction; history is not silently deleted.

### `public.community_contribution_audit_events`

Admin-only append-only audit records:
- verification event;
- revocation event;
- actor UUID;
- timestamp.

Staff actor UUID is not stored in the member-visible contribution source row.

## 4. Security model

Both new public tables have RLS enabled.

`community_contributions`:
- anon SELECT: false;
- authenticated SELECT: own verified identity history or admin;
- INSERT: admin-only via RLS + DB guard;
- UPDATE: admin-only via RLS + DB guard;
- DELETE: unavailable to authenticated.

`community_contribution_audit_events`:
- anon SELECT: false;
- authenticated SELECT: admin-only;
- authenticated direct INSERT/UPDATE/DELETE: false.

Private helpers:

### `private.tnc_guard_community_contribution()`
- `SECURITY INVOKER`;
- `search_path = ''`;
- direct EXECUTE revoked from anon/authenticated;
- enforces identity, context, evidence, date, metric, immutable-source and revoke-only lifecycle rules.

### `private.tnc_audit_community_contribution()`
- `SECURITY DEFINER` only because the trigger must write the sealed audit table;
- private schema;
- `search_path = ''`;
- direct EXECUTE revoked from anon/authenticated;
- requires authenticated actor identity.

No new public RPC was introduced.

## 5. QA

Dedicated QA:
- `scripts/p12-wu5-db-qa.sql`
- `scripts/p12-wu5-db-qa-gate.sql`

Fresh GitHub-hosted PostgreSQL 17 proves:
- unlinked identity cannot receive verified history;
- linked identity can receive a verified contribution;
- Journey canonical Project is derived;
- mismatched Journey/Project relationship is rejected;
- Project cannot be invented for a Journey without one;
- future contribution date is rejected;
- quantity without unit is rejected;
- documentary verification without media is rejected;
- sample media is rejected as contribution evidence;
- restricted media is rejected as contribution evidence;
- approved documentary media is accepted;
- editor cannot create verified Contribution History;
- owner sees only own history;
- unrelated member sees none;
- member cannot read audit actor data;
- verified source facts cannot be rewritten;
- active -> revoked works and is audited;
- revoked -> active is rejected;
- anon cannot read private history;
- authenticated cannot delete source rows;
- authenticated cannot directly mutate audit events;
- private helpers are not directly executable by API roles;
- after all identity links for a member are revoked, member SELECT returns no Contribution History while admin retains the source.

## 6. CI history

### PR CI #128 — FAIL before production mutation

The first WU5 run failed at the new DB test with:

`permission denied for table journeys`

Cause: the minimal WU5 ephemeral fixture did not mirror the production authenticated SELECT grants on canonical context/evidence tables used by the `SECURITY INVOKER` guard.

Production was inspected read-only and confirmed to grant authenticated SELECT on:
- `journeys`;
- `media_assets`;
- `ecosystem_projects`;
- `community_participant_links`.

Resolution:
- fixed the test fixture for production privilege parity;
- kept the guard `SECURITY INVOKER`;
- did **not** weaken production security to make the test pass.

No production mutation occurred during the failed run.

### PR CI #129 — PASS

Tested branch head:

`241dcfef4cb41e67e1bb0345ff426db2d1d11473`

PASS:
- P9-WU7 source QA;
- P10-WU3A runtime-context QA;
- P9 DB QA;
- P11-WU11 DB QA;
- P12-WU1 DB QA;
- P12-WU2 DB QA;
- P12-WU3 DB QA;
- P12-WU4 DB QA;
- P12-WU5 DB + identity-revocation QA;
- build;
- typecheck;
- Cloudflare dry-run.

PR #24:

`P12-WU5 verified Contribution History`

### Product merge

Product main merge SHA:

`ec525c889c8e931f7fe2ab3ab50c91a53f7633aa`

### Main CI #130 — PASS

Post-merge main CI repeated the full gate and passed:
- all regression DB suites;
- P12-WU5 QA;
- build;
- typecheck;
- Cloudflare dry-run.

No Cloudflare production deployment occurred.

## 7. Production preflight / postflight

### Preflight

Before migration:
- pilot status: `registration_open`;
- capacity: 30;
- confirmed rows: 1;
- confirmed people: 1;
- attendance resolved rows: 0;
- Community links: 0;
- Memory rows: 0;
- Reflection rows: 0;
- Reflection publication rows: 0;
- WU5 tables: absent;
- `pg_graphql`: OFF.

### Postflight

After `20260831020939 p12_wu5_contribution_history`:
- `community_contributions`: exists, RLS ON;
- `community_contribution_audit_events`: exists, RLS ON;
- contribution rows: **0**;
- audit event rows: **0**;
- Community links: 0;
- Memories: 0;
- Reflections: 0;
- Reflection publications: 0;
- anon contribution SELECT: false;
- anon audit SELECT: false;
- authenticated source DELETE: false;
- authenticated audit INSERT/UPDATE/DELETE: false;
- guard: SECURITY INVOKER, `search_path=''`, anon/auth direct EXECUTE false;
- audit helper: SECURITY DEFINER, `search_path=''`, anon/auth direct EXECUTE false;
- pilot remains `registration_open`, capacity 30, 1 confirmed row / 1 confirmed person;
- attendance resolved rows remain 0;
- `pg_graphql`: OFF.

Therefore the migration created capability only and did not manufacture any real Contribution fact.

## 8. Advisors

Security Advisor after migration:
- no WU5 security lint introduced;
- existing project warning remains `Leaked Password Protection Disabled`.

Reference:
https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

Performance Advisor after migration:
- existing project warnings remain;
- no new WU5 unindexed-foreign-key lint appeared;
- new WU5 indexes are reported as unused immediately after creation because production has zero Contribution rows and no feature traffic yet.

These fresh empty-table `unused_index` notices are not a reason to remove the operational indexes before real usage exists.

## 9. Activation discipline

WU5 is **built and production-schema ready**, but Community Auth remains activation-gated while Email is OFF.

Before the first real pilot event and legitimate operations, do not manufacture:
- Community identity links;
- attendance;
- Memories;
- Reflections;
- Contribution History;
- documentary evidence;
- impact claims.

Contribution records may only be created from real verified operational facts.

## 10. Closeout

**P12-WU5 — Contribution History: COMPLETE / PASS**

Next planned Phase 12 work:

**P12-WU6 — Community Roles & Host Network**

P11-WU6 — LIVE PILOT OPERATIONS remains **ACTIVE** for the real Journey on 2026-09-11 and its evidence-based post-event closeout.
