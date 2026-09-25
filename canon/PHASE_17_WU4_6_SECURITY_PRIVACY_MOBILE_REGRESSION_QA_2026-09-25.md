# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU4.6 — SECURITY / PRIVACY / MOBILE REGRESSION QA

Date: 2026-09-25  
Status: **COMPLETE / PASS — SOURCE / QA ONLY; NO WU4 PRODUCTION DDL**

## 1. Objective

Close the pre-cutover security, privacy and mobile regression gate for the complete P17 volunteer application / staffing / assignment rebase before WU4.Final production cutover.

This work unit verifies the integrated WU4 model:

`Staffing Need -> ranked Department preferences -> BTC review / Waitlist / Approval -> Participant -> Journey Role -> Department / optional Team assignment -> Attendance`

and preserves the canonical boundaries:

- application != participant;
- approval != assignment;
- assignment != attendance;
- Journey Role != Department != Team != Task != Skill;
- operational truth != public visibility;
- P16 legacy team strings are compatibility/history only;
- WU4 must not activate recruitment;
- WU4 must not manufacture Memory, social evidence or attendance.

## 2. Canonical-state correction

The conversation request named WU4.2, but canonical repository truth had already advanced.

Before this work started:

- WU4.2: COMPLETE / PASS;
- WU4.3: COMPLETE / PASS;
- WU4.4: COMPLETE / PASS;
- WU4.5: COMPLETE / PASS;
- WU4.6: NEXT.

Product main at the actual WU4.6 start:

`a993980d5984a7725a1ecacde0a7a0c694070b33`

Therefore WU4.2 was not rebuilt or reopened.

## 3. Security review basis

Relevant WU4 source contracts:

- `database/contracts/p17_wu4_1_staffing_preference_assignment.sql`
- `database/contracts/p17_wu4_2_public_staffing_projection.sql`
- `database/contracts/p17_wu4_4_participant_assignment_operations.sql`

Inherited identity/Vault hardening:

- `database/migrations/0056_p17_wu3_7_identity_reveal_rpc_hardening.sql`

Relevant application / Admin source:

- `src/lib/journeys/volunteer-registration.server.ts`
- `src/lib/journeys/volunteer-admin.ts`
- `src/lib/journeys/participant-assignments.ts`
- `src/components/journeys/volunteer-application-form.tsx`
- `src/components/admin/journeys/volunteer-recruitment-workspace.tsx`
- `src/components/admin/journeys/journey-staffing-needs-manager.tsx`
- `src/components/admin/journeys/journey-participant-assignment-manager.tsx`

## 4. RLS / grant regression

WU4.6 locks the following source invariants:

### Staffing needs

Planned source table:

`journey_staffing_needs`

- RLS required;
- explicit Data API grants;
- Admin-only insert/update;
- no normal hard-delete grant;
- public read is constrained to public-safe staffing rows;
- public read must not expose the operational Department source directly.

### Participant assignments

Planned source table:

`journey_participant_assignments`

- RLS required;
- explicit Data API grants;
- Admin-only read/write;
- no normal hard-delete grant;
- assignment changes preserve history.

### Authorization source

WU4 authorization must not use user-editable auth metadata.

The gate explicitly rejects reliance on:

- `raw_user_meta_data`;
- user-editable JWT metadata.

Admin authority remains based on the canonical role source / `private.has_role(...)`.

## 5. Privileged-function boundary

WU4 preserves the hardened function pattern:

> privileged implementation in non-exposed `private` schema + narrow public SECURITY INVOKER wrapper.

### Public staffing projection

Private implementation:

`private.tnc_public_staffing_options(uuid)`

- SECURITY DEFINER;
- fixed `search_path = ''`;
- returns only public-safe staffing projection.

Public wrapper:

`public.tnc_public_staffing_options(uuid)`

- SECURITY INVOKER;
- no privileged public-schema implementation.

### Participant assignment operations

Private privileged implementations remain outside public schema for:

- set assignment;
- end assignment;
- volunteer TNV-role bootstrap.

Stable public wrappers remain SECURITY INVOKER.

### Full identity reveal

Inherited WU3.7 hardening remains authoritative:

- privileged Vault decrypt helper lives in `private`;
- helper self-authorizes Admin;
- public RPC is SECURITY INVOKER;
- anon/public execution is explicitly revoked;
- reveal remains a deliberate Admin action.

## 6. Privacy / PII regression

WU4.6 verifies that ordinary Admin application projection does not contain:

- transient full identity-document input;
- Vault secret pointer.

The full CCCD/passport value remains on the existing transient-to-Vault path.

Sensitive volunteer data remains excluded from notification email, including:

- date of birth;
- full identity number;
- identity expiry;
- emergency contact;
- health notes.

The VI/EN consent copy continues to state that private safety/identity information:

- is for recruitment / safety / authority registration where required;
- is not published to Community / public profile / Memory / Reflection;
- is not evidence of attendance.

## 7. Lifecycle / staffing authority regression

The trusted volunteer submission path continues to use the WU2 authority model:

- lifecycle phase;
- application state.

It does not regress to legacy `journey.status = registration_open` as authority.

Every submitted Department preference is revalidated server-side against current public-safe staffing options before write.

P17 application write remains:

- `volunteer_structure_version = p17-wu4-v1`;
- ranked Department IDs;
- legacy `preferred_team = null`;
- legacy `assigned_team = null`.

Preference remains explicitly distinct from final BTC assignment.

## 8. Approval / assignment / attendance isolation

WU4.6 confirms:

- approval does not require legacy `assigned_team`;
- participant confirmation remains separate from Department/Team assignment;
- participant assignment operations do not write attendance;
- volunteer role bootstrap does not manufacture attendance;
- assignment history is append/close rather than destructive overwrite.

P17 rows remain blocked from the legacy P16 `assigned_team` writer.

No WU4 source path mutates:

- Journey Memory;
- Reflection;
- shared-experience graph;
- social presence/evidence truth.

## 9. Mobile product correction

A real product gap was found during WU4.6:

> the P17 volunteer form was responsive, but still rendered as one long form.

This conflicted with the locked canonical form direction requiring a progressive mobile-friendly application.

The form was rebased into four steps.

### Step 1 — Needs & preferences

VI:

`Nhu cầu & nguyện vọng`

EN:

`Needs & preferences`

Contains:

- current open staffing needs;
- Preference 1;
- optional Preference 2;
- optional Preference 3.

### Step 2 — Contact

VI:

`Thông tin liên hệ`

EN:

`Contact details`

Contains:

- full name;
- email;
- phone.

### Step 3 — Safety & identity

VI:

`An toàn & giấy tờ`

EN:

`Safety & documents`

Contains:

- birth date;
- identity type;
- identity number;
- expiry;
- emergency contact;
- health information.

### Step 4 — Consent & submit

VI:

`Đồng ý & gửi`

EN:

`Consent & submit`

Contains:

- optional applicant question;
- explicit privacy consent;
- Turnstile path when required;
- final submit.

## 10. Progressive-form invariants

The implementation preserves the same underlying schema and trusted write authority.

Each step uses field-level React Hook Form validation before advancing.

Mobile behavior:

- one-column content on the narrowest viewport;
- progressive status presented as 2 columns on narrow mobile and 4 at `sm`;
- responsive forms fan out at wider breakpoints only;
- Back / Continue / Submit remain touch-sized controls;
- only the final action is a submit action;
- Back / Continue do not accidentally submit the form.

No server-side validation, registration gate, dedupe, Turnstile or Vault authority was weakened to implement the progressive UX.

## 11. Admin mobile regression

WU4.6 confirms existing WU4 Admin surfaces remain responsive:

### Volunteer review

- controls wrap on narrow screens;
- metrics fan out only at `sm/lg`;
- application detail becomes two-column only on large screens.

### Staffing needs

- action groups wrap;
- form stacks before `md`.

### Participant assignment

- controls wrap;
- detail grids stack on mobile;
- Department / Team assignment controls remain usable without fixed desktop columns.

Sensitive identity reveal remains an explicit action:

`XEM THÔNG TIN AN TOÀN`

## 12. QA gate

Added:

`scripts/p17-wu4-6-security-privacy-mobile-regression-qa.ts`

The gate is wired into both:

- generic CI;
- dedicated P16-WU10B Volunteer Pilot workflow.

The gate covers:

- RLS;
- explicit grants;
- privileged function placement;
- public SECURITY INVOKER wrappers;
- no user-editable metadata authorization;
- Vault/PII boundaries;
- email privacy;
- bilingual consent;
- WU2 lifecycle authority;
- staffing revalidation;
- legacy P16 isolation;
- approval/assignment/attendance separation;
- no Memory/social truth mutation;
- recruitment HOLD;
- public progressive mobile UX;
- Admin mobile responsiveness.

## 13. QA false-positive and correction

First exact-head run correctly stopped before merge.

Failed runs:

- generic CI `36095300148`;
- dedicated Volunteer Pilot gate `36095300167`.

The WU4.6 assertion treated this legitimate read predicate:

`application_state = 'open'`

as if it were a mutation that opened recruitment.

That was a QA semantic false-positive.

The gate was corrected to distinguish:

- reading `application_state = open` as a lifecycle/public-staffing condition;
- actually mutating/inserting a Journey into OPEN state.

No product or security invariant was weakened.

## 14. Exact-head PASS evidence

Final PR head:

`2f6f375fdb59c6f8d95a3301faf652f98291204b`

PR:

`#92 — P17-WU4.6: Security privacy and mobile regression QA`

Exact-head workflows:

- generic CI `36095392567`: **SUCCESS**
- dedicated P16-WU10B Volunteer Pilot Gate `36095392549`: **SUCCESS**

Passed gates include:

- WU4.6 security/privacy/mobile regression;
- inherited P16 volunteer privacy gate;
- inherited P16 Vault / RLS DB gate;
- WU4.1 source + DB contract;
- WU4.2 source + public staffing DB contract;
- WU4.3 workflow gate;
- WU4.4 source + assignment DB contract;
- WU4.5 legacy compatibility gate;
- inherited P9–P17 source / DB regression suite;
- build;
- typecheck;
- Cloudflare dry-run.

## 15. Merge / post-merge

PR #92 was squash-merged.

Product main:

`24ca60628ba1289dc523b730d53cd7723c3498b6`

Post-merge main CI:

`36095579285` — **SUCCESS**

## 16. Production invariant after WU4.6

Production verification after merge confirms:

- `journey_staffing_needs`: absent;
- `journey_participant_assignments`: absent;
- `waitlisted` enum: absent;
- public staffing RPC: absent;
- set-assignment RPC: absent;
- volunteer-role bootstrap RPC: absent;
- non-closed application windows: **0**.

Therefore:

- no WU4 production DDL has been applied;
- progressive source UX cannot bypass the pre-cutover capability gate;
- recruitment remains **HOLD / CLOSED**;
- WU4.Final remains the sole production cutover authority.

## 17. Supabase Security Advisor observation

Security Advisor was refreshed during WU4.6.

Current external warning:

`auth_leaked_password_protection`

Title:

**Leaked Password Protection Disabled**

This warning is an existing project-level Supabase Auth configuration observation.

It is:

- not introduced by WU4;
- not a WU4 RLS/Vault regression;
- not silently marked resolved by this closeout.

It should remain visible as a separate Auth hardening item.

## 18. Decision

**P17-WU4.6 — COMPLETE / PASS.**

All WU4 source/application/assignment/security/mobile work units before cutover are now complete.

Next canonical work unit:

# **P17-WU4.Final — PRODUCTION CUTOVER & CANONICAL CLOSEOUT**

WU4.Final must:

- build explicit production migration/rollback from the already-PASS source contracts;
- run full ephemeral migration/rollback verification;
- verify exact production preconditions;
- apply only the approved WU4 schema/RPC cutover;
- keep all Journey application windows CLOSED;
- verify production RLS/grants/Vault/lifecycle/legacy compatibility;
- run post-cutover source/runtime verification;
- close P17-WU4 canon without activating the real recruitment pilot.
