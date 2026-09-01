# PHASE 14 — WU4 REAL MY TNC MEMBER EXPERIENCE

Date: 2026-09-01
Status: COMPLETE / PASS

## Objective
Validate the real production My TNC member experience end to end with a real verified account, while preserving strict truth boundaries between account identity, Journey claim, attendance, Memory, Contribution, Reflection, Community relationship, and CMS authorization.

WU4 is validation-first. It does not manufacture Journey history or expand Community truth merely to fill the interface.

## Canonical result
P14-WU4 passes through the **truthful no-Journey member lane**.

The current verified Community account is real and operational, but it still has no evidence-matched confirmed pilot participation. My TNC therefore correctly presents a verified account with a saved display name and zero Journey/Memory/Contribution/Relationship facts.

The production experience was validated across desktop and mobile, VI and EN, including save/reload, language switching, logout, reload, and re-login.

## Product repository
- Repo: `huynhtranhuythinh/tramnucuoi`
- Starting product main: `4582e8e6866714711631549ac4ed51cfb2d0c10d`
- Ending product main: `6c36d0e9035ec583ca9b3bd67300d4d3c20f1b9d`
- Product PR: #34 — `P14-WU4 My TNC own-data privacy boundary`
- PR head SHA: `dae4cccd5129c821a8d08bf40f44efb8761a7973`
- Merge SHA: `6c36d0e9035ec583ca9b3bd67300d4d3c20f1b9d`
- PR CI: run #156 PASS
- Post-merge main CI: run #157 PASS
- Database migration: none
- RLS/schema mutation: none

## Privacy boundary defect found and fixed
Source audit identified a real privacy boundary defect for accounts that also hold CMS staff access.

Several Community tables intentionally allow broader staff reads through RLS. My TNC originally relied on those policies without adding an explicit current-user filter for all personal queries. Once real member data existed, a staff account could therefore have rendered another member's Contribution, Relationship, or Reflection rows inside its personal My TNC surface.

WU4 fixed this at the product-query layer without weakening or changing staff RLS:
- `community_journey_memories` personal read now filters `user_id = signed-in user`;
- `community_contributions` personal read now filters `user_id = signed-in user`;
- `community_relationship_assignments` personal read now filters `user_id = signed-in user`;
- `journey_reflections` personal read now filters `user_id = signed-in user`.

A dedicated source regression gate was added:
- `scripts/p14-wu4-my-tnc-own-data-qa.ts`
- CI workflow now runs the WU4 ownership-boundary check.

This is defense in depth: CMS staff permissions remain available to staff tools, while My TNC always reads only the signed-in account's own personal rows.

## Production deployment
The WU4 product main was deployed directly from the canonical GitHub/local repository through the controlled Cloudflare deployment path.

Production Worker:
- Worker: `tramnucuoi`
- deployed source main: `6c36d0e9035ec583ca9b3bd67300d4d3c20f1b9d`
- Worker Version ID: `c010f061-3ff2-4ffe-93af-ccd6263ae392`

Lovable Publish was not used as production evidence. During WU4, the Lovable editor/project source was found to be out of sync with canonical GitHub history. No Lovable mutation was used to bypass this. Canonical production truth remains GitHub source + Supabase + Cloudflare Worker.

## Real account identity validation
Production Owner Review validated:
- verified email session resolves correctly;
- My TNC presents the account as a Community identity, not legal-identity verification;
- a real display name can be saved;
- display name persists after reload;
- display name persists across VI/EN navigation;
- display name persists after logout and a fresh Magic Link re-login;
- no CMS/Admin UI bleeds into My TNC despite the same real account holding a pre-existing CMS admin role.

The actual email and display-name values are intentionally excluded from canonical documentation.

## Truthful My TNC state
Final production postflight:
- Auth users: 1
- profiles: 1
- profiles with display name: 1
- active participant links: 0
- Community Memories: 0
- active Contributions: 0
- Reflections: 0
- verified Community relationships: 0
- CMS roles: 1 pre-existing role

The signed-in My TNC summary therefore truthfully remains:
- 0 linked Journey;
- 0 evidence-backed attended Memory;
- 0 active verified Contribution;
- 0 verified Host/partner assignment.

No fake Community record was introduced to make WU4 visually fuller.

## Pilot truth preservation
Pilot Journey:
- Journey ID: `19539f36-3ed4-4a22-96b9-c8a9b73c5283`
- event date: 2026-09-11
- production status remains pre-event / registration lane
- pilot attendance unresolved: 1
- pilot attendance resolved: 0

Therefore:
- confirmed registration is still not attendance;
- participant claim is still not attendance;
- attendance `NULL` remains unresolved;
- no Memory eligibility is inferred before real attendance evidence exists.

## Session lifecycle result
PASS.

Validated sequence:
1. signed-in production My TNC on desktop VI;
2. saved display name;
3. reload preserved saved name;
4. VI → EN preserved the authenticated account and saved name;
5. EN → VI preserved the same account state;
6. logout returned to the signed-out Magic Link surface;
7. reload remained signed out;
8. a newly requested Magic Link created a fresh session;
9. re-login restored the saved display name and truthful zero-state.

## Desktop and mobile runtime review
Desktop:
- VI signed-in: PASS
- EN signed-in: PASS
- truthful empty Journey state: PASS
- identity card / display-name behavior: PASS
- logout/re-login lifecycle: PASS

Mobile:
- signed-out VI entry surface: PASS
- Magic Link request surface: PASS
- signed-in VI My TNC: PASS
- signed-in EN My TNC: PASS
- responsive stacking and typography: PASS
- identity card and zero-state remain readable and truthful: PASS

Owner Review: PASS.

## Non-blocking UX / operational observations
Two observations are intentionally carried forward rather than reopening WU4:

### 1. VI/EN transition flicker
The language switch currently remounts the route. Session resolution completes before personal profile data, so a signed-in account may briefly see the generic `Welcome.` / `Chào anh/chị.` shell and an empty display-name field before the real profile hydrates.

This is presentation-state flicker only:
- session remains valid;
- no wrong personal data is persisted;
- final profile and truth state are correct.

Treat as Phase 15 UX polish: avoid rendering a personalized shell until identity/profile hydration is ready, or preserve hydrated state across locale navigation.

### 2. Repeated zero-result claim audit rows
During WU4 Owner Review, repeated route mounts/reloads/language switches caused the idempotent claim RPC to execute repeatedly. Final `community_claim_requests` count reached 39, with **zero non-zero claim results**.

This did not create participant links or Community truth facts. It is operational/audit-log noise, not a correctness failure. Future optimization may reduce redundant claim executions while preserving idempotency and evidence-backed reconciliation semantics.

## Security / RLS result
PASS.

WU4 introduced no schema/RLS mutation. The product fix narrows My TNC personal reads with explicit current-user ownership filters even where staff RLS is intentionally broader.

Supabase security review retained only the pre-existing project-level leaked-password-protection warning, unrelated to the passwordless Magic Link lane. No new WU4 RLS/privacy blocker remained after the source fix.

## Rollback
My TNC remains controlled by the existing activation mechanism.

Emergency rollback remains:
- rebuild/deploy with `VITE_APP_COMMUNITY_AUTH_ENABLED=false`.

WU4 added no database migration and therefore requires no database rollback.

## Gate to P14-WU5
WU4 proves that a real member can:
- sign in;
- maintain a non-legal display identity;
- see only their own My TNC data;
- receive truthful zero/eligible states;
- move between VI and EN;
- logout and re-login without stale personal state;
- use the experience on desktop and mobile.

This establishes the production member-experience baseline required before post-Journey Memory and Reflection operations.

## Result
**P14-WU4 — COMPLETE / PASS.**

Next: **P14-WU5 — Post-Journey Memory & Reflection Operations**.
