# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU5.FINAL — PARTICIPANT PERSONAL JOURNEY WORKSPACE
# PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — PRODUCTION ACTIVE**

## 1. Objective

Close P17-WU5 after activating the private Journey-centric operational workspace for a verified confirmed participant.

The production-active participant surface now supports:

- My Journey entry;
- canonical Journey lifecycle context;
- participant status;
- current Journey Role;
- current Department / optional Team;
- participant-safe Plan / Runbook projection;
- participant-safe Official Update delivery.

This remains separate from:

- Community Social Home;
- attendance evidence;
- Memory / Reflection / Impact;
- Admin Journey Control Center;
- application review / PII;
- generic account/CMS roles.

## 2. Access authority

Participant workspace authority is:

`auth.uid() -> active community_participant_links -> confirmed journey_participants`

It is not derived from:

- Admin / Editor account role;
- social identity;
- Community presence;
- Memory eligibility;
- attendance;
- legacy `preferred_team` / `assigned_team`.

Current operational assignment is:

`journey_participant_assignments.ended_at IS NULL`

Assignment does not imply attendance.

## 3. Production RPCs

Activated:

- `public.tnc_my_journey_workspaces()`;
- `public.tnc_my_journey_plan(uuid)`;
- `public.tnc_my_journey_official_updates(uuid)`.

Shared private authorization helper:

- `private.tnc_has_confirmed_journey_participation(uuid)`.

Security shape:

- privileged implementation remains in `private`;
- fixed `search_path = ''`;
- public wrappers remain `SECURITY INVOKER`;
- authenticated/service_role EXECUTE allowed;
- anon EXECUTE denied;
- WU3/WU4 source-table participant access was not broadened.

## 4. Product sequence evidence

### WU5.1 — Participant Access / Projection Source Contract

PR #94.

Exact head:

`ecb1cfaab6a252796b386eef5a5cecfdbba85b01`

Exact-head:

- generic CI `36100096269`: SUCCESS;
- dedicated gate `36100096311`: SUCCESS.

Merged main:

`f5f62069abfd8c8af75915a4c8bd715c31a823bf`

Post-merge main CI:

`36100208621`: SUCCESS.

### WU5.2 — My Journey Entry / Workspace Shell

PR #95.

Merged main:

`58061c89ad88c1be851933c5a617de14c73b506e`

Post-merge main CI:

`36100424899`: SUCCESS.

Operational Workspace is mounted before the existing P16 Social Home on both:

- `/cong-dong`;
- `/en/community`.

### WU5.3 — My Role / Department / Team

PR #96.

A stale inherited WU5.2 QA assertion initially rejected the now-canonical assignment fields. The gate was repaired to protect the durable semantic invariant instead:

- no legacy team authority;
- no social authority inference.

No security/product standard was weakened.

Corrected exact head:

`f1835c02bb8f099355b889dfd14a2741dfefc26e`

Both exact-head gates: SUCCESS.

Merged main:

`784238dc031cc154e636d6450e0f129c79b2dc7e`

Post-merge main CI:

`36100726567`: SUCCESS.

### WU5.4 — Participant-safe Plan

Canonical rebased PR #98.

Exact head:

`7f6decfda1e2c0b1a86183be1c6dfafa55f3b7be`

Exact-head generic/dedicated gates: SUCCESS.

Merged main:

`51ccf08b7d24ffff41c0c86a5ed419c2af90d989`

Post-merge main CI:

`36100979194`: SUCCESS.

A stale duplicate PR was explicitly closed and not merged.

### WU5.5 — Participant Official Update Delivery

PR #100.

Exact head:

`38437fac83e1b42c8a9d8cb8500a8b1d1a84b39b`

Exact-head:

- generic `36101568980`: SUCCESS;
- dedicated `36101569091`: SUCCESS.

Merged main:

`4d2848d4de1bbbd45affdc505b543fd8bb8908a0`

Post-merge main CI:

`36101709140`: SUCCESS.

Official Update remains distinct from Community `journey_updates`.

### WU5.6 — Security / Privacy / Mobile Regression

Clean rebased PR #102.

Exact head:

`fb98728b811d739ca5fcf88672a77a77518b517d`

Exact-head:

- generic `36101767803`: SUCCESS;
- dedicated `36101767820`: SUCCESS.

Merged main:

`5613bb37662b8b06b5a1684127153cbfaa601bb0`

Post-merge main CI:

`36101902088`: SUCCESS.

## 5. WU5.Final release gate

PR #103.

Initial release run caught a test-only SQL dollar-quote typo in the rollback assertion block after migration/function/functional checks had already executed. Production was still untouched.

The two malformed delimiters were corrected from a single-dollar token to a valid PostgreSQL `DO $$ ... $$;` block.

No migration business/security semantics changed.

Final exact head:

`9c6d1a805c61d26bc051168256f3922bd6350708`

Exact-head gates:

- generic CI `36102172225`: SUCCESS;
- dedicated gate `36102172228`: SUCCESS;
- WU5.Final source QA: PASS;
- migration + functional projection + rollback DB QA: PASS;
- all inherited gates: PASS;
- build: PASS;
- typecheck: PASS;
- Cloudflare dry-run: PASS.

PR #103 squash-merged.

Product main:

`f63e6a5657c20ff2b8f46d9f89b580c52e98f566`

Post-merge main:

- CI `36102300830`: SUCCESS;
- Cloudflare production build: SUCCESS;
- Cloudflare staging build: SUCCESS.

## 6. Production migration

Applied successfully to Supabase:

`20260925062925 / p17_wu5_final_participant_workspace_projections`

Source:

`database/migrations/0059_p17_wu5_final_participant_workspace_projections.sql`

Rollback source:

`database/rollbacks/p17_wu5_final_participant_workspace_projections.sql`

Cutover is read-only capability activation:

- no new operational table;
- no fake seed data;
- no Journey row mutation;
- no application mutation;
- no assignment mutation;
- no attendance mutation;
- no Memory / Reflection / Impact mutation;
- no Community truth mutation.

## 7. Production before / after verification

Before cutover:

- workspace RPC: absent;
- Plan RPC: absent;
- Official Update RPC: absent;
- Journeys: 5;
- non-closed Application Windows: 0;
- participants: 3;
- confirmed participants: 3;
- active participant links: 2;
- Memory rows: 2;
- current assignments: 0;
- Runbook items: 0;
- Official Updates: 0;
- resolved attendance rows: 0.

After cutover:

- workspace RPC: present;
- Plan RPC: present;
- Official Update RPC: present;
- authorization helper: present;
- authenticated EXECUTE: true;
- anon EXECUTE: false;
- non-closed Application Windows: 0;
- participants: 3;
- confirmed participants: 3;
- active participant links: 2;
- Memory rows: 2;
- current assignments: 0;
- Runbook items: 0;
- Official Updates: 0;
- published Official Updates: 0;
- resolved attendance rows: 0.

The only intended production state change is activation of the WU5 read-projection functions.

## 8. Canonical UX/truth boundaries preserved

- application != participant;
- approval != assignment;
- assignment != attendance;
- participant status != attendance;
- Journey Role != Department != Team != Task != Skill;
- Official Update != Community post;
- Operational Workspace != Community;
- Team != social group;
- verified participation != shared real-world experience;
- workspace != Memory.

## 9. Recruitment state

Recruitment remains:

# **HOLD / CLOSED**

All Application Windows remain CLOSED.

WU5 does not authorize opening recruitment.

## 10. Final decision

# **P17-WU5 — COMPLETE / CLOSED / PASS**

Participant Personal Journey Workspace is production-active with least-privilege read projections and preserved privacy/evidence boundaries.

Next Phase 17 work must start from this canonical truth and must not reopen WU5 absent a demonstrated regression.
