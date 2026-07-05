# GBrain Configuration Reference

## 1. Installation

| Property | Value |
|---|---|
| **Version** | 0.42.44.0 |
| **Install method** | Global bun package (`/home/dev-p/.bun/bin/gbrain`) |
| **Binary** | Symlink → `/home/dev-p/.bun/install/global/node_modules/gbrain/src/cli.ts` |
| **Runtime** | Bun 1.3.14 |
| **Update status** | UP_TO_DATE |

## 2. Engine Config — `~/.gbrain/config.json`

```json
{
  "engine": "pglite",
  "database_path": "/home/dev-p/.gbrain/brain.pglite",
  "embedding_model": "openai:text-embedding-3-large",
  "embedding_dimensions": 1536,
  "expansion_model": "openai:gpt-5.2",
  "chat_model": "openai:gpt-5.2",
  "schema_pack": "gbrain-base-v2",
  "mcp": { "publish_skills": true },
  "self_upgrade": { "mode": "notify", "mode_prompted": true }
}
```

### Key settings

| Setting | Value | Notes |
|---|---|---|
| `engine` | `pglite` | Embedded PostgreSQL 17 (migrated from Supabase) |
| `database_path` | `~/.gbrain/brain.pglite/` | 323 MB on disk, PG v17 |
| `embedding_model` | `openai:text-embedding-3-large` | 1536 dimensions |
| `expansion_model` | `openai:gpt-5.2` | Used for query expansion |
| `chat_model` | `openai:gpt-5.2` | Used for doctor/features/LLM-dependent commands |
| `self_upgrade.mode` | `notify` | Notifies on new versions, doesn't auto-upgrade |

### Reranker config

```json
search.reranker.enabled: true
```

- **Provider:** ZeroEntropy (`zerank-2`)
- **API key:** `ZEROENTROPY_API_KEY` (set in `~/.bashrc` and OpenCode MCP environment)
- **Performance:** 442ms avg, $0.05/M tokens, 2.2× faster than OpenAI reranker

## 3. Environment Variables

| Variable | Purpose | Set in |
|---|---|---|
| `OPENAI_API_KEY` | Embedding, expansion, and chat models | Shell environment |
| `ZEROENTROPY_API_KEY` | Search reranking | `~/.bashrc` + OpenCode MCP env |

## 4. MCP Configuration

Configured in `~/.config/opencode/opencode.jsonc`:

```json
"mcp": {
  "gbrain": {
    "type": "local",
    "command": ["/home/dev-p/.bun/bin/gbrain", "serve"],
    "enabled": true,
    "environment": {
      "OPENAI_API_KEY": "{env:OPENAI_API_KEY}",
      "ZEROENTROPY_API_KEY": "{env:ZEROENTROPY_API_KEY}"
    }
  }
}
```

MCP tools available to agents: `gbrain_search`, `gbrain_query`, `gbrain_code_def`, `gbrain_code_refs`, `gbrain_code_callers`, `gbrain_code_callees`, `gbrain_list_sources`, `gbrain_get_page`, and others.

## 5. Sources (10 registered)

| Source ID | Type | Pages | Chunks | Embedded | Path |
|---|---|---|---|---|---|
| `default` | Federated | 143 | 455 | 100% | — |
| `project-library` | Isolated | 318 | 1683 | 100% | `/mnt/c/projects/mb-3-cs/project-library` |
| `mb-3-cs` | Isolated | 165 | 1090 | 100% | `/mnt/c/projects/mb-3-cs/mb-3-cs` |
| `mb4ecom` | Isolated | 1 | 2 | 100% | `/mnt/c/projects/mb4ecom/mb4ecom` |
| `mb4ecom-project-library` | Isolated | 2 | 15 | 100% | `/mnt/c/projects/mb4ecom/mb4ecom-project-library` |
| `mb5pdlf` | Isolated | 1 | 2 | 100% | `/mnt/c/projects/mb5pdlf/mb5pdlf` |
| `mb5pdlf-project-library` | Isolated | 50 | 195 | 100% | `/mnt/c/projects/mb5pdlf/mb5pdlf-project-library` |
| `pdlf` | Isolated | 18 | 132 | 100% | `/mnt/c/pdlf` |
| `scratch` | Isolated | 1 | 2 | 100% | `/mnt/c/projects/scratch/scratch` |
| `scratch-project-library` | Isolated | 1 | 2 | 100% | `/mnt/c/projects/scratch/scratch-project-library` |

**Totals:** 700 pages | 2901 chunks | 100% embedding coverage

## 6. Key Commands

### Maintenance (requires stopped `gbrain serve`)

| Command | Purpose |
|---|---|
| `gbrain sync --all` | Sync all sources from git repos |
| `gbrain sync --source <id>` | Sync single source |
| `gbrain extract --stale` | Extract links/timeline from stale pages |
| `gbrain sources list` | List all registered sources with page counts |
| `gbrain sources status --json` | Per-source dashboard (sync, staleness, failures) |
| `gbrain doctor --json` | Full health check |
| `gbrain stats --json` | Page/chunk/embedding stats |
| `gbrain ze-switch` | Interactive ZeroEntropy provider switch |

### Always available

| Command | Purpose |
|---|---|
| `gbrain version` | Print version |
| `gbrain --help` | Full help |
| `gbrain serve` | Start MCP server (blocks) |
| `gbrain query "question"` | Search brain (requires running serve or DB access) |

## 7. PGLite Operational Rule

**PGLite is single-writer.** The `gbrain serve` process owns the exclusive database connection. Before running CLI commands that need database access (`sync`, `sources`, `doctor`, `stats`, `extract`):

```bash
# Stop serve
kill $(pgrep -f "gbrain serve")

# Run CLI commands
gbrain sync --all
gbrain extract --stale

# Restart serve (or let OpenCode MCP restart it next session)
nohup /home/dev-p/.bun/bin/gbrain serve > /dev/null 2>&1 &
```

## 8. Current Health (2026-07-06)

| Metric | Value |
|---|---|
| Health score | ~90/100 |
| Sync failures | 12 auto-skipped (historical, from default federated source) |
| Oversized pages | 2 (>50KB: docs/mybizz-application-overview, docs/design) |
| Reranker failures | Resolved — `ZEROENTROPY_API_KEY` configured |
| CLI connectivity | Resolved — operational (with serve stopped for maintenance) |
| GStack detection | `gbrain_local_status: ok` |

## 9. Workspace Docs

All GBrain workspace docs are fully populated:

| File | Lines | Status |
|---|---|---|
| `workspace/gbrain/README.md` | 211 | Populated |
| `workspace/gbrain/gbrain-operating-model.md` | 66 | Populated |
| `workspace/gbrain/gbrain-source-tiers.md` | 70 | Populated |
| `workspace/gbrain/gbrain-overview.md` | 83 | Populated |
| `workspace/gbrain/gbrain-install-plan.md` | 82 | Populated (updated for PGLite) |
| `workspace/gbrain/gbrain-repo-strategy.md` | 102 | Populated (paths fixed) |
| `workspace/gbrain/gbrain-sources.md` | 103 | Populated |
| `workspace/gbrain/gbrain-sync-policy.md` | 107 | Populated |
| `workspace/gbrain/gbrain-ingestion-plan.md` | 114 | Populated |
| `workspace/gbrain/gbrain-log.md` | 20 | Populated (2026-07-06) |

## 10. Migration History

- **Pre-2026-07:** Supabase engine, v0.18.2, OpenRouter embeddings proxy, local `embedding.ts` patch
- **2026-07-06:** Migrated to PGLite, v0.42.44.0, native OpenAI embeddings, ZeroEntropy reranker, 10 sources, no local patches needed
