# TRẠM NỤ CƯỜI — MEDIA RUNTIME CANON

Date: 2026-08-28  
Status: **CANONICAL — WU9 GATE A RECONCILIATION**

## Current decision

Bunny remains the canonical provider for **new CMS media uploads when `MEDIA_ACTIVE_PROVIDER=bunny` is active**.

Supabase remains canonical for DB/Auth/RLS/CMS metadata.

For every existing media asset, **the current `media_assets` row is authoritative for provider, bucket, path and URL resolution**.

## Current six-asset state

A fresh read-only inspection of canonical Supabase project `iwiqprhoohkxvjyxojto` on 2026-08-28 found:

- 6 total `media_assets` rows;
- all 6 public;
- all 6 have `provider='supabase'`;
- all 6 have `bucket='media'`;
- all 6 current URLs point to the canonical Supabase Storage hostname;
- no current `media_assets` row is Bunny-backed.

Therefore the authoritative current state of these six assets is **Supabase-backed**.

## Relationship to 2026-08-27 infrastructure evidence

The 2026-08-27 evidence that the Owner completed a six-asset Bunny replacement remains a historical record of that observed session. It is not deleted or rewritten.

However, for release decisions, the newer 2026-08-28 runtime inspection supersedes the historical statement that the six *current* canonical rows are Bunny-backed.

The available read-only evidence does not establish why the current rows are Supabase-backed again. Do not speculate about rollback, restore or recreation without evidence.

## Release rules

- Do not migrate or delete these six assets merely to make documentation match an earlier session.
- WU9/WU10 must use their actual current provider metadata.
- Migration `0025` is provider-agnostic and should classify the rows that actually exist at apply time.
- WU8 upload validation supports both Supabase and Bunny boundaries.
- Any future intentional migration of these six assets to Bunny requires its own verified operation and Owner gate.

This record is newer than `canon/INFRA_CANON_2026-08-27.md` for the narrow question of the **current six-asset provider state**. All other infrastructure canon remains unchanged.
