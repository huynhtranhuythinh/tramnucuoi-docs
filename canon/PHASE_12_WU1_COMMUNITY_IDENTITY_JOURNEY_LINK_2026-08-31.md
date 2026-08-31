# TRẠM NỤ CƯỜI — PHASE 12 / P12-WU1
# COMMUNITY IDENTITY & JOURNEY LINK FOUNDATION

Date: 2026-08-31
Status: **COMPLETE / PASS**

## Objective

Create the first bridge from Journey operations into the Living Community OS without rewriting operational history and without fabricating a personal Memory.

A historical or anonymous Journey participant must be able to become related to a real authenticated community identity later, while the original Journey participant snapshot remains operational truth.

## Discovery

The live pilot currently has one confirmed participant / one confirmed person, but both `journey_participants.user_id` and the corresponding `journey_applications.user_id` are null.

A direct `auth.uid()`-only My Memories feature would therefore exclude the first real pilot participant.

The existing `/auth` UX is staff-only. Existing `user_roles` contains staff authorization (`admin`, `editor`) and must not be used as an implicit community-membership role.

## Architecture

WU1 introduced `public.community_participant_links`.

Separation of truths:
- `journey_participants` = operational Journey participation snapshot;
- `community_participant_links` = verified identity relationship;
- future Memory = a derived/personal experience built from verified identity + real participation/attendance + published Journey material.

WU1 does not assert attendance, no-show, Memory or impact.

## Identity-link rules

- `user_id` references `auth.users`.
- `participant_id` references `journey_participants` with `ON DELETE RESTRICT`.
- only `admin_verified` is accepted in WU1.
- public/self-claim method is deliberately fail-closed until WU2.
- one active identity per participant snapshot.
- no duplicate active user/participant relationship.
- no PII duplication (email, phone, full name are not copied to the link table).
- corrections use revoke → replacement; verified ownership is never reassigned in place.
- revoked history is immutable.
- verifier and revoker actor UUIDs are retained as audit data.

## Authorization

- anon: no access.
- authenticated community user: SELECT only their own link through RLS.
- authenticated admin: read/create/revoke links through existing `private.has_role('admin')` authorization.
- no authenticated DELETE.
- service role remains inside the existing backend secret boundary.
- no community account receives a CMS role as a consequence of this feature.

Private trigger function:
`private.tnc_guard_community_participant_link()`

Properties:
- `SECURITY INVOKER`
- `search_path = ''`
- no direct EXECUTE for anon/authenticated
- trigger-only behavior
- DB binds `verified_by` to current authenticated admin
- DB stamps revocation actor/time
- verified audit fields immutable

## Source evidence

Product PR: #18 — `P12-WU1 community identity and Journey link foundation`

Product main merge SHA:
`e48262879be0c7d4448dbfceb2c69147f982e64f`

Files:
- `docs/FUTURE_ROADMAP.md`
- `db/migrations/0032_p12_wu1_community_identity_foundation.sql`
- `scripts/p12-wu1-db-qa.sql`
- `.github/workflows/ci.yml`

## QA evidence

PR CI #102: PASS.

Verified:
- P9-WU7 source abuse-protection QA
- P10-WU3A runtime-context QA
- P9-WU7 DB QA
- P11-WU11 transactional capacity/cutoff QA
- P12-WU1 community identity DB QA
- TypeScript typecheck
- application build
- Cloudflare dry-run only

Post-merge main CI #103: PASS with the same gates.

No Cloudflare production deployment occurred in WU1.

## Production migration

Supabase project:
`iwiqprhoohkxvjyxojto`

Applied migration:
`20260831004142 p12_wu1_community_identity_foundation`

Migration was additive and seeded no identity rows.

## Production postflight

Verified after migration:
- `community_participant_links` exists;
- RLS enabled;
- link rows = 0;
- anon SELECT = false;
- authenticated own-link SELECT available through RLS;
- authenticated DELETE = false;
- guard function is invoker-security with empty search path and no direct anon/authenticated execution;
- guard + updated_at triggers are present;
- pilot Journey remains `registration_open`;
- capacity = 30;
- confirmed participant rows = 1;
- confirmed people = 1;
- current pilot participant/application user IDs remain null;
- application/participant semantic drift = 0;
- `pg_graphql` remains OFF.

## Advisors

Security Advisor:
- no new WU1 security lint;
- pre-existing warning remains `Leaked Password Protection Disabled`.
- remediation: https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

Performance Advisor:
- existing project advisories remain (unindexed FKs, legacy RLS initplan issues, unused indexes, existing multiple permissive policy on user_roles);
- `community_participant_links_user_status_idx` is reported unused immediately because WU1 intentionally has zero production link rows. Retain it until real query traffic gives meaningful evidence.

## Non-goals / gates preserved

WU1 did not:
- open public signup;
- auto-claim a participant;
- mutate existing participant identity snapshot;
- fabricate attendance or no-show;
- create Memory rows;
- add follower/feed/chat/gamification;
- create Contribution history;
- deploy Cloudflare runtime code.

P11-WU6 remains ACTIVE through the real 2026-09-11 Journey and verified closeout.

## Next gate

**P12-WU2 — Community Account Onboarding & Participant Claim**

WU2 must create a community-facing account path independent of the staff-only `/auth` UX and must prove ownership before creating an active participant link.
