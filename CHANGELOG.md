# Changelog

## 2026-10-22

### Added

- **`/llms-full.txt`** — full-corpus index. Every record on one line
  (`ID | name | category | confidence | source`), so an LLM can ingest the whole
  catalogue without parsing the 12 MB JSON.
- **Independent verifier** (`/verify/verify_trustbase.py`). Downloads nothing,
  phones home to nothing — recomputes all hashes offline.
- **`body_hash`** on every record — covers the **entire** record body.

### Fixed

- **`record_hash` did not protect 33% of records.**
  The published recipe covers `{id, list, public, sourced, description}`.
  But `knowledge_cards` (58) and `trade_flows` (1,751) have none of those fields,
  so their `record_hash` depended only on `id` + table name — editing a value
  left the hash unchanged. `body_hash` now covers the full record.
  (Verified by per-table tamper test: 11/11 tables detect modification.)
- **`description` schema regression.** A collection pipeline wrote `description`
  as a bare string, flattening the `{en, zh}` bilingual structure. This silently
  invalidated 1,417 `record_hash` values. Restored to `{en, zh}`; pipeline fixed.
- **Malformed `source_url` values** containing trailing annotation text
  (e.g. `example.com（pending verification`) are now cleaned before fetching.

### Changed

- Snapshot evidence coverage: **38.2% → 62.7%**
- UN Comtrade `trade_flows` evidence backfilled via API response freezing.
  Where the API value differs from the stored value, the stored value is
  **kept** and the discrepancy recorded as `source_revised: true` — no silent overwrite.

---

## 2026-10-20

- Data snapshot 5,487 records; `record_hash` on 100%.

## 2026-10-11

- Initial public release.
