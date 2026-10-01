---
title: "wp10-verify-graphify-gbrain-mcp-2026-08-14"
doc-id: "mb-997177c7"
updated: "2026-10-01"
---
# Work Package 10 — Verify Graphify and GBrain MCP Tools

**Date:** 2026-08-14T151511+0200
**Operation:** Read-only verification of both MCP servers (Graphify + GBrain) after Package 9.
**Result:** All data checks passed. GBrain MCP fully available. Graphify MCP server functional and correct — but its tools are NOT surfaced to the agent in this session. No files changed; git clean; no GBrain statistics changed.

---

## 1. GBrain MCP — available and responding

The GBrain MCP tools are exposed to this session's agent tool set (`gbrain_*` functions). Verified directly:

| Tool | Result |
|---|---|
| `gbrain_get_stats` | Responds: **1118 pages, 2269 chunks, 382 links** (matches expected exactly) |
| `gbrain_query` | Responds with ranked results (`encrypt vault secret` → 3 hits, policy/ADR chunks) |

- Statistics were read twice; identical both times (1118 / 2269 / 382). No GBrain statistics changed.
- No GBrain write operations performed (no `put_page`, `remember`, `sync`, `sources_add`, etc.).

## 2. Graphify MCP — server functional; tools NOT exposed to agent

**Server-side verification** (via a direct stdio MCP handshake against the configured command
`python3 -m graphify.serve <repo>/graphify-out/graph.json`, serverInfo `graphify 0.9.40`):

| Tool | Result |
|---|---|
| `graph_stats` | Responds: **Nodes 537, Edges 434**, Communities 155, EXTRACTED 96% / INFERRED 4% / AMBIGUOUS 0% |
| `get_node("encrypt")` | Responds (fuzzy top-match `TestEncryptionService`); `get_node("encrypt()")` resolves the exact node `encrypt()` at `server_code/server_shared/encryption_service.py` L13 |
| `get_neighbors("encrypt")` | Responds; neighbors returned with all `[EXTRACTED]` edges |
| `shortest_path("get_vault_secret" → "encrypt", undirected=true)` | Responds: `get_vault_secret() <--contains [EXTRACTED]-- server_shared/vault_service.py --imports [EXTRACTED]--> encrypt()` — **both path edges EXTRACTED** |
| `query_graph(question="get_vault_secret")` | Responds: BFS depth=3, 6 nodes + edges returned |

`tools/list` returns 10 tools, including all five required: `graph_stats`, `get_node`,
`get_neighbors`, `shortest_path`, `query_graph` (plus `get_community`, `god_nodes`,
`list_prs`, `get_pr_impact`, `triage_prs`).

**Key finding (discrepancy flagged):** the Graphify MCP tools are **not present in this
session's agent tool list**. The agent's available functions contain only `gbrain_*` MCP
tools (plus `bash`/`edit`/`read`/`glob`/`grep`/`write`/etc.). There is no `graph_stats`,
`get_node`, `get_neighbors`, `shortest_path`, or `query_graph` function exposed to the
agent. The Graphify MCP server itself is configured (`opencode.json` → `mcp.graphify`,
`type: local`, `enabled: true`, added in HEAD commit `5ce243e chore: add Graphify scope
configuration`), installed, and responds correctly when invoked directly — but OpenCode
did not surface its tools to the agent in this session.

No error is thrown by the server. The observation is an absence of tool exposure, which is
most consistent with the session not having (re)loaded the project-level `mcp.graphify`
config. Per instruction, no configuration was modified and nothing was installed.

## 3. Two tool sets are distinct

- Graphify (10 tools): `graph_stats`, `get_node`, `get_neighbors`, `get_community`, `god_nodes`, `shortest_path`, `query_graph`, `list_prs`, `get_pr_impact`, `triage_prs`.
- GBrain (~80 tools): `gbrain_get_stats`, `gbrain_query`, `gbrain_*`.

Separate namespaces, no overlap. Distinct.

## 4. Environment integrity

- `git status --porcelain` (code repo `mb-3-cs`) → **empty** at task start and at task end. No files changed; working tree clean.
- No file edits made to the code repo (no client_code / server_code / docs / tests / theme changes).
- Graph not rebuilt, `cluster-only` not run, no hooks installed, no packages added/removed, no PostgreSQL connection, no commit/push, no GBrain sync.

---

## Next Steps

- **Plan:** confirm whether the Graphify MCP tools should be exposed to the agent; if so, a
  session restart (to reload the project-level `mcp.graphify` config) is the likely fix.
  If the tools are expected to be present and still are not after a restart, investigate the
  OpenCode MCP client's load/connect path for the `graphify` local server (no config change
  without explicit instruction).
- **Build:** none — this was a read-only verification. No code or configuration changes made.
- Housekeeping (asked separately): commit/push approval; GBrain sync approval.
