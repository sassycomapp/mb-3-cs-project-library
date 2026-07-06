# GStack (Workspace Docs)

Working notes and operating docs for how GStack is used in the Mybizz CS workspace, running on:

- OpenCode Desktop on Windows 11,
- OpenCode server in WSL (Ubuntu),
- Qwen 3.6 Plus as the primary model,
- GStack skills as the workflow layer,
- GBrain (Supabase + OpenRouter) as the advisory memory layer.

GStack here is a **workflow and orchestration layer** around OpenCode. It does not override OpenCode’s permissions, project rules, or protected‑file behavior.

---

## 1. Role of this folder

This folder contains the **design and operating docs** for GStack in this workspace. It is not the GStack code itself (which lives in `C:\mybizz\gstack`), and it is not a live config directory.

Use these docs to answer:

- “How should Plan and Build coordinate?”
- “What workflows should exist for docs and code work?”
- “How should handoffs and cross‑repo work be structured?”
- “How does GStack sit underneath OpenCode and above GBrain?”
- “Which skills/commands should the agents actually use?”

Operational wiring (skills, commands) and status are tracked in:

- `workspace\opencode-setup.md`
- `workspace\workspace-description.md`
- `workspace\workspace-docmap.md`

---

## 2. Files in this folder

### 2.1 Core populated docs

- `gstack-operating-model.md`  
  Defines the GStack operating model:

  - GStack as a workflow/orchestration layer around OpenCode.
  - Authority order:
    1. User decision,
    2. Repo files and canonical tracking,
    3. OpenCode rules (global + project),
    4. GStack workflows,
    5. GBrain memory.
  - Scope across:
    - `project-library`,
    - `mb-3-cs`,
    - `devlog`,
    - `todo`,
    - `gstack`,
    - and GBrain assets.

- `gstack-workflows.md`  
  Defines the main GStack workflows:

  - `docs-plan` (planning in `project-library`),
  - `code-plan-build` (implementation in `mb-3-cs`),
  - `status-handover` (cross-session and cross-repo status and handoff).

- `gstack-handoff-contract.md`  
  Defines required structure when work crosses:

  - Plan → Build,
  - Build → Plan,
  - docs ↔ code,
  - one session ↔ another.

  Includes Plan→Build and Build→Plan templates and the minimum quality bar.

- `gstack-overview.md`  
  High-level explanation of what GStack is in this workspace, how it relates to OpenCode and GBrain, and a summary of the three workflows.

- `gstack-install-plan.md`  
  Records how GStack is installed and integrated:

  - repo locations,
  - WSL script normalization,
  - validation steps,
  - deliberate choice to use skills and docs rather than `.opencode\gstack` config.

- `gstack-role-map.md`  
  Maps:

  - GStack workflows,
  - OpenCode agents (`plan`, `build`),
  - repos (`project-library`, `mb-3-cs`),
  - and your responsibilities.

- `gstack-command-map.md`  
  Maps desired workflows to:

  - GStack-related skills (slash commands),
  - optional `.opencode\commands\` entries,
  - and when/where to use them.

- `gstack-open-adaption.md`  
  Explains how stock GStack concepts are adapted to:

  - OpenCode permissions and AGENTS,
  - protected-file and backup rules,
  - shared devlog/todo,
  - your personal working style.

### 2.2 This README

- `README.md` (this file)  
  Index and high-level overview of the GStack docs for this workspace.

---

## 3. Relationship to other workspace docs

- **OpenCode configuration and status:**
  - `workspace\opencode-setup.md`  
    Setup/status tracker for:
    - global + project config,
    - permissions and protected files,
    - WSL server bridge,
    - GStack and GBrain status.

- **Workspace overview and structure:**
  - `workspace\workspace-description.md`  
    Human-facing guide to:
    - how to start sessions on this specific machine,
    - how `project-library` vs `mb-3-cs` are used,
    - where GStack and GBrain fit.

  - `workspace\workspace-docmap.md`  
    Path-centric map of all major config and doc files, including this folder.

- **GBrain docs:**
  - `workspace\gbrain\*`  
    GBrain operating model, source tiers, ingestion/sync policy, and repo strategy.

GStack workflows may use GBrain for recall, but always under the authority rules defined there.

---

## 4. Current status and next steps

Current status:

- Core GStack operating model, workflows, handoff contract, and role/overview docs are in place and aligned with the actual OpenCode setup.
- GStack is accessible to agents via skills (slash commands) when sessions run against the WSL server and the correct project paths.
- No `.opencode\gstack` config exists; this is a deliberate choice to keep authority with OpenCode and use GStack primarily as a workflow layer.

Next steps:

1. Keep `gstack-command-map.md` updated with the **small set of workflows and skills you actually use regularly**.
2. If you ever add `.opencode\commands\` wrappers for GStack workflows, document them there and in `opencode-setup.md`.
3. Periodically review `gstack-role-map.md` to ensure Plan/Build responsibilities match how you’re actually working.