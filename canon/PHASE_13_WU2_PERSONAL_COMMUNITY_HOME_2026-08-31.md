# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 13 / P13-WU2 — PERSONAL COMMUNITY HOME / MY TNC
# CANONICAL CLOSEOUT

Date: 2026-08-31
Status: **COMPLETE / PASS — PUBLIC ACTIVATION REMAINS GATED**

## 1. Objective

P13-WU2 turns the Phase 12 Community foundation from a sequence of technical panels into one coherent personal Community experience: **MY TNC**.

The work unit does not create a new social network, dashboard score system or database domain. It reorganizes existing verified truth into a person-first experience.

Canonical experience order:

**Identity → Journeys & Memories → Contributions → Community Relationships → Reflections**

## 2. Stable route decision

Community continues to use the existing bilingual routes:

- `/cong-dong`
- `/en/community`

No new account/dashboard route was introduced.

Both routes remain `noindex` and are not promoted into the primary public navigation while Community Auth activation is gated.

## 3. Source implementation

Product repository:
`huynhtranhuythinh/tramnucuoi`

Product PR:
- #27 `P13-WU2 Personal Community Home / My TNC`

Final PR head:
`54c5aad8524b92ad4838b712a2f95cb3b6e93094`

Merged product main SHA:
`a6f3dfa4d3033d5855b1e3906d6d48beec7619ef`

Primary new component:
`src/components/community/community-experience-shell.tsx`

Updated route owners:
- `src/routes/cong-dong.tsx`
- `src/routes/en.community.tsx`

The routes now render one `CommunityExperienceShell` rather than separately stacking the Phase 12 foundation components.

## 4. Signed-out experience

Before verified Community Auth, the route presents a restrained Community entry experience:

- explains what a Community account connects;
- asks for the email used for Journey registration;
- preserves the verified-email Magic Link path;
- explains privacy and truth boundaries;
- never claims registration equals attendance.

This source exists behind the existing activation gate. Production Email remains OFF, therefore WU2 does not represent Community onboarding as publicly activated.

## 5. Signed-in MY TNC experience

After verified sign-in, the same route becomes a personal relationship home.

### Identity / welcome

MY TNC shows:
- verified Community email context;
- optional personal display name;
- a narrative relationship summary;
- sign-out.

Display name is explicitly a Community presentation field, not legal-identity verification.

### Journeys & Memories

The personal Journey section preserves Phase 12 attendance semantics:

- attendance unresolved → not an attended Memory;
- verified attendance `0` → no-show / confirmed history only;
- verified attendance `>0` → attended;
- only `memory_eligible=true` receives the evidence-backed Memory treatment.

Upcoming/active Journey context is presented without pretending future attendance already happened.

MY TNC links back to the operational Journey surface rather than manufacturing a second Journey record.

### Contributions

MY TNC presents only active verified Contribution records available to the authenticated person.

It may show:
- contribution type;
- date;
- localized title/description;
- quantity + unit when meaningful;
- verification basis;
- linked Journey / Project.

WU2 does not aggregate unlike units and does not introduce a Community score or impact score.

### Community Relationships

The experience translates verified facts into human-readable relationships:

- Explorer — verified Community account context;
- Participant — only when evidence-backed attended Memory exists;
- Contributor — only when active verified Contribution exists;
- Host — only from verified Host assignment;
- Partner representative — only from verified Partner-representative assignment.

These are relationships, not permissions. CMS authorization remains separate.

### Reflections

MY TNC presents the authenticated person's Reflection state:
- pending;
- published;
- rejected / not published.

A new Reflection form is shown only when the underlying Journey is completed and the person has an eligible attended Memory. Existing moderation semantics remain authoritative.

No generic Community feed was introduced.

## 6. Bilingual architecture

VI and EN use the same experience component and the same truth rules.

Bilingual copy covers:
- signed-out onboarding;
- My TNC welcome;
- Journey/Memory states;
- Contribution labels;
- Relationship labels;
- Reflection workflow;
- empty states and errors.

The language switch preserves the Community surface between `/cong-dong` and `/en/community`.

## 7. Empty-state contract

WU2 treats empty states as valid production truth.

If there is no verified Journey, Memory, Contribution, Host/Partner assignment or Reflection, the UI says so rather than seeding demonstration facts.

No fake Community data was created for this work unit.

## 8. QA / CI history

Initial PR CI run #138 passed all database/trust regressions and build, then failed TypeScript strict checking because three claim-result fields on `Record<string, unknown>` used dot access while `noPropertyAccessFromIndexSignature` requires bracket access.

The fix changed only:
- `eligible_count`
- `newly_linked_count`
- `already_linked_count`

to strict bracket access. Compiler strictness was not weakened.

Final PR CI run #139: **PASS**.

Passed gates:
- P9 source protection regression;
- P10 runtime-context regression;
- P9 database gate/rollback regression;
- P11 transactional capacity/cutoff QA;
- P12-WU1 through P12-WU7 database QA;
- build;
- typecheck;
- Cloudflare dry-run.

Post-merge main CI run #140 on `a6f3dfa4d3033d5855b1e3906d6d48beec7619ef`: **PASS** with the same full gate set.

## 9. Runtime / production boundary

P13-WU2 required:
- **no database migration**;
- **no Supabase production mutation**;
- **no Cloudflare production deploy**;
- **no Email activation**;
- **no Turnstile activation**;
- **no fake data**.

Existing guards remain authoritative:
- Email: OFF;
- Turnstile: OFF;
- Community Auth public activation: GATED;
- `pg_graphql`: OFF;
- CMS authorization roles: `admin | editor` only.

P11-WU6 Live Pilot Operations remains authoritative and unaffected.

## 10. What WU2 achieved

Before WU2, `/cong-dong` behaved like several Phase 12 capability panels stacked together.

After WU2, source architecture has one personal Community home that answers a human question:

> “Tôi đã đi cùng Trạm ở đâu, điều gì thực sự trở thành Ký ức, tôi đã góp gì, và tôi đang có mối quan hệ nào với cộng đồng?”

This is the first Phase 13 implementation that makes the Phase 12 Living Community OS foundation legible as a coherent product experience.

## 11. Next work unit

**P13-WU3 — JOURNEY COMMUNITY EXPERIENCE — BEFORE / DURING / AFTER**

WU3 should make each Journey express its lifecycle for a person while preserving the same registration, attendance, Memory and documentary truth boundaries.

WU3 is not implemented by this closeout.

## 12. Final declaration

**P13-WU2 — PERSONAL COMMUNITY HOME / MY TNC: COMPLETE / PASS**

**PUBLIC COMMUNITY AUTH ACTIVATION: STILL GATED WHILE EMAIL IS OFF**

**NEXT: P13-WU3 — JOURNEY COMMUNITY EXPERIENCE**
