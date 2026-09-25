# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU6.7
# NOTIFICATION / SAFETY / SHARED-JOURNEY COMPATIBILITY

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Objective

Rebase existing P16 social notification and safety foundations onto the P17 Journey Post / Comment / Reply model while preserving the canonical evidence boundary:

> Social activity never creates attendance, Memory, Contribution, Impact or Shared Journey truth.

## 2. Notification rebase

Existing private in-app notification foundation is retained.

P17 notification types:

- `question_reply` — legacy compatibility;
- `post_comment`;
- `comment_reply`;
- `interaction_appreciation`;
- `post_appreciation`.

Additional source provenance:

- `source_post_id`;
- `source_post_appreciation_id`.

Existing preference model is reused:

- `in_app_replies` governs Question Reply / Post Comment / Comment Reply;
- `in_app_appreciations` governs Interaction Appreciation / Post Appreciation.

No global notification count/ranking was introduced.

## 3. Presence decoupling

P16 notification visibility previously depended on active Journey Presence.

WU6.7 removes Journey Presence from notification authority.

Notification visibility now depends on the governed source itself:

- current social identity;
- recipient ownership;
- source Post / Interaction visibility;
- current social block state;
- moderation visibility;
- source lifecycle.

Ephemeral DB QA intentionally contained **zero Journey Presence rows** while Post Comment and Comment Reply notifications were created and read successfully.

## 4. Notification lifecycle hardening

WU6.7 preserves the P16 split between:

- client mark-as-read updates; and
- privileged source-lifecycle projection updates.

The first WU6.7 DB run exposed a real regression where the recipient read guard blocked privileged source-state withdrawal during moderation.

The fix restored the database-owner/service-role lifecycle branch already established by P16-WU6 hardening while retaining source-field immutability for ordinary authenticated clients.

This was an architecture correction, not a test relaxation.

## 5. Safety without exposing identity UUIDs

Journey Community V2 clients do not need target social identity UUIDs to Report or Block visible content.

Server-derived RPCs:

- `public.tnc_report_journey_post`;
- `public.tnc_block_journey_post_author`;
- `public.tnc_report_journey_interaction`;
- `public.tnc_block_journey_interaction_author`.

The server derives:

- current reporter/blocker identity;
- target identity;
- Journey context;

and validates the user can currently read the target content.

Safety actions are independent of Interaction Window state.

A closed Interaction Window stops new social creation/editing; it does not remove Report/Block capability for readable content.

## 6. Social Reports

`social_reports` gains exact Journey Post targeting:

`target_post_id`

Report target contexts remain mutually exclusive.

A Report remains:

- private;
- a human-review signal;
- not a public accusation;
- not attendance evidence;
- not an automatic punishment.

Urgent categories remain priority ordering for human review, not automated guilt/risk scoring.

## 7. Proportionate Post moderation

Admin may moderate one Journey Post without suspending the author's whole social identity.

Post moderation is one-way in WU6.7:

`draft/active -> moderated`

Requirements:

- Admin authority;
- reason code;
- Post source/body/media/publication history cannot be rewritten by the moderation transition.

Private audit:

`private.journey_community_post_moderation_events`

Existing Interaction moderation remains compatible.

Admin Safety queue can atomically:

- moderate reported Journey Post + resolve triaged report;
- moderate reported Journey Interaction + resolve triaged report.

A report must first be `triaged`.

## 8. Safety surfaces

Report / Block is available on:

- participant Journey Posts;
- public Journey Posts for authenticated users with active social identity;
- participant Comment / Reply;
- public-context Comment / Reply.

V2 safety actions use Post/Interaction IDs, not author identity UUIDs.

## 9. Notification source lifecycle

When source content is withdrawn or moderated:

- related source notifications become `withdrawn`;
- notification history is not silently deleted;
- RLS projection hides invalid/blocked sources.

Blocking changes the viewer's social projection without rewriting Journey history.

## 10. Shared Journey compatibility

Canonical Shared Journey evidence remains attendance-backed.

The inherited P16 source still requires:

- `evidence_basis = attendance`;
- positive verified attendance.

WU6.7 contract contains no insert/update/delete path for:

`journey_shared_experience_edges`

Ephemeral DB QA verified after Posts, Comments, Replies, Appreciations, Notifications, Reports, Blocks and Moderation:

- all attendance fields remain unresolved/null;
- Shared Journey edge count remains 0;
- all Application Windows remain closed.

## 11. Product source truth

Product PR:

`#111 — P17-WU6.7: Notification safety and Shared-Journey compatibility`

Final exact head:

`6e437043239c5167438a8c4658488f80efcad27f`

Merged product main:

`3f4360f41d042b9cc9e7ee5db9bc1e9dee2001ee`

Key source:

- `database/contracts/p17_wu6_7_notification_safety_shared_compat.sql`
- `scripts/p17-wu6-7-notification-safety-shared-qa.ts`
- `scripts/p17-wu6-7-notification-safety-shared-db-qa.sql`
- `src/lib/journeys/community-v2.ts`
- `src/components/journeys/journey-community-v2.tsx`
- `src/components/journeys/journey-post-conversation.tsx`
- `src/components/community/social-notifications-page.tsx`
- `src/routes/_authenticated/admin.social-safety.tsx`

## 12. Corrective QA history

WU6.7 exact-head QA found:

1. **Real architecture regression**  
   privileged notification source-lifecycle sync was blocked by recipient mark-read guard.  
   Fixed by restoring the P16 privileged lifecycle separation.

2. **Fixture permission error**  
   a private moderation audit assertion ran while still impersonating the authenticated Admin role.  
   The assertion was moved back to fixture owner; no private-table client grant was added.

3. **Fixture dollar-quote corruption**  
   edited assertion blocks contained malformed `DO` delimiters.  
   Delimiters were corrected only.

No product/security standard was weakened.

## 13. Exact-head evidence

Exact head:

`6e437043239c5167438a8c4658488f80efcad27f`

- dedicated gate `36123291668`: **SUCCESS**
- generic CI `36123291605`: **SUCCESS**

PASS:

- WU6.7 source/security compatibility QA;
- WU6.7 PostgreSQL notification/safety/shared-evidence QA;
- all inherited WU6 gates;
- build;
- typecheck;
- Cloudflare dry-run.

## 14. Post-merge evidence

Main:

`3f4360f41d042b9cc9e7ee5db9bc1e9dee2001ee`

- main CI `36123476885`: **SUCCESS**
- Cloudflare production: **SUCCESS**
- Cloudflare staging: **SUCCESS**

## 15. Production state

No WU6.7 Supabase production migration was applied.

Journey Community v2 remains source-first / fail-closed until WU6.Final.

Recruitment remains:

**HOLD / CLOSED**

## 16. Final decision

# **P17-WU6.7 — COMPLETE / CLOSED / PASS**

Proceed directly to:

# **P17-WU6.8 — MOBILE / VI-EN / PRIVACY / SECURITY REGRESSION QA**
