# PHASE 14 — WU2 CONTROLLED MY TNC ACTIVATION

Date: 2026-08-31
Status: COMPLETE / PASS

## Scope
Activate My TNC production Auth in a controlled, reversible release after P14-WU1 readiness passed, without changing Community truth semantics or CMS permissions.

## Product repository
- Repo: huynhtranhuythinh/tramnucuoi
- Source branch: p14-wu2-controlled-my-tnc-activation
- Product PR: #33
- PR head: fab265a767e98547b3b02aaf440e628f0053de6d
- Merged product main: 4582e8e6866714711631549ac4ed51cfb2d0c10d
- PR CI: #154 PASS
- Post-merge main CI: #155 PASS

## Production deployment
- Cloudflare Worker: tramnucuoi
- Activation build flag: VITE_APP_COMMUNITY_AUTH_ENABLED=true
- Fail-closed repository default remains: VITE_APP_COMMUNITY_AUTH_ENABLED=false
- Production Worker Version: a47535cc-b6af-4b92-90d2-e6917f8051a4
- No database migration
- No RLS change
- No Supabase schema mutation

## Controlled activation evidence
- `/cong-dong` signed-out Community Auth surface: PASS
- `/en/community` signed-out Community Auth surface: PASS
- Living Community public surface remains available: PASS
- Production My TNC UI Magic Link request from `/cong-dong`: PASS
- Auth `/otp`: 200
- Magic Link template VI/EN: PASS
- Magic Link verify from `/cong-dong`: 303
- Auth login event: PASS
- Auth `/user`: 200
- Signed-in My TNC surface: PASS
- Profile creation for the real verified Owner account: PASS
- Logout `/logout`: 204
- Re-open used one-time link: rejected as invalid/expired, PASS
- Session remains signed-out after logout: PASS

## Truth and permission postflight
For the verified Owner account after activation test:
- profiles: 1 real profile
- community_participant_links: 0
- community_journey_memories: 0
- community_contributions: 0
- journey_reflections: 0
- community_relationship_assignments: 0
- user_roles: 1 pre-existing admin role created 2026-08-22, not created by WU2

Therefore activation did not infer attendance, Memory, Contribution, Reflection, relationship, or CMS permission from authentication.

## Rollback
Rollback is a rebuild/deploy with `VITE_APP_COMMUNITY_AUTH_ENABLED=false` using the P14-WU2 rollback scripts added to the product repo. No DB rollback is required. Public Living Community remains independent.

## Canonical decision
P14-WU2 is COMPLETE / PASS.

My TNC production Auth is now ON under the controlled release. The next work unit is P14-WU3 — Real Participant Claim & Identity Reconciliation.
