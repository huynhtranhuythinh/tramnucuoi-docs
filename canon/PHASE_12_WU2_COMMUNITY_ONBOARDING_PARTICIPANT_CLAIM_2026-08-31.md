# PHASE 12 — COMMUNITY OS CONTINUATION
# P12-WU2 — COMMUNITY ACCOUNT ONBOARDING & PARTICIPANT CLAIM

Date: 2026-08-31
Status: **IMPLEMENTATION COMPLETE / PASS — ACTIVATION GATED**

## Objective

Create a public community-account path that can safely link a verified person to previously confirmed Journey participation without granting CMS privileges, exposing registration PII, rewriting operational participant snapshots, or inferring attendance/Memory/impact.

## Product boundary

Staff identity and community identity share Supabase Auth as the root identity system, but authorization remains separate:

- `/auth` remains staff-oriented;
- community routes are `/cong-dong` and `/en/community`;
- community accounts receive no `user_roles` privilege by default;
- possession of a verified email may establish ownership only for confirmed Journey records submitted with that same normalized email;
- identity claim is not attendance;
- identity claim is not Memory;
- identity claim is not impact evidence.

## Implemented flow

1. User enters the email used for Journey registration.
2. Supabase Auth verifies control of that email through the community sign-in flow.
3. Authenticated claim logic considers only:
   - confirmed Journey applications;
   - confirmed Journey participants;
   - normalized application email equal to the caller's verified Auth email;
   - operational `user_id` snapshots either NULL or already equal to the caller.
4. An active `community_participant_links` row records verified identity ownership.
5. Claim returns counts only; application PII and participant identifiers are not returned by the public RPC.
6. Repeat claim is idempotent.
7. A participant already actively linked to another identity fails closed.

## Source / CI

Initial WU2 PR: `#19`

Initial WU2 merge SHA:
`a0ec16f39386965c2fa103337340f56f68c47fb9`

Main CI after initial merge: `#108` — PASS.

The first production migration caused Supabase Security Advisor lint 0029 because the public claim RPC was an authenticated-callable `SECURITY DEFINER` function. This was treated as a WU2 defect and corrected before closeout.

Security hotfix PR: `#20`

Final WU2 product main SHA after hotfix:
`f648238f37cbf695330d7a0b74ba6f5e432a82b1`

Hotfix PR CI: `#109` — PASS, including:
- WU2 verified-email claim database QA;
- privilege-isolation checks;
- spoof prevention;
- ownership-conflict behavior;
- rollback/idempotency checks;
- build;
- typecheck;
- Cloudflare dry-run.

No Cloudflare Worker deployment occurred.

## Production migrations

1. `20260831005435 p12_wu2_verified_email_participant_claim`
2. `20260831005907 p12_wu2_claim_security_invoker_hotfix`

Final architecture:

- public `claim_my_journey_participations()` = `SECURITY INVOKER`;
- private `tnc_process_community_claim_request()` = trigger-only `SECURITY DEFINER`;
- anon cannot execute the private processor;
- authenticated cannot execute the private processor directly;
- `community_claim_requests` is a minimal own-user RLS envelope and stores no applicant PII;
- caller-supplied result counts are overwritten by authoritative private processing.

## Production postflight

After WU2:

- community participant link rows: `0`;
- community claim request rows: `0`;
- pilot Journey remains `registration_open`;
- capacity remains `30`;
- confirmed participant rows remain `1`;
- confirmed people remain `1`;
- application `user_id` remains NULL for the pilot participant;
- participant `user_id` remains NULL for the pilot participant;
- semantic drift remains `0`;
- `pg_graphql` remains OFF.

No fake account, claim, attendance, Memory, evidence or impact data was created.

## Advisors

After the hotfix, Security Advisor no longer reports an exposed authenticated `SECURITY DEFINER` claim RPC.

Remaining security warning is pre-existing:
- Leaked Password Protection Disabled.

Reference:
https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection

Performance Advisor still contains existing project warnings and unused-index observations; WU2 does not claim the project is performance-warning free.

## Activation gate

WU2 source and database implementation are complete, but public activation is intentionally gated.

Before exposing community onboarding in production navigation, verify the production Supabase Auth delivery path, including:

- Site URL;
- allowed redirect URLs for `/cong-dong` and `/en/community`;
- Magic Link / OTP email delivery;
- production email/SMTP configuration.

Canonical runtime still has **Email OFF**, therefore:

- no Cloudflare deploy was performed for community onboarding;
- community routes remain non-promoted / noindex;
- the feature must not be represented as publicly active yet.

## Final declaration

**P12-WU2 — IMPLEMENTATION COMPLETE / PASS — ACTIVATION GATED**

Next product layer: **P12-WU3 — My Journey Memory**, where verified identity ownership may expose a personal Journey archive while attendance remains authoritative and Memory cannot be inferred from identity claim alone.
