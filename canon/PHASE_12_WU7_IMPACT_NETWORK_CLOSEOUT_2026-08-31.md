# TRẠM NỤ CƯỜI — PHASE 12 / P12-WU7
# IMPACT NETWORK — CANONICAL CLOSEOUT

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

P12-WU7 completes the last foundation layer of Phase 12 without turning TRẠM NỤ CƯỜI into an ESG/reporting SaaS or generic KPI database.

Canonical product graph remains:

**People ↔ Journey ↔ Project ↔ Memory ↔ Contribution ↔ Community Relationship ↔ Impact Network**

Impact Network is an output/trust layer of the Living Community OS. It does not manufacture source facts and does not replace Journey, Community, documentary, Contribution, or WU10 impact truth.

## 2. Canonical architecture decision

WU7 intentionally keeps the existing WU10 tables as the canonical impact-claim records:

- `journey_impact_items`
- `journey_impact_snapshots`

No competing impact-claim store was introduced.

WU7 adds only the missing trust graph:

1. **Organization entity truth** — an Organization/Fund/CSR/Institution entity can exist without asserting partnership, funding, endorsement, or impact.
2. **Verified Organization ↔ Project[/Journey] relationship truth** — explicit staff verification, with immutable verified facts and correction by revoke → replacement.
3. **Partner representative bridge** — the WU6 personal `partner_representative` relationship remains Person ↔ Project truth and is separately bridged to a verified Organization relationship. Community role still does not equal CMS permission.
4. **Impact provenance links** — a canonical WU10 impact claim can be linked to recorded attendance, active verified Contribution, reviewed documentary media, or verified Partner relationship.
5. **Fail-closed verified impact** — a new `verified` claim requires at least one currently valid provenance edge. Revoking its provenance demotes the claim to `needs_evidence` rather than leaving a stale verified badge.
6. **PII-safe query projection** — current verified impact can be read without exposing person/auth/staff identifiers.
7. **No blind aggregation** — WU7 preserves claim value/unit semantics and introduces no cross-unit impact score.

`legacy_public` remains an explicit backward-compatibility state for existing public impact content. It is not relabeled as WU7 verified impact and is excluded from the new verified Impact Network projection.

## 3. Product implementation

Product repository:
`huynhtranhuythinh/tramnucuoi`

PR:
- #26 `P12-WU7 Impact Network & Phase 12 foundation closeout`

Merged product main SHA:
`75f9511de8442fcd632429b21cfc56fb727aed7b`

Primary migration source:
`db/migrations/0040_p12_wu7_impact_network.sql`

Dedicated QA:
`scripts/p12-wu7-db-qa-v2.sql`

Minimal admin infrastructure UI:
`/admin/impact-network`

No Cloudflare production UI deployment was performed in WU7.

## 4. Production migration

Supabase project:
`iwiqprhoohkxvjyxojto`

Applied production migration:
`20260831053522 p12_wu7_impact_network`

Created foundation objects include:

- `partner_organizations`
- `project_partner_relationships`
- `project_partner_relationship_audit_events`
- `partner_representative_organization_links`
- `partner_representative_org_link_audit_events`
- `impact_provenance_links`
- `impact_provenance_audit_events`
- `impact_network_verified_claims`
- `impact_network_partner_projection`
- private provenance validation/count functions and verification/revocation guards.

The migration seeds **zero** Organizations, relationships, provenance, attendance, Contribution, Memory, Reflection, or impact facts.

## 5. Trust contract

### Organization is not partnership

Creating an Organization entity does not assert:
- partnership;
- funding;
- CSR support;
- institutional endorsement;
- impact.

A Project relationship must be explicitly verified.

### Partner representative is not permission

WU6 `partner_representative` remains a personal Community relationship. Linking that person to an Organization relationship:
- requires same canonical Project truth;
- is separately verified;
- has revoke → replacement history;
- never creates `admin` or `editor` authorization.

Production `app_role` remains exactly:
- `admin`
- `editor`

### Impact is not registration/activity marketing

WU7 does not automatically convert any of the following into impact:
- registration;
- confirmed participation;
- unresolved attendance;
- media upload;
- Reflection;
- planned activity;
- Organization existence;
- Partner representative assignment.

A verified claim must be explicit and provenance-backed.

### Provenance source validity

Supported WU7 source types:
- `attendance` — attendance must be recorded; `NULL` remains unresolved and invalid as impact evidence;
- `contribution` — Contribution must be active, Journey-scoped to the claim, and respect canonical Project mapping;
- `documentary_media` — Journey media must be documentary evidence and reviewed (`approved` or `restricted` trust state);
- `partner_relationship` — relationship must be verified and match canonical Project/Journey/time context.

No source record is itself converted into an impact value automatically.

## 6. QA evidence

PR CI required multiple iterations because the isolated QA fixture initially did not mirror production grants/RLS closely enough. These failures were fixture-parity failures; production was untouched and the trust guards were not weakened.

Final PR head:
`e4e96b8ccbc4a03f542b3fc09ad8003b357dcd56`

Final PR CI: **PASS**.

Post-merge main CI run #137 on:
`75f9511de8442fcd632429b21cfc56fb727aed7b`

Result: **PASS**.

Passed gates include:
- P9 abuse-protection QA;
- P10 runtime-context QA;
- P11 transactional capacity/cutoff QA;
- P12-WU1 through P12-WU7 database contract QA;
- build;
- typecheck;
- Cloudflare dry-run.

WU7 QA proves at minimum:
- no fake Organization/relationship/provenance seed;
- Journey/Project relationship drift is rejected;
- a new verified impact claim without provenance is rejected;
- unresolved attendance is rejected as provenance;
- non-documentary media is rejected as provenance;
- provenance revocation demotes verified truth;
- `legacy_public` does not enter the verified projection;
- Partner representative linkage does not create CMS authorization;
- verified projection exposes no person/staff PII fields.

## 7. Production postflight

After production migration, all WU7 objects exist and production remains fact-clean:

- profiles = 0
- Community participant links = 0
- Memories = 0
- Reflections = 0
- Contributions = 0
- Community relationship assignments = 0
- Community relationship audit events = 0
- Partner Organizations = 0
- Project/Partner relationships = 0
- Partner representative ↔ Organization bridges = 0
- Impact provenance links = 0

Existing impact compatibility data remains unchanged:
- `journey_impact_items` `legacy_public` = 4
- verified impact items = 0
- `journey_impact_snapshots` `legacy_public` = 1
- verified impact snapshots = 0

No real impact was fabricated to make WU7 appear populated.

## 8. Pilot continuity

P11-WU6 — LIVE PILOT OPERATIONS remains **ACTIVE**.

Pilot Journey:
`19539f36-3ed4-4a22-96b9-c8a9b73c5283`

Postflight remains:
- title: `Trạm Cơm Chay Yêu Thương — Đổi Nụ Cười · Mùng 1 Tháng 8`
- event date: 2026-09-11
- status: `registration_open`
- capacity: 30
- confirmed rows: 1
- confirmed people: 1
- confirmed rows with unresolved attendance: 1

Therefore WU7 did not pre-empt the real event with attendance, no-show, Memory, Reflection, Contribution, Host/Partner, documentary, or impact facts.

## 9. Security / access postflight

Security Advisor after WU7 reports no WU7-specific new security lint. The existing project-level warning remains:
- `Leaked Password Protection Disabled`.

WU7 runtime ACL verification confirms:
- `impact_network_verified_claims` is readable by `anon`, `authenticated`, and `service_role`;
- the private provenance validation/count helpers required by the projection are executable by those runtime roles;
- `impact_network_partner_projection` is not granted to `anon`;
- verified claim projection contains no `user_id`, `verified_by`, email, phone, or personal-name columns.

`pg_graphql` remains OFF.

WU7 did not change Email delivery, Turnstile, Community Auth activation, or CMS authorization vocabulary. The existing Community Auth public activation gate therefore remains in force while Email is OFF.

## 10. Performance Advisor postflight

No new WU7 unindexed-foreign-key warning was introduced.

Performance Advisor naturally reports new WU7 indexes as `unused_index` while all WU7 tables contain zero rows and have had no live traffic. These INFO findings are not treated as a reason to delete required lookup/audit/provenance indexes at foundation creation time.

Existing project-level Performance Advisor findings remain outside WU7 scope unless separately prioritized.

## 11. WU7 closeout declaration

**P12-WU7 — IMPACT NETWORK: COMPLETE / PASS**

The final missing foundation link now exists without manufacturing impact:

**People ↔ Journey ↔ Project ↔ Memory ↔ Contribution ↔ Community Relationship ↔ Impact Network**

No P12-WU8 is opened.
