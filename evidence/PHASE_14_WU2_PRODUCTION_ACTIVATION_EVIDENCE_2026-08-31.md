# P14-WU2 Production Activation Evidence — 2026-08-31

## Release
- Product main: 4582e8e6866714711631549ac4ed51cfb2d0c10d
- Worker Version: a47535cc-b6af-4b92-90d2-e6917f8051a4
- Build activation: VITE_APP_COMMUNITY_AUTH_ENABLED=true
- Default fail-closed source value remains false

## Runtime sequence
1. Signed-out VI `/cong-dong` displayed My TNC Magic Link form.
2. Signed-out EN `/en/community` displayed My TNC Magic Link form.
3. VI UI requested Magic Link; Supabase Auth `/otp` returned 200 with referer `/cong-dong`.
4. Production email arrived from Trạm Nụ Cười <hello@notify.tramnucuoi.com> with bilingual template.
5. Link verification returned 303; login event recorded; `/user` returned 200.
6. Signed-in My TNC displayed verified email and real profile onboarding surface.
7. Logout returned 204.
8. Reusing the one-time link returned invalid/expired behavior and the browser remained signed out.

## Data postflight
The real verified account produced exactly one profile row. It produced no participant link, Memory, Contribution, Reflection, or Community relationship.

The account already had one admin CMS role from 2026-08-22. WU2 did not create or modify that role.

## Result
PASS. No rollback triggered.
