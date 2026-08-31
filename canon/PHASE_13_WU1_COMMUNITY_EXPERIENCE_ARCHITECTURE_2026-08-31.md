# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 / P13-WU1
# COMMUNITY EXPERIENCE ARCHITECTURE

Date: 2026-08-31
Status: **COMPLETE / PASS**

## 1. Objective

Define the Phase 13 information architecture and experience contract before implementation changes.

WU1 is intentionally architecture-first and requires no database or production mutation.

The trusted Phase 12 graph is the source of truth:

**Identity → Journey → Memory → Reflection → Contribution → Community Roles → Host Network → Partner / Impact Network**

The UX must reveal this graph without weakening its semantics.

## 2. Current-source audit

Product HEAD audited:
- `75f9511de8442fcd632429b21cfc56fb727aed7b`

### Current Community route

`src/routes/cong-dong.tsx` currently renders, in sequence:
1. `CommunityAccountPage`
2. `CommunityRolesPanel`
3. `CommunityContributionsPanel`
4. `CommunityReflectionsPanel`

English mirrors the same architecture at `/en/community`.

Finding:

The route exposes the Phase 12 capabilities but the information architecture is **panel-oriented rather than person-oriented**. A user experiences separate technical domains instead of one coherent relationship with Trạm.

### Current Community Account component

`CommunityAccountPage` currently owns several responsibilities at once:
- Magic Link sign-in;
- participant claim;
- authentication/session state;
- My Journey archive loading;
- attendance-state explanation;
- sign-out;
- signed-out education.

Finding:

This was appropriate for foundation delivery but is too broad for a durable Community product shell. Phase 13 should separate **identity/session plumbing** from **personal experience modules**.

### Current public navigation

Primary navigation currently contains:
- About;
- Ecosystem;
- operational Journeys;
- Field Journal;
- Get Involved.

Community is intentionally not yet in `NAV_PAGES`.

Finding:

This correctly preserves the Email/Auth activation gate. P13-WU1 does **not** add Community to production navigation.

### Current Journey editorial detail

Public Field Journal detail is an editorial story surface with Project relationship and evidence gallery.

Finding:

It is strong as storytelling, but it is not the signed-in person's Journey lifecycle surface. Phase 13 must keep **public editorial story** and **personal Journey relationship state** distinct but connected.

## 3. Canonical experience model

Phase 13 adopts four experience layers.

### Layer A — Public Story World

Purpose:
- explain Trạm;
- show Projects;
- show public Journeys / Field Journal;
- show approved documentary evidence;
- invite participation.

Identity requirement: none.

Primary surfaces already exist and should remain editorial.

### Layer B — Personal Community Home / “My TNC”

Purpose:
- answer “Tôi đang có mối liên hệ gì với Trạm?”

Identity requirement: authenticated Community identity.

This becomes the signed-in center of gravity for Phase 13.

Canonical information order:

1. **Identity / Welcome**
2. **My next or active Journey context** when truth exists
3. **My Journeys & Memories**
4. **My Contributions**
5. **My Community Relationships / Roles**
6. **My Reflections**
7. links back into relevant Project / Journey public stories

This order is relationship-first, not database-table order.

### Layer C — Journey Relationship Experience

Purpose:
- make Before / During / After Journey understandable for the person.

The same Journey may appear differently depending on authoritative state.

#### Before Journey
Possible truthful states:
- public opportunity only;
- application submitted;
- confirmed participation;
- identity linked;
- registration closed/full/cancelled as operational truth indicates.

Do not display confirmed participation as attendance.

#### During Journey
Possible truthful states:
- confirmed and event active;
- attendance still unresolved;
- approved Field Updates / documentary context may appear where public/trusted.

Do not infer attendance merely because event time has started.

#### After Journey
Possible truthful states:
- attendance unresolved;
- verified no-show (`0`);
- verified attended (`>0`);
- Memory eligible;
- Reflection available/pending/moderated;
- Contribution verified;
- related Community role/relationship visible if verified.

### Layer D — Public Living Community Surface

Purpose:
- show the living ecosystem through verified stories, Journeys, Contributions, Hosts/Partners and impact context.

This layer comes after My TNC and Journey relationship UX are coherent.

It must be editorial and relationship-based, never an infinite generic feed.

## 4. Route architecture

### Preserve existing canonical bilingual routes

Keep:
- `/cong-dong`
- `/en/community`

Do not introduce a breaking rename to `/my-tnc` in Phase 13.

“My TNC” is an experience concept / signed-in heading, while the stable public route remains Community.

Reason:
- WU2/WU3 already own callback URLs and claim semantics here;
- preserving route stability avoids unnecessary Auth redirect drift;
- bilingual paths already exist.

### Community route behavior

#### Signed out
`/cong-dong` is a restrained Community entry surface:
- what Community account means;
- why verified email matters;
- privacy/truth explanation;
- sign-in action;
- no fabricated activity preview.

While Email remains OFF, this state stays activation-gated/non-promoted.

#### Signed in
The same route becomes **My TNC / Personal Community Home**.

No second mandatory dashboard route is required for WU2.

This keeps one stable mental model:

**Cộng đồng → khi đăng nhập, đây là không gian của tôi trong Cộng đồng.**

## 5. Personal Home content hierarchy

### Hero / identity band

Show:
- warm personal greeting where a safe display name exists;
- otherwise neutral verified-account language;
- active relationship summary expressed narratively, not as a vanity score.

Avoid:
- “Impact score”;
- points;
- streaks;
- rank;
- follower counts.

### Journey / Memory section

Card hierarchy should prioritize:
1. current/upcoming confirmed Journey relationship;
2. attended Memories;
3. unresolved historical Journey links;
4. verified no-show as history, visually quiet and never shame-oriented.

Memory should use documentary/editorial visuals when source evidence exists, not synthetic celebratory badges.

### Contribution section

Show verified contributions as human-readable moments:
- what was contributed;
- Journey/Project context;
- date;
- explicit quantity/unit only when meaningful.

Do not total unlike units.

### Relationship / role section

Roles are presented as **relationships**, not permissions or achievement badges.

Examples:
- “Host của Journey …”
- “Đại diện đối tác cho Project …”
- “Đã đóng góp cho …”

Participant should remain evidence-derived from attendance/Memory.

### Reflection section

Show a person's own Journey Reflections with transparent state:
- draft/submitted where applicable;
- pending moderation;
- published;
- rejected/withdrawn according to canonical WU4 semantics.

Public publication state must remain separate from authorship ownership.

## 6. Empty-state architecture

Production is currently intentionally fact-clean, so empty states are first-class Phase 13 UX, not temporary placeholders.

Canonical empty states must explain **why** data is absent without inventing activity.

Examples:

### No linked Journey
“Chưa có Journey nào được xác minh với tài khoản này.”

### Attendance unresolved
“Journey đã được nối với tài khoản, nhưng attendance chưa được ghi nhận.”

### No Memory
Do not say “Bạn chưa tạo Ký ức.” Memory is evidence-backed, not manually manufactured.

Prefer:
“Chưa có Journey nào đủ căn cứ để trở thành Ký ức tham dự.”

### No Contribution
“Chưa có đóng góp nào được Trạm xác minh trong hồ sơ này.”

### No role relationship
Do not encourage users to self-assign roles.

Prefer:
“Các mối quan hệ Host / Partner sẽ xuất hiện khi được Trạm xác minh.”

## 7. Component architecture direction for P13-WU2

Refactor responsibility without changing Phase 12 database truth.

Recommended separation:

- `CommunityExperienceShell`
  - locale + layout + auth-state orchestration
- `CommunitySignInCard`
  - signed-out Magic Link UX
- `MyTncHome`
  - signed-in personal hierarchy
- `MyJourneyTimeline`
  - Journey / attendance / Memory presentation
- `MyContributions`
  - verified Contribution presentation
- `MyCommunityRelationships`
  - roles/relationships presentation
- `MyReflections`
  - owned Reflection state

Existing Phase 12 data access can be reused initially. WU2 should not create a new database merely to make the UI cleaner.

## 8. Visual direction

Phase 13 remains aligned with the established documentary editorial system:
- generous whitespace;
- warm paper/background tones;
- ink/earth typography;
- yellow accent as signal, not gamification;
- strong editorial display typography;
- documentary imagery when evidence-backed;
- calm motion/transitions;
- no SaaS dashboard chrome;
- no card-grid overload;
- no social-feed visual language.

Personal Home should feel like a **living personal field journal / community archive**, not an account settings page.

## 9. Bilingual architecture

VI / EN are equal product surfaces.

Rules:
- route parity remains mandatory;
- component structure shared where practical;
- copy can be culturally natural rather than mechanically literal;
- empty-state semantics must remain identical in truth meaning;
- no feature may ship VI-only unless explicitly Owner-approved as a temporary internal preview.

## 10. Privacy / identity presentation

Phase 13 must default to minimal identity exposure.

For personal surfaces:
- own email may be shown only as account verification context;
- applicant registration PII remains private;
- public Community surfaces should not expose auth/user UUIDs or staff verifier identifiers;
- Partner representative / Host public presentation requires a future explicit publication decision, not just existence of an internal verified relationship.

## 11. Activation boundary

P13-WU1 changes no runtime activation state.

Preserved:
- Email OFF;
- Turnstile OFF;
- Community Auth public activation GATED;
- Community route not added to primary public navigation;
- `pg_graphql` OFF;
- no Cloudflare production deployment required;
- no production database mutation required.

P13-WU2 may build source-level My TNC experience behind the existing activation gate.

Public navigation activation belongs to P13-WU6 unless a separately reviewed earlier gate explicitly authorizes it.

## 12. P11 pilot relationship

The 2026-09-11 live pilot remains operationally authoritative.

Phase 13 UI implementation must render unresolved states truthfully before the event and consume real attendance/Memory facts after they exist.

It must not accelerate visual polish by seeding fake pilot facts.

## 13. WU1 decision

P13-WU1 requires no migration and no product-source mutation.

Its deliverable is the canonical experience architecture that constrains WU2–WU6.

## 14. Closeout

**P13-WU1 — COMMUNITY EXPERIENCE ARCHITECTURE: COMPLETE / PASS**

Next work unit:

**P13-WU2 — PERSONAL COMMUNITY HOME / MY TNC**

WU2 should refactor the current stacked Community surface into the signed-in personal relationship experience defined here, while keeping Community Auth activation gated and production facts untouched.
