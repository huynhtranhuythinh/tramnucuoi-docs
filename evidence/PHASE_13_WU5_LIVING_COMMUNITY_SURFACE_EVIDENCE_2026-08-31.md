# TRẠM NỤ CƯỜI — P13-WU5 EVIDENCE
# LIVING COMMUNITY SURFACE

Date: 2026-08-31  
Result: **COMPLETE / PASS**

## 1. Purpose

This record captures the auditable evidence chain for P13-WU5 from production-source audit through implementation, pull-request QA, merge and post-merge verification.

## 2. Baseline

Product base main before WU5:
`cb465399f4860e4dfa842e2008e62547dbce8fde`

WU5 branch:
`p13-wu5-living-community-surface`

No production mutation was authorized as part of implementation.

## 3. Production read-only audit

Supabase project:
`iwiqprhoohkxvjyxojto`

Observed before implementation:

| Fact | Count |
|---|---:|
| Public Journeys | 4 |
| Registration-open Journeys | 1 |
| Preparing Journeys | 0 |
| Completed Journeys | 1 |
| Published Journey Updates | 5 |
| Published Field Journal posts | 10 |
| Journey media links | 11 |
| Public documentary media assets | 5 |
| Public Reflection publications | 0 |
| Active Community Contributions | 0 |
| Verified Community relationship assignments | 0 |
| Verified Impact items | 0 |
| Verified Impact snapshots | 0 |

Interpretation:
- there is real public Journey/field content to support a Living Community surface;
- there is not yet real public Reflection/Contribution/relationship/verified-impact activity;
- therefore empty states are required and fake population is prohibited.

## 4. Publication-policy evidence

Production policy inspection confirmed:

### `journey_reflection_publications`
- policy: `published Journey reflections are public`;
- roles: `anon, authenticated`;
- command: `SELECT`;
- projection is public by design.

### Private Reflection source
`journey_reflections`:
- no anon read policy;
- own/staff authenticated read only;
- staff moderation remains private-source behavior.

### Journey Updates
Public read requires:
- update status `published`;
- parent Journey in a public operational lifecycle state.

### Journeys
Public read remains limited to:
- `registration_open`;
- `preparing`;
- `completed`.

WU5 did not modify any policy or grant.

## 5. Public Reflection schema evidence

`journey_reflection_publications` production columns:
- `reflection_id`;
- `journey_id`;
- `body`;
- `locale`;
- `published_at`;
- `updated_at`.

No:
- `user_id`;
- `memory_id`;
- email;
- phone;
- moderator identity.

This is the only Reflection table WU5's public reader consumes.

## 6. Product source evidence

Added:
- `src/lib/community/living-community.server.ts`
- `src/lib/community/living-community.functions.ts`
- `src/components/community/living-community-surface.tsx`

Updated:
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`

### Reader contract
`living-community.server.ts`:
- reuses `fetchPublicJourneys(locale)`;
- reads published `journey_updates` through anon/RLS;
- reads `journey_reflection_publications` through anon/RLS;
- drops rows whose parent Journey is not in the public Journey projection;
- does not read profiles/private Community identity tables.

### UI contract
`living-community-surface.tsx` provides:
- Living Community editorial hero;
- Now/Next Journey;
- public Journey ecosystem;
- From-the-field timeline;
- identity-minimized public Reflections;
- truthful Reflection empty state;
- explicit public-identity publication boundary.

Both VI/EN routes mount the same architecture.

Both routes remain `noindex` under the existing activation gate.

## 7. Product PR evidence

Product PR:
**#30 — P13-WU5 Living Community Surface**

PR head:
`1b4bcff7bb5e5b886aa4ded9b3e766de2e284fa4`

Changed files:
- 5

Implementation PR explicitly states:
- no database migration;
- no RLS change;
- no fake data;
- no Email activation;
- no Turnstile activation;
- no Cloudflare production deployment.

## 8. PR CI evidence

PR CI:
**#146 — PASS**

Successful gates:
- P9-WU7 source abuse-protection QA;
- P10-WU3A Cloudflare runtime-context regression QA;
- P9 ephemeral database gate / rollback QA;
- P11-WU11 transactional capacity / cutoff QA;
- P12-WU1 identity / Journey link QA;
- P12-WU2 verified-email participant claim QA;
- P12-WU3 personal Journey Memory QA;
- P12-WU4 Reflection moderation/publication QA;
- P12-WU5 Contribution History QA;
- P12-WU6 Community Roles / Host Network QA;
- P12-WU7 Impact Network / provenance QA;
- build;
- strict typecheck;
- Cloudflare dry-run.

No corrective CI commit was needed; WU5 passed the PR gate on the first implementation head.

## 9. Merge evidence

PR #30 merged successfully into product `main`.

Merge/main SHA:
`029d444a32529b23cc0171309e8bc81ae9792957`

Commit title:
`P13-WU5 Living Community Surface`

## 10. Post-merge CI evidence

Main CI:
**#147 — PASS**

The post-merge run repeated and passed:
- all inherited P9–P12 database/security regressions;
- build;
- strict typecheck;
- Cloudflare dry-run;
- runner cleanup.

This verifies the merged main state, not only the PR head.

## 11. Production mutation evidence

WU5 did not apply a migration and did not intentionally mutate production data.

No:
- Supabase DDL;
- Supabase DML;
- Reflection seed;
- profile seed;
- relationship seed;
- Contribution seed;
- impact seed;
- Cloudflare production deploy.

The Supabase interaction performed for WU5 was read-only audit/policy/schema inspection.

## 12. Trust invariants preserved

Verified by design plus inherited CI:

1. registration is not attendance;
2. confirmed participation is not Memory;
3. Participant remains evidence-derived;
4. Contribution remains explicitly verified and unit-aware;
5. Community role does not grant CMS permission;
6. internal verified relationship does not authorize public identity publication;
7. public Reflection comes only from the identity-minimized publication projection;
8. no private Reflection source or applicant PII is read by the public Living Community reader;
9. no generic social feed/follower/ranking system was introduced;
10. missing real activity remains a truthful empty state.

## 13. Activation evidence

WU5 does not change the activation state.

Still preserved:
- Email OFF;
- Turnstile OFF;
- Community Auth public activation GATED;
- Community route not promoted in primary public navigation;
- Community route remains `noindex`;
- no Cloudflare production deployment in WU5.

## 14. Final evidence declaration

The evidence supports:

**P13-WU5 — LIVING COMMUNITY SURFACE: COMPLETE / PASS**

Product main:
`029d444a32529b23cc0171309e8bc81ae9792957`

Next gate:
**P13-WU6 — PUBLIC ACTIVATION & POLISH**
