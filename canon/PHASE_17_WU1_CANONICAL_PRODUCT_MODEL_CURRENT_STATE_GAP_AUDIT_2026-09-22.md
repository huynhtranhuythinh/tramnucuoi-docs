# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU1 — CANONICAL PRODUCT MODEL & CURRENT-STATE GAP AUDIT

Date: 2026-09-22  
Status: **AUDIT BASELINE COMPLETE / EVIDENCE-BOUNDED / NOT A RUNTIME RELEASE PASS**  
Product mutation: **NONE**  
Production database mutation: **NONE**  
Feature-flag mutation: **NONE**  
Cloudflare deploy: **NONE**  
Public recruitment: **HOLD**

## 1. Purpose

P17-WU1 compares the current product source, production database and evidence-backed production runtime against:

`canon/PRODUCT_REASSESSMENT_JOURNEY_OPERATING_MODEL_CANON_2026-09-22.md`

The Phase 17 product thesis remains:

> TRẠM NỤ CƯỜI is a Journey-Based Social & Operating Platform.  
> Hành Trình / Journey is the primary operational, participation and social object.

This audit does **not** authorize build work. It establishes what can be reused, what must be recomposed, what needs controlled modification, and what must remain untouched.

## 2. Evidence boundary

This audit directly inspected:

- current GitHub `main`;
- Product Reassessment canon and document index;
- real source code paths for Journey, Community, My TNC, registration, volunteer recruitment, admin and feature gates;
- production Supabase project `iwiqprhoohkxvjyxojto`;
- production schema, migrations, RLS/policies, relevant functions/triggers, Journey rows and non-PII aggregate operational counts;
- indexed public production pages available through web retrieval;
- prior canonical production/runtime evidence.

This environment could **not** directly obtain a live authenticated browser session against `/cong-dong`, `/en/community` or `/journeys/*`, and has no connected Cloudflare control plane. Those runtime surfaces are therefore explicitly classified **UNKNOWN / NEED EVIDENCE** where appropriate.

No unknown is silently converted into PASS.

---

# 3. CURRENT CANONICAL TRUTH

## 3.1 Product canon

Primary Phase 17 product canon:

`canon/PRODUCT_REASSESSMENT_JOURNEY_OPERATING_MODEL_CANON_2026-09-22.md`

Creation commit:

`c6587854f7f417400ed3687bd11be1bd1d154d44`

Index update commit:

`53c73af353e53eb6b76ec5f8d36ebbc22a81e916`

Document index confirms:

- Product Review / Reassessment: COMPLETE;
- Product model: LOCKED;
- P16-WU10: SUSPENDED / PRODUCT REBASE HOLD;
- Phase 17 roadmap direction: canonical;
- build authorization: not granted at WU1 start.

## 3.2 Current product GitHub main

Current product repository:

`huynhtranhuythinh/tramnucuoi`

Current `main` HEAD at corrected audit time:

`dc75bf19c15fdf39cf1ad95fdf650b1453ac97f3`

Commit date:

2026-09-15

### Source-lineage correction

The initial WU1 audit incorrectly inferred the repository HEAD from a recent-commit search. Direct inspection of `refs/heads/main` shows the actual main head is `dc75bf19…`.

The canonical P16-WU10A source closeout recorded:

`dadff1a2e2ffdaaa5b5a441660133178b9504117`

GitHub comparison against the actual current main is clean:

- status: **ahead**;
- ahead by 19 commits;
- behind by 0;
- merge base = `dadff1a2e2ffdaaa5b5a441660133178b9504117`.

Therefore there is **no P16-WU10A → current-main lineage divergence**.

### Production-applied migration/source correction

The initial WU1 audit also searched the wrong migration root for WU10B. Exact production-applied source is present on current main under `database/migrations/`:

- `0052_p16_wu10b_volunteer_application_team_assignment.sql`
  - blob `e00f49b648cbeae074c01f2b628404536de6dcd6`;
- `0053_p16_wu10b_full_identity_vault.sql`
  - blob `94d2e0ec598678bf2733583d9d66db835bd1fe9a`.

Matching rollback files and WU10B source/DB QA are also present. P16-WU10B implementation files on current main are byte-identical to the verified HF1 branch artifacts inspected during Gate 0.

Therefore current GitHub main **does contain the canonical WU10B migration source needed to reconstruct that production delta**.

### Generated source / QA inheritance drift

`src/integrations/supabase/types.ts` on current main is materially stale. It exposes only an early schema subset and omits current Journey/application/social tables present in production.

The committed `src/routeTree.gen.ts` is also stale relative to route source, but this is an expected generated-artifact condition: repository CI explicitly runs `bun run build` before `typecheck` so TanStack Router regenerates the route registry. It is not independently a source-of-truth blocker.

A real CI inheritance gap remains: WU10B has its own branch-specific workflow and passed there, while generic main CI does not yet rerun WU10B source/DB QA on future main changes.

Phase 17 Gate 0 should therefore canonicalize generated Supabase types and carry WU10B QA into the inherited main CI gate.

## 3.3 Production Supabase truth

Project:

`iwiqprhoohkxvjyxojto`

State at audit:

**ACTIVE_HEALTHY**

Production migrations include the full Journey / Community chain through:

- P12 identity, claim, Memory, Reflection, Contribution, relationships;
- P14 attendance date authority;
- P15 English project titles;
- P16 0042–0051;
- 20260914093429 / 0052 volunteer application & team assignment;
- 20260915092902 / 0053 full identity vault.

### Current Journey truth

Production contains five Journey rows.

Most relevant current rows:

#### A. `trung-thu-em-va-cay-2026`

- status: `registration_open`;
- application mode: `volunteer_v1`;
- structured dates: 2026-09-19 → 2026-09-20;
- public story text references 18–20/09/2026;
- Project: none;
- applications: 1;
- application status: confirmed;
- assigned application: 1;
- participants: 1;
- attendance unresolved: 1;
- verified no-show: 0;
- verified attended: 0.

#### B. `tram-com-chay-yeu-thuong-doi-nu-cuoi-mung-1-thang-8-2026`

- status: `registration_open`;
- structured date: 2026-09-14;
- content schedule references a different date in prior pilot copy;
- applications: 8;
- confirmed: 2;
- rejected: 1;
- submitted: 5;
- participants: 2;
- attendance unresolved: 2;
- verified no-show: 0;
- verified attended: 0.

### Current Memory truth

`community_journey_memories` currently has two rows, both for the older pilot Journey:

- `attendance_state = unresolved`;
- `attended_party_size = NULL`;
- `memory_eligible = false`.

No Memory may be presented as verified attended truth from those rows.

### Current closeout truth

`journey_closeout_reviews` exists, but currently has **zero rows**.

Existing closeout schema currently covers:

- attendance gap note;
- Field Update reviewed;
- documentary reviewed;
- final narrative reviewed;
- pending/passed review status;
- reviewer/audit fields.

It does **not** yet model the full Phase 17 closeout canon including beneficiary reconciliation, resources/donations, distributed vs surplus, financial summary, partners, unresolved issues and official Memory/public-impact release.

## 3.4 Current registration / participant truth model

Existing application workflow enum:

`submitted → reviewing → accepted → confirmed`, with `rejected`.

The database prevents invalid transitions.

`journey_participants` remains a separate table from `journey_applications`.

Attendance fields remain separate:

- `attended_party_size = NULL` = unresolved;
- `attended_party_size = 0` = verified no-show;
- `attended_party_size > 0` = verified attended;
- attendance timestamp and actor are required together with the value.

This is strongly aligned with the Phase 17 truth invariants.

## 3.5 Volunteer pilot truth

Current volunteer implementation is a pilot-specific extension on `journey_applications`.

It stores:

- birth date;
- identity-document type;
- last four characters;
- emergency contact;
- health notes;
- one `preferred_team`;
- one `assigned_team`;
- applicant question;
- versioned operational privacy consent;
- assignment audit fields;
- identity-document expiry;
- Vault secret reference.

The full identity-document number is moved into Supabase Vault before row insertion completes; the staging input is nulled and constrained to remain NULL.

The public volunteer form explicitly states that sensitive data is for government/authority registration, Journey safety and operations and is not Community / Memory / attendance evidence.

### Important staffing-model gap

Volunteer teams are hard-coded in source and database constraints:

- Media;
- Hậu cần / Nấu ăn;
- Cắt tóc;
- Hoạt náo;
- Truyền thông;
- Điều phối;
- Hỗ trợ chung.

This conflicts with the new product canon where Departments / Teams / Responsibilities are Journey-configurable operational objects rather than fixed global product roles.

## 3.6 Identity / claim / privacy foundation

Production contains:

- `community_participant_links`;
- `community_claim_requests`;
- `community_journey_memories`;
- `social_identities`;
- Journey social presence;
- relationship consent;
- shared-experience edges;
- social blocks / reports / moderation controls;
- Journey interactions;
- social notifications;
- Reflection source/publication separation.

Verified-email participant claim remains distinct from attendance.

Social identity defaults remain private / opt-in.

Shared-experience edges are derived from verified attendance and do not derive from social presence.

These foundations remain compatible with the Phase 17 canon.

## 3.7 Security findings

### Identity-document reveal RPC

`public.tnc_reveal_volunteer_identity_document(uuid)` is a `SECURITY DEFINER` RPC with EXECUTE exposed to `authenticated`.

Supabase Advisor flags that callable surface.

Direct function inspection confirms the function itself immediately requires:

- authenticated user; and
- `private.has_role('admin')`.

Therefore this audit does **not** classify it as a demonstrated ordinary-user data disclosure.

However, because the function reveals highly sensitive identity data, the callable surface should be tightened before public recruitment reactivation; preferably sensitive reveal should move behind a narrower server/admin boundary rather than relying on a browser-callable authenticated RPC with an internal role guard.

### Leaked password protection

Supabase security guidance continues to report leaked-password protection disabled.

This is pre-existing security hardening debt. It should not be bundled into WU2 IA work, but should be resolved before a broader public-account/recruitment activation gate.

## 3.8 Feature flags

Source defines fail-closed flags for:

- Community Auth;
- Journey Community Room;
- Social Safety Hardening;
- Journey Interaction v1;
- Shared Experience Graph;
- Social Notifications v1;
- Journey Social Continuity.

The last directly verified P16 runtime-boundary evidence before WU10A recorded the P16 social feature flags OFF.

P16-WU10A later reached source-complete / CI-pass but its canonical closeout explicitly stated live Cloudflare deployment evidence was still pending.

Current GitHub includes a later 2026-09-16 commit titled around P16 production inspection/deploy, but this audit has no Cloudflare control-plane evidence proving the exact active production flag set on 2026-09-22.

Therefore:

**Current P16 runtime feature-flag state = UNKNOWN / NEED CLOUDFLARE RUNTIME EVIDENCE.**

Do not infer ON merely because source exists.

## 3.9 Public runtime truth

Indexed public production currently presents:

- `/hanh-trinh` as **Nhật ký thực địa / Field Journal**, not the operational Journey index;
- English `/en/journey` similarly as Field Notes;
- homepage “Journey” content as editorial field notes;
- public Get Involved primarily as collaboration / ecosystem information.

This conflicts with the Phase 17 public IA direction where **Hành Trình / Journey** is the primary participation and discovery object.

The public Privacy Policy still states that the website does not operate a public user-account system, which is stale relative to the production My TNC/Auth work completed after the policy date.

Operational `/journeys/*` and authenticated Community runtime could not be independently fetched by the current audit tools, so their exact live behavior remains UNKNOWN.

---

# 4. REUSE MAP

## KEEP

### Project ↔ Journey relationship

Current Journey supports nullable `project_id`.

This matches canon:

- Project = long-term context;
- Journey may belong to Project;
- not every Journey requires Project.

### Journey operational truth separation

Keep:

- applications separate from participants;
- participants separate from attendance;
- nullable attendance semantics;
- attendance actor/timestamp evidence;
- claim separate from attendance;
- Memory eligibility separate from Memory row existence.

### Evidence / Memory / shared experience

Keep:

- `community_participant_links`;
- `community_journey_memories`;
- Reflection source vs publication projection;
- verified Contributions;
- `journey_shared_experience_edges`;
- mutual relationship consent;
- social block/report suppression;
- revocation instead of rewriting verified operational history.

### Social privacy foundation

Keep:

- opt-in social identity;
- private defaults;
- Journey-scoped social presence;
- consent audit events;
- no follower/friend/DM/popularity model;
- Journey Question / Reply / Appreciation foundation.

### Security / anti-abuse foundation

Keep:

- Journey registration abuse protection;
- replay/contact-window dedupe;
- Turnstile escalation architecture;
- activation gates;
- fail-closed feature gates;
- RLS-first private data model;
- identity-document Vault storage pattern.

## DO NOT TOUCH without explicit evidence

- attendance NULL / 0 / >0 semantics;
- claim/participant separation;
- Memory evidence boundary;
- shared-experience evidence derivation;
- social/private operational truth separation;
- block/report privacy invariants;
- identity Vault encryption-at-rest pattern;
- existing registration abuse controls.

---

# 5. GAP MAP

| Domain | Classification | Current truth | Phase 17 gap |
| --- | --- | --- | --- |
| Journey lifecycle | **MODIFY** | `draft / registration_open / preparing / completed / archived` conflates recruitment and lifecycle | Separate Journey phase from application window and closeout/Memory transition. Past Journeys can remain registration-open indefinitely today. |
| Public Journey | **RECOMPOSE + MODIFY** | Operational Journey exists at `/journeys`, while public `/hanh-trinh` is Field Journal | Make Hành Trình the public discovery object; preserve Field Journal under a different editorial route with redirects. |
| Applications | **KEEP foundation / MODIFY workflow** | Strong application workflow and abuse controls | Add Waitlist semantics and stop using pilot-specific application mode as long-term staffing architecture. |
| Participants | **KEEP truth / MODIFY product projection** | participant != application, attendance separate | Add Journey Role without overloading participant_type; create safe participant-facing projection. |
| Teams / Departments | **MODIFY** | one hard-coded preferred_team + assigned_team | Replace fixed team vocabulary with Journey-configurable Department / Team / responsibility / assignment objects. |
| Event Management | **RECOMPOSE + EXTEND** | Admin Journey page contains several managers | Recompose into one Journey Control Center; add Setup, People, Teams, Plan, Resources, Comms, Day-of, Closeout without turning into ERP/Jira. |
| Participant Area | **MISSING PRODUCT LAYER / MODIFY** | Global Community/My TNC exists; no safe operational participant workspace | Add Personal Journey Workspace inside the Journey with status, role, Department, Team Lead, assignment, checklist, schedule, meeting point, updates and Community. |
| Community | **KEEP foundation / RECOMPOSE** | Journey Room + question/reply/appreciation + global Community entry | Keep Journey-scoped model; remove global-Community-as-primary mental model. Later add governed participant publishing window without global composer. |
| Donation | **MODIFY / EXTEND** | verified Contribution exists; no Need→Pledge→Received→Distributed model | Build Journey resource/donation operations later; do not build payment custody by default. |
| Attendance | **KEEP / DO NOT TOUCH** | strong nullable evidence model | Recompose Day-of UX later; preserve truth model. |
| Closeout | **MODIFY / EXTEND** | narrow closeout review exists; zero current rows | Expand to canonical reconciliation and manual Closeout; Journey must not auto-complete from date. |
| Memory | **KEEP foundation / RECOMPOSE** | evidence-backed Memory projection and Reflection publication separation | Make Memory the post-closeout Journey mode; do not infer from elapsed date. |
| My TNC | **KEEP foundation / RECOMPOSE** | private archive/history is strong | Keep as personal continuity layer; stop making global Community the path users must understand to reach their Journey work. |
| Public Trust | **MODIFY** | ecosystem storytelling is strong; operational Journey/results are not primary; Privacy copy stale | Rebase public IA around who/what/results/trust/participation; update privacy truth before recruitment. |
| VI / EN | **KEEP architecture / RECOMPOSE editorial priorities** | reciprocal locale routes/copy exist | One truth model; VI and EN may emphasize different information without literal 1:1 duplication. |
| Admin | **RECOMPOSE** | capable but fragmented button-based managers in one page | Journey Control Center IA around one Journey; preserve existing domain managers where sound. |
| Source integrity | **CRITICAL MODIFY / PRECONDITION** | GitHub lineage diverged; migrations 0052/0053 missing; generated DB types stale; route tree stale | Canonicalize source before new Phase 17 mutation. |

---

# 6. KEY PRODUCT / IMPLEMENTATION FINDINGS

## 6.1 The main problem is not a weak database foundation

The current system already has strong evidence boundaries.

A broad rewrite would unnecessarily endanger:

- attendance truth;
- claims;
- Memory;
- shared-experience evidence;
- social consent;
- RLS;
- registration abuse controls.

Phase 17 should **recompose around Journey**, not rebuild the platform from zero.

## 6.2 Current IA contains two competing meanings of “Hành Trình”

Current source/runtime uses:

- `/hanh-trinh` = editorial Field Journal;
- `/journeys` = operational Journey.

This is incompatible with Owner-locked terminology.

A specific implementation defect also exists in `AuthenticatedJourneySocialHome`:

- Journey links are constructed as `/hanh-trinh/{slug}`;
- operational Journey detail is actually `/journeys/{slug}`.

The Social Home discovery CTA likewise points to the Field Journal route.

This is direct evidence that WU2 must establish one canonical route/IA authority rather than patching individual links.

## 6.3 Registration state is incorrectly carrying Journey lifecycle responsibility

Public application RLS currently allows submission whenever parent Journey status is `registration_open`.

There is no independent application-window state.

Because past Journeys remain `registration_open`, the database still considers them application-open.

This is a lifecycle-model gap, not simply a stale-content issue.

## 6.4 Participant Workspace cannot be built safely from current application RLS

`journey_applications` and `journey_participants` are admin-only for direct read.

That is appropriate for sensitive operations, especially volunteer identity/health data.

Therefore Phase 17 should **not** solve Participant Area by loosening application-table RLS.

Instead, create a narrow participant-facing operational projection/API containing only safe fields:

- participation state;
- Journey Role;
- Department/Team;
- Team Lead display information where permitted;
- assignments;
- checklist;
- schedule/meeting point;
- official updates.

Sensitive application data remains private operations only.

## 6.5 Journey Role does not exist as the new canon requires

Current `participant_type` values are:

- individual;
- family;
- contributor;
- partner_rep.

These are not the Owner-locked Journey Roles:

1. Ban tổ chức;
2. Tình nguyện viên;
3. Bản địa.

Do not reinterpret `participant_type` as Journey Role.

Add a separate role model later, preserving old values for compatibility.

## 6.6 Volunteer staffing is hard-coded pilot data

The fixed `VOLUNTEER_TEAMS` source list and matching DB CHECK constraints were reasonable for a controlled P16 pilot.

They are not acceptable as the long-term Journey Operating System.

The correct future model is:

Journey staffing need → Department → capacity/requirements → applicant preferences → BTC assignment → optional Team/Lead → responsibility/task.

## 6.7 Community is closer to canon than the public IA suggests

The social foundation already avoids:

- global posting;
- follower graph;
- DM;
- public popularity counts;
- attendance inference from social activity.

This should be preserved.

The rebase is mainly:

- Journey-first navigation;
- approved-participant publishing eligibility;
- Publishing Window vs Interaction Window;
- public/participant/official publication separation;
- Participant Workspace integration.

---

# 7. RISK MAP

## R1 — Generated schema / inherited QA drift — HIGH

Canonical WU10B migrations are present on GitHub main, but generated Supabase TypeScript types do not reflect the production schema and generic main CI does not yet inherit the WU10B QA suite.

Impact:

- future Phase 17 code may be authored against stale compile-time database contracts;
- later main changes could regress WU10B privacy/identity behavior without rerunning its dedicated tests;
- source/database drift becomes harder to detect early.

Mitigation:

**WU2 Gate 0: regenerate production-backed types and carry WU10B source/DB QA into generic CI before lifecycle/schema work.**

## R2 — HEAD/path verification methodology — CLOSED FINDING

The initial WU1 audit used recent-commit search as a HEAD proxy and searched `db/migrations` instead of the WU10B `database/migrations` root.

Corrected evidence shows:

- actual main = `dc75bf19…`;
- P16-WU10A is an ancestor;
- WU10B 0052/0053 source and rollbacks are present.

Mitigation:

Future canonical audits must read the branch ref directly and inspect repository-owned path conventions before declaring source absence.

## R3 — Lifecycle/public recruitment drift — HIGH

Past Journey rows remain `registration_open`.

Impact:

- public/database registration gate can remain open after real Journey date;
- lifecycle presentation can mislead users;
- new pilot recruitment could accidentally reopen wrong Journey.

Mitigation:

keep public recruitment HOLD; WU2 separates Journey phase from application window.

## R4 — Route / IA ambiguity — HIGH

`/hanh-trinh` means Field Journal while operational Journey uses `/journeys`.

Impact:

- broken navigation;
- social home links to wrong object type;
- SEO/public understanding conflict.

Mitigation:

WU2 owns canonical route migration and redirect compatibility.

## R5 — Sensitive volunteer data exposure surface — MEDIUM/HIGH

Full ID is correctly Vault-protected, but reveal RPC is authenticated-callable and relies on an internal admin check.

Mitigation:

before public recruitment reactivation, move reveal behind a narrower server/admin boundary and retest Advisor/RLS.

## R6 — Privacy notice drift — HIGH before public recruitment

Public Privacy Policy still describes no public account system.

Volunteer flow now collects higher-sensitivity operational data under specific consent.

Mitigation:

update privacy/account/recruitment notice before WU11 public activation.

## R7 — Hard-coded staffing vocabulary — MEDIUM

Current team model cannot generalize per Journey.

Mitigation:

preserve pilot records, introduce dynamic Department/Team model later, and migrate mappings without rewriting historical assignment truth.

## R8 — UX regression during recomposition — MEDIUM/HIGH

Many sound pieces already exist.

Mitigation:

reuse domain components and truth helpers; change composition/navigation before replacing logic.

## R9 — Closeout insufficiency — MEDIUM/HIGH

Current closeout is narrower than Phase 17 canon.

Mitigation:

do not call past-date Journey “complete” from elapsed time; expand canonical closeout in WU9.

## R10 — Future Supabase migration grants — MEDIUM

Future Supabase Data API behavior is moving toward explicit grants for newly created public tables.

Mitigation:

Phase 17 migrations should explicitly declare grants/RLS rather than relying on implicit defaults.

---

# 8. RECOMMENDED P17 IMPLEMENTATION ARCHITECTURE

## 8.1 Architecture principle

Use one Journey aggregate with separate governed layers rather than one giant table and rather than multiple parallel products.

### Layer A — Journey Core

Owns:

- title/story;
- location/date;
- Project link;
- lifecycle phase;
- public discovery metadata.

### Layer B — Lifecycle & Windows

Owns separately:

- Journey phase;
- application/recruitment state;
- Publishing Window;
- Interaction Window;
- public publication state.

Do not make one status field represent all four.

### Layer C — Participation

Preserve:

`Application → Approval → Participant → Attendance`

Add Journey Role separately.

### Layer D — Staffing & Assignment

Journey-configurable:

- Department;
- staffing need;
- preference;
- assignment;
- Team;
- Team Lead;
- responsibility/checklist.

Do not encode skill or Department as Journey Role.

### Layer E — Journey Operations

Control Center composes:

- Setup;
- People;
- Teams;
- Plan/Runbook;
- Resources;
- Communications;
- Day-of;
- Attendance/Evidence;
- Closeout.

This remains Journey-specific, not a general ERP/project-management system.

### Layer F — Social & Publication

Reuse:

- Social Identity;
- Journey Presence;
- interactions;
- safety;
- relationship consent;
- verified shared-experience edge.

Add later:

- participant-authored Journey content;
- governed publishing eligibility/window;
- participant-only/public/official publication separation.

### Layer G — Evidence / Closeout / Memory

Reuse evidence/attendance/Memory sources.

Expand manual closeout.

Only after reconciliation may verified operational truth become Public Impact / Memory Mode.

### Layer H — My TNC projections

My TNC stays a personal projection, not an alternative source of truth.

It should consume governed Journey participation/Memory/Contribution/relationship projections.

## 8.2 Prefer projections over opening sensitive tables

Participant Workspace should read a dedicated safe projection or governed RPC, not raw `journey_applications`.

Public Journey should read public-safe projections.

Admin operations may access private operational source under existing role boundaries.

## 8.3 Preserve historical compatibility

Do not rewrite historical pilot truth to fit new labels.

Use compatibility mappings and additive migration where needed.

Examples:

- old `assigned_team` becomes legacy assignment input to the new Department mapping;
- old `participant_type` remains historical metadata, not Journey Role;
- existing completed/archive records remain valid unless explicit reconciliation says otherwise.

---

# 9. RECOMMENDED P17-WU2 SCOPE
# JOURNEY LIFECYCLE & INFORMATION ARCHITECTURE

WU2 should remain narrowly focused on **canonical Journey identity, lifecycle and route/IA authority**.

It should not build Event Management, Departments, Donation, Community publishing or full Participant Workspace yet.

## WU2 Gate 0 — Source canonicalization

Before product mutation:

1. verify current `main` directly from `refs/heads/main`;
2. verify exact 0052/0053 migration and rollback blobs against the WU10B verified branch and production catalog;
3. regenerate/verify Supabase TypeScript types against production schema;
4. carry WU10B source and ephemeral DB QA into generic main CI;
5. rely on the existing build-before-typecheck contract to regenerate TanStack route tree;
6. establish an exact new Phase 17 baseline SHA;
7. run inherited CI/QA before changing product semantics.

No production DB mutation is required for this source-canonicalization gate.

## WU2.1 — Canonical public route ownership

Recommended target:

- VI `/hanh-trinh` = operational Journey index;
- VI `/hanh-trinh/:slug` = operational Journey detail;
- EN `/en/journeys` or one chosen canonical reciprocal route = operational Journey;
- current Field Journal moves to an explicit editorial route such as `/nhat-ky` / `/en/journal`.

WU2 must define redirects so old indexed editorial links are not silently broken.

The exact EN route should be locked once, not duplicated across `/journey` and `/journeys`.

## WU2.2 — Separate lifecycle from recruitment

Current `registration_open` should stop being both:

- Journey phase; and
- application authorization.

Recommended conceptual split:

### Journey phase

- draft;
- upcoming;
- active;
- closeout_pending;
- memory;
- archived.

### Application state

- closed;
- open;
- paused.

Optional open/close timestamps may be added only if operations needs them; manual authority remains available.

Elapsed dates must not auto-create attendance, closeout, impact or Memory truth.

## WU2.3 — Closeout transition contract

Define:

- active → closeout_pending after operations;
- closeout_pending remains unresolved until BTC reconciliation passes;
- memory only after manual closeout;
- archived remains an administrative archival state, not a substitute for Memory.

Do not auto-transition based solely on `end_date`.

## WU2.4 — Canonical Journey page IA

Before Journey:

- objective;
- beneficiary/local context;
- date/location;
- activities;
- participant/TNV needs;
- resource needs;
- partners;
- registration/participation CTA.

Approved participant:

the same Journey page later opens Personal Journey Workspace; no separate Volunteer Dashboard route.

Active Journey:

prioritize operational instructions, official updates and Journey-scoped Community.

Post-closeout:

prioritize verified result, official story/media, distributed resources, impact/financial summary where appropriate, partners and Memory.

WU2 should define slots/regions; later WUs implement domain content.

## WU2.5 — Public navigation rebase

Target public top-level direction:

- Trang chủ;
- TNC;
- Dự án;
- Hành Trình;
- Tác động;
- Đồng hành;
- My TNC when authenticated.

“Cộng đồng” should not be required as a primary global destination for understanding the product.

Community remains a Journey capability.

## WU2.6 — Data authority for dates and critical operational facts

Structured Journey fields are authoritative for:

- date;
- location;
- lifecycle;
- application state;
- capacity.

Editorial story text may describe context but should not independently duplicate authoritative operational values without validation.

WU2 should eliminate the current structured-date vs story-copy drift pattern.

## WU2 exit evidence

WU2 may be declared complete only after evidence demonstrates:

- canonical source can reconstruct current production schema;
- one unambiguous Journey route family per locale;
- Field Journal no longer competes for the Hành Trình product name;
- lifecycle and application state are separately modeled;
- old links have compatibility redirects;
- past registration-open production Journeys have an explicit reconciliation plan;
- no attendance/Memory/claim/security invariant regressed;
- no public recruitment was activated as a side effect;
- VI/EN route and lifecycle parity verified;
- mobile Journey index/detail smoke verified.

---

# 10. WU1 DECISION

## Reuse decision

**DO NOT REWRITE THE FOUNDATION.**

The existing system has valuable, correctly separated truth and privacy layers.

Phase 17 should:

> **preserve evidence/security foundations, canonicalize source truth, then recompose product experience around Journey.**

## Immediate blocker before WU2 mutation

The highest-priority engineering blocker is:

> **GitHub main contains the required WU10B migration source, but its generated Supabase type contract and inherited CI coverage are stale.**

These must be canonicalized before new Phase 17 schema work.

## Production operational blocker

Public recruitment remains:

**HOLD**

because:

- past Journey statuses are still application-open;
- current runtime flag state is not directly verified;
- Privacy copy is stale;
- volunteer sensitive-data reveal surface needs tightening before broad activation;
- P17 lifecycle/IA rebase has not yet been implemented.

## P17-WU1 closeout state

**P17-WU1 AUDIT BASELINE: COMPLETE**

This is **not** a production release PASS and does not authorize WU2 implementation automatically.

Known runtime uncertainties remain explicitly recorded and must be resolved at the appropriate WU2/WU11 evidence gates.

No product code, database, production flag or deployment was changed by P17-WU1.
