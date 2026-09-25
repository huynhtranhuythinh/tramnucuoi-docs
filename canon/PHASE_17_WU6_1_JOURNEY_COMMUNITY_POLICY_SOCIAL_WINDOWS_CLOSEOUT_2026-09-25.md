# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.1
# JOURNEY COMMUNITY POLICY / SOCIAL WINDOW SOURCE CONTRACT
# CANONICAL CLOSEOUT

Date: 2026-09-25  
Status: **COMPLETE / PASS — SOURCE-ONLY; PRODUCTION CUTOVER DEFERRED TO WU6.FINAL**

## 1. Objective

Create the canonical Journey-owned Community policy foundation required by the P17 Journey Community Rebase.

WU6.1 defines:

- independent Publishing Window;
- independent Interaction Window;
- optional open-ended Interaction Window;
- visitor interaction policy;
- participant-post public publication mode;
- Admin-only policy management;
- fail-closed public-safe policy projection.

WU6.1 does not create social posts and does not mutate production.

## 2. Canonical source

Product source:

`database/contracts/p17_wu6_1_journey_community_policy.sql`

QA:

- `scripts/p17-wu6-1-journey-community-policy-source-qa.ts`;
- `scripts/p17-wu6-1-journey-community-policy-db-qa.sql`.

Product PR:

`#104 — P17-WU6.1: Journey Community policy and social windows`

Final exact head:

`b2e62ed432dd90ca63dbe9c3670c64826c51f4fd`

Squash-merged product main:

`ade399456fbf50de4e3649483532abd7b3dd0859`

Post-merge evidence:

- main CI `36106064276`: **SUCCESS**;
- Cloudflare production build: **SUCCESS**;
- Cloudflare staging build: **SUCCESS**.

## 3. Policy model

Canonical table:

`public.journey_community_policies`

Fields include:

- Journey id;
- publishing enabled/open/close;
- interaction enabled/open/close;
- visitor interaction enabled;
- post publication mode;
- created/updated audit actors;
- timestamps.

Absence of a policy row means:

- publishing disabled;
- interaction disabled;
- visitor interaction disabled;
- publication mode = `participants_only`.

No default policy row is seeded.

## 4. Social window rules

Publishing Window:

- explicit configured open timestamp;
- explicit configured close timestamp;
- no hardcoded ±N-day rule.

Interaction Window:

- explicit configured open timestamp;
- close timestamp may be null when Admin intentionally allows open-ended interaction.

Publishing and Interaction are independent.

Ephemeral QA proves the following configuration:

- before both windows: publishing CLOSED / interaction CLOSED;
- Interaction Window may open before Publishing Window;
- during overlapping window: both OPEN;
- after Publishing Window closes: publishing CLOSED while open-ended interaction remains OPEN.

## 5. Publication policy modes

Locked values:

- `participants_only`;
- `review_required`;
- `participant_choice`.

Safe default:

`participants_only`

These modes configure future participant-post publication behavior. WU6.1 itself does not create posts or public publication.

## 6. Security model

`journey_community_policies`:

- RLS enabled;
- no anon direct table access;
- authenticated table grants are constrained by Admin-only RLS;
- service_role retains control-plane access.

Admin authority:

`private.has_role('admin'::public.app_role)`

Global Editor is not Journey Community policy authority.

Write audit is server-enforced by:

`private.tnc_guard_journey_community_policy()`

The guard:

- requires Admin;
- server-derives `created_by` / `updated_by`;
- preserves immutable Journey/created audit identity.

Derived private helpers use:

- non-exposed `private` schema;
- `SECURITY DEFINER`;
- `search_path = ''`.

Public-safe projection:

`public.tnc_journey_community_policy(uuid,timestamptz)`

remains a `SECURITY INVOKER` wrapper and returns only Journey policy/window state.

It contains no:

- participant PII;
- application data;
- attendance;
- Memory;
- social identity;
- social interaction rows.

## 7. Corrective QA evidence

Three test-only corrections occurred before the final exact-head PASS.

### 7.1 Admin authorization source assertion

The source QA initially expected at least six textual Admin-authorization occurrences while the canonical contract contains five real authorization points.

The assertion was corrected to the actual invariant count.

No policy/authorization was removed or weakened.

### 7.2 Editor denial exception shape

The DB fixture initially expected PostgreSQL `insufficient_privilege`.

The server audit guard correctly rejected the Editor first with an explicit application exception:

`Admin required to manage Journey Community policy`

The fixture was corrected to accept any valid server-side denial while retaining a sentinel that fails if Editor insert actually succeeds.

No permission was widened.

### 7.3 Independent-window fixture and dollar quoting

A time assertion initially tested 19/09 as though interaction should still be closed even though the fixture intentionally configured Interaction Window to open on 18/09.

The QA was improved to prove four states:

1. both closed;
2. interaction-only open;
3. both open;
4. publishing closed while open-ended interaction stays open.

A test-only PostgreSQL `DO` delimiter accidentally became `do $`; it was corrected to `do $$`.

No product contract semantics changed.

## 8. Final exact-head gates

Final exact head:

`b2e62ed432dd90ca63dbe9c3670c64826c51f4fd`

Dedicated gate:

`36105915239` — **SUCCESS**

Generic CI:

`36105915403` — **SUCCESS**

Both passed:

- WU6.1 source QA;
- WU6.1 PostgreSQL social-window DB QA;
- inherited source/security/DB gates;
- build;
- typecheck;
- Cloudflare dry-run.

## 9. Production boundary

WU6.1 performed:

- no Supabase migration;
- no production DB mutation;
- no social policy row seed;
- no social identity/presence/post/interaction creation;
- no feature-flag activation;
- no recruitment activation;
- no attendance mutation;
- no Memory/Reflection/Impact mutation.

Production recruitment remains:

**HOLD / CLOSED**

WU6.Final owns production DDL/cutover.

## 10. Truth boundaries preserved

- Journey policy != Journey lifecycle;
- publishing right != attendance;
- interaction right != participant status;
- participant access != public consent;
- public publication != official TNC reuse;
- social activity != shared real-world experience;
- Community policy != Official Update;
- no policy row = fail closed.

## 11. Final decision

# **P17-WU6.1 — COMPLETE / PASS**

Proceed to:

# **P17-WU6.2 — ORIGINAL JOURNEY POST + PUBLISHING AUTHORIZATION FOUNDATION**
