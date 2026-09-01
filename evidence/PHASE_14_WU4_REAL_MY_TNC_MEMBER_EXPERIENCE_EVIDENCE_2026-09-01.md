# P14-WU4 Real My TNC Member Experience — Evidence

Date: 2026-09-01
Result: PASS

## Baseline
- Product main before WU4 fix: `4582e8e6866714711631549ac4ed51cfb2d0c10d`
- Docs main before WU4 closeout: `405a848d479dcb612a128e4670860820b6f97b4b`
- Starting Worker Version: `a47535cc-b6af-4b92-90d2-e6917f8051a4`

## Source audit
`src/components/community/community-experience-shell.tsx` was audited against WU4 requirements.

Verified before mutation:
- verified-session Magic Link flow exists;
- My TNC auto-runs `claim_my_journey_participations()`;
- truthful bilingual no-Journey copy exists;
- attendance unresolved / no-show / attended states are distinct;
- Memory eligibility depends on backend truth;
- Participant, Contributor, Host/Partner and Reflection states are derived from separate evidence-backed sources;
- logout clears personal UI state;
- display name is explicitly non-legal identity.

## Privacy defect and fix evidence
Production/source RLS audit showed:
- Memories are own-read only;
- Contributions, Relationships and Reflections intentionally permit broader staff read lanes;
- the current real Community account also holds a pre-existing CMS admin role.

My TNC therefore required an explicit own-user boundary independent of wider staff RLS.

Product fix added `.eq("user_id", nextUserId)` to My TNC reads for:
- `community_journey_memories`;
- `community_contributions`;
- `community_relationship_assignments`;
- `journey_reflections`.

Dedicated regression gate:
- `scripts/p14-wu4-my-tnc-own-data-qa.ts`
- workflow hook: `P14-WU4 My TNC own-data boundary QA`

## Product PR / CI
Product PR #34:
- title: `P14-WU4 My TNC own-data privacy boundary`
- head: `p14-wu4-my-tnc-own-data-boundary`
- head SHA: `dae4cccd5129c821a8d08bf40f44efb8761a7973`
- merged: PASS
- merge SHA: `6c36d0e9035ec583ca9b3bd67300d4d3c20f1b9d`
- files changed: 3
- migration: none

CI:
- PR CI run #156: PASS
- post-merge main CI run #157: PASS
- dedicated WU4 ownership QA: PASS
- build: PASS
- typecheck: PASS
- existing P9/P11/P12 gates: PASS
- Cloudflare dry-run: PASS

## Production deploy evidence
Controlled Cloudflare deploy output reported:
- assets upload: success, 52 files
- Worker: `tramnucuoi`
- deploy: success
- production Worker Version ID: `c010f061-3ff2-4ffe-93af-ccd6263ae392`

Product main at final verification remained:
`6c36d0e9035ec583ca9b3bd67300d4d3c20f1b9d`.

## Production Owner Review — desktop
### VI signed-in baseline
PASS:
- verified Community account recognized;
- no CMS/Admin controls shown in My TNC;
- summary rendered 0 Journey / 0 Memory / 0 Contribution / 0 Host-Partner relationship;
- no-Journey state rendered truthfully.

### Display-name persistence
PASS:
- display name saved successfully;
- UI immediately personalized the greeting;
- database count changed from 0 to 1 profile with non-blank display name;
- refresh preserved the saved name.

PII values are intentionally omitted from this evidence document.

### VI ↔ EN
PASS:
- authenticated session persisted;
- saved display name persisted;
- bilingual zero-state semantics remained equivalent;
- returning to VI preserved the same account state.

Observed non-blocking presentation flicker:
- route remount briefly renders generic greeting while profile data rehydrates;
- final session/profile data is correct;
- tracked as Phase 15 UX polish.

### Logout / reload / re-login
PASS:
- logout returned to signed-out Magic Link surface;
- reload remained signed out;
- new Magic Link login created a fresh authenticated session;
- saved display name persisted across the fresh session;
- truthful zero-state remained unchanged.

## Production Owner Review — mobile
PASS on iPhone/Safari for:
- signed-out VI Community/Auth surface;
- Magic Link request state;
- signed-in VI My TNC;
- signed-in EN My TNC;
- identity card stacking;
- large editorial typography responsiveness;
- zero-state readability;
- no visible horizontal overflow or broken layout attributable to the app.

Browser bottom chrome partially covered viewport content in screenshots; this is Safari UI, not app layout failure.

## Final production truth postflight
Final read-only production audit:
- Auth users: 1
- profiles: 1
- profiles with display name: 1
- active participant links: 0
- claim requests: 39
- claim rows with any non-zero result: 0
- Memories: 0
- active Contributions: 0
- Reflections: 0
- verified Community relationships: 0
- CMS roles: 1
- pilot attendance unresolved: 1
- pilot attendance resolved: 0

The claim-request count increased during repeated Owner Review route mounts/reloads/language switches, but all requests remained zero-result. No participant link or downstream Community fact was created.

## Pilot truth
Pilot Journey `19539f36-3ed4-4a22-96b9-c8a9b73c5283`:
- event date: 2026-09-11
- confirmed participant remains pre-event;
- attendance remains `NULL` / unresolved;
- no no-show inferred;
- no attended Memory inferred.

## Security result
PASS.

No WU4 DDL/RLS change was introduced. The product-query fix adds a stricter own-user boundary while preserving broader staff permissions for staff tools.

Supabase Security Advisor retained only the pre-existing project-level leaked-password-protection warning, unrelated to this Magic Link member lane. No new WU4 privacy/RLS blocker remained.

## Lovable sync observation
During production work, the Lovable editor/source history was found out of sync with canonical GitHub main. WU4 production deploy did not depend on Lovable Publish.

A safety archive branch was created in the product GitHub repository:
`archive/lovable-editor-2026-09-01`
pointing to the preserved Lovable editor snapshot `0a0ad617639dffef4468ad52672739370f3f6d44`.

This infrastructure sync issue is separate from WU4 correctness and should be repaired independently rather than by rebuilding My TNC from stale Lovable source.

## Final result
PASS — real account, real session lifecycle, truthful no-Journey lane, own-data privacy boundary, desktop/mobile VI/EN runtime, no fake Community data, and successful production deployment.
