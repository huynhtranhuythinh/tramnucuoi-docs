# TRẠM NỤ CƯỜI — SOCIAL HOME REDESIGN
# CANONICAL BUILD PLAN
# JOURNEY-BASED SOCIAL NETWORK EXPERIENCE

**Date:** 2026-09-26  
**Status:** APPROVED IMPLEMENTATION PLAN / NO BUILD STARTED BY THIS DOCUMENT  
**Depends on:** `canon/SOCIAL_HOME_JOURNEY_SOCIAL_UX_CANON_V1_2026-09-26.md`

---

## 1. PURPOSE

Translate the locked Social UX V1 into a safe implementation sequence without reopening product discovery.

The build must:

- preserve current Journey operational truth;
- preserve privacy/consent/evidence boundaries;
- remain mobile-first;
- avoid a second parallel social system;
- reuse compatible Phase 16/17 social and Journey foundations;
- implement only capabilities required by the approved UX;
- verify each Work Unit before moving forward.

---

## 2. BUILD PRINCIPLES

1. **UX canon is authoritative for experience.**
2. **GitHub is source truth.**
3. **Supabase is database / operational truth.**
4. **Production runtime is deployment truth.**
5. Do not redesign the product while implementing it.
6. Do not manufacture data to make mockups appear complete.
7. Do not weaken Journey/participant/attendance/evidence/privacy truth for UI convenience.
8. Mobile is canonical; desktop follows after core mobile behavior is stable.
9. Existing compatible social capability should be reused instead of replaced.
10. Every WU requires QA evidence before closeout.

---

## 3. PROPOSED WORK UNIT SEQUENCE

### WU1 — CURRENT SOCIAL SURFACE & CAPABILITY RECONCILIATION

Goal:
- map approved UX onto current source/database/runtime;
- identify reusable Phase 16/17 capability;
- identify obsolete UI that must be replaced;
- produce exact implementation delta.

No redesign.

Deliverables:
- route/surface inventory;
- existing social capability matrix;
- flag/runtime state;
- data contract map;
- migration/no-migration decision;
- replacement plan.

Exit:
- evidence-backed implementation map.

---

### WU2 — SOCIAL DESIGN SYSTEM & GLASS NAVIGATION FOUNDATION

Goal:
Implement shared visual primitives before screen-by-screen work.

Scope:
- social typography;
- spacing;
- avatar system;
- card shell;
- Journey state presentation;
- TNC yellow accent;
- glass header;
- floating glass bottom navigation;
- scroll hide/reveal behavior;
- safe-area behavior;
- mobile navigation state.

Must support:
- Home;
- Journey;
- Journey Room;
- Community;
- Profile;
- Notifications;
- Search;
- Detail surfaces.

Exit:
- reusable components verified mobile + desktop breakpoints;
- no page-specific duplicate navigation implementations.

---

### WU3 — MOBILE SOCIAL HOME V1

Goal:
Implement approved Social Home — Phương án 4.

Scope:
- full-bleed Journey hero context;
- Companion avatar strip;
- Journey-aware composer affordance;
- Journey Pulse;
- living feed;
- feed grammar presentation;
- state-specific Home for Anonymous / Member / Applicant / Companion;
- Journey card variants;
- relationship context where truth exists.

No generic global composer.

Exit:
- five-second Home test PASS for each user state.

---

### WU4 — JOURNEY SOCIAL ROOM V1

Goal:
Implement canonical Journey Social Room.

Scope:
- Cùng nhau;
- Đồng hành;
- Khoảnh khắc;
- full-bleed Journey header;
- lifecycle modes:
  - Forming;
  - Preparing;
  - Experiencing;
  - Remembering;
- lifecycle-aware composer entry;
- people strip;
- Journey information secondary surface;
- active Journey media-first behavior;
- Memory Mode after completion.

Exit:
- before/during/after Journey UAT PASS.

---

### WU5 — AUTHORSHIP / COMPOSER / VISIBILITY

Goal:
Enforce approved Journey-based authorship.

Required rule:

> No Journey, no original social post.

Scope:
- authorship eligibility;
- participant / authorized-role creation;
- Moment;
- Story;
- Reflection / Nhìn lại;
- Organizer Official Update;
- media-first / camera-first UX;
- visibility selection;
- safe defaults;
- edit/delete own content;
- upload lifecycle and failure handling.

Security:
- server/database enforcement must match UI affordance;
- hiding composer is not sufficient enforcement.

Exit:
- unauthorized original creation blocked;
- authorized Journey creation PASS;
- visibility boundaries PASS.

---

### WU6 — INTERACTION / COMMENTS / NOTIFICATIONS

Goal:
Create familiar but disciplined social conversation.

Scope:
- Cảm xúc;
- comments;
- one-level reply presentation;
- permitted external-member interaction;
- Journey follow;
- interested state where supported;
- notification center;
- reaction aggregation;
- comment/reply direct routing;
- Journey-critical notices;
- companion notifications;
- memory/reunion notifications;
- deep links.

No engagement leaderboard.

Exit:
- contextual notification routing PASS;
- interaction permissions PASS;
- no notification flood behavior in active Journey scenarios.

---

### WU7 — COMPANION & SHARED-EXPERIENCE SURFACES

Goal:
Implement the TNC-specific relationship experience.

Scope:
- Cộng đồng;
- Người từng đồng hành;
- Sắp đồng hành;
- Cùng đội;
- Gặp lại nhau;
- People Preview;
- relationship context on Home/Journey/Profile;
- verified shared Journey history presentation.

Truth rule:
- do not infer past shared experience from application or online interaction alone.

Exit:
- relationship claims trace to canonical truth;
- privacy visibility PASS.

---

### WU8 — PROFILE / MY JOURNEY IDENTITY

Goal:
Implement Journey-derived social identity.

Scope:
- profile header;
- Journey history;
- Moments grouped by Journey;
- Reflections / memories;
- people previously accompanied;
- shared-history context for viewer;
- own-profile vs other-profile behavior;
- settings entry.

No generic profile wall.
No primary follower metric.

Exit:
- Profile tells Journey identity correctly for zero-, one- and multi-Journey users.

---

### WU9 — SEARCH / MEDIA / SHARE / DETAIL SURFACES

Goal:
Complete core browsing and object-level interactions.

Scope:
- Search:
  - Journey;
  - People;
  - accessible Moments/Stories;
- Post/Moment Detail;
- Comment Sheet;
- Media Viewer;
- share/deep link;
- Journey context menu;
- navigation return/scroll continuity.

Exit:
- deep-link return to correct object;
- media navigation PASS;
- accessible content only.

---

### WU10 — DESKTOP SOCIAL ADAPTATION

Goal:
Translate mobile canonical experience to desktop.

Scope:
- Desktop Social Home;
- Desktop Journey Room;
- Desktop Profile;
- desktop navigation;
- contextual right rail where valuable;
- larger media surfaces;
- consistent social terminology.

Do not turn desktop into an admin/dashboard product.

Exit:
- desktop preserves mobile hierarchy and meaning.

---

### WU11 — SYSTEM STATES / LOW-NETWORK / RESPONSIVE HARDENING

Scope:
- loading;
- skeletons;
- empty;
- upload processing;
- upload failure;
- offline / poor network;
- no permission;
- pending application;
- content unavailable;
- responsive breakpoints;
- safe-area / keyboard;
- scroll/nav behavior;
- upload text preservation on failure.

Exit:
- all canonical states have human UX;
- no technical/system leakage.

---

### WU12 — SECURITY / PRIVACY / MOBILE / VI-EN REGRESSION QA

Verify:

- authorship enforcement;
- Journey access scope;
- publication/visibility boundaries;
- participant/companion truth;
- no unauthorized private content exposure;
- social moderation/reporting compatibility;
- mobile iOS/Android-class browser behavior;
- VI/EN parity where product supports bilingual experience;
- deep links;
- navigation;
- media;
- comments;
- notifications;
- no regressions to operational Journey workflows.

Exit:
- evidence-backed QA PASS.

---

### WU13 — PRODUCTION PILOT & OWNER UAT

Use a real Journey / controlled participant set.

Verify role-by-role:

- Anonymous;
- Member;
- Applicant;
- Companion;
- Organizer;
- Owner/Admin only where relevant.

UAT paths:

- discover;
- apply;
- approval transition;
- enter Journey Room;
- create Moment;
- comment/reply;
- media;
- notification;
- before/during/after lifecycle;
- memory;
- companion relationship;
- next-Journey continuity.

Do not manufacture pilot truth.

---

### WU FINAL — PRODUCTION CUTOVER & CANONICAL CLOSEOUT

Required evidence:

- product main SHA;
- merged PR references;
- CI PASS;
- Supabase migration truth if any;
- production runtime version;
- production flags;
- role-based UAT;
- mobile/desktop visual QA;
- security/privacy regression results;
- Owner approval.

Only then mark Social Home Redesign implementation:

**COMPLETE / PASS.**

---

## 4. IMPLEMENTATION PRIORITY

### P0 — Core identity
WU1–WU5

Without these, product still does not become the approved TNC Social experience.

### P1 — Social continuity
WU6–WU9

Turns the core Journey Room into a complete social product.

### P2 — Completion / release
WU10–WU Final

Desktop, hardening, production evidence and closeout.

---

## 5. BUILD ORDER RULE

Do not start with database changes.

Implementation order inside each WU should normally be:

1. confirm current truth;
2. map UX to existing capability;
3. decide minimum data delta;
4. build reusable UI;
5. enforce authorization;
6. QA local/source;
7. merge;
8. verify runtime/production only when that WU requires deployment.

---

## 6. UX ACCEPTANCE GATES

No WU may close if it violates any of these:

- Journey remains primary social object;
- no generic original post outside Journey;
- approved participant/authorized role governs authorship;
- Companion relationships come from Journey truth;
- publication remains separate from operational truth;
- Glass Navigation System is consistent;
- mobile remains canonical;
- social UI remains familiar;
- no admin/dashboard language leaks into participant UX;
- no engagement-farming mechanics.

---

# FINAL BUILD-PLAN STATUS

**APPROVED / READY TO START WU1 — CURRENT SOCIAL SURFACE & CAPABILITY RECONCILIATION.**

No implementation code has been authorized by this planning document alone; it defines the sequence for the next build room.
