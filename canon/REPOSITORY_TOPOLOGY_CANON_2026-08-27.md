# TRẠM NỤ CƯỜI — REPOSITORY TOPOLOGY CANON

Date: 2026-08-27  
Status: CANONICAL  
Scope: Local/GitHub repository ownership and cross-repository operating rules.

## 1. Canonical repositories

### Source / product repository

- Owner local working copy: `~/dev/tramnucuoi`
- GitHub: `huynhtranhuythinh/tramnucuoi`
- Purpose: application source, runtime configuration, database migrations, tests, deployment scripts, CMS implementation and code-adjacent operational documentation.
- Builder: Lovable.

### CTO / project documentation repository

- Owner local working copy: `~/dev/tramnucuoi-docs`
- GitHub: `huynhtranhuythinh/tramnucuoi-docs`
- Purpose: current canon, evidence, audits, architecture decisions, Work Unit specifications, Owner approvals, release sequencing, handoffs and project history.

GitHub is the remote source of truth for both repositories. The two `~/dev/...` paths are Owner-managed local working copies.

## 2. Branch semantics

### Product repository

- `develop` = development / staging integration.
- `main` = production-approved source.
- Normal product work starts from and returns to `develop`.
- Promotion to `main` requires CTO QA and an explicit Owner production gate.
- Production Worker must build only production-approved `main` source.

### Documentation repository

- `main` = canonical documentation branch.
- Documentation changes must distinguish current canon, supporting evidence, current handoff and historical records according to `README.md`.
- Documentation commits do not authorize product deployment or database activation.

## 3. Responsibility boundary

The product repository owns what the system executes.

The documentation repository owns what the Owner and CTO currently accept as true, why that conclusion is trusted, and how work must continue.

A product implementation is not considered production-complete merely because code exists. Appropriate deployment/runtime evidence and Owner approval remain required.

## 4. Cross-repository rules

1. Product code, migration SQL and deployment configuration stay in `tramnucuoi`.
2. Current architectural truth, evidence, Work Unit decisions and release gates stay in `tramnucuoi-docs`.
3. A code-adjacent document may remain in the product repository when it is needed to build, test or operate the code.
4. When a product-repo technical record also affects project continuation, the docs repo must carry a canonical summary and an exact product path/commit reference.
5. Do not maintain two conflicting documents that both claim to be canonical.
6. Never copy secret values into either repository. Variable names may be documented; values may not.
7. Database migrations, production deploys, provider changes, Auth changes, destructive cleanup and production email activation remain Owner-gated regardless of documentation status.

## 5. Phase 8.3 sequencing rule

The product migrations below remain source artifacts in `tramnucuoi` and are intentionally unapplied until their approved release sequence:

- `0023_phase_8_3_privacy_policy.sql`
- `0024_phase_8_3_website_use_notice.sql`
- `0025_phase_8_3_editorial_trust_workflow.sql`

A documentation approval records scope and sequencing; it does not itself apply a migration or deploy production.

## 6. Current documentation priority

Use this order when continuing technical work:

1. latest Owner-approved file in `canon/`;
2. current file in `handoff/`;
3. supporting file in `evidence/`;
4. relevant numbered-domain documentation;
5. historical phase archive;
6. old product-repo implementation notes.

This record supersedes any earlier assumption that `huynhtranhuythinh/tramnucuoi` is the sole canonical repository for both product source and CTO documentation.
