# GBrain + GStack Brain Sync — Final Assessment

## Assessment Date: 2026-06-10

---

## 1. Setup Complete — All Systems Operational

### GBrain (PGlite) — FULLY WORKING

| Component | Status | Detail |
|---|---|---|
| GBrain CLI | **Working** | v0.25.0 on PATH |
| Engine | **PGlite (local)** | `/home/dev-p/.gbrain/brain.pglite` |
| Health score | **70/100** | All checks OK — see [Health Score Explanation](#health-score-explanation) |
| Doctor | **OK** | All critical checks pass |
| Pages stored | **263 pages** | 262 default + 1 gbrain-repo |
| Embeddings | **100% coverage** | 0 missing |
| Schema version | **31 (latest)** | Up to date |
| Query | **Working** | ADR search returns results |

### GBrain Sources

| Source | Type | Pages | Status |
|---|---|---|---|
| `default` | federated | 262 | Working |
| `gbrain-repo` | federated | 1 | Working, embedded |

### GStack Brain Sync — FULLY INITIALIZED

| Component | Status | Detail |
|---|---|---|
| `~/.gstack/` git repo | **Active** | Initialized and committed |
| Remote | `https://github.com/sassycomapp/gbrain-repo.git` | Connected and pushed |
| `gstack_brain_sync_mode` | **full** | Everything syncs |
| `gstack_brain_git` | **true** | Git repo configured |
| Source wireup | **Complete** | `gbrain-repo` registered in GBrain |

### OPENAI API Key

| Component | Status | Detail |
|---|---|---|
| Key | **Set** | Added to `~/.bashrc` |
| Source | `source ~/.bashrc` | Loaded in current session |
| Embedding | **Working** | 1 chunk embedded successfully |

---

## 2. Health Score Explanation

### Current Score: 70/100

The GBrain health score is designed for **personal knowledge bases with entity pages** (people, companies, deals). Your brain stores **ADRs and project documentation**, which is a different use case.

### Score Breakdown

| Component | Score | Explanation |
|---|---|---|
| `embed` | 35/35 | Perfect — 100% embedding coverage |
| `links` | 2/25 | Only 2 links exist — the extractor only finds links to entity directories (`people/`, `companies/`, etc.) |
| `timeline` | 1/15 | Only 1 page has timeline entries — ADRs don't have date-based timelines |
| `orphans` | 1/15 | Most pages have no inbound/outbound links — ADRs are standalone documents |
| `dead-links` | 10/10 | Perfect — no broken links |

### Why This Is Expected

The `gbrain extract links` command only recognizes links like:
- `[[people/slug]]` (wikilinks to entity directories)
- `[Name](companies/slug)` (markdown links to entity directories)

Your ADR pages use paths like `adr/002-navigation-lambda-link-open-form` and `docs/adr-index`, which **don't match** the entity directory whitelist (`people|companies|meetings|concepts|deal|civic|project|...`).

The `graph_coverage` metric specifically measures **entity pages** (type `person` or `company`). Since your pages are ADRs (type `doc` or `adr`), the entity page count is 0, making the coverage calculation meaningless.

### Bottom Line

**70/100 is a healthy score for a project documentation brain.** All critical checks pass:
- 262 pages indexed
- 100% embedding coverage
- Search working (query returns relevant results)
- Schema up to date
- All critical checks passing

The warnings about links, timelines, and orphans are **expected** for standalone documentation — they don't indicate any functional problem.

---

## 3. How Both Systems Work Together

| System | What it remembers | Where it stores | How to query |
|---|---|---|---|
| GBrain (PGlite) | Project documents (ADRs, specs, docs) | Local PGlite database | `gbrain query "<term>"` |
| GStack Brain Sync | Working state (learnings, plans, retros) | GitHub private repo | GStack workflows |

Both are connected via the `gbrain-repo` federated source. Your working memories from GStack are searchable alongside your 262 project documents.

---

## 4. Essential Commands

### GBrain CLI (run in WSL, any directory)

| Command | Purpose |
|---|---|
| `gbrain doctor` | Health check |
| `gbrain sources list` | List registered sources |
| `gbrain query "<term>"` | Search memories |
| `gbrain embed --stale` | Re-embed pages with missing embeddings |
| `gbrain extract links --source db` | Extract entity links from database |
| `gbrain extract timeline --source db` | Extract timelines from database |
| `gbrain integrity auto` | Auto-repair integrity issues |

### GStack Brain Sync (run in WSL, any directory)

| Command | Purpose |
|---|---|
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --once` | Sync pending changes |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --status` | Check sync health |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-gbrain-detect` | JSON status of both systems |

### OpenCode Commands

| Command | Purpose |
|---|---|
| `/sync-gbrain` | Run the sync-gbrain workflow |
| `/setup-gbrain` | Run the setup-gbrain workflow |
| `/setup-gbrain doctor` | Run GBrain health check via GStack |

---

## 5. Setup Complete Summary

| Step | Status |
|---|---|
| GBrain (PGlite) working | **Done** |
| GStack brain sync initialized | **Done** |
| GitHub repo connected | **Done** |
| Source wireup complete | **Done** |
| OPENAI_API_KEY set | **Done** |
| Embeddings at 100% | **Done** |
| Health score 70/100 | **Done** (expected for ADR/doc content) |

---

## 6. Full Operating Guide

For complete operating instructions, see: `docs/gbrain-operating-guide.md`

---

## 7. Environment Variables

To ensure the API key is available in new sessions, it's saved in `~/.bashrc`:

```bash
export OPENAI_API_KEY="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"
```

This is automatically loaded when you open a new WSL terminal.
