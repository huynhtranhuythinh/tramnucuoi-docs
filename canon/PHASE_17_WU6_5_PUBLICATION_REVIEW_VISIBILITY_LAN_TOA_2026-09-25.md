# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.5
# PUBLICATION REVIEW / VISIBILITY / “LAN TỎA”

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Add explicit public publication workflow and public-safe Journey Community visibility without weakening participant privacy or manufacturing operational truth.

Core rule:

> A Journey Post is participant-only by default. Public visibility is always a separate explicit Post-level decision governed by the Journey's publication policy.

## 2. Publication modes

Journey policy supports:

- `participants_only`
- `review_required`
- `participant_choice`

None of these automatically publishes an existing Post.

### participants_only

No public publication request or direct public transition is allowed.

### review_required

Author explicitly requests public publication:

`participants_only/public_unpublished -> public_review_pending`

A current Journey BTC reviewer or global Admin may then:

- approve -> `public`
- reject -> `participants_only`

Global Editor is not publication reviewer authority.

An author who is also a reviewer cannot self-approve through the reviewer path.

### participant_choice

Author may explicitly transition an eligible Post to:

`public`

The choice remains Post-specific and reversible.

## 3. Public unpublication

Author or authorized reviewer may transition:

`public -> public_unpublished`

This removes platform-provided public visibility while preserving participant history and operational Journey truth.

Public source content must be unpublished or leave review state before owner editing.

## 4. Reviewer authority

Publication review authority is:

- global Admin; or
- currently confirmed Journey participant with active Journey Role `btc`.

Global Editor alone is not sufficient.

Reviewers may change publication state only.

They may not rewrite:

- Post body;
- content kind;
- author/provenance;
- participation;
- attendance;
- Memory;
- Shared Journey truth.

## 5. Private publication audit

New private audit source:

`private.journey_community_post_publication_events`

Records:

- Post;
- Journey;
- previous publication state;
- next publication state;
- action;
- actor user;
- actor kind;
- time.

Actions include:

- request publication;
- cancel request;
- publish;
- reject publication;
- unpublish.

The audit is not exposed to Community clients.

## 6. Public Journey Post projection

New public-safe RPC:

`public.tnc_public_journey_community_feed(uuid)`

Returns only:

- active Posts;
- explicitly `public` Posts;
- enabled/non-hidden social identities;
- caller-visible Posts respecting authenticated block preference;
- chronological ordering.

It excludes:

- auth UUID;
- participant ID;
- attendance;
- Memory;
- moderation actor;
- Appreciation count;
- reaction count;
- engagement ranking.

## 7. Private Storage remains private

Community media remains in private bucket:

`journey-community-private`

WU6.5 does not convert the bucket to public.

Public visibility grants only exact Storage sign/get operations for media belonging to an active public Post.

Bucket listing remains denied.

Public signed media TTL:

`60 seconds`

Composer object cache control:

`60 seconds`

This reduces public visibility persistence after unpublish. Signed URLs remain independently valid until their short expiry.

## 8. Public-safe object key rebase

Before production cutover, Community media object keys were rebased from:

`<auth-user-uuid>/<journey>/<post>/<file>`

to:

`<journey>/<post>/<file>`

Storage ownership remains server-bound through:

- `storage.objects.owner_id`;
- Post ownership;
- Journey/Post path binding;
- RLS.

Reason:

> Public signed URLs must not expose account UUIDs.

No production WU6 data existed, so no object migration was needed.

## 9. Interaction visibility snapshot

New additive field:

`journey_interactions.visibility_scope`

Values:

- `participants_only`
- `public_context`

Rules:

- existing interaction rows default to `participants_only`;
- Comment created while parent Post is public snapshots `public_context`;
- Reply inherits parent visibility scope;
- scope is immutable.

This prevents historical participant-only conversation from becoming public merely because the parent Post is later published.

## 10. Public conversation projection

New public-safe RPC:

`public.tnc_public_journey_post_conversation(uuid)`

Returns only:

- active public-context Comment/Reply rows;
- on a currently public Post;
- respecting social visibility/blocking;
- with valid visible parent lineage.

Historical participant-only conversation is never retroactively exposed.

## 11. “Lan tỏa”

“Lan tỏa” / Share appears only for:

`publication_state = public`

It links to a Journey-scoped Post anchor on the current Journey page.

Participant-only, pending-review and unpublished Posts do not receive a public sharing control.

No global free-form social object is introduced.

## 12. Public audience behavior

Anon and authenticated non-participants may read only the public projection.

Participant-only feed remains protected.

Public UI explicitly states that public social content does not prove:

- attendance;
- shared real-world experience;
- Memory;
- Impact.

## 13. Source truth / PR

Product PR:

`#108 — P17-WU6.5: Publication review, visibility and Lan toa`

Final exact head:

`9184e40112b19181f5b5727ca406468496d8aeaf`

Merged product main:

`c25cb9a33c27e57f0793efde80e7673ecfd6b7fd`

Key source:

- `database/contracts/p17_wu6_5_publication_visibility_share.sql`
- `scripts/p17-wu6-5-publication-visibility-source-qa.ts`
- `scripts/p17-wu6-5-publication-visibility-db-qa.sql`
- `scripts/p17-wu6-5-publication-share-ui-qa.ts`
- `src/lib/journeys/community-v2.ts`
- `src/components/journeys/journey-community-v2.tsx`

Inherited WU6.3 media path fixtures/contracts were intentionally rebased before production.

## 14. Corrective QA history

Initial exact-head failure came from the historical WU6.3 composer regression gate, which still prohibited any public sharing.

WU6.5 intentionally introduces public sharing.

The historical gate was made forward-compatible:

if later sharing exists, it must be explicitly WU6.5-marked and gated by:

`post.publication_state === "public"`

No product privacy or authorization rule was weakened.

## 15. Exact-head verification

Final exact head:

`9184e40112b19181f5b5727ca406468496d8aeaf`

Runs:

- generic CI `36116932760`: **SUCCESS**
- dedicated gate `36116932701`: **SUCCESS**
- Cloudflare staging branch build: **SUCCESS**

Verified:

- WU6.1 inherited policy source/DB QA;
- WU6.2 inherited Post QA;
- WU6.3 private-media/source/composer/DB QA;
- WU6.4 interaction source/UI/DB QA;
- WU6.5 publication source QA;
- WU6.5 publication/share UI QA;
- WU6.5 PostgreSQL DB QA;
- inherited regression suite;
- build;
- typecheck;
- Cloudflare dry-run.

## 16. Post-merge verification

Product main:

`c25cb9a33c27e57f0793efde80e7673ecfd6b7fd`

Post-merge:

- main CI `36117148155`: **SUCCESS**
- Cloudflare production build: **SUCCESS**
- Cloudflare staging build: **SUCCESS**

## 17. Production state

No WU6.5 Supabase production migration was applied.

No production Journey Community v2 activation occurred.

Production still has no WU6 source tables/RPCs until WU6.Final.

Recruitment remains:

**HOLD / CLOSED**

## 18. Final decision

# **P17-WU6.5 — COMPLETE / CLOSED / PASS**

Proceed directly to:

# **P17-WU6.6 — ADMIN JOURNEY COMMUNITY CONTROLS**
