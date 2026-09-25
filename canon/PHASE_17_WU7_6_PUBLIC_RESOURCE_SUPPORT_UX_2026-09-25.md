# TRẠM NỤ CƯỜI — PHASE 17 / P17-WU7.6
# PUBLIC JOURNEY DONATION / RESOURCE UX

Date: 2026-09-25
Status: **COMPLETE / CLOSED / PASS**

Product PR: #119 — P17-WU7.5-7.7: Resource operations experience
Final exact head: `ef19964b3236095e1d062187d6b19e2cdc7a7fbf`
Exact-head CI: `36148904512` — SUCCESS
Inherited dedicated gate: `36148904427` — SUCCESS
Squash-merged main: `f30097d56c4c7f55eef7114c7b016aef1ca070e7`
Post-merge main CI: `36149106668` — SUCCESS
No production WU7 DDL was applied in WU7.5–WU7.7. Recruitment remained HOLD/CLOSED.

## Delivered

Source contract:
`database/contracts/p17_wu7_6_public_resource_support.sql`

Public experience:
- `src/lib/journeys/public-resource-support.ts`
- `src/components/journeys/journey-resource-support.tsx`
- composed into the canonical Journey detail page for Upcoming / Active Journey contexts.

Canonical behavior:
- only explicitly public/open Resource Needs are projected;
- Received / Remaining aggregates appear only when BTC explicitly enables progress visibility;
- donor identity, Pledge identity, internal notes and verifier identity are excluded;
- authenticated users may submit a private Pledge;
- Pledge remains intent, not receipt truth;
- cash support is an HTTPS external official-channel notice only;
- TNC does not process/custody funds and does not infer a donation from link clicks;
- public resource support is independent from volunteer Application Window state.

WU7.6 ephemeral DB QA: PASS.

# FINAL STATUS
**P17-WU7.6 — COMPLETE / CLOSED / PASS**
