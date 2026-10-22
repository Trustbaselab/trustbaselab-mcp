# Trustbase Lab MCP Server

> **Verified industry data, exposed over MCP — with hashes anyone can recompute offline.**

[![MCP](https://img.shields.io/badge/MCP-compatible-blue)](https://modelcontextprotocol.io)
[![Endpoint](https://img.shields.io/badge/endpoint-trustbaselab.com%2Fmcp-green)](https://trustbaselab.com/mcp)
[![Records](https://img.shields.io/badge/records-5487-lightgrey)](https://trustbaselab.com/upload/data.json)
[![Tools](https://img.shields.io/badge/tools-59-lightgrey)](https://trustbaselab.com/mcp)

---

## Don't trust us — check us

Most data providers *ask* you to trust their provenance. This one ships a verifier.

```bash
curl -O https://trustbaselab.com/verify/verify_trustbase.py
curl -O https://trustbaselab.com/upload/data.json
python3 verify_trustbase.py data.json       # fully offline
```

That recomputes **every hash for every record** locally. If anything doesn't match,
you don't have to take our word for it — you have proof.

**Recipe (implement it yourself if you prefer):**

```
record_hash = "sha256:" + sha256(canonical_json({ id, list, public, sourced, description }))
body_hash   = "sha256:" + sha256(canonical_json(record minus evidence))
canonical_json = json.dumps(obj, ensure_ascii=False, sort_keys=True, separators=(",", ":"))
```

Two hashes on purpose:

| hash | covers | why |
|---|---|---|
| `record_hash` | 5 core fields | stable identity across versions |
| `body_hash` | **every field** except `evidence` | detects tampering with **any** content |

---

## What's in it

5487 records across 11 tables:

| `domain_benchmarks` | 3285 |
| `trade_flows` | 1751 |
| `fitness` | 67 |
| `articles` | 64 |
| `vendors` | 58 |
| `knowledge_cards` | 58 |
| `policies` | 49 |
| `techs` | 44 |
| `domestic` | 39 |
| `grades` | 37 |
| `international` | 35 |

Domains: recycled carbon black (grades, substitution boundaries, prices) ·
carbon & CBAM policy · telecom & spectrum · health & nutrition limits ·
compute infrastructure & PUE · UN Comtrade trade flows

---

## Quick start

```bash
# List tools
curl -sS -X POST https://trustbaselab.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | jq '.result.tools[].name'

# Call one
curl -sS -X POST https://trustbaselab.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{
        "name":"query_companies",
        "arguments":{"query":"carbon black","limit":5}}}' | jq
```

- **Transport**: streamable HTTP
- **Auth**: none required (anonymous works)
- **Protocol**: `2026-07-28`

---

## Machine-readable entry points

| URL | what |
|---|---|
| `https://trustbaselab.com/mcp` | MCP endpoint, 59 tools |
| `https://trustbaselab.com/trust.json` | trust signal: grade, 13 clauses, coverage, **and disclosed gaps** |
| `https://trustbaselab.com/llms.txt` | LLM index layer |
| `https://trustbaselab.com/llms-full.txt` | **full corpus index** — every record, one per line |
| `https://trustbaselab.com/upload/data.json` | full data with evidence hashes |
| `https://trustbaselab.com/verify/verify_trustbase.py` | **offline verifier** |
| `https://trustbaselab.com/license.json` | license + per-record `r_level` |
| `https://trustbaselab.com/status.json` | freshness / status |
| `https://trustbaselab.com/coverage.json` | coverage stats |

---

## Honest gaps (we publish these on purpose)

- **Certification is self-assessed**, not third-party audited.
  Current: V-A / 0.9218 across 13 published clauses.
- **Snapshot coverage is 94.7%**, not 100%. Uncovered records are listed in
  `/trust.json` → `gaps`.
- **Zero confirmed external users** as of 2026-10-22. This is a new server.
- **Some source pages block automated fetching** (HTTP 403). Those records carry
  `snapshot_fetch_error` instead of a snapshot — we don't fabricate coverage.
- **`r_level` may be stricter than CC BY 4.0.** Check per record before redistributing.

If a claim here is wrong, [open an issue](https://github.com/Trustbaselab/trustbaselab-mcp/issues).

---

## License

Data: CC BY 4.0. **But** per-record `r_level` can be stricter (R2 = quote only,
R3 = conditional). See `https://trustbaselab.com/license.json`.

---

*Data file sha256: `424ffbaafa6d6df09d89b3b1ea345fc4e6d7c4d98ca7f60bcf6e66701be9d5cf` · exported 2026-10-22T00:52:26 · data v0.12 / schema 1.10*
