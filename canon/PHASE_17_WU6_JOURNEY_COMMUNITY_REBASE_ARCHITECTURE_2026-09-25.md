# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6
# JOURNEY COMMUNITY REBASE
# CURRENT-STATE AUDIT & CANONICAL ARCHITECTURE

Date: 2026-09-25  
Status: **WU6.0 COMPLETE / PASS — ARCHITECTURE LOCKED; SOURCE IMPLEMENTATION NEXT**

## 1. Purpose

P17-WU6 rebases the existing P16 social foundation onto the Owner-locked Journey Operating Model.

This is not a new generic social network.

Canonical product statement:

> Every social contribution originates from a Journey. Publishing, interaction, public visibility and real-world evidence are separate permissions/truths.

WU6 must preserve the strongest P16 identity, consent, moderation, blocking, reporting, notification and shared-experience foundations while correcting product semantics that no longer match the P17 canon.

## 2. Canonical starting truth

Product source main at WU6 start:

`f63e6a5657c20ff2b8f46d9f89b580c52e98f566`

Docs main at WU6 start:

`bb95ff37c50fe7d7d57101014fdcaa7c7e671794`

Latest production DB cutover before WU6:

`20260925062925 / p17_wu5_final_participant_workspace_projections`

Recruitment remains:

**HOLD / CLOSED**

WU6 does not authorize reopening any Journey Application Window.

## 3. Production social baseline

Production audit at WU6.0:

- Journeys: 5;
- confirmed participants: 3;
- active participant links: 2;
- attended participants with positive verified attendance: 0;
- Memory rows: 2;
- social identities: 0;
- active social identities: 0;
- Journey social presences: 0;
- Journey interactions: 0;
- appreciations: 0;
- social notifications: 0;
- blocks: 0;
- reports: 0;
- moderation controls: 0;
- shared-experience edges: 0;
- open Application Windows: 0.

This means P16 social foundations are deployed but no real social rows need migration/reconciliation before the P17 rebase.

No fake social data will be seeded.

## 4. P16 foundations to preserve

Keep and reuse where compatible:

### Identity / consent

- `social_identities`;
- `social_identity_cards`;
- social consent audit;
- explicit social enable/disable;
- private-by-default identity/presence settings.

### Journey social presence

- `journey_social_presences` remains an explicit consent/visibility surface.

However, Presence is reinterpreted narrowly:

> Journey Presence means the person has chosen to appear socially in that Journey context.

It is **not** the authority for all posting or interaction.

### Safety

Preserve:

- blocks;
- reports;
- moderation controls;
- interaction restrictions;
- urgent human-review categories;
- privacy-minimized Admin moderation;
- no automated guilt/risk scoring.

### Notifications

Preserve the existing private recipient-scoped notification foundation.

WU6 may extend it for new post/comment events, but must not create global engagement counters or attention-maximizing loops.

### Shared Journey evidence

Preserve the attendance-backed `journey_shared_experience_edges` foundation.

Social activity must never create those edges.

## 5. Current P16 product gaps

### Gap A — no original Journey Post model

Current P16 interaction v1 provides:

- Question;
- Reply;
- Appreciation.

It does not provide the Owner-locked core social post model:

- text;
- photo/album;
- video;
- text + media.

WU6 must introduce an original Journey Post object.

### Gap B — Journey Presence is over-coupled to interaction authority

Current `tnc_can_write_journey_interaction` requires an active `journey_only` Journey Presence.

New canon allows, subject to Journey policy:

- approved participants to create original posts during the Publishing Window;
- non-participants to interact with public Journey content.

Therefore:

`Presence visibility != publishing authority != interaction authority`

### Gap C — no configurable Journey social windows

There is currently no Journey-owned:

- Publishing Window;
- Interaction Window.

These must be explicit per-Journey configuration.

No fixed `±15 days` rule may be hardcoded.

### Gap D — no post publication/privacy state

Current interactions are participant-room digital activity only.

P17 requires explicit separation between:

1. participant-only content;
2. public Journey content;
3. official TNC content.

Participant publishing rights do not imply public publication rights.

### Gap E — no Memory-mode social window behavior

P17 requires:

- new original posts may close after Publishing Window;
- comments/reactions may remain open after publishing closes;
- interaction may close later or remain open indefinitely;
- this must not depend merely on calendar date having passed.

## 6. WU6 capability model

WU6 separates four capabilities.

### 6.1 Read capability

A viewer may read:

- participant-only content only when they are an eligible confirmed participant for that Journey;
- public Journey content according to publication state/policy;
- own content even when later withdrawn, where product safety policy allows owner history access;
- Admin content under moderation authority.

Read capability must respect blocks and moderation.

### 6.2 Publish capability

Original Journey Post publishing requires:

1. authenticated user;
2. active social identity;
3. verified active participant link;
4. linked Journey participant remains `confirmed`;
5. current time is inside the configured Publishing Window;
6. Journey community publishing is enabled;
7. user is not socially suspended/restricted.

Publishing does **not** require verified attendance.

Publishing does **not** require the user to expose a Journey Presence card.

### 6.3 Interaction capability

Interaction is separate from publishing.

Depending on Journey policy:

- confirmed participants may comment/react on content they can read;
- authenticated non-participants may comment/react on **public** Journey posts if visitor interaction is enabled;
- interaction must be inside the configured Interaction Window;
- Interaction Window may remain open after Publishing Window closes;
- null Interaction close time may represent no scheduled close when explicitly enabled by policy.

Interaction does not create participant, attendance or shared-experience truth.

### 6.4 Public publication capability

A participant-created post begins privacy-first.

Public publication is governed independently by Journey policy.

Canonical policy modes:

- `participants_only`
- `review_required`
- `participant_choice`

Safe default:

`participants_only`

Meaning:

#### participants_only

Participant posts cannot become public from the participant workflow.

#### review_required

Author may request public publication; BTC/Admin approval is required before public visibility.

#### participant_choice

Author may explicitly choose public visibility without BTC review.

Even in `participant_choice`, public visibility requires an explicit post-level choice.

No Journey policy may silently convert an existing participant-only post to public.

## 7. Journey Community policy object

WU6 will add one optional Journey-scoped policy source.

Recommended canonical table:

`journey_community_policies`

Conceptual fields:

- `journey_id`;
- `publishing_enabled`;
- `publishing_opens_at`;
- `publishing_closes_at`;
- `interaction_enabled`;
- `interaction_opens_at`;
- `interaction_closes_at` nullable when intentionally open-ended;
- `visitor_interaction_enabled`;
- `post_publication_mode`;
- audit fields.

Absence of a policy row means:

**community publishing/interaction fail closed**

This table is Journey operational configuration and Admin-controlled.

It is not participant-writable.

## 8. Original Journey Post object

Canonical source object:

`journey_community_posts`

Conceptual fields:

- id;
- Journey id;
- author social identity id;
- author participant id used as publishing provenance;
- body;
- locale;
- content kind;
- social state;
- participant/public publication state;
- created/updated timestamps;
- withdrawal/moderation timestamps.

Important:

`author_participant_id` proves only the approved/confirmed participation context that authorized posting.

It does not prove attendance.

### Post social state

Keep social lifecycle independent from publication:

- `active`;
- `withdrawn`;
- `moderated`.

### Publication state

Recommended explicit states:

- `participants_only`;
- `public_review_pending`;
- `public`;
- `public_unpublished`.

This keeps author withdrawal/moderation separate from public publication workflow.

## 9. Media model

Core product formats remain:

- text;
- photo/album;
- video;
- text + media.

WU6 must not invent a second media storage platform.

Participant social media should reuse the existing media/storage foundation where technically safe, with a narrow post-media relation.

Public publication of media follows the post publication decision.

Official TNC reuse remains a separate future curation/provenance action and is not implied by public post visibility.

## 10. Comment / reply / appreciation model

Existing P16 Question / Reply / Appreciation foundation should be reused where semantically sound, not deleted merely because product language changed.

WU6 may extend or recompose interaction storage so that:

- comments belong to a Journey Post;
- replies remain Journey-scoped;
- appreciation remains calm/private in v1;
- no public like count/ranking is required;
- existing P16 question/reply rows remain technically compatible.

The final schema should prefer additive compatibility over destructive table renaming.

## 11. Journey Presence after rebase

Journey Presence remains useful for:

- explicit “show me in this Journey community” consent;
- participant people/directory visibility;
- relationship continuity after evidence + mutual consent.

It must not be required merely to:

- read one's participant-only Journey posts;
- publish as an eligible participant;
- comment/react on a public post as an allowed non-participant.

This fixes the largest semantic coupling in P16.

## 12. Non-participant interaction

A non-participant:

- cannot create an original Journey Post;
- cannot read participant-only content;
- may interact only with a public Journey Post;
- must be authenticated with an active social identity;
- must pass current interaction window/policy;
- must pass block/moderation/restriction rules.

This capability never creates:

- Journey participant record;
- Journey Presence participant status;
- attendance;
- Memory;
- shared-experience edge.

## 13. Admin / BTC Community controls

Journey Control Center should eventually expose a Community module for:

- Publishing Window;
- Interaction Window;
- visitor interaction policy;
- participant-post publication mode;
- public publication review queue where required;
- moderation link/state.

Do not build a generic social administration suite.

## 14. Community feed composition

Primary feed remains Journey-scoped.

Ordering:

**chronological / recent activity by default**

No engagement-ranking algorithm.

A Journey Community surface may compose:

- participant Journey Posts;
- public-safe Journey content;
- Official Updates only as clearly distinct operational notices;
- existing verified/public editorial content where product composition benefits.

Official Update must never be visually or semantically collapsed into a participant social post.

## 15. My TNC / global surfaces

WU6 does not create a global free-form composer.

My TNC may later aggregate relevant items, but every social item retains:

- Journey origin;
- Journey link;
- Journey policy.

No global social post exists without a Journey.

## 16. Privacy / public sharing

Participant-only content:

- no public sharing control;
- no public URL exposure beyond authenticated Journey access.

Public Journey Post:

- may receive “Lan tỏa” / sharing affordance;
- sharing must preserve Journey context where possible.

Changing a post from public to unpublished must remove platform-provided public visibility without rewriting operational Journey truth.

## 17. Children / vulnerable community protection

Journey policy must be capable of forcing:

`participants_only`

for sensitive Journeys.

Participation consent does not imply public image/content publication consent.

WU6 must not infer child/vulnerability status automatically.

Safety remains policy/human-review driven.

## 18. Security architecture

Follow current Supabase hardening pattern:

- RLS enabled on every new public table;
- explicit table grants;
- privileged helpers remain in non-exposed `private` schema;
- `SECURITY DEFINER` only when justified, with `search_path = ''`;
- public RPC wrappers, when needed, remain `SECURITY INVOKER`;
- no `raw_user_meta_data` / user-editable JWT metadata authorization;
- no broad source-table grant merely to make frontend queries convenient;
- tests include member/non-member/cross-Journey cases.

Current Supabase documentation continues to recommend explicit grants plus RLS for Data API objects and restricted EXECUTE for functions.

## 19. Production advisor baseline

Security Advisor at WU6 start:

- no WU6-specific finding;
- pre-existing warning: **Leaked Password Protection Disabled**.

Reference:

`https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection`

Performance Advisor contains existing informational/warning items, including unindexed foreign keys, RLS init-plan items, unused indexes and existing multiple-permissive policies.

WU6 must not claim those pre-existing items as regressions introduced by WU6.

Any new WU6-specific advisor finding must be addressed before WU6.Final closeout.

## 20. Explicit non-goals

WU6 does not:

- reopen recruitment;
- alter Journey lifecycle automatically;
- write attendance;
- create Memory;
- create Reflection;
- create Impact;
- manufacture Shared Journey edges;
- build Friend/Follower mechanics;
- build popularity ranking;
- create Reels/Stories/livestream/polls/groups;
- build a global composer;
- turn Official Updates into Community posts;
- create automatic official-media reuse rights;
- infer friendship from Team/Journey/interactions.

## 21. Canonical WU6 sequence

### WU6.0 — Current-State Audit & Journey Community Architecture Lock
**COMPLETE / PASS**

### WU6.1 — Journey Community Policy / Social Window Source Contract
Next.

### WU6.2 — Original Journey Post + Publishing Authorization Foundation

### WU6.3 — Journey Community Feed & Participant Composer

### WU6.4 — Comment / Reply / Appreciation Rebase

### WU6.5 — Publication Review / Visibility / “Lan tỏa”

### WU6.6 — Admin Journey Community Controls

### WU6.7 — Notification / Safety / Shared-Journey Compatibility

### WU6.8 — Mobile / VI-EN / Privacy / Security Regression QA

### WU6.Final — Production Cutover & Canonical Closeout

## 22. Release strategy

WU6.1–WU6.8 are source-first unless a later sub-unit explicitly records a separate approved production dependency.

WU6.Final owns:

- production migration package;
- rollback;
- ephemeral migration/rollback QA;
- exact-head release gates;
- production preflight;
- production apply;
- post-cutover verification;
- Security/Performance Advisors;
- canonical closeout.

No WU6 source PR by itself authorizes production social activation.

## 23. Runtime activation strategy

Existing P16 social feature flags remain inherited until explicitly rebased.

WU6 should eventually introduce one clear Journey Community v2 activation contract rather than accumulating independent ambiguous flags indefinitely.

However, old flags must not be deleted until compatibility and rollback behavior are proven.

At WU6.0:

- no feature flag is changed;
- no Worker runtime setting is changed;
- no production social row is created.

## 24. Final WU6.0 decision

# **P17-WU6.0 — COMPLETE / PASS**

Architecture is locked.

Proceed directly to:

# **P17-WU6.1 — JOURNEY COMMUNITY POLICY / SOCIAL WINDOW SOURCE CONTRACT**
