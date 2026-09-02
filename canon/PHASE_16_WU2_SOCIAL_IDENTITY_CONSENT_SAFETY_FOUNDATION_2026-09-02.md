# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 16 / P16-WU2 — SOCIAL IDENTITY, CONSENT & SAFETY FOUNDATION

Date: 2026-09-02  
Status: **COMPLETE / PASS**  
Production mutation: **CONTROLLED / VERIFIED PASS**  
Social UX activation: **OFF / NOT PART OF WU2**

## 1. OWNER APPROVAL

Owner explicitly approved direct controlled production migration:

> **APPROVE P16-WU2 DIRECT PRODUCTION MIGRATION — apply 0042/0043 under controlled QA, preserve all Journey truth, no social UX activation until verification PASS.**

During controlled verification, two additive hardening migrations (`0044`, `0045`) were added before closeout. They narrow audit/consent mutation paths and provide self-cleaning production QA; they do not expand social product scope.

## 2. GOVERNING PRODUCT CANON

> **TRẠM NỤ CƯỜI is a Journey-Based Social Network.**
>
> **Journey — not Post and not Person — is the primary social object.**
>
> **Digital relationships may begin around preparation for a real Journey, but claims of shared real-world experience arise only from appropriate evidence.**
>
> **TNC social features exist to extend meaningful real-world relationships, not maximize time-on-platform, popularity or content volume.**
>
> **Public visibility is always a separate consent/publication decision from private operational truth.**

P16-WU2 adds the privacy/safety substrate required before Journey Community Room, Journey Stream, shared-experience graph or member-generated interactions can be activated.

## 3. IMMUTABLE WU2 RULES

- `social identity != account identity`
- `Journey Presence != attendance`
- `social visibility != operational truth`
- registration != attendance
- confirmed registration != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance > 0 = verified attended
- participant claim != attendance
- participant claim != Memory eligibility
- account != participant
- account != attendance
- Memory only arises from appropriate real evidence
- Reflection remains evidence-gated
- withdrawal, blocking, reporting or moderation MUST NOT rewrite registration, participant, attendance, Memory, Reflection, Contribution or verified relationship truth

## 4. PRODUCTION DATA MODEL ACTIVATED

Seven additive social tables now exist in production:

1. `social_identities`
2. `social_identity_cards`
3. `journey_social_presences`
4. `social_blocks`
5. `social_consent_events`
6. `social_reports`
7. `social_moderation_controls`

### `social_identities`

Private user-controlled social source, separate from `profiles` and `auth.users`.

Account existence does not create social visibility. Social participation is explicit opt-in.

### `social_identity_cards`

Identity-minimized Journey-social projection. It deliberately excludes auth UUID, email, registration PII, participant id, attendance, Memory id, Contribution data and moderator identifiers.

It is not a global member directory.

### `journey_social_presences`

Explicit per-Journey digital presence choice.

WU2 scopes:

- `private`
- `journey_only`

Presence means only that a person chooses to be visible in that Journey's digital community context. It may exist before the real Journey occurs and MUST NOT assert attendance.

### `social_consent_events`

System-written consent/visibility event ledger. It is not attendance evidence.

### `social_blocks`

Block applies to social visibility/interactions only. It does not erase or rewrite shared operational history.

### `social_reports`

Reporter-private / admin-review safety reports. Report targets have no policy exposing reporter/report details.

### `social_moderation_controls`

Admin-only controls:

- `hide_identity`
- `hide_presence`
- `social_suspend`

Moderation can reduce or restore social visibility. It cannot grant consent or mutate Journey truth.

## 5. PRODUCTION MIGRATIONS

Applied successfully to Supabase project `iwiqprhoohkxvjyxojto`:

- `20260902085800` — `0042_p16_wu2_social_identity_consent_safety`
- `20260902085811` — `0043_p16_wu2_social_guard_hardening`
- `20260902085936` — `0044_p16_wu2_social_identity_transition_hardening`
- `20260902090136` — `0045_p16_wu2_production_qa`

Product source branch:

`p16-wu2-social-identity-consent-safety`

Base production SHA:

`d9a67f58ef25f650fe2e378683b6d92fb36a0137`

Source artifacts:

- `db/migrations/0042_p16_wu2_social_identity_consent_safety.sql`
- `db/migrations/0043_p16_wu2_social_guard_hardening.sql`
- `db/migrations/0044_p16_wu2_social_identity_transition_hardening.sql`
- `db/migrations/0045_p16_wu2_production_qa.sql`
- `db/rollbacks/p16_wu2_social_identity_consent_safety.sql`

## 6. HARDENING ADDED DURING CONTROLLED MIGRATION

### 0043 — audit-field hardening

- grants only the required private suspension helper to authenticated callers;
- prevents ordinary client updates from rewriting social activation/deactivation timestamps;
- prevents ordinary Journey Presence updates from rewriting presence audit timestamps and consent provenance outside legitimate decisions.

### 0044 — transition hardening

Further closes the enabled -> disabled edge:

- `activated_at` / `deactivated_at` remain system-controlled;
- a new social consent version is accepted only on a real disabled -> enabled transition;
- profile edits and withdrawal preserve the prior consent provenance.

## 7. PRODUCTION QA — PASS

`0045_p16_wu2_production_qa` is a self-cleaning production QA migration. Any failed assertion would abort the migration transaction.

Verified successfully:

- all 7 social tables have RLS enabled;
- anonymous SELECT is denied on all 7 social tables;
- account existence alone creates no social identity/card;
- social activation creates only the explicit social projection;
- client-provided fake activation timestamps are neutralized;
- consent activation is audited;
- a confirmed participant may opt into Journey digital presence while attendance is still unresolved;
- therefore Journey Presence is demonstrably separate from attendance;
- profile edits cannot rewrite consent/audit provenance;
- Journey Presence withdrawal cannot rewrite historical joined/consent provenance;
- explicit re-enable can record a new consent version and system-stamped join time;
- visibility changes can carry a new consent decision;
- blocking suppresses target social visibility;
- reports begin `open`;
- non-admin moderation attempts are rejected by the trigger;
- admin suspension prevents social visibility expansion;
- disabling social identity removes its card and withdraws active Journey Presence;
- social actions do not mutate participant attendance or Memory truth;
- synthetic QA rows are fully deleted before migration success.

## 8. BASELINE / POST-FLIGHT TRUTH

### Before WU2

- `journey_participants`: 1
- `community_journey_memories`: 1
- `community_participant_links`: 1
- `journey_reflections`: 0
- `journey_reflection_publications`: 0
- `community_contributions`: 0
- `community_relationship_assignments`: 0
- attendance unresolved: 1
- verified attended: 0
- attended people: 0
- social tables: none

### After WU2 + production QA

Social foundation rows:

- social identities: 0
- social identity cards: 0
- Journey social presences: 0
- blocks: 0
- consent events: 0
- reports: 0
- moderation controls: 0

Operational truth remains:

- `journey_participants`: 1
- `community_journey_memories`: 1
- `community_participant_links`: 1
- `journey_reflections`: 0
- `journey_reflection_publications`: 0
- `community_contributions`: 0
- `community_relationship_assignments`: 0
- attendance unresolved: 1
- verified attended: 0
- attended people: 0
- Memory state: `unresolved`
- `memory_eligible=false`

**Result: Journey truth preserved exactly.**

## 9. RLS / EXPOSURE VERIFICATION

All seven WU2 social tables have RLS enabled.

Anonymous table SELECT is denied on every WU2 social table.

Private helper functions remain in schema `private`.

Anonymous callers have no EXECUTE on WU2 private helpers.

Authenticated EXECUTE is granted only to helpers required by RLS/client-safe authorization paths; internal guards, audit functions and projection-sync functions remain non-callable directly by authenticated clients.

## 10. SECURITY / PERFORMANCE ADVISORS

### Security Advisor

No new WU2-created security warning was reported.

One pre-existing warning remains:

- `auth_leaked_password_protection` disabled.

This is a separate account-security hardening item and should be resolved before broad social/account rollout when current plan/capability permits.

### Performance Advisor

No WU2-specific missing-FK-index or RLS-initplan warning was introduced.

Several new WU2 indexes are currently reported as unused because the social foundation contains zero real social rows and the feature has not launched. They are retained intentionally for expected Journey/social query paths and MUST NOT be removed merely because they are unused immediately after creation.

Older non-WU2 advisor items remain independently open.

## 11. PRODUCT BOUNDARIES AFTER WU2

Still NOT activated:

- public people directory
- public participant presence
- Friend
- Follow
- DM/private messaging
- generic user status posts
- global infinite feed
- reaction/like counters
- online presence
- real-time location
- precise live location

No social UI was activated in WU2.

## 12. REAL JOURNEY — 2026-09-11

Journey:

`19539f36-3ed4-4a22-96b9-c8a9b73c5283`

At WU2 closeout:

- participant rows: 1
- attendance unresolved: 1
- verified attended: 0
- attended people: 0

Therefore WU3 may pilot only BEFORE-stage concepts such as Journey Room preparation and explicit Journey digital presence.

Forbidden until real attendance evidence exists:

- `Đã cùng đi`
- `Đã tham dự`
- attended Memory
- shared real-world experience edge
- impact claims derived from social presence

P14-WU5 remains the independent real-evidence lane for AFTER-stage truth.

## 13. SOURCE / BUILDER SAFETY

Repository production truth is `main`, while repository default branch remains `develop` and is materially diverged.

Therefore Lovable must not implement future social work blindly against the repository default branch. P16 implementation must originate from explicitly verified production `main` or a branch based on the current production-main SHA.

## 14. CLOSEOUT DECISION

- Social identity separate from account/profile identity: **PASS**
- Distinct social UUID from auth UUID: **PASS**
- Explicit social opt-in: **PASS**
- Explicit per-Journey Presence: **PASS**
- Journey Presence != attendance: **PASS / IMMUTABLE**
- Private + Journey-only visibility: **PASS**
- Public member directory: **NOT ENABLED**
- Withdrawal / consent audit: **PASS**
- Blocking foundation: **PASS**
- Reporting foundation: **PASS**
- Admin moderation foundation: **PASS**
- Journey truth preservation: **PASS**
- Production QA: **PASS**
- Social UX activation: **OFF**

# P16-WU2 — COMPLETE / PASS

Next gate:

**P16-WU3 — JOURNEY COMMUNITY ROOM & TYPED JOURNEY STREAM**

WU3 must build on the WU2 privacy/safety substrate and continue to preserve the Truth Ledger. A Journey Room membership/presence is social context, not proof of attendance.