# TRẠM NỤ CƯỜI — ADMIN V3 MVP CANONICAL CLOSEOUT

**Date:** 2026-09-26  
**Status:** CANONICAL / OWNER-ACCEPTED MVP  
**Product repo:** `huynhtranhuythinh/tramnucuoi`  
**Docs repo:** `huynhtranhuythinh/tramnucuoi-docs`  
**Canonical product commit:** `f2195b84b5092e3632550b0fcbf04de1a4579df0`  
**Preview:** `https://id-preview--743468e9-ff94-40ee-9611-4cef7ce5b47f.lovable.app/admin`  
**Production deploy:** COMPLETED / OWNER UAT PASS
**Production Worker version:** `b18d2ffb-c99f-42a6-ad07-2235ff90dacd`

---

## 1. Purpose

Admin V3 is the canonical admin experience for TRẠM NỤ CƯỜI.

It replaces the previous technical/control-panel mental model with an Owner / operations-first workspace.

Canonical principle:

> Admin exists to help the person operating Trạm Nụ Cười understand what is happening, what needs attention, what can be done next, and what changed after an action.

Admin V3 must not require the Owner to understand database entities, technical lifecycle terminology, backend architecture, Phase/WU naming, schema, RLS, source-truth mechanics, deployment concepts, or infrastructure details.

Complexity stays behind the UI.

---

## 2. Locked product principles

1. **Task/outcome centric, not entity centric.**
2. **Vietnamese-first, natural operational language.**
3. **Data must be translated into attention + next action where possible.**
4. **No fake capability and no fake operational data.**
5. **Technical details stay out of normal Owner UI.**
6. **Progressive disclosure over giant control-center pages.**
7. **Grouped information architecture, not a flat menu.**
8. **Modules must be understandable independently and connect naturally to Hành Trình.**
9. **Existing business rules, auth, permissions, and real data remain underneath unless separately changed by a future approved phase.**
10. **Future redesign must preserve this Owner mental model unless Owner explicitly reopens the canon.**

---

## 3. Locked Admin V3 design language

Admin V3 visual language is locked as:

- premium, modern, technology-forward, intelligent;
- dark navy / charcoal admin shell and sidebar;
- TNC yellow/gold as the primary brand accent;
- warm cream/off-white workspace background;
- premium white cards with soft layered shadows;
- rounded card system, approximately 14–18px;
- strong typography hierarchy and large Vietnamese headings;
- compact status chips;
- semantic green / amber / red / blue states;
- subtle depth, grid, glass or gradient effects where useful;
- restrained hover/motion, not decorative animation;
- human warmth without becoming playful or visually noisy.

The Dashboard and Journey Portfolio established the reference visual language for all later modules.

---

## 4. Canonical left navigation

### TỔNG QUAN
- Trang chủ

### HÀNH TRÌNH
- Tất cả Hành Trình
- Chuẩn bị
- Ngày diễn ra
- Tổng kết

### CON NGƯỜI
- Đăng ký tham gia
- Tình nguyện viên
- Cộng đồng
- Phân công

### NỘI DUNG & GIAO TIẾP
- Thông báo
- Nội dung
- Hình ảnh & tư liệu

### TỔ CHỨC
- Bộ phận & Nhóm
- Vai trò & Quyền
- Thành viên quản trị

### HỆ THỐNG
- Cấu hình chung
- Bảo mật
- Tích hợp

The left navigation is module-grouped by design. Future permissioning should align to these module boundaries where appropriate, but detailed RBAC must not be invented merely for presentation.

---

## 5. Canonical module contracts

### 5.1 Trang chủ / Dashboard

Purpose:

> Daily Operating Health Center of TNC.

Within roughly 10–20 seconds, Owner should understand:
- whether anything requires attention;
- which Hành Trình needs action;
- people/application status;
- content/media items needing attention;
- recent activity;
- the most useful next action.

MVP includes:
- operational health/status summary;
- KPI cards using real data;
- “Việc cần xử lý hôm nay”;
- upcoming Hành Trình;
- staffing/people signals;
- recent activity;
- quick actions.

Future direction, not MVP:
- historical trend dashboards;
- rich interactive time-series charts;
- drill-down analytics comparable to a market/stock terminal experience.

---

### 5.2 Hành Trình

Purpose:

> The primary operational workspace for planning, running, and closing Journeys.

“Tất cả Hành Trình” includes a compact **Journey Portfolio Dashboard** before the list.

Portfolio view answers:
- how many Journeys exist;
- which are active/upcoming/completed;
- which need attention;
- participant/capacity signals when safely derivable;
- what is coming next.

Journey cards show real available information:
- title;
- date;
- location;
- human-readable status;
- participant/application signals;
- readiness/completeness;
- next action;
- image/cover when available.

Journey detail/workspace canonical tabs:
- Tổng quan
- Nhân sự
- Công việc
- Lịch trình
- Nội dung
- Thông báo
- Kết quả
- Cài đặt

Existing real application, participant, attendance, updates, media, and impact capabilities remain the underlying business mechanisms.

---

### 5.3 Con người

Purpose:

> Operational home for people participating in and forming the TNC community.

#### Đăng ký tham gia
Cross-Journey application review:
- pending applications;
- Journey context;
- review/reject/confirm actions using existing real mutations;
- natural Vietnamese statuses.

#### Tình nguyện viên
Directory/history of people actually confirmed or participating:
- confirmed people;
- recent Journey;
- participation history;
- attendance if available;
- safe identity handling without incorrect deduplication.

#### Cộng đồng
Community relationship/history view derived from real Journey participation:
- participating people;
- repeat participation when safely derivable;
- represented Journeys;
- Journey history.

It must not pretend a social graph, followers, likes, or relationship data exist when they do not.

#### Phân công
Staffing/assignment operations view.

Current MVP must remain honest about present capability:
- Journey staffing context is real;
- detailed department/team/role/task assignment must not be fabricated where backend capability is absent.

Future product direction:
- Hành Trình → Bộ phận → Nhu cầu → Người → Vai trò/Nhiệm vụ → Đủ/Thiếu.

---

### 5.4 Nội dung & Giao tiếp

Purpose:

> Help Owner understand what needs to be communicated, completed, published, or managed visually.

#### Thông báo
Current MVP uses real Journey update/publication capability.
It distinguishes public Journey updates from future direct/broadcast notification capability.

It must not pretend delivery, recipient, or broadcast metrics exist when unsupported.

#### Nội dung
Editorial operations hub, not a CMS control panel.

Canonical content areas:
- Trang chủ
- Dự án
- Tin & câu chuyện
- Nội dung Hành Trình

Key Owner signals:
- content needing completion;
- missing translation where real;
- recent updates;
- Hành Trình missing public content.

#### Hình ảnh & tư liệu
Visual media workspace:
- thumbnail-first library;
- real media totals/types;
- upload;
- search/filter;
- review state if real;
- Journey/content context where derivable.

Technical storage concepts must not surface in normal Owner UX.

---

### 5.5 Tổ chức

Purpose:

> Make organizational responsibility understandable to Owner.

#### Bộ phận & Nhóm
Current MVP is an actionable setup/readiness workspace because organization entities are not yet fully configured.

Canonical setup model:
1. Tạo Bộ phận
2. Tạo Nhóm & thêm thành viên
3. Chọn người phụ trách

These must not become fake actions if mutations do not exist.

#### Vai trò & Quyền
Owner-facing question:

> “Vai trò nào được làm gì?”

Current role information must be based on actual roles/capabilities.
Do not present a fake permission matrix or granular module permissions if backend enforcement is broad.

#### Thành viên quản trị
Directory of real people who can access Admin:
- identity;
- actual role;
- current account;
- real access metadata where available.

Do not display auth diagnostics, IDs, tokens, or backend limitation explanations.

---

### 5.6 Hệ thống

Purpose:

> Present platform-wide Owner settings, access protection, and external integrations in plain language.

#### Cấu hình chung
Shows product-level identity and settings that are meaningful to Owner.

Normal UI must not expose:
- staging;
- provider configuration;
- API keys;
- env;
- production flags;
- deployment details;
- database/runtime internals.

Developer utilities such as email-test tooling do not belong in normal Owner settings.

#### Bảo mật
Owner-facing security status:
- current account;
- current role;
- two-step authentication;
- registered authenticator;
- links to roles/admin members where useful.

No invented security scores, sessions, events, or device history.

#### Tích hợp
Shows real business integrations only when configurable.

Current MVP correctly represents that no business integration requires management in Admin.

Future integration categories may be described as “Chưa mở”, but must not appear as active providers or fake connect flows.

---

## 6. Technical language boundary

The following terms are not allowed in normal Owner-facing Admin UI unless explicitly placed in a developer-only/advanced context:

- schema
- RLS
- source truth
- Phase
- WU
- lifecycle architecture
- participant row
- activation gate
- CMS
- env
- secret
- token
- worker
- deployment
- API key
- provider
- staging
- production internals
- database implementation terms

Internal code/comments/docs may retain technical terminology.

---

## 7. Owner UAT closeout

Owner visually reviewed live preview screens throughout the rebuild, including:
- Dashboard;
- Journey Portfolio and Journey list;
- People modules;
- Content and Communication modules;
- Organization modules;
- System modules.

Owner feedback drove multiple correction passes, including:
- grouped navigation;
- stronger visual technology/premium direction;
- Journey Portfolio Dashboard;
- distinct People submodules;
- removal of technical/CMS language;
- Organization actionable zero states;
- System Owner-facing recomposition.

Final closeout status:

**Admin V3 MVP = OWNER ACCEPTED / CANONICAL.**

Production deployment, smoke verification, and final Owner UAT have now been completed. Owner confirmed the production Admin is stable and accepted the final compact explanation/tooltip pattern.

---

## 8. Verification evidence

Canonical product SHA:

`f2195b84b5092e3632550b0fcbf04de1a4579df0`

Final verification before canonical close:
- Production build: PASS
- Production cutover: PASS
- Cloudflare Worker deploy: PASS
- Production Owner smoke/UAT: PASS
- Final explanation → tooltip sweep verified in production
- No database/schema/auth/business-rule changes were introduced by the tooltip sweep
- Known technical debt: generated Admin V3 route-tree typing remains a separate follow-up; it did not block production build or Owner UAT

Preview:
`https://id-preview--743468e9-ff94-40ee-9611-4cef7ce5b47f.lovable.app/admin`

---

## 9. Intentional MVP limitations / deferred backlog

The following are explicitly deferred and must not be treated as regressions:

1. Detailed department/team organizational data is not fully configured.
2. Full assignment model by department/role/task is not yet implemented in Admin.
3. Direct broadcast notifications to arbitrary recipient lists are not yet a standalone capability.
4. Name/logo/language platform settings are not all directly editable from Admin.
5. No configurable business integrations are currently exposed.
6. No session/device/security-event history is currently available.
7. Dashboard historical analytics and animated time-series charts are deferred.
8. Fine-grained module permission management is deferred until supported by real backend enforcement.

---

## 9A. Final production UX refinement — explanation → tooltip pattern

Final production UAT established an additional canonical UX rule:

- Long explanatory paragraphs should not occupy the main operating surface when they are supporting/help content.
- Keep the primary label, status, warning, decision, and next action visible.
- Move contextual definitions and usage explanations behind a small accessible information control `(i)`.
- The information control must work with hover/focus and click/touch.
- Do not hide blockers, required actions, important warnings, or operational state inside tooltips.

This pattern was applied across relevant Admin V3 surfaces including Journey operations, attendance, tasks, content/settings/integration explanations and other explanatory-heavy areas. Dashboard and other surfaces intentionally retain information that Owner needs to see immediately.

Owner reviewed the production result on 2026-09-26 and confirmed it is satisfactory.

---

## 10. Change control / no-reopen rule

From this closeout forward:

- Admin V3 is the canonical Admin experience.
- Future work must extend this mental model, not revert to the old technical admin pattern.
- Do not reopen the full Admin IA, visual language, or Owner mental model unless:
  1. Owner explicitly requests reassessment; or
  2. production evidence shows a real blocker.
- New capabilities should fit into the existing module hierarchy first.
- Backend complexity must not leak into Owner UX merely because the underlying architecture changes.

---

## 11. Final release state

**ADMIN V3 MVP = PRODUCTION / OWNER ACCEPTED / CANONICAL CLOSED.**

Production Cutover, smoke QA, final tooltip refinement, and Owner UAT are complete. Further work is incremental product evolution only; it must not reopen the Admin V3 foundation without explicit Owner decision or production evidence of a blocker.

Required before calling Admin V3 production-released:
- Owner approval to deploy;
- sync/merge to canonical product main as appropriate;
- production deployment;
- authenticated smoke QA on production;
- verification of key routes and primary actions;
- rollback awareness.

Until that gate passes, status remains:

**CANONICAL MVP / OWNER ACCEPTED / NOT YET PRODUCTION RELEASED.**
