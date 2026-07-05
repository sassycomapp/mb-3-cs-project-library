# OpenCode Workspace Description & Usage Guide

**What this is:** Multi-project OpenCode environment on Windows 11 via WSL2. Web UI only (`opencode web`). All projects share the same global OpenCode config, GStack skills, and GBrain memory layer.

---

## 0. Current state (2026-07-06)

### Environment

- **Interface:** OpenCode Web UI only (`opencode web` from WSL). No Desktop app, no Desktop bridge.
- **Shell:** `/bin/bash` in WSL (Ubuntu 22.04.5 LTS)
- **Provider:** OpenRouter. Model selected from UI picker — not hardcoded in config.
- **Config root:** `~/.config/opencode/` (WSL: `/home/dev-p/.config/opencode/`)

### Projects

| Project | Code | project-library | Workflow |
|---|---|---|---|
| mb-3-cs | `C:\projects\mb-3-cs\mb-3-cs` | `C:\projects\mb-3-cs\project-library` | GStack → transitioning to PDLF |
| mb4ecom | `C:\projects\mb4ecom\mb4ecom` | `C:\projects\mb4ecom\mb4ecom-project-library` | PDLF |
| mb5pdlf | `C:\projects\mb5pdlf\mb5pdlf` | `C:\projects\mb5pdlf\mb5pdlf-project-library` | PDLF (test) |
| scratch | `C:\projects\scratch\scratch` | `C:\projects\scratch\scratch-project-library` | None |

### Shared infrastructure

| Tool | Location | Status |
|---|---|---|
| GStack | `C:\_mb2-cs-app\gstack` | Operational — 61 skills available |
| GBrain | `~/gbrain` (v0.18.2) | Operational — Supabase + OpenRouter embeddings |
| PDLF | `C:\pdlf` | Framework — staging for rollout across all projects |

### Tracking (per-project)

| Project | Devlog | Judgement Trail | Todo |
|---|---|---|---|
| mb-3-cs | `devlog-index.md` | `judgement-trail.md` | `wip/todo.md` |
| mb4ecom | `pdlf/devlog.md` | PDLF ledger | — |
| mb5pdlf | `pdlf/devlog.md` | PDLF ledger | — |

---

## 1. Global layer

- **Config:** `~/.config/opencode/opencode.jsonc`
  - Default agent: `plan`
  - All permissions: `ask` (conservative)
  - External directory access: `C:\_mb2-cs-app\**`, `C:\projects\**`, `C:\pdlf\**`
  - MCP: GBrain enabled
  - No model hardcoded — selected from UI picker

- **Rules:** `~/.config/opencode/AGENTS.md`
  - Global behaviour rules for all sessions
  - Plan reads, Build writes — Plan never writes. Ever.
  - Task completion reporting, devlog/judgement-trail tracking, two housekeeping questions per task

- **Skills:** `~/.config/opencode/skills/` — 61 skill directories (GStack, OpenClaw, browser, iOS, PDLF, utility)

---

## 2. How to start a session (Web UI)

### 2.1 Start the WSL terminal

1. Open **Ubuntu 22.04.5 LTS** (WSL).
2. Navigate to the desired project, or start from home:

   ```bash
   cd /mnt/c/projects/mb-3-cs/project-library
   ```

3. Start OpenCode Web UI:

   ```bash
   opencode web
   ```

4. The Web UI opens in your default browser. Select the desired model from the picker.

### 2.2 Opening different projects

The Web UI operates from the directory where `opencode web` was started. To switch projects:

1. Close the current Web UI session.
2. `cd` to the target project's directory.
3. Run `opencode web` again.

Or use `/mnt/c/projects/mb-3-cs/mb-3-cs` (or equivalent) as a path reference in chat to direct the agent's context.

### 2.3 Which project to use

| Task | Project | Agent |
|---|---|---|
| Documentation, planning, ADRs, specs | `<project>/project-library` | `plan` |
| Code implementation, refactors, tests | `<project>/mb-*-cs` or `<project>/<slug>` | `build` |
| Initial planning, architecture | `<project>/project-library` | `plan` |

---

## 3. mb-3-cs — Mybizz Consulting Services

- **Code:** `C:\projects\mb-3-cs\mb-3-cs` (WSL: `/mnt/c/projects/mb-3-cs/mb-3-cs/`)
- **Docs:** `C:\projects\mb-3-cs\project-library` (WSL: `/mnt/c/projects/mb-3-cs/project-library/`)
- **Workflow:** GStack-managed, transitioning to PDLF

### Usage

- Start in code repo for implementation: `cd /mnt/c/projects/mb-3-cs/mb-3-cs && opencode web`
  - Default agent: `build`
  - Plan agent available for architecture/planning tasks
- Start in project-library for docs/planning: `cd /mnt/c/projects/mb-3-cs/project-library && opencode web`
  - Default agent: `plan`
  - Build agent available for document tooling and mechanical edits

### Protections

- `anvil.yaml`: denied (all agents)
- `parameters.yaml`, `templates.yaml`, `**/form_template.yaml`: denied
- `standard-page.html`, `theme.css`: ask
- Plan never writes. Build writes on explicit approval.

### GStack skills

Available via the global skillpack and the 49 project-local command files in `.opencode/commands/`. Key workflows:
- Planning: `/plan-ceo-review`, `/plan-eng-review`, `/plan-design-review`, `/office-hours`
- Implementation: `/build`, `/investigate`
- Review: `/review`, `/qa`, `/design-review`
- Release: `/ship`, `/land-and-deploy`

---

## 4. mb4ecom — E-Commerce (PDLF)

- **Code:** `C:\projects\mb4ecom\mb4ecom` (WSL: `/mnt/c/projects/mb4ecom/mb4ecom/`)
- **Docs:** `C:\projects\mb4ecom\mb4ecom-project-library` (WSL: `/mnt/c/projects/mb4ecom/mb4ecom-project-library/`)
- **Workflow:** PDLF-managed — invoke with `/pdlf mb4ecom`

### Usage

- Start in code repo: `cd /mnt/c/projects/mb4ecom/mb4ecom && opencode web`
- Start in project-library: `cd /mnt/c/projects/mb4ecom/mb4ecom-project-library && opencode web`
- PDLF invocation: `/pdlf mb4ecom` or `/pdlf continue`
- PDLF state: `~/.gstack/projects/mb4ecom/pdlf-state.md`

### Protections

- `anvil.yaml`: denied
- `**/form_template.yaml`: denied
- All other edits: ask

---

## 5. mb5pdlf — PDLF Test Project

- **Code:** `C:\projects\mb5pdlf\mb5pdlf` (WSL: `/mnt/c/projects/mb5pdlf/mb5pdlf/`)
- **Docs:** `C:\projects\mb5pdlf\mb5pdlf-project-library` (WSL: `/mnt/c/projects/mb5pdlf/mb5pdlf-project-library/`)
- **Workflow:** PDLF-managed — test project for the PDLF pipeline

This project exists specifically to test the PDLF development framework end-to-end with a deliberately small app (Daily Intentions Tracker). Content can change freely. Test honesty about the PDLF process matters more than shipping a product.

### Usage

- Start in code repo: `cd /mnt/c/projects/mb5pdlf/mb5pdlf && opencode web`
- Start in project-library: `cd /mnt/c/projects/mb5pdlf/mb5pdlf-project-library && opencode web`
- PDLF invocation: `/pdlf mb5pdlf` or `/pdlf continue`

---

## 6. scratch — Test Workspace

- **Code:** `C:\projects\scratch\scratch`
- **Docs:** `C:\projects\scratch\scratch-project-library`

Bare scaffold with no configuration. Use for throwaway experiments and quick tests.

---

## 7. Cross-project concerns

### Reading another project's docs

All projects can read each other's files via the global `external_directory` (covers `C:\projects\**`). No additional per-project config needed. The Plan/Build agent simply reads the path.

### GBrain cross-contamination

GBrain stores pages from multiple projects. When querying:
- Always tag results with source project
- Cross-project learnings are welcome; decisions (ADRs, ISAs) are not transferable unless explicitly confirmed
- Scope queries to the relevant GBrain source when possible

### PDLF relationship

All projects are PDLF projects or transitioning to PDLF. The PDLF orchestrator (`C:\pdlf\`) is the governing framework.
- PDLF uses the same OpenCode global config, agents, and permissions model.
- PDLF's own skill is installed in `~/.config/opencode/skills/pdlf/`.
- PDLF-managed projects use their own AGENTS.md (simpler than mb-3-cs's GStack-era AGENTS.md) and their own PDLF ledger for state.

---

## 8. GStack (current role)

- **Repo:** `C:\_mb2-cs-app\gstack` / `/mnt/c/_mb2-cs-app/gstack`
- **Skills:** 61 skill directories in `~/.config/opencode/skills/`
- **Role:** Workflow/orchestration layer accessed via skills. Key workflows: planning, implementation, review, QA, release, GBrain setup, learning, context management.
- **Docs:** `project-library/workspace/gstack/` — operating model, workflows, handoff contract

---

## 9. GBrain (current role)

- **Location:** `~/gbrain`, version `v0.18.2`
- **Engine:** Supabase (PostgreSQL), project `project-library`
- **Embeddings:** `text-embedding-3-large` via OpenRouter
- **Role:** Advisory memory layer. Recalled content must defer to current repo files, ADRs, and user decisions.
- **Docs:** `project-library/workspace/gbrain/` — operating model, source tiers
- **MCP:** Enabled in global config (`/home/dev-p/.bun/bin/gbrain serve`)

---

## 10. Command system

49 project-local command files in `project-library/.opencode/commands/` providing slash-command access to GStack skills. Each is a thin wrapper that delegates to the corresponding skill. Standard pattern:

```md
# /skill-name

Run the gstack-skill-name workflow.

When you run this command, you should:
- Load and use the `gstack-skill-name` skill.
- Follow the skill instructions exactly.
- Respect the current workspace rules, AGENTS.md, and OpenCode permissions.
- Keep changes minimal and fit for purpose.
```

No global commands directory exists — commands are defined at project level.
