# OpenCode System Docmap — Multi-Project

**What this docmap is / how to use it:**
This document registers the relevant OpenCode system, workspace, and integration files with their paths and roles. Human-readable and agent-scannable. Path-focused, explicit, free of speculative implementation detail.

---

## 1. Global config & rules

- **Global AGENTS rules**
  `~/.config/opencode/AGENTS.md` (WSL: `/home/dev-p/.config/opencode/AGENTS.md`)
  Global behavior rules for all OpenCode sessions. Defines Plan/Build roles (Plan never writes), session discipline, behavioral principles, safety rules, task completion reporting, and the project precedence model.

- **Global OpenCode config**
  `~/.config/opencode/opencode.jsonc` (WSL: `/home/dev-p/.config/opencode/opencode.jsonc`)
  Global configuration: OpenRouter provider, conservative permissions (all `ask`), external_directory covering `C:\_mb2-cs-app\**`, `C:\projects\**`, and `C:\pdlf\**`. MCP gbrain enabled. No hardcoded models — model selected from UI picker.

- **Global skills directory**
  `~/.config/opencode/skills/` (WSL: `/home/dev-p/.config/opencode/skills/`)
  61 skill subdirectories — GStack skills, OpenClaw variants, browser skills, iOS skills, the PDLF orchestrator, and utility skills. Used across all projects.

- **Global commands directory**
  `~/.config/opencode/commands/`
  Does not exist. Commands are defined at project level.

- **Global agents directory**
  `~/.config/opencode/agents/`
  Does not exist. Agents are defined at project level.

---

## 2. Shared infrastructure (`C:\_mb2-cs-app\`)

Windows: `C:\_mb2-cs-app\` — WSL: `/mnt/c/_mb2-cs-app/`

Formerly the single-project workspace root (mb-3-cs only). Now repurposed as shared infrastructure:

| Path | Role |
|---|---|
| `_mb2-cs-app/gstack/` | GStack tooling repo — binaries, skills, node_modules |
| `_mb2-cs-app/gbrain/` | GBrain application (Node.js/Bun project with OpenAI SDK) |
| `_mb2-cs-app/skills/` | Large collection of ~40+ skill definitions (GStack/OpenCode) |
| `_mb2-cs-app/scripts/` | Legacy backup scripts |
| `_mb2-cs-app/matt-skills-teach/` | Empty — intended for `teach` Matt Pocock skill redirects |
| `_mb2-cs-app/backup-mybizz/` | Pre-migration backup of mb-3-cs repos (old paths) |
| `_mb2-cs-app/desktop-mybizz/` | Developer desktop companion files (learning, management, temp) |

---

## 3. Projects (`C:\projects\`)

Windows: `C:\projects\` — WSL: `/mnt/c/projects/`

### 3.1 mb-3-cs — Mybizz Consulting Services (GStack-managed)

| Repo | Windows Path | WSL Path | Default Agent |
|---|---|---|---|
| project-library | `C:\projects\mb-3-cs\project-library\` | `/mnt/c/projects/mb-3-cs/project-library/` | `plan` |
| code | `C:\projects\mb-3-cs\mb-3-cs\` | `/mnt/c/projects/mb-3-cs/mb-3-cs/` | `build` |

- `project-library/opencode.json`: plan agent, deny anvil.yaml, read from `C:\projects\mb-3-cs\mb-3-cs\**`
- `mb-3-cs/opencode.json`: build agent, deny anvil.yaml/parameters/templates/form_template.yaml, ask on standard-page.html/theme.css, read from `C:\projects\mb-3-cs\project-library\**`
- Both have `.opencode/agents/` (plan.md, build.md)
- `project-library/` has `.opencode/commands/` (49 command files)
- Tracks via `project-library/devlog-index.md` and `project-library/judgement-trail.md`
- Currently transitioning to PDLF workflow

### 3.2 mb4ecom — E-Commerce (PDLF-managed)

| Repo | Windows Path | WSL Path | Default Agent |
|---|---|---|---|
| project-library | `C:\projects\mb4ecom\mb4ecom-project-library\` | `/mnt/c/projects/mb4ecom/mb4ecom-project-library/` | `plan` |
| code | `C:\projects\mb4ecom\mb4ecom\` | `/mnt/c/projects/mb4ecom/mb4ecom/` | `build` |

- `mb4ecom-project-library/opencode.json`: plan agent, deny anvil.yaml, read from `C:\projects\mb4ecom\mb4ecom\**`
- `mb4ecom/opencode.json`: build agent, deny anvil.yaml/form_template.yaml, read from `C:\projects\mb4ecom\mb4ecom-project-library\**`
- No per-project agent definitions (uses global agents with plan/build defaults)
- PDLF state: `~/.gstack/projects/mb4ecom/pdlf-state.md`

### 3.3 mb5pdlf — PDLF Test Project (PDLF-managed)

| Repo | Windows Path | WSL Path | Default Agent |
|---|---|---|---|
| project-library | `C:\projects\mb5pdlf\mb5pdlf-project-library\` | `/mnt/c/projects/mb5pdlf/mb5pdlf-project-library/` | `plan` |
| code | `C:\projects\mb5pdlf\mb5pdlf\` | `/mnt/c/projects/mb5pdlf/mb5pdlf/` | `build` |

- `mb5pdlf-project-library/opencode.json`: plan agent, deny anvil.yaml, read from `C:\projects\mb5pdlf\mb5pdlf\**`
- `mb5pdlf/opencode.json`: build agent, deny anvil.yaml/form_template.yaml, read from `C:\projects\mb5pdlf\mb5pdlf-project-library\**`
- Deliberately small app (Daily Intentions Tracker) for testing the PDLF pipeline end-to-end
- PDLF state: `~/.gstack/projects/mb5pdlf/pdlf-state.md`

### 3.4 scratch — Scratch/Test Workspace

| Repo | Windows Path | WSL Path |
|---|---|---|
| project-library | `C:\projects\scratch\scratch-project-library\` | `/mnt/c/projects/scratch/scratch-project-library/` |
| code | `C:\projects\scratch\scratch\` | `/mnt/c/projects/scratch/scratch/` |

- Bare scaffold. No opencode.json, no AGENTS.md, no agent definitions.

---

## 4. PDLF orchestrator (`C:\pdlf\`)

Windows: `C:\pdlf\` — WSL: `/mnt/c/pdlf/`

The PDLF orchestrator — a contiguous, stage-gated, memory-compounding development framework. Shared across all projects. Contains:

| Path | Role |
|---|---|
| `pdlf/skill/` | PDLF skill definitions (SKILL-v4.md) |
| `pdlf/templates/` | Pre-project document templates |
| `pdlf/registry/` | Project slug → path resolution (projects.json) |
| `pdlf/docs/` | PDLF documentation |
| `pdlf/standards-library/` | Shared standards |

PDLF uses a Postgres ledger (database `pdlf`) for project state, not flat files.

---

## 5. Project registry

`C:\projects\_ projects-registry\project-inventory.txt`
(WSL: `/mnt/c/projects/_ projects-registry/project-inventory.txt`)

TSV catalog mapping source IDs to paths and roles. Covers all 4 projects plus the PDLF framework.

---

## 6. Per-project tracking (devlog & todos)

Each project maintains its own tracking files inside its project-library repo:

| Project | Devlog Index | Judgement Trail | Active Todo |
|---|---|---|---|
| mb-3-cs | `project-library/devlog-index.md` | `project-library/judgement-trail.md` | `project-library/wip/todo.md` |
| mb4ecom | `project-library/pdlf/devlog.md` | Managed by PDLF ledger | — |
| mb5pdlf | `project-library/pdlf/devlog.md` | Managed by PDLF ledger | — |
| scratch | — | — | — |

---

## 7. Workspace reference docs (project-library /workspace)

All under `C:\projects\mb-3-cs\project-library\workspace\` (WSL: `/mnt/c/projects/mb-3-cs/project-library/workspace/`):

- **Workspace description** — `workspace/system-operation-reference/system config reference/workspace-description.md`
  Multi-project workspace description and Web UI usage guide.

- **Workspace docmap** — `workspace/system-operation-reference/system config reference/workspace-docmap.md`
  This file. Path-centric register of all config, orchestration, and memory artifacts.

- **OpenCode 10 config files** — `workspace/system-operation-reference/system config reference/opencode-10-config-files.md`
  Full snapshot of all 10 OpenCode configuration files with paths and current content.

- **GStack config reference** — `workspace/system-operation-reference/system config reference/gstack-config-reference.md`
  GStack installation, runtime config, skills inventory, project state, binary paths.

- **GBrain config reference** — `workspace/system-operation-reference/system config reference/gbrain-config-reference.md`
  GBrain engine config, 10 sources (700 pages), MCP, PGLite rules, health, migration history.

- **OpenCode setup & status** — `workspace/system-operation-reference/opencode reference/opencode-setup.md`
  Detailed setup log and policy/status tracker.

- **GStack config reference** — `workspace/system-operation-reference/system config reference/gstack-config-reference.md`
  Full GStack configuration snapshot: installation paths, runtime config, skills inventory, project state, session data, binary path override, operational notes.

- **GBrain config reference** — `workspace/system-operation-reference/system config reference/gbrain-config-reference.md`
  Full GBrain configuration snapshot: engine config, 10 registered sources (700 pages), environment variables, MCP configuration, PGLite operational rules, current health, migration history.

### GStack docs
`workspace/gstack/` — GStack operating model, workflows, handoff contract, plus scaffolded docs.

### GBrain docs
`workspace/gbrain/` — GBrain operating model, source tiers, plus scaffolded docs.

### System operation reference
`workspace/system-operation-reference/` — Reference docs for models, hotkeys, skills, workflows, Cloudflare, Namecheap, and other operational topics.

### Config reference documents (6 files)

All in `workspace/system-operation-reference/system config reference/`:

| File | Scope |
|---|---|
| `workspace-docmap.md` | This file — path register for all system artefacts |
| `workspace-description.md` | Multi-project usage guide |
| `opencode-10-config-files.md` | OpenCode configuration snapshot (all 10 files) |
| `gstack-config-reference.md` | GStack installation, runtime config, skills, commands |
| `gbrain-config-reference.md` | GBrain engine config, 10 sources, MCP, PGLite rules |
| `matt-skills-config-reference.md` | Matt Pocock skills installation, routing, per-project state |
| `matt-skills-operations-guide.md` | Matt skills user guide + cheatsheet |

---

## 8. Environment: Web UI only

- **Interface:** OpenCode Web UI (`opencode web` from WSL). No Desktop app, no Desktop bridge.
- **Shell:** `/bin/bash` (configured in global `opencode.jsonc`)
- **Model:** Selected from UI picker. Not hardcoded in config.
- **WSL paths:** All paths use `/mnt/c/...` notation within WSL. Windows paths (`C:\...`) used in `opencode.json` configs.

---

## 9. GStack — integration state

- **Repo:** `C:\_mb2-cs-app\gstack` (Windows) / `/mnt/c/_mb2-cs-app/gstack` (WSL)
- **Status:** Operational — shell scripts normalized for WSL, `gstack-config --help` confirmed runnable
- **Integration:** GStack skills (61 total) available via the global skills directory. Per-project command files in `.opencode/commands/` route to individual GStack skills.
- **No GStack-specific config** at project or global `.opencode/` level — integrated via skillpack.

---

## 10. GBrain — integration state

- **Location:** `~/.bun/bin/gbrain` (WSL), version `v0.42.44.0`
- **Engine:** PGLite (embedded PostgreSQL 17), data at `~/.gbrain/brain.pglite/` (323 MB)
- **Sources:** 10 registered — 700 pages, 2901 chunks, 100% embedding coverage
- **Embeddings:** `text-embedding-3-large` via OpenAI native
- **Reranker:** ZeroEntropy (`zerank-2`), enabled via `ZEROENTROPY_API_KEY`
- **MCP:** Enabled in global `opencode.jsonc` — `gbrain serve` via `/home/dev-p/.bun/bin/gbrain`
- **PGLite note:** Single-writer engine. Stop `gbrain serve` before running CLI sync/doctor commands.
- **Role:** Advisory memory layer. Final authority rests with current repo files, ADRs, and user decisions.

---

## 11. Future placeholders

- **Per-project agent definitions** for mb4ecom, mb5pdlf, and scratch (currently none)
- **Per-project command files** for mb4ecom, mb5pdlf, and scratch (currently none)
- **PDLF workflow integration** for mb-3-cs (transitioning from GStack-only to PDLF)
- **GBrain source-tier ingestion** — scaffolded docs awaiting concrete content
