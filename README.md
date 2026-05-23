# project-library

Documentation, planning, workspace governance, and GStack / GBrain integration for the Mybizz CS workspace.

---

## 1. Role of this repo

`project-library` is the **planning and documentation workspace** for the Mybizz CS system. It is responsible for:

- Architecture and refactor decisions (ADRs)
- Application documentation (design docs, specifications, policies)
- GStack-generated outputs (reports, planning outputs, task lists)
- Visual design references (wireframes, mockups)
- Workspace-level governance (how tools and agents should behave)
- Orchestration and memory design (GStack and GBrain docs)

No live application code lives here; that is in `mb-3-cs`.

---

## 2. Key directories

### Four-folder governance

```
adr/           — Architecture Decision Records
docs/          — Application documentation
gstack-outputs — GStack-generated outputs
wireframes/    — Visual design artifacts
```

#### `adr/` — Architecture Decision Records

- One decision per file
- Clear context, the decision taken, rationale
- Consequences, scope and constraints
- Explains why the system is built the way it is

#### `docs/` — Application documentation

- Design documents and specifications
- UI standards and architecture summaries
- Component mappings and policy documents
- Feature documentation
- Durable documentation describing the actual application

#### `gstack-outputs/` — GStack-generated outputs

- Office-hours reports
- CEO review reports
- ENG review reports
- Planning outputs and task lists
- Handoff notes
- Temporary or session-based artifacts

These are useful records of agent work but not canonical app documentation.

#### `wireframes/` — Visual design artifacts

- Wireframes and mockups
- Reference screens
- Static UI layout examples
- Design references (not policy documents)

### Additional folder

#### `workspace/` — Workspace orchestration

- `workspace-description.md` — how this OpenCode + WSL workspace runs
- `workspace-docmap.md` — path-centric index of config and docs
- `opencode-setup.md` — OpenCode setup/status tracker
- `gstack/` — GStack operating model and workflow docs
- `gbrain/` — GBrain operating model and source-tier docs

---

## 3. OpenCode configuration (docs project)

- `AGENTS.md`  
  Documentation-workspace rules:
  - provenance and trust model,
  - required workflow order (refine ADRs → validate AM/MM docs → update working docs),
  - how canonical devlog/todo files are used,
  - Plan vs Build responsibilities.

- `opencode.json`  
  Project-level OpenCode config:
  - `default_agent` = `plan`,
  - conservative permissions (`edit` / `webfetch` / `bash` all `ask`),
  - read access to:
    - `C:\_mb2-cs-app\mb-3-cs\`
    - `C:\_mb2-cs-app\devlog\`
    - `C:\_mb2-cs-app\todo\`.

- `.opencode\agents\`  
  Project-local agent definitions:
  - `plan` = primary planning/analysis/validation/architecture agent.
  - `build` = tightly scoped helper for non-live-code document changes.

- `.opencode\commands\`  
  Project-local commands for this repo. Preferred pattern on this machine instead of relying on the global `OPENCODE_CONFIG_DIR` environment variable.

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

## 6. Artifact governance

### File placement rules

- If the file is a GStack session output, place it in `gstack-outputs/`.
- If the file is a durable app document, place it in `docs/`.
- If the file is an architectural decision, place it in `adr/`.
- If the file is a screen reference or mockup, place it in `wireframes/`.

### Lifecycle guidance

- GStack outputs start in `gstack-outputs/`.
- Important decisions or stable guidance may be promoted into `docs/` or `adr/`.
- Wireframes remain in `wireframes/` unless replaced by updated visual references.

### Special notes

- `DESIGN.md` belongs in `docs/` because it is an application UI design document.
- Do not place long-lived app specifications in `gstack-outputs/`.
- Do not place transient planning reports in `docs/` unless promoted.
- Do not place ADRs inside `docs/` or `gstack-outputs/`.

### Path handling

File paths may be passed to GStack as Windows paths (e.g., `C:\_mb2-cs-app\project-library\adr\`), but the true staging location is `/mnt/c/` in WSL.

---

## 7. Where to start

If you are new to this repo and want the big picture:

1. Read `workspace/workspace-description.md` for an end-to-end explanation of how this workspace runs on this machine (OpenCode Desktop + WSL + GStack + GBrain).
2. Use `workspace/workspace-docmap.md` as the detailed path index.
3. Consult `AGENTS.md` and `opencode.json` before asking OpenCode to make changes, so agent behavior stays aligned with the intended rules.
4. Review the four-folder governance structure to understand where files belong.
