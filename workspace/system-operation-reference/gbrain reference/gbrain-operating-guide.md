# GBrain Operating Guide

## Overview

GBrain is the advisory memory layer for the Mybizz CS workspace. It stores and indexes project documents (ADRs, specs, docs) as searchable vector memories using a local PGlite database.

**Current Setup:**
- Engine: PGlite (local embedded database)
- Database: `/home/dev-p/.gbrain/brain.pglite`
- Pages: 700 (10 sources across all projects)
- Embeddings: 100% coverage
- Health Score: ~90/100

---

## Health Score Explanation

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

Your ADR pages use paths like `adr/2-navigation-lambda-link-open-form` and `docs/adr-index`, which **don't match** the entity directory whitelist (`people|companies|meetings|concepts|deal|civic|project|...`).

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

## Sources

GBrain has two federated sources:

| Source | Type | Pages | Description |
|---|---|---|---|
| `default` | federated | 262 | Project documents (ADRs, specs, docs) |
| `gbrain-repo` | federated | 1 | GStack brain working state (learnings, plans, retros) |

Both sources are searchable via `gbrain query`.

---

## Essential Commands

### GBrain CLI Commands

All GBrain commands are run in **WSL** from any directory (GBrain is on PATH).

#### Health & Status

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `gbrain doctor` | Full health check | WSL | Any directory |
| `gbrain sources list` | List all registered sources | WSL | Any directory |
| `gbrain --version` | Show GBrain version | WSL | Any directory |

#### Search & Query

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `gbrain query "<search term>"` | Search memories with vector similarity | WSL | Any directory |
| `gbrain query "<term>" --limit 10` | Search with custom result limit | WSL | Any directory |

#### Embeddings & Data Quality

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `gbrain embed --stale` | Re-embed pages with missing embeddings | WSL | Any directory |
| `gbrain extract links --source db` | Extract entity links from database pages | WSL | Any directory |
| `gbrain extract timeline --source db` | Extract timeline entries from database pages | WSL | Any directory |
| `gbrain extract all --source db` | Extract both links and timelines | WSL | Any directory |
| `gbrain integrity check` | Check integrity issues (bare tweets, external links) | WSL | Any directory |
| `gbrain integrity auto` | Auto-repair integrity issues | WSL | Any directory |

#### Configuration

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `gbrain features` | Show brain score and feature suggestions | WSL | Any directory |
| `gbrain features --auto-fix` | Auto-fix all fixable issues | WSL | Any directory |
| `gbrain integrations list` | List available integrations | WSL | Any directory |

### GStack Brain Sync Commands

GStack brain sync commands are run in **WSL** from the GStack bin directory.

#### Setup & Sync

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-init` | One-time setup (creates git repo + remote) | WSL | Any directory |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --once` | Sync pending changes to remote | WSL | Any directory |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --status` | Check sync health and queue depth | WSL | Any directory |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-restore` | Pull brain from remote to new machine | WSL | Any directory |

#### Configuration

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-config set gbrain_sync_mode full` | Enable full sync (everything) | WSL | Any directory |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-config set gbrain_sync_mode off` | Disable sync | WSL | Any directory |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-config get gbrain_sync_mode` | Check current sync mode | WSL | Any directory |

#### Diagnostics

| Command | Purpose | Run In | Path |
|---|---|---|---|
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-gbrain-detect` | JSON status of both GBrain and GStack sync | WSL | Any directory |
| `bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-gbrain-source-wireup` | Register GStack brain as GBrain source | WSL | Any directory |

### OpenCode Commands

These commands are run in **OpenCode** (not WSL):

| Command | Purpose | Run In |
|---|---|---|
| `/sync-gbrain` | Run the sync-gbrain workflow | OpenCode |
| `/setup-gbrain` | Run the setup-gbrain workflow | OpenCode |
| `/setup-gbrain doctor` | Run GBrain health check via GStack | OpenCode |

---

## Environment Variables

GBrain requires the `OPENAI_API_KEY` environment variable for embedding new content.

### Current Configuration

The key is saved in `~/.bashrc` and loaded automatically in new WSL sessions:

```bash
export OPENAI_API_KEY="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"
```

### If Embedding Fails

If you see `The OPENAI_API_KEY environment variable is missing or empty`:

```bash
source ~/.bashrc
```

Or export directly:

```bash
export OPENAI_API_KEY="your-key-here"
```

---

## GStack Brain Sync

### What It Does

GStack brain sync pushes your working state (learnings, plans, retros, designs) to a private git repo so your memory follows you across machines and becomes indexable by GBrain.

### Current Configuration

- Remote: `https://github.com/sassycomapp/gbrain-repo.git`
- Sync mode: `full` (everything syncs)
- Git repo: `~/.gstack/` (initialized)
- Worktree: `~/.gstack-brain-worktree/` (GBrain indexes this)

### How It Works

1. GStack writes working state to `~/.gstack/` (learnings, plans, retros)
2. `gstack-brain-sync` pushes changes to the private GitHub repo
3. `gstack-gbrain-source-wireup` registers the repo as a GBrain source
4. GBrain indexes the repo content alongside your project documents

### What Syncs

| Privacy Mode | What Syncs |
|---|---|
| `off` | Nothing (default) |
| `artifacts-only` | Plans, designs, retros, learnings — skips timelines + developer-profile |
| `full` | Everything in the allowlist, including behavioral state |

### What Does NOT Sync

By design, these stay local:
- Credentials (`.auth.json`, `auth-token.json`)
- Machine-specific state (browser profiles, caches)
- Question preferences

---

## Troubleshooting

### GBrain CLI Not Found

If `gbrain` is not on PATH:

```bash
# Check if it's installed
which gbrain

# If not found, check the repo
ls /mnt/c/_mb2-cs-app/gbrain/src/cli.ts

# Run via bun
cd /mnt/c/_mb2-cs-app/gbrain && bun run src/cli.ts doctor
```

### Embedding Fails

If embedding fails with API key error:

```bash
# Reload environment
source ~/.bashrc

# Or export directly
export OPENAI_API_KEY="your-key"

# Retry
gbrain embed --stale
```

### GStack Brain Sync Issues

If sync fails:

```bash
# Check status
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --status

# Check git status
cd ~/.gstack && git status

# Check remote connectivity
cd ~/.gstack && git fetch origin
```

### Health Score Warnings

The following warnings are **expected** and don't affect functionality:

| Warning | Expected With | Action |
|---|---|---|
| `pgvector: Could not check` | PGlite engine | None — PGlite doesn't expose pgvector as an extension |
| `jsonb_integrity: Could not check` | PGlite engine | None — PGlite limitation |
| `graph_coverage: Entity link coverage 0%` | ADR/doc content | None — content doesn't use entity directory patterns |
| `brain_score: 49/100` | ADR/doc content | None — scoring is for entity-based knowledge bases |
| `integrity: 6 bare-tweet phrases` | Any content | Optional: `gbrain integrity auto` |
| `resolver_health: Could not find skills directory` | Missing skills dir | Non-critical — skills still work |

---

## Quick Reference

### Daily Workflow

```bash
# Search memories
gbrain query "onboarding flow"

# Check health (optional)
gbrain doctor

# Sync GStack working state (happens automatically during GStack workflows)
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --once
```

### After Adding New Content

```bash
# If new pages were added, re-embed
gbrain embed --stale

# Check health
gbrain doctor
```

### After GStack Session

```bash
# Sync working state to remote
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --once

# Verify sync
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --status
```

---

## Architecture

### Two Systems Working Together

| System | What It Remembers | Where It Stores | How To Query |
|---|---|---|---|
| GBrain (PGlite) | Project documents (ADRs, specs, docs) | Local PGlite database | `gbrain query "<term>"` |
| GStack Brain Sync | Working state (learnings, plans, retros) | Private git repo (GitHub) | GStack workflows |

Both are connected via the `gbrain-repo` federated source in GBrain. Your working memories from GStack are searchable alongside your 262 project documents.

### File Locations

| Item | Location |
|---|---|
| GBrain CLI | On PATH (installed globally) |
| GBrain config | `~/.gbrain/config.json` |
| GBrain database | `~/.gbrain/brain.pglite/` |
| GStack home | `~/.gstack/` |
| GStack brain remote | `~/.gstack-brain-remote.txt` |
| GStack brain worktree | `~/.gstack-brain-worktree/` |
| GStack bin scripts | `/mnt/c/_mb2-cs-app/gstack/bin/` |
| GBrain repo | `/mnt/c/_mb2-cs-app/gbrain/` |

---

## Related Documentation

- `workspace/gbrain/gbrain-overview.md` — High-level overview of GBrain in this workspace
- `workspace/gbrain/gbrain-operating-model.md` — Authority model and operating rules
- `workspace/gbrain/gbrain-source-tiers.md` — Source hierarchy for ingestion and recall
- `workspace/gbrain/gbrain-sources.md` — Concrete path inventory for each tier
- `workspace/gbrain/gbrain-ingestion-plan.md` — What to ingest and in what order
- `workspace/gbrain/gbrain-sync-policy.md` — Freshness rules and sync triggers
- `workspace/gbrain/gbrain-install-plan.md` — Installation and configuration details
- `gstack-outputs/gbrain-assessment.md` — Full assessment results from 2026-06-10

---
# OpenCode vs WSL
work through OpenCode for everything except maintenance.

Use | Where 
Asking questions, research, coding tasks | OpenCode
git push, gbrain sync, gbrain embed | WSL
gbrain doctor (health check)| WSL

In OpenCode, GBrain is wired in as MCP tools — the agent can search and query your 700 pages automatically without you doing anything in WSL.

**After Doing Work in Project-Library**
Run these in WSL in order, every time:
bash 
1. cd /mnt/c/_mb2-cs-app/project-library
2. git add -A
3. git commit -m "/plan-ceo-review done"
4. git push origin main
5. gbrain sync
6. gbrain embed --stale
That's the complete sequence: GitHub updated, GBrain updated, new content embedded.

**Telling the Agent to Use GBrain in OpenCode**
GBrain is available automatically in every OpenCode session. 
To make it explicit at the start of a task, add this to your prompt:
Instruction: [Before starting, search GBrain for any existing context on [topic].]

Or for a bigger task:
Instruction: [Search GBrain for: [topic 1], [topic 2], [topic 3] — then begin.]

The agent will call gbrain_search and gbrain_query directly. It has access to all 627 pages of your project library, ADRs, and docs as research sources without you needing to paste anything in.