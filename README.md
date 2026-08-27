# TRẠM NỤ CƯỜI — CTO DOCUMENTATION REPOSITORY

Repository này là **canonical documentation repository** của dự án:

# TRẠM NỤ CƯỜI — WEBSITE 2026

Owner: Jean Huỳnh  
CTO / Product Architect: ChatGPT  
Builder: Lovable

---

## 1. PURPOSE

`tnc-docs` lưu trữ:

- product vision;
- architecture decisions;
- database design;
- infrastructure decisions;
- operational runbooks;
- audit records;
- phase handoffs;
- canonical system state;
- verification evidence;
- release and deployment history.

Repository này **không phải source-code repository**.

Source/product code được lưu riêng tại:

`~/dev/tramnucuoi`

Documentation canonical được lưu tại:

`~/dev/tramnucuoi-docs`

---

# 2. REPOSITORY STRUCTURE

```text
tramnucuoi-docs/
├── 00_INDEX/
├── 01_PRODUCT_VISION/
├── 02_PLATFORM/
├── 03_OPERATIONS/
├── 04_ROADMAP/
├── 05_DATABASE_DESIGN/
├── 06_AUDIT/
├── 07_JOURNEY_MVP/
├── 08_HANDOFF/
│
├── canon/
├── evidence/
├── handoff/
│
└── README.md
```

---

# 3. CORE DOCUMENT CLASSES

## `canon/`

**Current Truth — trạng thái chuẩn hiện tại của hệ thống.**

Canon trả lời câu hỏi:

> “Hệ thống TRẠM NỤ CƯỜI hiện tại chính xác đang như thế nào?”

Nội dung phù hợp:

- canonical architecture;
- canonical infrastructure state;
- canonical providers;
- canonical domains;
- canonical production/staging topology;
- approved configuration;
- approved operational rules;
- current release state;
- current product decisions.

Canon **không dùng để ghi nhật ký thử nghiệm dài dòng**.

Nếu canon và tài liệu lịch sử mâu thuẫn, **canon mới nhất đã được Owner/CTO chấp thuận là nguồn ưu tiên**.

Ví dụ:

```text
canon/
└── INFRA_CANON_2026-08-27.md
```

---

## `evidence/`

**Proof — bằng chứng dùng để xác nhận PASS / FAIL.**

Evidence trả lời câu hỏi:

> “Dựa vào đâu chúng ta kết luận trạng thái canonical này là đúng?”

Nội dung phù hợp:

- test results;
- smoke-test records;
- production verification;
- deployment evidence;
- migration verification;
- screenshots referenced by description;
- PR / commit references;
- DNS verification;
- provider verification;
- Owner-confirmed manual tests;
- PASS / FAIL results.

Evidence không tự định nghĩa architecture mới.

Architecture hoặc trạng thái chuẩn phải được ghi vào `canon/`.

Ví dụ:

```text
evidence/
└── INFRA_EVIDENCE_2026-08-27.md
```

---

## `handoff/`

**Current Continuation Package — gói bàn giao hiện hành.**

Handoff trả lời câu hỏi:

> “Nếu mở một topic/session mới ngay bây giờ, cần biết gì để tiếp tục đúng?”

Nội dung phù hợp:

- current phase status;
- completed work;
- open gates;
- Owner approvals;
- blockers;
- next recommended action;
- exact repositories / branches / runtimes;
- references tới canon và evidence liên quan.

`handoff/` chỉ giữ các handoff **đang hữu ích để tiếp tục công việc hiện tại**.

Khi handoff trở thành lịch sử phase, có thể archive vào thư mục phase tương ứng như `08_HANDOFF/`.

---

# 4. NUMBERED DIRECTORIES

Các thư mục đánh số lưu **project history và domain documentation**.

## `00_INDEX`
Master indexes và navigation.

## `01_PRODUCT_VISION`
Product vision, positioning, principles và North Star.

## `02_PLATFORM`
Platform architecture, application architecture và system design.

## `03_OPERATIONS`
Operational procedures, deployment rules, runbooks và maintenance.

## `04_ROADMAP`
Roadmap, milestones, future phases và strategic sequencing.

## `05_DATABASE_DESIGN`
Database schema, RLS, migrations và data architecture.

## `06_AUDIT`
Technical audits, gap analyses và formal review records.

## `07_JOURNEY_MVP`
Journey MVP implementation history và related documentation.

## `08_HANDOFF`
Historical handoff archive theo Phase 8.

Ví dụ:

```text
08_HANDOFF/
├── 00_PHASE_8_HANDOFF_INDEX.md
├── 08_PHASE_8_1/
├── 08_PHASE_8_2/
└── 08_PHASE_8_3/
```

---

# 5. `handoff/` VS `08_HANDOFF/`

### `handoff/`
Current / active handoff.

Dùng để tiếp tục công việc hiện tại giữa:

Owner → CTO → Builder → session/topic tiếp theo.

### `08_HANDOFF/`
Historical Phase 8 archive.

Dùng để lưu lại quá trình đã hoàn thành theo phase/work unit.

Quy tắc:

> Current handoff lives in `handoff/`.  
> Historical phase handoff lives in numbered phase archive.

---

# 6. CANONICAL PRIORITY ORDER

Khi có nhiều tài liệu nói về cùng một vấn đề, ưu tiên theo thứ tự:

1. Owner-approved current `canon/`
2. Current `handoff/`
3. Current evidence supporting the canon
4. Domain documentation in numbered directories
5. Historical handoff / audit records
6. Old implementation notes

Không được dùng tài liệu cũ để ghi đè canonical decision mới hơn.

---

# 7. EVIDENCE RULE

Không đánh dấu một infrastructure, migration hoặc release gate là `PASS` chỉ vì code đã tồn tại.

PASS cần evidence thích hợp.

```text
Code implemented
≠
Production verified
```

Một production infrastructure PASS thường cần tối thiểu:

```text
implementation
+ deployment
+ runtime configuration
+ functional smoke test
+ rollback understanding
```

---

# 8. OWNER GATE RULE

Các thay đổi có rủi ro cao cần Owner Gate trước khi thực hiện.

Bao gồm:

- production deploy;
- database migration;
- DNS cutover;
- auth changes;
- provider migration;
- storage deletion;
- production email activation;
- destructive cleanup;
- security policy changes.

Preferred flow:

```text
AUDIT
→ PLAN
→ STAGING
→ VERIFY
→ OWNER GATE
→ PRODUCTION
→ EVIDENCE
→ CANON
```

---

# 9. PRODUCTION RELEASE MODEL

Canonical release model:

```text
Lovable
    ↓
GitHub develop
    ↓
Cloudflare staging
    ↓
QA / verification
    ↓
Owner Gate
    ↓
GitHub main
    ↓
Cloudflare production
```

Branch semantics:

```text
develop = staging / development
main    = production-approved
```

Do not blindly merge `develop → main` when unrelated unfinished work exists.

Prefer narrow, reversible production releases when necessary.

---

# 10. CLOUDFLARE RUNTIME CONFIG RULE

Dashboard-managed runtime variables and secrets are canonical runtime configuration.

**Never deploy Cloudflare Worker without `--keep-vars`.**

Canonical production commands:

```bash
bun run cf:build
bun run cf:prod:deploy
```

Production Worker:

```text
tramnucuoi
```

Staging Worker:

```text
tramnucuoi-staging
```

Production Worker builds only from:

```text
main
```

---

# 11. SECRETS POLICY

Never store secret values in:

- Git;
- Markdown documentation;
- screenshots committed to repository;
- frontend source code;
- chat handoff documents.

Documentation may record **secret variable names**, but never values.

Examples:

```text
RESEND_API_KEY
BUNNY_STORAGE_ACCESS_KEY
```

are acceptable.

Their actual values are not.

---

# 12. CURRENT CORE PROVIDERS

At the time this repository convention was established:

```text
DNS / Edge / Runtime
→ Cloudflare

Source Control
→ GitHub

Builder
→ Lovable

Database / Auth / CMS Metadata
→ External Supabase

Media Storage / CDN
→ Bunny

Transactional Email
→ Resend
```

Exact current state must always be confirmed against the latest files in:

```text
canon/
```

---

# 13. NAMING CONVENTION

Recommended naming:

```text
<SCOPE>_CANON_YYYY-MM-DD.md
<SCOPE>_EVIDENCE_YYYY-MM-DD.md
<PHASE>_HANDOFF_YYYY-MM-DD.md
```

Examples:

```text
INFRA_CANON_2026-08-27.md
INFRA_EVIDENCE_2026-08-27.md
PHASE_8_3_HANDOFF_2026-08-27.md
```

Prefer stable names for evergreen operational documents.

---

# 14. DOCUMENT UPDATE RULE

When architecture changes:

1. update implementation;
2. verify staging;
3. pass Owner Gate;
4. verify production;
5. record evidence;
6. update canon;
7. update handoff if continuation context changes.

Do not rewrite historical evidence to make it match a newer state.

Evidence is historical proof.

Canon represents current truth.

---

# 15. CTO STARTUP CHECK

Before starting a new technical work unit, CTO should review:

```text
README.md
canon/
handoff/
```

Then inspect only the relevant numbered-domain documentation.

This prevents old phase history from accidentally overriding current canonical decisions.

---

# 16. GOLDEN RULE

> Canon tells us what is true now.  
> Evidence tells us why we trust it.  
> Handoff tells us how to continue.  
> Historical folders tell us how we got here.

---

TRẠM NỤ CƯỜI — WEBSITE 2026  
CTO Documentation Repository
