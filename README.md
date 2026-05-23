# project-library

Documentation, planning, workspace governance, devlog, prompts, and GStack / GBrain integration for the Mybizz CS workspace.

---

## 1. Role of this repo

`project-library` is the **planning and documentation workspace** for the Mybizz CS system. It is responsible for:

- Architecture and refactor decisions (ADRs and related docs)
- Mapping multi‑vertical to single‑vertical behavior (MM/AM docs)
- Handover notes and open tasks
- Workspace‑level governance (how tools and agents should behave)
- Orchestration and memory design (GStack and GBrain docs)

No live application code lives here; that is in `mb-3-cs`.

---

## 2. Key directories

- `adr\`  
  Architecture Decision Records and related reference notes.

- `docs\`  
  Supporting documentation, including AM/MM method documents and reference material.

- `handovers\`  
  Handoff notes between sessions, people, or phases (Plan ↔ Build, docs ↔ code).

- `open-tasks\`  
  Workspace‑local task lists and planning notes that complement the canonical `todo` repo.

- `prompts\`  
  Prompt and build‑rules drafts for OpenCode/GStack usage.

- `brain-resources\`  
  Early material and resources intended to support future GBrain ingestion (not yet wired).

- `workspace\`  
  Workspace‑level orchestration and integration docs:
  - `workspace-description.md` — how this OpenCode + WSL workspace runs on this machine.
  - `workspace-docmap.md` — path‑centric index of config, orchestration, and memory docs.
  - `opencode-setup.md` — OpenCode setup/status tracker.
  - `gstack\` — GStack operating model and workflow docs.
  - `gbrain\` — GBrain operating model and source‑tier docs.

---

## 3. OpenCode configuration (docs project)

- `AGENTS.md`  
  Documentation‑workspace rules:
  - provenance and trust model,
  - required workflow order (refine ADRs → validate AM/MM docs → update working docs),
  - how canonical devlog/todo files are used,
  - Plan vs Build responsibilities.

- `opencode.json`  
  Project‑level OpenCode config:
  - `default_agent` = `plan`,
  - conservative permissions (`edit` / `webfetch` / `bash` all `ask`),
  - read access to:
    - `C:\_mb2-cs-app\mb-3-cs\`
    - `C:\_mb2-cs-app\devlog\`
    - `C:\_mb2-cs-app\todo\`.

- `.opencode\agents\`  
  Project‑local agent definitions:
  - `plan` = primary planning/analysis/validation/architecture agent.
  - `build` = tightly scoped helper for non‑live‑code document changes.

- `.opencode\commands\`  
  Project‑local commands for this repo. Preferred pattern on this machine instead of relying on the global `OPENCODE_CONFIG_DIR` environment variable.

---

## 4. Shared tracking (devlog & todo)

`project-library` shares canonical tracking with `mb-3-cs`:

- Devlog:
  - `C:\_mb2-cs-app\devlog\README.md`
  - `C:\_mb2-cs-app\devlog\index.md`

- Todos:
  - Active backlog: `C:\_mb2-cs-app\todo\todo.md`
  - Archive: `C:\_mb2-cs-app\todo\todo-archive.md`

Plan is allowed to write canonical tracking where rules permit; Build may read but must hand off tracking changes.

---

## 5. GStack & GBrain docs

This repo contains the **design docs** for GStack and GBrain in the Mybizz CS workspace:

- GStack (workflow/orchestration):  
  `workspace\gstack\`
  - `gstack-operating-model.md`
  - `gstack-workflows.md`
  - `gstack-handoff-contract.md`
  - plus additional scaffolding files for commands, roles, overview, and installation.

- GBrain (advisory memory):  
  `workspace\gbrain\`
  - `gbrain-operating-model.md`
  - `gbrain-source-tiers.md`
  - plus additional scaffolding files for ingestion, sync, and repo strategy.

These documents define how workflows and memory **should** behave. Actual runtime wiring (commands, ingestion, recall) is implemented incrementally and tracked in `workspace\opencode-setup.md`.

---

## 6. Where to start

If you are new to this repo and want the big picture:

1. Read `workspace\workspace-description.md` for an end‑to‑end explanation of how this workspace runs on this machine (OpenCode Desktop + WSL + GStack + GBrain).
2. Use `workspace\workspace-docmap.md` as the detailed path index.
3. Consult `AGENTS.md` and `opencode.json` before asking OpenCode to make changes, so agent behavior stays aligned with the intended rules.