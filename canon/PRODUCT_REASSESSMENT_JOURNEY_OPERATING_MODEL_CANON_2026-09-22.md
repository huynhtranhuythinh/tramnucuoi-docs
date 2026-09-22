# TRẠM NỤ CƯỜI — WEBSITE 2026
# PRODUCT REASSESSMENT & JOURNEY OPERATING MODEL CANON

**Date:** 2026-09-22  
**Status:** CANON LOCKED / PRODUCT REVIEW COMPLETE / BUILD HOLD  
**Owner:** Jean Huỳnh  
**CTO / Product Architect / Product Strategist:** ChatGPT  
**Canonical docs repo:** huynhtranhuythinh/tramnucuoi-docs  
**Product repo:** huynhtranhuythinh/tramnucuoi  
**Production:** https://tramnucuoi.com  
**Supabase project ref:** iwiqprhoohkxvjyxojto  
**Cloudflare Worker:** tramnucuoi

---

## 1. PURPOSE

This document closes the Product Review / Reassessment Room opened after team feedback and becomes the canonical product reference for the next build cycle.

It supersedes any earlier product interpretation that conflicts with the decisions below, while preserving earlier technical/security/identity/privacy invariants that remain compatible.

This document does **not** authorize implementation.

Until a separate Owner instruction explicitly says **APPROVE BUILD**:

- no migration;
- no production mutation;
- no feature-flag change;
- no deploy;
- no Lovable implementation;
- no code refactor solely to match this canon.

P16-WU10 remains suspended under Product Rebase Hold.

---

## 2. FINAL PRODUCT THESIS

TRẠM NỤ CƯỜI is a **Journey-Based Social & Operating Platform**.

**Hành Trình — not Post and not Person — is the primary operational, participation and social object.**

TNC exists to make a real Hành Trình:

- easy to discover;
- easy to understand;
- easy to join;
- easy to operate;
- easy to contribute to;
- easy to verify;
- easy to remember;
- and capable of reconnecting people around future real-world Hành Trình.

TNC social features exist to extend meaningful real-world relationships, not maximize time-on-platform, popularity, follower counts or content volume.

The public website does not need to explain this architecture to first-time visitors. Public users should simply understand who TNC is, what TNC does, what has actually happened, why TNC can be trusted, and how to participate.

---

## 3. CANONICAL TERMINOLOGY

Owner-locked terminology:

- VI: **Hành Trình**
- EN: **Journey**

Do not use “Chuyến Đi” as the primary UX term.

“Event” may remain in legacy/internal technical names where changing it creates unnecessary engineering risk, but public UX should use Hành Trình / Journey.

---

## 4. CORE TRUTH INVARIANTS — KEEP

The following invariants remain canonical:

- registration != attendance
- approved participation != attendance
- attendance NULL = unresolved
- attendance 0 = verified no-show
- attendance > 0 = verified attended
- participant claim != attendance
- account != participant
- social activity != evidence
- operational truth != public visibility
- same Journey context != proof that two people physically attended together
- relationship role != CMS permission
- public visibility is a separate consent/publication decision
- no fake Community
- no fake impact
- no fake attendance
- no fake shared experience

Historic participation may later be linked to a newly created account through verified claim, but the claim only links identity to existing truth. It never manufactures or changes attendance, role, team, evidence or historical participation.

---

## 5. PRIMARY PRODUCT OBJECTS

### 5.1 Dự án / Project

A Project represents a long-term impact objective.

It answers:

> TNC is trying to create what change, for whom, and over what longer-term context?

A Project may contain many Journeys over months or years.

A Journey may belong to a Project, but Project membership should not be mandatory for every Journey.

Project-level information may include:

- long-term objective;
- beneficiary context;
- geography;
- impact model;
- accumulated verified results;
- related Journeys;
- partners;
- stories;
- project-specific extensions.

### 5.2 Hành Trình / Journey

A Journey represents a real-world activity with a concrete time, place, purpose and participant context.

It is the primary object for:

- public discovery;
- volunteer recruitment;
- participant approval;
- teams and assignments;
- operational planning;
- donations/resources;
- official communications;
- attendance;
- evidence;
- Community;
- closeout;
- results;
- Memory;
- shared Journey relationships.

### 5.3 Campaign

Campaign is **not a required core product object** at this stage.

In current TNC operating reality, material donation needs normally exist to prepare a specific Journey, and TNC itself takes those resources into the field rather than raising them as an independent campaign and simply transferring them elsewhere.

Therefore donation is, by default, a **Journey capability**, not a separate top-level Campaign object.

The word “campaign” may still be used editorially/communications-wise where useful.

If future operations create truly independent campaigns, the product model may be revisited with Owner approval.

---

## 6. JOURNEY LIFECYCLE

One Journey object changes emphasis over time. Do not build separate products for Before / During / After.

### BEFORE

Primary needs:

- public understanding;
- registration;
- volunteer staffing;
- donations/resources;
- approval;
- team assignment;
- runbook;
- preparation;
- official updates;
- Community preparation.

### DURING

Primary needs:

- mobile operational view;
- attendance;
- runbook;
- team responsibilities;
- official updates;
- live Community;
- evidence capture;
- resource distribution tracking.

### AFTER

Primary needs:

- reflection period;
- attendance resolution;
- donation/resource reconciliation;
- closeout;
- verified results;
- official story;
- official media;
- impact publication;
- Memory Mode;
- reconnection.

A Journey must not be considered fully complete merely because its calendar date has passed.

---

## 7. JOURNEY SOCIAL WINDOWS

Social permissions are Journey-governed and time-governed.

The platform must conceptually separate:

### Publishing Window

When eligible approved participants may create new Journey posts.

Default policy may use a configurable interval before and after the Journey date, such as 15 days before through 15 days after, but this must not be hardcoded.

Admin can configure the publishing window per Journey.

### Interaction Window

When eligible users may continue reacting, commenting and replying.

This may be:

- time-limited;
- extended;
- or unlimited.

Publishing may close while interaction remains open indefinitely.

### Sharing / Publication Policy

Public sharing is separate from both publishing and interaction.

A participant's ability to create a post does not automatically make that post public or shareable.

---

## 8. SOCIAL MODEL

There is no global free-form posting.

Every social post has a Journey origin.

### Non-participant

A normal TNC user who is not an approved participant may, subject to Journey policy:

- browse public content;
- react;
- comment;
- follow the Journey;
- donate;
- register for participation.

They may not create an original Journey post.

### Approved participant

Approved participants may receive Journey publishing rights during the configured publishing window.

Approved participation allows preparation and social contribution before the real-world date, but does not prove physical attendance.

### Verified participant

Verified attendance/evidence is required before the platform may state that a person actually participated in the Journey or shared the real-world experience.

### Social graph

TNC prioritizes a **Shared Journey Graph**, not a Friend/Follower Graph.

Do not make follower counts, influencer mechanics, popularity rankings or friend acquisition the foundation of TNC.

Shared Journey truth may support later consented reconnection, but:

- same Journey != friendship;
- same team != friendship;
- interaction != shared attendance.

---

## 9. FAMILIAR INTERACTION, TNC MEANING

Social UX should use familiar interaction conventions so users do not need to learn a new social interface.

Examples of familiar mechanics:

- avatar + display name;
- timestamp;
- composer;
- image/video post;
- reaction;
- comment;
- reply;
- notification;
- three-dot action menu.

TNC should not clone Facebook's attention economy.

Brand terminology may replace selected familiar words when the new term remains immediately understandable.

Examples still subject to UX testing:

- Share -> “Lan tỏa”
- Like/reaction -> possible TNC-specific wording such as “Ghi nhận” or “Đồng cảm”
- “Bình luận” should remain familiar unless evidence shows a better term

Do not change vocabulary merely for branding if it makes the interface harder to understand.

---

## 10. JOURNEY ROLES

Journey Role is Journey-scoped, not a global account role.

There are exactly three primary Journey roles:

1. **Ban tổ chức (BTC)**
2. **Tình nguyện viên (TNV)**
3. **Bản địa**

A person may be BTC in one Journey, TNV in another, Bản địa in another, and only a visitor elsewhere.

Department, Team, Team Lead, skill and task are not Journey roles.

---

## 11. BENEFICIARY != PARTICIPANT

Local beneficiaries, especially children or families receiving support, must not automatically become social participants.

Examples:

- local teacher coordinating logistics -> may be Bản địa participant
- village representative helping operations -> may be Bản địa participant
- child receiving gifts -> beneficiary, not automatically participant
- 300 children receiving support -> beneficiary population, not 300 social accounts

Do not create social profiles for children merely to attach them to a Journey.

---

## 12. PARTICIPANT CLAIM

A participant may exist without an account, especially a Bản địa participant.

Later, that person may create a TNC account and submit a verified claim to link the account to the historical participant record.

Claim rules:

- claim does not create a new historical participation record;
- claim does not alter attendance;
- claim does not alter Journey Role;
- claim does not alter team assignment;
- claim does not create evidence;
- one historical participant record must not silently link to multiple active accounts;
- disputed claims require human review;
- successful/rejected claims retain an audit trail.

After a verified claim, eligible historical Journey/Memory context may appear in My TNC.

---

## 13. VOLUNTEER STAFFING MODEL

Keep these concepts separate:

- Journey Role
- Department / Team
- Task / Assignment
- Skill

Example:

- Role: TNV
- Department: Media
- Assignment: Photograph gift distribution
- Skill: Photography

Departments are configured per Journey.

They must not be globally hardcoded as mandatory for every Journey.

Reusable templates are allowed, but the actual staffing structure belongs to the Journey.

---

## 14. VOLUNTEER APPLICATION

Volunteer registration must be generated from Journey staffing needs rather than duplicating configuration in a separate form builder.

BTC defines needs such as:

- Media — 5 people
- Logistics — 10 people
- Medical — 2 people
- Activities — 8 people

The public application reflects the currently open needs.

Application should support department preferences rather than letting applicants dictate final assignment.

Recommended user logic:

- Preference 1
- Preference 2
- optional Preference 3

Final assignment belongs to BTC.

The application lifecycle must keep separate truth for:

- Pending
- Approved
- Waitlist
- Declined
- Assigned
- Attended / No-show

Approval and assignment are logically distinct.

---

## 15. APPLICATION FORM DESIGN

Use progressive, mobile-friendly steps rather than one long form.

Conceptual sections:

1. Journey context and basic requirements
2. Personal/contact information actually required
3. Department preferences
4. Conditional skills/experience questions
5. Availability/logistics
6. Consent and submit

Do not collect data merely “just in case.”

After submission, clearly communicate:

> Registration has been received and is pending BTC review.

Submission is not confirmation of participation.

---

## 16. EVENT MANAGEMENT / JOURNEY CONTROL CENTER

BTC operates a Journey through one Journey Control Center.

Do not build a generic enterprise project-management suite.

Core areas:

1. Journey Setup
2. People & Applications
3. Teams & Assignments
4. Plan & Runbook
5. Resources & Donations
6. Communications
7. Community
8. Day-of Operations
9. Attendance / Evidence
10. Closeout & Results

The overview should answer:

> What is missing or needs action now?

Useful summary signals may include:

- required volunteers vs approved;
- unassigned participants;
- missing resources;
- incomplete preparation items;
- days remaining;
- social state;
- unresolved attendance;
- closeout readiness.

---

## 17. RUNBOOK, NOT ASANA/JIRA

Journey planning should remain lightweight and Journey-specific.

A task generally needs no more than:

- what;
- team;
- responsible person;
- due time/date;
- status;
- note.

Simple states are sufficient:

- Chưa làm
- Đang làm
- Hoàn thành

A Journey runbook should support a chronological operational schedule, e.g. meeting time, departure, arrival, activities, distribution and return.

Do not build:

- full Gantt;
- sprint mechanics;
- story points;
- deep dependency graph;
- payroll;
- time tracking;
- enterprise procurement;
- generic document drive;
- ERP;
- generic CRM.

---

## 18. PARTICIPANT AREA

After approval, the user should not be sent to a technical “Volunteer Dashboard.”

They should remain inside the same Journey, now with a personalized participant experience.

The Participant Area is a **Personal Journey Workspace**.

It should quickly answer:

- Am I approved?
- What is my Journey Role?
- Which team am I in?
- What do I need to do?
- What do I need to prepare?
- When and where do I need to be?
- Who is my team/lead?
- What official updates have changed?

Conceptual participant navigation should remain small, for example:

- Hôm nay / Tổng quan
- Kế hoạch
- Nhóm của tôi
- Cộng đồng
- Thông tin Hành Trình

The exact final labels require UX review.

---

## 19. OFFICIAL UPDATE != COMMUNITY POST

Operational communications must not be buried inside the social feed.

BTC must be able to issue official updates to audiences such as:

- all participants;
- BTC only;
- TNV;
- a specific department/team.

Official updates may include:

- time changes;
- meeting location changes;
- preparation instructions;
- weather/logistics changes.

A BTC member may also write a normal social post. That does not make it an Official Update.

---

## 20. COMMUNITY CONTENT MODEL

Keep post types intentionally small.

Core social post formats:

- text;
- photo/album;
- video;
- text + media.

Official Update is a distinct content/communication type.

Do not add by default:

- Reels clone;
- Stories clone;
- poll platform;
- quiz;
- livestream platform;
- generic groups;
- event-inside-event;
- article CMS inside Community.

Feed ordering should default to chronological/recent activity rather than engagement-maximizing ranking.

---

## 21. PRIVACY / CONSENT / PUBLICATION

Participant access is not public consent.

Participant publishing rights are not public publication rights.

Public publication rights are not the same as permission for TNC to reuse content as official media.

Conceptually separate:

1. Participant-only content
2. Public Journey content
3. Official TNC content

Participant posts should be privacy-first by default.

For sensitive Journeys, participant posts may be restricted to Journey participants unless BTC explicitly approves publication.

### Higher protection subjects

Children and vulnerable/local communities require stronger publication controls.

Do not assume:

- participation consent = photography consent;
- photography consent = public publication consent;
- public publication consent = perpetual official reuse permission.

TNC should preserve provenance when Community media is curated into official Journey media.

---

## 22. “LAN TỎA” / SHARING

A sharing action may appear only for content that is eligible for public publication.

Participant-only or restricted content must not receive a platform-provided public sharing action.

Where possible, sharing should preserve Journey context rather than distributing an orphaned post.

---

## 23. SOCIAL DELETION VS OPERATIONAL TRUTH

A user may control their own social content within policy:

- edit;
- delete;
- change allowed visibility;
- request public unpublication.

These actions must not rewrite independent operational truth such as:

- attendance;
- participant status;
- resource receipt;
- assignment history;
- canonical evidence.

Social moderation must not become a mechanism for rewriting operational history.

---

## 24. DONATION MODEL

Donation is a Journey capability with two distinct flows.

### 24.1 Hiện kim / Cash

TNC may explain the purpose and need, then link users to the appropriate official external channel.

The Web App should not become a payment/custody platform unless Owner later approves a separate financial architecture.

### 24.2 Hiện vật / In-kind

Track at least:

- Need
- Pledge
- Received
- Remaining

During/after Journey, also distinguish:

- Received
- Distributed
- Remaining / surplus / unresolved

Example:

Need: 1,000 shirts  
Donor pledges: 10  
BTC actually receives: 9  
Remaining need is based on verified received quantity, not the pledge.

Pledge != Received.

Received != Distributed.

---

## 25. CLOSEOUT

A Journey must not automatically become fully complete because the date passed.

Closeout is a human-confirmed operational step.

At minimum BTC should reconcile:

- attendance;
- completed/changed/cancelled activities;
- beneficiaries at an appropriate level;
- in-kind resources received and distributed;
- Journey-level financial summary;
- evidence;
- partners actually involved;
- official story;
- official media;
- unresolved issues/follow-up.

A Journey may be marked as:

> Hành Trình đã diễn ra — kết quả đang được tổng hợp

before final closeout.

Do not publish final impact claims from unresolved operational data.

---

## 26. VERIFIED RESULTS & PROJECT ROLLUP

Public result claims should derive from verified operational truth wherever the platform has structured evidence.

Flow:

Operational Truth -> Verified Result -> Public Impact

Verified Journey results may roll up into Project-level impact.

Do not aggregate from:

- registrations;
- unverified attendance;
- unreceived pledges;
- planned activities;
- marketing estimates presented as fact.

---

## 27. MEMORY MODE

After the active publishing period, a Journey should not disappear.

It transitions into Memory Mode.

Typical default:

- no new original posts after publishing closes;
- existing content remains viewable according to privacy;
- reactions/comments may remain open according to Interaction Window;
- official story/results remain accessible;
- participant Memory remains accessible;
- eligible Shared Journey context may support later reconnection.

Memory Mode must not be presented merely as “Event CLOSED.”

It represents a real past experience with continuing social value.

---

## 28. MY TNC

My TNC is a personal aggregation/home surface.

It is not a global posting surface.

It may show:

- upcoming approved Journeys;
- pending applications;
- waitlist states;
- important Journey updates;
- interactions relevant to the user;
- verified attended Journey history;
- Memories.

Every aggregated social item retains its Journey origin and links back to the Journey.

No global composer.

---

## 29. PUBLIC WEBSITE / DISCOVERY / TRUST

The public website has five main jobs:

1. Explain who TNC is.
2. Show what TNC is doing.
3. Show what has actually been achieved.
4. Build trust with traceable evidence, people, partners and transparency.
5. Make participation/contribution obvious.

The homepage should not explain internal social architecture.

### Public Journey page

Before the Journey, emphasize:

- purpose;
- beneficiary context;
- time/place;
- activities;
- volunteer needs;
- donation needs;
- partners;
- requirements;
- registration.

After closeout, emphasize:

- verified results;
- official story;
- official media;
- beneficiaries at appropriate granularity;
- resources distributed;
- financial summary where approved;
- partners;
- public Community Memory where allowed;
- related/next Journey.

---

## 30. PUBLIC INFORMATION ARCHITECTURE

Canonical IA direction:

- Trang chủ
- TNC
- Dự án
- Hành Trình
- Tác động
- Đồng hành
- My TNC for authenticated users

Do not expose internal product architecture as top-level public navigation.

Do not make Community, Memory or Event Management primary public nav items merely because they exist technically.

Use object-centered navigation:

> User opens a Journey; the system reveals the correct experience based on identity, role, lifecycle and permissions.

---

## 31. TRUST MODEL

Trust should be demonstrated, not merely claimed.

Important trust surfaces:

- current team and governance;
- team by term/mandate;
- organizational history;
- verified Journey history;
- scoped partners;
- donation/resource transparency;
- financial/operational summaries where appropriate;
- evidence/media provenance;
- privacy/safeguarding;
- official contact information.

### Team by term

Approved.

Current operating team should be represented by term/mandate where appropriate, with historical continuity rather than silently replacing prior teams.

### Ambassadors

Approved as a distinct public category.

Ambassadors / Đại sứ đồng hành must not be confused with governance or operational authority.

### Contact

Official Facebook, hotline and other authoritative contact methods should be easy to find.

---

## 32. PARTNER ATTRIBUTION

Partner relationships must be scoped.

A partner may be associated with:

- TNC overall;
- a Project;
- a specific Journey.

Partner types may include:

- Bảo trợ
- Đồng hành
- Tài trợ
- Bảo trợ truyền thông
- Đối tác địa phương
- other Admin-configured categories

Do not create a context-free logo wall as the main representation of partnership.

Closeout should confirm actual Journey partner participation where relevant.

---

## 33. TRÚC SÀO EXTENSION

The following team feedback is approved as a **Project-specific extension**, not a mandatory core feature for all Projects:

- map/location of planted bamboo;
- GPS/geospatial representation where data quality supports it;
- donor attribution;
- linkage to related Journey provenance;
- carbon-related data/certificate records.

Important invariant:

> Trees planted != carbon credits automatically generated.

Any carbon claim/certificate requires independent provenance and evidence.

Do not infer certified carbon impact solely from plant counts.

---

## 34. “REVIEW” FEEDBACK

Do not build generic star ratings by default.

For TNC, a more credible model is:

- participant reflection;
- participant story;
- “Chia sẻ sau Hành Trình”;
- verified participant testimonial where appropriate.

If public testimonials are curated, they remain subject to consent/publication rules.

---

## 35. VI / EN STRATEGY

TNC remains bilingual VI / EN.

Use one platform and one truth model.

Do not assume English should be a literal word-for-word copy of Vietnamese editorial hierarchy.

Vietnamese audiences may prioritize:

- upcoming Journeys;
- joining;
- current needs;
- contribution.

International donor/fund/partner audiences may need earlier access to:

- organization identity;
- governance;
- impact methodology;
- verified outcomes;
- transparency;
- safeguarding/privacy;
- project continuity;
- partnership contact.

Structured public content should support bilingual authoring where required.

Internal operational content does not need to be bilingual unless there is a real operating need.

---

## 36. MOBILE PRIORITY

Participation and field operations must be genuinely mobile-first.

Especially on Journey day, BTC/TNV should not need desktop-style dashboards.

Mobile should prioritize:

- what is happening now;
- next schedule item;
- official update;
- team/task;
- attendance/check-in where applicable;
- Community;
- critical operational actions.

Avoid heavy decorative presentation that pushes core actions below long mobile pages.

---

## 37. ANTI-SCOPE BOUNDARY

Do not build the following as core product direction without new Owner-approved evidence:

- global user posting;
- global Facebook-style feed;
- follower economy;
- influencer metrics;
- popularity ranking;
- engagement streaks;
- generic user-created public groups;
- generic friend graph as the primary relationship model;
- Reels/Stories clone;
- generic Messenger clone;
- full enterprise project-management suite;
- Gantt/dependency platform;
- ERP;
- accounting system;
- payroll;
- warehouse ERP;
- generic CRM;
- TNC-controlled crowdfunding/payment custody;
- generic cloud drive.

Feature test:

> Does this materially help people discover, prepare for, participate in, operate, verify, remember or reconnect around a real Journey?

If not, default to not building it.

---

## 38. PRODUCT LAYERS — FINAL MAP

TNC consists conceptually of six product layers:

1. **Public / Trust** — discovery, credibility, participation entry
2. **Project** — long-term impact context
3. **Journey** — primary operational + participation object
4. **Journey Community** — social before/during/after Journey
5. **Memory & Relationship** — verified shared experience and reconnection
6. **My TNC** — personal aggregation of relevant Journey history/activity

Admin is not a separate seventh product. It is a management perspective over Project/Journey truth.

---

## 39. END-TO-END USER FLOW

Visitor
-> understands TNC
-> discovers Project / Journey
-> sees real purpose, needs and trust evidence
-> submits TNV application
-> Pending
-> BTC approves
-> participant access opens
-> team/assignment is confirmed
-> prepares through Web App
-> participates in Journey Community within policy/window
-> attends real-world Journey
-> attendance/evidence is verified
-> BTC closeout
-> verified results become public
-> Journey enters Memory Mode
-> verified Shared Journey context exists
-> user may reconnect
-> user may join a future Journey

This is the canonical Living Loop after product reassessment.

---

## 40. PRIORITY REBASE

### P0 — Before next public Journey use

- reconcile Journey lifecycle/date/configuration truth;
- resolve existing pilot drift before public recruitment;
- complete required controlled QA;
- preserve identity/security/privacy protections;
- make participation states truthful;
- ensure critical mobile UX is understandable.

### P1 — Journey operations on TNC Web App

- Journey lifecycle rebase;
- staffing needs;
- departments/teams;
- volunteer application rebase;
- approval + assignment;
- Participant Area;
- official communications;
- lightweight runbook;
- in-kind donation operations;
- attendance/day-of operations;
- closeout;
- Community rebase.

### P2 — Long-term product value

- Memory Mode;
- Shared Journey Graph;
- reconnection;
- Project impact rollup;
- public impact/trust rebase;
- scoped partners.

### P3 — Project-specific / advanced

- Trúc Sào map/geospatial extension;
- carbon evidence/certificate extension;
- advanced impact visualization;
- future donor tools based on real requirements.

---

## 41. ROADMAP REBASE

Do not resume P16-WU10 under its old scope.

Phase 16 remains valuable as the social identity, consent, privacy, safety, interaction and shared-experience foundation.

The next implementation program, once explicitly approved, should be:

# PHASE 17 — JOURNEY OPERATING SYSTEM & PRODUCT EXPERIENCE REBASE

Proposed sequencing:

1. P17-WU1 — Canonical Product Model & Current-State Gap Audit
2. P17-WU2 — Journey Lifecycle & Information Architecture
3. P17-WU3 — Event Management Foundation
4. P17-WU4 — Volunteer Application & Assignment Rebase
5. P17-WU5 — Participant Journey Experience
6. P17-WU6 — Journey Community Rebase
7. P17-WU7 — Donation & Resource Operations
8. P17-WU8 — Day-of Operations & Attendance
9. P17-WU9 — Closeout, Results & Memory
10. P17-WU10 — Public Discovery & Trust Rebase
11. P17-WU11 — Production Pilot Re-activation

This sequence is canonical roadmap direction, not build authorization.

---

## 42. CURRENT PILOT / PRODUCTION HOLD

P16-WU10 volunteer pilot remains suspended for product rebase.

Current review does not mutate production dates or Journey data.

Earlier project context recorded a date drift between initial brief and production for the Trung Thu pilot. The later 30/09/2026 date used during product discussion was an illustrative operating example and must not be treated as an automatic production data correction.

P17-WU1 must inspect GitHub, Supabase and production truth before any Journey is reopened publicly.

Public recruitment remains HOLD until a new explicit gate passes.

---

## 43. CANON PRECEDENCE

For future implementation:

1. Read this Product Reassessment Canon first for product intent and product boundaries.
2. Preserve compatible technical/security/privacy truth from prior Phase 16 canon.
3. In P17-WU1, audit actual source/database/runtime before designing implementation changes.
4. Where an older product/UX assumption conflicts with this document, this document is authoritative unless later superseded by a newer Owner/CTO-approved canon.
5. Do not interpret this canon as permission to delete functioning foundations without evidence. Prefer reuse/recomposition where technically sound.

---

## 44. FINAL STATUS

- Product Review / Reassessment: **COMPLETE**
- New product model: **LOCKED**
- Project / Journey / Donation model: **LOCKED**
- Journey lifecycle: **LOCKED**
- Event Management model: **LOCKED**
- Volunteer staffing/application model: **LOCKED**
- Participant Area: **LOCKED**
- Participant Model: **LOCKED**
- Social Model: **LOCKED**
- Privacy / Consent / Publication model: **LOCKED**
- Closeout / Results / Memory model: **LOCKED**
- Public Website / Discovery / Trust model: **LOCKED**
- Operations/Admin model: **LOCKED**
- VI/EN strategy: **LOCKED**
- Anti-scope boundary: **LOCKED**
- Phase 17 roadmap direction: **APPROVED AS CANONICAL DIRECTION**
- P16-WU10: **SUSPENDED / PRODUCT REBASE HOLD**
- Build authorization: **NOT YET GRANTED**
- Production mutation caused by this review: **NONE**

---

# CANONICAL ONE-SENTENCE TEST

> TNC should help a real Hành Trình become easier to discover, join, operate, verify, remember and reconnect around — without turning TNC into a generic social network or generic enterprise management suite.
