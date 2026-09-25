# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.4
# COMMENT / REPLY / APPRECIATION REBASE

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Rebase the existing P16 Journey interaction foundation onto the P17 Journey Community model without creating a second conversation system.

WU6.4 adds Post-linked Comment / Reply and calm Appreciation while preserving legacy Question / Reply compatibility.

## 2. Canonical semantics

- Interaction Window is independent from Publishing Window.
- Journey Presence is not interaction authority.
- Readable participant content may be discussed by confirmed participants.
- Future public visitor interaction remains gated by public Post visibility + explicit Journey policy.
- Withdrawal of own interaction remains available after Interaction Window closes.
- Edit/new interaction requires the Interaction Window to be open.
- Appreciation remains sender-private: no public count, reaction list, ranking or popularity signal.
- Social activity does not create attendance, Memory, Shared Journey or Impact truth.

## 3. Compatibility

Existing `journey_interactions` is retained.

Additive P17 shape:

- `post_id` nullable;
- interaction types: `question`, `comment`, `reply`;
- legacy Question/Reply rows remain valid with `post_id IS NULL`;
- Post replies inherit the exact parent Post context server-side.

## 4. Privacy hardening

Reply visibility now inherits parent visibility.

A Reply is not projected when its parent Comment/Question is:

- inactive;
- invalid as a parent type;
- hidden from the current viewer due to governed social visibility/blocking.

This prevents orphan Reply leakage when a viewer blocks the parent author.

## 5. Post Appreciation

New source:

`journey_community_post_appreciations`

Properties:

- RLS enabled;
- sender/admin read only;
- owner insert/update only;
- no anon table access;
- no self-appreciation;
- own Appreciation may be withdrawn after Interaction Window closes.

## 6. Governed projections

Rebased feed:

`public.tnc_journey_community_feed(uuid)`

adds caller-private:

`appreciated_by_me`

New conversation projection:

`public.tnc_journey_post_conversation(uuid)`

returns only governed active Comment/Reply rows. It excludes participant IDs, attendance, public reaction counts and ranking.

## 7. Product UX

Journey Community v2 now supports:

- Comment;
- Reply;
- Appreciation;
- edit own active interaction while Interaction Window is open;
- withdraw own interaction even after Interaction Window closes;
- VI / EN Interaction Window messaging.

Appreciation language is intentionally calm:

- VI: `TRÂN TRỌNG`
- EN: `APPRECIATE`

No like count is shown.

## 8. Source truth / PR

Product PR:

`#107 — P17-WU6.4: Comment Reply Appreciation rebase`

Final exact head:

`a552346f03264ae1f9950c24ccf020d66a0d41dd`

Merged product main:

`6f2f454f23401da25f43e329738b1243d098b3e1`

Key source:

- `database/contracts/p17_wu6_4_comment_reply_appreciation_rebase.sql`
- `scripts/p17-wu6-4-interaction-rebase-source-qa.ts`
- `scripts/p17-wu6-4-interaction-rebase-db-qa.sql`
- `scripts/p17-wu6-4-conversation-ui-qa.ts`
- `src/components/journeys/journey-post-conversation.tsx`
- `src/components/journeys/journey-community-v2.tsx`
- `src/lib/journeys/community-v2.ts`

## 9. Corrective QA history

Two exact-head failures were test-harness issues:

1. UI QA initially asserted a brittle prop spelling. The UI already derived a stricter capability:
   `Boolean(activeIdentity && interactionOpen)`.
   The QA was corrected to protect that semantic invariant.

2. The new blocked-parent PostgreSQL assertion block contained malformed dollar quoting (`do $` instead of `do $$`).
   Only the fixture delimiter was corrected.

No authorization, privacy or product standard was weakened.

## 10. Exact-head verification

Final exact-head runs:

- generic CI `36114399953`: **SUCCESS**
- dedicated gate `36114399789`: **SUCCESS**

Verified PASS:

- WU6.4 source QA;
- WU6.4 UI QA;
- WU6.4 PostgreSQL DB QA;
- all inherited gates;
- build;
- typecheck;
- Cloudflare dry-run.

## 11. Post-merge verification

Post-merge main:

`6f2f454f23401da25f43e329738b1243d098b3e1`

- main CI `36114557321`: **SUCCESS**
- Cloudflare production build: **SUCCESS**
- Cloudflare staging build: **SUCCESS**

## 12. Production state

No WU6.4 Supabase production migration was applied.

Journey Community v2 remains source-first / fail-closed until WU6.Final.

Recruitment remains:

**HOLD / CLOSED**

## 13. Final decision

# **P17-WU6.4 — COMPLETE / CLOSED / PASS**

Proceed directly to:

# **P17-WU6.5 — PUBLICATION REVIEW / VISIBILITY / “LAN TỎA”**
