# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU9 — MOBILE / VI-EN / CROSS-SURFACE / PRIVACY QA

Date: 2026-09-04
Status: **COMPLETE / PASS**
Production database mutation: **NONE**
Cloudflare Worker deploy: **NONE**
P16 social feature flags: **OFF**

## 1. Objective

P16-WU9 validates and hardens the Journey-Based Social Network source across four dimensions before the real pilot gate:

1. mobile usability;
2. VI/EN parity;
3. cross-surface consistency;
4. privacy/fail-closed behavior.

WU9 is a source-only QA/corrective pass. It introduces no Supabase schema migration and performs no production runtime activation.

## 2. Canonical source baseline and branch

WU9 feature branch:

`p16-wu9-mobile-vi-en-privacy-qa`

Branch base / production main before WU9:

`05742cdff1aefaac48a1723ca5eaa3e3499543af`

PR:

`#58 — P16-WU9: Mobile VI-EN Cross-Surface Privacy QA`

Final PR head after QA correction:

`f6fe69ab5893111054778502e36aaac018c07573`

PR #58 squash-merged successfully.

Final product `main` SHA:

`b25546fbea4514f51432436aa4e5e8f2e7f2a6de`

Final main tree SHA:

`788f731235cb179f024a8a848e17d91594c910d7`

## 3. Corrective findings and fixes

### 3.1 English Community canonical metadata

Audit found the English Community route `/en/community` was passing the Vietnamese path `/cong-dong` into localized canonical metadata generation.

WU9 corrects the EN route to canonicalize to:

`/en/community`

VI remains:

`/cong-dong`

This preserves correct bilingual route identity and alternate-link semantics.

### 3.2 Mobile Journey interaction controls

Journey Question / Reply / Appreciation / Report / Block / Withdraw controls were hardened to retain the existing 44px mobile touch-target baseline.

Corrective changes include:

- shared mobile action class using `touch-target`;
- narrower reply composer indentation on small screens;
- narrower reply thread indentation on small screens;
- report actions wrap instead of overflowing on narrow screens;
- spacing remains larger again at `sm` breakpoints.

No interaction semantics were changed.

### 3.3 Mobile notification controls

Private notification surfaces retain 44px-equivalent touch targets for:

- Journey return CTA;
- mark-as-seen action;
- notification preferences.

No global bell/count/ranking mechanic was introduced.

### 3.4 Reversible blocking surface

Private Blocked People management retains the WU8 privacy boundary:

- query selects only `id, blocked_display_name_snapshot, created_at`;
- blocked social identity ID is not re-exposed;
- unblock deletes only the selected own block row under RLS;
- mobile unblock control has a 44px touch target.

### 3.5 Admin Social Safety privacy

Admin Social Safety remains:

- authenticated/admin gated;
- reporter identity omitted from UI projection;
- human-review oriented;
- not one-click punitive moderation;
- no risk score, vulnerability score or moderation ranking.

## 4. VI/EN parity contract

WU9 verifies the reciprocal Community surfaces:

- VI `/cong-dong`
- EN `/en/community`

Both retain:

- Community relationship surface;
- private social safety settings;
- locale-correct canonical metadata.

Private notifications remain reciprocal and non-indexable:

- VI `/cong-dong/thong-bao`
- EN `/en/community/notifications`

Both retain:

`robots = noindex, nofollow`

and reciprocal `hrefLang` alternatives.

## 5. Privacy / fail-closed contracts

WU9 source QA verifies:

- notifications require an authenticated session;
- the current social identity is resolved from the signed-in `session.user.id`;
- the notification surface reads the recipient-protected `social_notifications` projection;
- a row whose actor identity card or Journey is no longer visible is dropped from presentation;
- no global notification engagement counter is introduced;
- notification copy in both locales explicitly rejects attendance inference;
- Block settings do not re-expose `blocked_social_identity_id`;
- urgent Report language means priority human review only, not proof/guilt;
- blocking does not imply rewriting Journey operational history;
- Admin Safety omits reporter identity and does not create moderation controls directly from the review queue;
- user-facing social surfaces contain no vulnerability/minor/toxicity/risk scoring or public moderation ranking concepts.

## 6. Activation state

WU9 verifies that all P16 social surfaces remain fail closed in `.env.example`:

- `VITE_APP_JOURNEY_COMMUNITY_ROOM_ENABLED=false`
- `VITE_APP_SHARED_EXPERIENCE_GRAPH_ENABLED=false`
- `VITE_APP_JOURNEY_INTERACTION_V1_ENABLED=false`
- `VITE_APP_SOCIAL_NOTIFICATIONS_V1_ENABLED=false`
- `VITE_APP_SOCIAL_SAFETY_HARDENING_ENABLED=false`
- `VITE_APP_JOURNEY_SOCIAL_CONTINUITY_ENABLED=false`

The activation chain remains governed by downstream dependencies; WU9 does not enable any of them.

## 7. Automated QA

WU9 adds repository-owned source QA:

`script(s)/p16-wu9-mobile-bilingual-privacy-qa.ts`

and wires it into the full CI pipeline before inherited ephemeral database gates.

### CI #245 — initial FAIL

PR CI #245 / run `33734360361` failed only at the new WU9 gate.

Root cause:

The WU9 assertion checked an incorrect environment variable name:

`VITE_APP_JOURNEY_SHARED_EXPERIENCE_GRAPH_ENABLED`

while canonical runtime configuration uses:

`VITE_APP_SHARED_EXPERIENCE_GRAPH_ENABLED`

This was a QA assertion error, not a runtime configuration/product error.

No runtime flag was changed to make the test pass.

### CI #246 — PASS

After correcting the assertion, PR CI #246 / run `33749883765` completed SUCCESS on exact head:

`f6fe69ab5893111054778502e36aaac018c07573`

All 44 verification steps passed, including:

- inherited source QA P9–P16;
- P16-WU9 source QA;
- all inherited ephemeral database / rollback gates;
- application build;
- TypeScript typecheck;
- Cloudflare dry-run.

## 8. Post-merge main CI

PR #58 was squash-merged into `main` as:

`b25546fbea4514f51432436aa4e5e8f2e7f2a6de`

Post-merge main CI:

- CI #247
- run `33753660608`
- event: `push`
- exact head SHA: `b25546fbea4514f51432436aa4e5e8f2e7f2a6de`
- status: completed
- conclusion: **SUCCESS**

Therefore the merged production source passed the full canonical gate independently of the PR merge test.

## 9. Runtime / database boundary

WU9 performed:

- no Supabase migration;
- no production database mutation;
- no Cloudflare Worker deploy;
- no social feature activation;
- no fake Journey attendance;
- no fake Memory / Reflection / Contribution;
- no fake social activity.

P14-WU5 remains the independent real-evidence lane for the Journey dated 2026-09-11.

## 10. Final status

**P16-WU9 — MOBILE / VI-EN / CROSS-SURFACE / PRIVACY QA = COMPLETE / PASS.**

Phase 16 readiness after WU9:

- WU1 COMPLETE / PASS
- WU2 COMPLETE / PASS
- WU3 COMPLETE / PASS
- WU4 COMPLETE / PASS
- WU5 COMPLETE / PASS
- WU6 COMPLETE / PASS
- WU7 COMPLETE / PASS
- WU8 COMPLETE / PASS
- WU9 COMPLETE / PASS

Next canonical gate:

**P16-WU10 — REAL PILOT EVIDENCE & OWNER GATE**

WU10 must use real evidence only and must not infer attendance, Memory, Reflection, Contribution, shared-experience relationships or impact before appropriate evidence exists.
