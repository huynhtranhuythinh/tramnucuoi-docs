# TRẠM NỤ CƯỜI — PHASE 16 / WU8
# ADVANCED MODERATION, BLOCKING, REPORTING & VULNERABLE-COMMUNITY HARDENING

Date: 2026-09-03
Status: COMPLETE / PASS — SOURCE, ARCHITECTURE & PRODUCTION DATABASE FOUNDATION

## 1. Scope and canonical principles

P16-WU8 hardens the Journey-Based Social Network safety layer without changing private operational Journey truth.

Canonical principles:

- Social safety changes social visibility and interaction capability only.
- Blocking, reporting, restriction or hiding MUST NOT rewrite registration, attendance, Memory, Contribution, impact or shared-experience evidence.
- No user is classified as vulnerable, minor or high-risk by inference or profiling.
- `child_safety`, `sexual_safety` and `threat_or_violence` are report behavior/context categories only.
- `urgent` means priority human review only; it is never proof, guilt or an automatic sanction.
- No auto-hide, auto-suspend, toxicity score, risk score, public report count, ranking or shaming mechanism was introduced.
- Social feature activation remains fail closed.

## 2. Product/source truth

Repository: `huynhtranhuythinh/tramnucuoi`
Production branch: `main`

PR: #57 — `P16-WU8: Advanced Moderation Blocking Reporting Safety`

PR final head:
`4145e28028ad48d0822b82a9b232fa782e6fc1ee`

Squash merge production-main SHA:
`05742cdff1aefaac48a1723ca5eaa3e3499543af`

Production-main tree:
`acc334ef8bc43ad463bb1f8d0817571388b47674`

Canonical source migration:
`db/migrations/0051_p16_wu8_advanced_moderation_safety.sql`

Canonical migration blob:
`249082499228a71e02a909e627a32759319eb5f8`

Rollback:
`db/rollbacks/p16_wu8_advanced_moderation_safety.sql`

Important rollback caveat:
rollback intentionally fails closed if any real `interaction_restrict` moderation history exists. Safety history must not be silently erased to make rollback convenient.

## 3. CI evidence

### PR CI iterations

- CI #240 / run `33671503035` — FAIL. Inherited WU5 source QA expected the old activation contract. Fixed to require Journey Room + social safety hardening + WU5 interaction flag.
- CI #241 / run `33671685313` — FAIL. Ephemeral WU8 harness lacked canonical WU5 SELECT prerequisites required by the SECURITY INVOKER report guard. Migration 0051 was updated to reassert the already-existing production SELECT grants on `journey_social_presences` and `journey_interactions`.
- CI #242 / run `33671835314` — FAIL. QA correctly proved authenticated cannot inspect private report audit data. Test harness was fixed by resetting role before operator/private audit assertions; migration privacy was not weakened.
- CI #243 / run `33672050451` — COMPLETE / SUCCESS on exact head `4145e28028ad48d0822b82a9b232fa782e6fc1ee`.

CI #243 passed all inherited source QA, P16-WU8 source QA, inherited ephemeral DB QA, P16-WU8 PostgreSQL 17 DB + rollback QA, build, typecheck and Cloudflare dry-run.

### Production-main CI

After PR #57 squash merge, main CI #244 / run `33707804208`, job `100500639455`, exact SHA `05742cdff1aefaac48a1723ca5eaa3e3499543af` completed SUCCESS.

Every CI step passed, including the P16-WU8 source gate, P16-WU8 ephemeral DB + rollback gate, build, typecheck and Cloudflare dry-run.

## 4. Source implementation

### 4.1 Reversible block management

`public.social_blocks` gains `blocked_display_name_snapshot`.

The block guard is `SECURITY DEFINER`, `search_path=''`, not directly executable by `public`, `anon` or `authenticated`.

The server derives the blocked display-name snapshot so a user can later manage/unblock without re-querying or re-exposing the blocked person's profile.

### 4.2 Reports and human-review lifecycle

Report categories now include:

- `harassment`
- `privacy`
- `impersonation`
- `unsafe_content`
- `spam`
- `other`
- `threat_or_violence`
- `sexual_safety`
- `child_safety`

`social_reports.priority` is server-derived as `standard` or `urgent`.

Urgent categories are only:

- `threat_or_violence`
- `sexual_safety`
- `child_safety`

Valid report lifecycle:

- `open -> triaged`
- `open -> dismissed`
- `triaged -> resolved`
- `triaged -> dismissed`

Backward reopen is rejected.

A private moderation audit ledger records report status transitions.

### 4.3 Duplicate-report protection

Partial unique index:
`social_reports_one_live_interaction_report_idx`

It prevents multiple simultaneous `open`/`triaged` reports from the same reporter for the same exact interaction, while allowing a later report after the previous case is closed.

### 4.4 Proportionate interaction restriction

New moderation control:
`interaction_restrict`

It prevents a social identity from creating new Questions / Replies / Appreciations while preserving identity, Journey Presence, existing interaction history and private operational truth.

It does not erase history.

### 4.5 Activation gate

Environment variable:
`VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED=false`

WU5 interaction activation is now fail closed and additionally requires the WU8 safety gate.

All P16 social UX flags remain OFF at WU8 closeout.

### 4.6 UX surfaces

Journey interaction UX:

- block another author;
- confirmation before block;
- report categories including urgent safety concerns;
- bilingual copy stating urgent means priority human review, not proof or automatic punishment.

Private Community account safety settings:

- own blocked-list management only;
- query exposes only block row ID, server-derived display-name snapshot and creation time;
- blocked social identity ID is intentionally not re-exposed;
- unblock deletes only the user's own block row.

Admin Social Safety queue:

- admin-only query;
- reporter identity omitted from queue UI;
- shows target, Journey context, category, details, priority and status;
- human lifecycle transitions only;
- does not one-click create moderation controls.

## 5. Production preflight before migration

Supabase project ref:
`iwiqprhoohkxvjyxojto`

Production ledger ended at:
`20260902141453_0050_p16_wu6_notification_lifecycle_hardening`

Before migration:

- `social_blocks = 0`
- `social_reports = 0`
- `social_moderation_controls = 0`
- `journey_interactions = 0`
- WU8 objects absent
- relevant public tables had RLS enabled
- authenticated already had the canonical SELECT privileges on `journey_social_presences` and `journey_interactions`; 0051 only reasserted these prerequisites.

Truth sentinel before migration:

- `community_journey_memories`: 1 row
- `attendance_state = 'unresolved'`
- `memory_eligible = false`

No production social data or Journey truth was mutated during preflight.

## 6. Owner production gate

Owner explicitly approved:

`APPROVE P16-WU8 PRODUCTION DATABASE MIGRATION — apply canonical 0051 advanced moderation safety foundation to Supabase production, keep all social feature flags OFF, no Worker deploy, then run full post-migration verification + Security/Performance Advisors and close WU8 only if all gates PASS.`

No production DDL was executed before this approval.

## 7. Production migration

Canonical 0051 was applied from exact production-main source.

Production migration ledger entry:

`20260903075748_p16_wu8_advanced_moderation_safety`

Apply result: SUCCESS.

No Worker deployment occurred.
No social feature flag was enabled.
No email/push activation occurred.

## 8. Post-migration verification

### 8.1 Schema

Verified present:

- `social_blocks.blocked_display_name_snapshot`
- `social_reports.priority`
- report category constraint including the three new safety categories
- priority constraint `standard | urgent`
- private `social_report_moderation_events`
- `social_reports_priority_status_idx`
- `social_reports_one_live_interaction_report_idx`
- moderation control constraint including `interaction_restrict`
- target consistency constraint for `interaction_restrict`
- `private.tnc_report_priority(text)`
- `private.tnc_social_identity_interaction_restricted(uuid)`
- updated `private.tnc_can_write_journey_interaction(uuid,uuid)`
- report status audit trigger/function
- updated block guard
- updated report guard.

### 8.2 Function security

Verified:

- `tnc_guard_social_block`: SECURITY DEFINER, `search_path=''`, authenticated cannot direct EXECUTE.
- `tnc_guard_social_report`: SECURITY INVOKER, `search_path=''`, authenticated cannot direct EXECUTE trigger function.
- `tnc_audit_social_report_status`: SECURITY DEFINER, `search_path=''`, authenticated cannot direct EXECUTE.
- `tnc_social_identity_interaction_restricted`: SECURITY DEFINER, `search_path=''`, authenticated cannot direct EXECUTE.
- `tnc_can_write_journey_interaction`: SECURITY DEFINER, `search_path=''`, authenticated EXECUTE only as intended for the guarded write path.
- `tnc_report_priority`: SECURITY INVOKER SQL mapping helper, `search_path=''`, authenticated EXECUTE as required by the SECURITY INVOKER report trigger.

### 8.3 Private audit ACL

`private.social_report_moderation_events` ACL after migration:

- `postgres`: owner/all privileges
- `service_role`: SELECT/INSERT/UPDATE/DELETE
- no `public`
- no `anon`
- no `authenticated`.

### 8.4 RLS

RLS remains enabled on:

- `journey_social_presences`
- `journey_interactions`
- `social_blocks`
- `social_reports`
- `social_moderation_controls`.

### 8.5 Production data and truth preservation

Post-migration counts remain:

- `social_blocks = 0`
- `social_reports = 0`
- `social_moderation_controls = 0`
- `journey_interactions = 0`
- `private.social_report_moderation_events = 0`.

Truth sentinel remains unchanged:

- `attendance_state = 'unresolved'`
- `memory_eligible = false`
- rows = 1.

Therefore WU8 introduced no fabricated attendance, Memory, relationship, Contribution or impact claim.

## 9. Supabase Advisors

### Security Advisor — PASS for WU8 delta

No WU8-specific security finding was introduced.

The only Security Advisor warning remains the pre-existing Auth configuration warning:

- `auth_leaked_password_protection` — Leaked Password Protection Disabled.

This is not caused by WU8 and is not silently modified in this work unit.

Remediation reference:
`https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection`

### Performance Advisor — PASS for WU8 delta with expected INFO

No blocking WU8 performance finding was introduced.

New WU8 indexes are reported as unused immediately after migration:

- `private.social_report_moderation_events.social_report_moderation_events_report_idx`
- `public.social_reports.social_reports_priority_status_idx`

This is expected because production currently contains zero social reports / moderation events. They are functional indexes for future workload and are not removed merely because they have not yet been exercised.

Pre-existing advisor items remain outside WU8 scope, including:

- unindexed FK INFO on `public.social_notifications(journey_id)`;
- several older unindexed FKs;
- older RLS init-plan WARNs;
- older unused-index INFOs;
- `user_roles` multiple permissive SELECT-policy WARN.

No unrelated optimization was silently bundled into WU8.

## 10. Runtime / release state at closeout

- Product source merged to `main`.
- Production database migration applied and verified.
- Cloudflare Worker: NOT DEPLOYED by WU8.
- P16 social feature flags: OFF.
- Journey interaction activation remains fail closed behind the social-safety hardening flag.
- No fake social activity, reports, moderation actions, Memory or impact data created.

## 11. Final WU8 gate

PASS criteria satisfied:

- exact production-main source verified;
- PR CI PASS;
- main CI PASS;
- production preflight PASS;
- explicit Owner approval recorded;
- canonical migration applied successfully;
- schema/constraint/function/ACL/RLS verification PASS;
- operational truth sentinel unchanged;
- Security Advisor: no WU8-specific finding;
- Performance Advisor: no blocking WU8-specific finding;
- all social flags remain OFF;
- no Worker deploy.

## FINAL STATUS

**P16-WU8 COMPLETE / PASS — SOURCE, ARCHITECTURE & PRODUCTION DATABASE FOUNDATION**

Next gate:

**P16-WU9 — Mobile / VI-EN / Cross-Surface / Privacy QA**

Independent real-world evidence lane remains open:

**P14-WU5 — real Journey evidence / 2026-09-11.**

No attendance or post-Journey outcome may be fabricated before real evidence exists.
