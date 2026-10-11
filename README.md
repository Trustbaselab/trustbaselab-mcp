# Trustbase Lab — Industry Data MCP Server

[![mcpindex](https://mcpindex.ai/api/v1/badge/com-trustbaselab-rcb-data)](https://mcpindex.ai/server/com-trustbaselab-rcb-data)


Trusted data infrastructure for the AI era: **sourced, traceable industry records with stable IDs**, exposed over the Model Context Protocol.

> **Scope of this repository.** This repo is a **description / directory-listing anchor** for the MCP server. The server itself is **hosted and closed-source** — no source code is published here. Everything below points to live, public endpoints you can verify right now.

---

## What it is

Trustbase Lab publishes machine-readable industry data designed so that an AI can **find a fact, verify where it came from, and cite it by a stable ID** — rather than guessing.

Each published record carries:

- a **stable record ID** (retrievable directly, not just searchable)
- a **source URL**
- a **confidence grade**

## Connect

| Field | Value |
|---|---|
| Endpoint | `https://trustbaselab.com/mcp` |
| Transport | `streamable-http` |
| Protocol version | `2025-06-18` |
| Server version | `1.15.1` |
| Server card | `https://trustbaselab.com/.well-known/mcp/server-card.json` |
| Agent card | `https://trustbaselab.com/.well-known/agent-card.json` |
| Endpoint declaration | `https://trustbaselab.com/.well-known/mcp.json` |
| LLM index | `https://trustbaselab.com/llms.txt` |

The server exposes **read-only tools** only — no write, no side effects.

## Domains covered

**Multiple industry domains**, each with its own record set, source citations and stable ID space:

1. **Chemical recycling (rCB)** — recovered carbon black grades, producers, trade flows, price benchmarks
2. **Carbon & CBAM** — CBAM obligations and timeline, carbon markets, accounting standards
3. **New-energy battery** — EU battery regulation timeline, recycled-content thresholds, passport schema, recycling targets
4. **Telecom & space** — 3GPP/ITU standards evolution, spectrum allocation, constellation status
5. **Health & nutrition** — dietary limits and guidelines, physical-activity guidance, official clarifications
6. **Compute & energy** — compute facilities, PUE benchmarks, power-consumption statistics, green-power deals

<sub>Six domains are live today and expanding. Domain count is deliberately not a commitment — new domains are added only when their record sets meet the same sourcing bar.</sub>

## How records are made citable

Every fact set is also published as a human-readable, numbered citation list:

- Citations index: `https://trustbaselab.com/citations`
- Machine-readable data index: `https://trustbaselab.com/schema`

## Principles

- **Read-only.** Tools query and compute; they never mutate anything.
- **Sourced.** No unsourced numbers are published.
- **Traceable.** Every fact resolves back to a citable ID.
- **Bounded.** Positions are published at city/park granularity only; no precise coordinates, no personal data.

## Contact

- Website: https://trustbaselab.com
- General: business@trustbaselab.com
- Security policy: https://trustbaselab.com/.well-known/security.txt
