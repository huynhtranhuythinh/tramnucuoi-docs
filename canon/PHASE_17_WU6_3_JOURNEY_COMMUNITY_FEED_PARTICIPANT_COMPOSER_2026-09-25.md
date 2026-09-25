# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.3
# JOURNEY COMMUNITY FEED & PARTICIPANT COMPOSER

Date: 2026-09-25  
Status: **COMPLETE / PASS — SOURCE FOUNDATION MERGED; PRODUCTION COMMUNITY V2 STILL OFF**

## 1. Objective

Build the source foundation for a real Journey-scoped participant feed and composer while preserving privacy-first social truth.

WU6.3 adds:

- Community v2 fail-closed activation;
- participant-only chronological Journey feed;
- text/photo/album/video/text+media composer;
- private Supabase Storage lane for participant-only Community media;
- signed/authenticated private media delivery;
- server-side media path, MIME, shape and ownership enforcement;
- owner edit/withdraw controls;
- P16 fallback composition while v2 remains OFF.

## 2. Product source truth

Product PR:

`#106 — P17-WU6.3: Journey Community feed and participant composer`

Final PR exact head:

`71f735aae764bd1034547dfb94defef1d8de2ebc`

Squash-merged product main:

`cfa147086f2f8361fbdd236814500541e56aad0c`

Key files:

- `database/contracts/p17_wu6_3_journey_community_feed_media.sql`
- `src/lib/journeys/community-v2-activation.ts`
- `src/lib/journeys/community-v2.ts`
- `src/components/journeys/journey-community-v2.tsx`
- `scripts/p17-wu6-3-community-feed-media-source-qa.ts`
- `scripts/p17-wu6-3-community-feed-composer-qa.ts`
- `scripts/p17-wu6-3-community-feed-media-db-qa.sql`

## 3. Activation boundary

New environment flag:

`VITE_APP_JOURNEY_COMMUNITY_V2_ENABLED=false`

Community v2 requires:

- Community Auth enabled;
- Social Safety Hardening enabled;
- explicit Community v2 flag enabled.

V2 does **not** depend on the superseded P16 Journey Room / Interaction flags.

At source closeout the flag remains false.

Journey detail composition is:

- v2 enabled -> render Community v2;
- otherwise -> preserve P16 Journey Room behavior;
- old P16 Interaction surface is suppressed only when v2 is enabled.

Therefore merging source does not activate a duplicate or conflicting social UI.

## 4. Private Community media

Production audit found the existing Supabase Storage bucket:

`media`

is public.

Participant-only Community media therefore must not use that bucket.

WU6.3 source contract creates a separate private bucket:

`journey-community-private`

Properties:

- private;
- max object size 50 MiB;
- bounded image/video MIME types;
- no anonymous read policy;
- no object UPDATE/upsert policy.

Supabase documentation confirms private bucket downloads remain subject to Storage RLS and may be delivered with authenticated download or short-lived signed URLs.

This remains the same Supabase Storage platform; WU6.3 does not introduce a second media infrastructure.

## 5. Storage object authority

Canonical object path:

`<auth.uid>/<journey_id>/<post_id>/<unique-filename>`

Upload requires:

- authenticated object owner;
- a real owned draft Community Post;
- matching Journey and Post path;
- current WU6.2 publishing authority.

This prevents using the private bucket as arbitrary participant file storage.

Cross-Journey path substitution is rejected by DB QA.

## 6. Draft -> active media lifecycle

Text-only post:

- may become active immediately.

Media post:

1. create privacy-first draft Post;
2. upload private Storage object(s);
3. link governed post-media rows;
4. server derives media kind from actual Storage MIME metadata;
5. activate only if media shape is valid.

Server-side activation rules:

- photo = exactly 1 image;
- album = 2–10 images;
- video = exactly 1 video;
- text + media = 1–10 media items;
- text-only = 0 media.

Active-post media is immutable.

Draft / withdrawn owner cleanup remains available.

## 7. Feed model

Participant feed RPC:

`public.tnc_journey_community_feed(journey_id)`

Properties:

- authenticated only in WU6.3;
- active readable posts only;
- chronological: newest first;
- no engagement ranking;
- every row retains Journey origin;
- no participant/application/attendance identifiers exposed;
- private media returned as object keys, not public URLs;
- client obtains short-lived signed URLs under Storage RLS.

No global composer exists.

## 8. Participant composer authority

Client derives participant provenance through the WU5 participant workspace projection.

Composer requires:

- confirmed participant workspace;
- open Publishing Window;
- enabled social identity.

It does not require Journey Presence.

Journey Presence remains separate optional social visibility.

Composer UI explicitly states:

- post is Journey-scoped;
- participant-only by default;
- public publication is a separate later decision;
- posting is not attendance evidence.

## 9. User control

WU6.3 UI supports:

- create text/media Journey Post;
- edit own body while publishing authority remains open;
- withdraw own active Post.

Withdrawal removes the social item from governed feed visibility without changing operational Journey truth.

## 10. DB / privacy proof

Dedicated PostgreSQL QA proves:

- private Community bucket exists in the fixture;
- existing public `media` bucket remains public/unchanged;
- media post begins as `draft + participants_only`;
- MIME/media kind is server-derived;
- valid media post activates;
- cross-Journey upload denied;
- active media relation deletion denied;
- active Storage object deletion denied;
- confirmed peer can read feed + private media;
- authenticated outsider cannot read feed/media;
- anon cannot read private media;
- video with image cannot activate;
- one-image album cannot activate;
- two-image album can activate;
- withdrawn-post media cleanup works;
- recruitment remains closed;
- attendance fields remain unresolved/null.

## 11. QA corrective history

First exact-head attempt passed all source and DB gates plus build, but TypeScript caught:

- index-signature access for Vite env values;
- `exactOptionalPropertyTypes` on optional Storage `contentType`.

The source was corrected to compiler-safe bracket env access and conditional upload option construction.

No database, privacy or authorization contract was weakened.

## 12. Exact-head evidence

Final PR head:

`71f735aae764bd1034547dfb94defef1d8de2ebc`

Final PR:

- generic CI: **SUCCESS**
- dedicated inherited gate: **SUCCESS**
- WU6.3 private-media source QA: **PASS**
- WU6.3 composer source QA: **PASS**
- WU6.3 PostgreSQL DB QA: **PASS**
- build: **PASS**
- typecheck: **PASS**
- Cloudflare dry-run: **PASS**

Post-merge main:

`cfa147086f2f8361fbdd236814500541e56aad0c`

Post-merge main verify:

**SUCCESS**

Staging Worker build pipeline:

**SUCCESS**

## 13. Production boundary

No WU6 Supabase migration has been applied.

Production migration ledger remains at WU5.Final.

No private Community bucket exists in production from this work unit yet.

`VITE_APP_JOURNEY_COMMUNITY_V2_ENABLED=false`

Therefore WU6.3 creates no production social content and activates no new Community runtime.

## 14. Final decision

# **P17-WU6.3 — COMPLETE / CLOSED / PASS**

Next:

# **P17-WU6.4 — COMMENT / REPLY / APPRECIATION REBASE**
