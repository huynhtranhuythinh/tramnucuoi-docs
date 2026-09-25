# TRẠM NỤ CƯỜI — WEBSITE 2026
# PHASE 17 / P17-WU7.2 — PLEDGE / DONOR INTENT CONTRACT

Date: 2026-09-25  
Status: **COMPLETE / CLOSED / PASS — SOURCE CONTRACT MERGED; NO PRODUCTION DATABASE CUTOVER**

## 1. Canonical truth

Pledge is authenticated donor intent only.

`Need != Pledge != Received != Distributed`

A donor may create and see their own Pledge. New Pledges are normalized by the server to `pending`. A donor may cancel a pending Pledge, but may not self-accept or self-reject it. Admin/BTC review authority owns `accepted` / `rejected` transitions and server-stamped reviewer audit truth.

Donor identity remains private operational truth.

## 2. Source evidence

Product repo: `huynhtranhuythinh/tramnucuoi`

PR: `#116 — P17-WU7.2: Pledge donor intent contract`

Final PR exact head:
`9754203d7650b0b0668fd2af7941fd85f81f7231`

Source contract:
`database/contracts/p17_wu7_2_resource_pledge.sql`

QA:
- `scripts/p17-wu7-2-pledge-source-qa.ts`
- `scripts/p17-wu7-2-pledge-db-qa.sql`

Exact-head runs:
- generic CI `36146312507` — **SUCCESS**
- dedicated inherited gate `36146312513` — **SUCCESS**

Squash-merged main:
`4240c1e5294b4f95ffdfc0bf19386311a4b342e1`

Post-merge main CI:
`36146552830` — **SUCCESS**

## 3. Security / truth boundaries

Planned source table:
`public.journey_resource_pledges`

Properties:
- same-Journey FK to Resource Need;
- positive pledged quantity;
- statuses: `pending | accepted | cancelled | rejected`;
- own-or-Admin read;
- authenticated donor insert;
- own-or-Admin update with server transition guard;
- no anon direct source access;
- no normal DELETE grant;
- server-derived donor identity on INSERT;
- server-derived Admin review actor/time on acceptance/rejection.

WU7.2 does not create:
- Receipt;
- Distribution;
- payment/custody;
- participant;
- attendance;
- Memory;
- Shared Journey evidence;
- Impact.

## 4. Production invariant

WU7.2 is source-only.

No production migration was applied.

Recruitment remains **HOLD / CLOSED**.

# FINAL STATUS

**P17-WU7.2 — COMPLETE / CLOSED / PASS**

Next:
**P17-WU7.3 — RECEIPT VERIFICATION**
