# GStack Install and Integration Plan (Mybizz CS)

## 1. Purpose

This document records how GStack is installed and integrated into the Mybizz CS workspace and how it is **actually used** with OpenCode.

It is not a generic guide; it is specific to this Windows + WSL setup.

---

## 2. Components

- **GStack repository**
  - Windows: `C:\_mb2-cs-app\gstack`
  - WSL: `/mnt/c/_mb2-cs-app/gstack`
  - Status:
    - Repo cloned.
    - `node_modules` present.
    - Shell scripts in `bin/` converted to LF for WSL.
    - Commands like `./bin/gstack-config --help` run successfully in WSL.

- **OpenCode integration**
  - GStack is exposed to agents primarily via **skills** (slash commands), not via `.opencode\gstack` config.
  - No GStack config lives in:
    - `C:\Users\dev-p\.config\opencode\gstack\`
    - `project-library\.opencode\gstack\`
    - `mb-3-cs\.opencode\gstack\`
  - Instead, behavior is described in:
    - `workspace\opencode-setup.md`
    - `workspace\workspace-description.md`
    - `workspace\workspace-docmap.md`
    - this `workspace\gstack\` folder.

---

## 3. Install steps (completed)

1. **Clone GStack repo**
   - Clone upstream GStack into `C:\_mb2-cs-app\gstack`.
   - Ensure WSL sees it at `/mnt/c/_mb2-cs-app/gstack`.

2. **Normalize scripts for WSL**
   - Convert `gstack\bin\*.sh` to LF endings.
   - Confirm commands such as:
     - `./bin/gstack-config --help`
     - other basic helpers
     run successfully in WSL.

3. **Validate WSL + OpenCode + GStack**
   - Run OpenCode server in WSL:
     - `opencode serve --hostname 0.0.0.0 --port 4096`.
   - Connect OpenCode Desktop to `http://localhost:4096` as `wsl-opencode`.
   - Confirm agents can:
     - see `/mnt/c/_mb2-cs-app/project-library` and `/mnt/c/_mb2-cs-app/mb-3-cs`,
     - call GStack-related skills (e.g. `/setup-gbrain`, plan/build/status workflows).

4. **Document the operating model**
   - Write:
     - `gstack-operating-model.md`
     - `gstack-workflows.md`
     - `gstack-handoff-contract.md`
   - Align them with the actual OpenCode configs and workspace docs.

---

## 4. Integration approach

We deliberately **do not** currently use:

- `.config\opencode\gstack\` for global GStack config,
- `.opencode\gstack\` in each repo.

Instead:

- GStack runs via skillpacks and CLI pieces where needed.
- OpenCode remains the source of truth for:
  - what commands agents are allowed to run,
  - which tools can operate on which files,
  - protected-file behavior.

This keeps GStack:

- powerful for workflow,
- narrow in authority.

---

## 5. Future integration options (optional)

If needed later, we may:

- add project-local `.opencode\commands\` entries that wrap GStack workflows (for example: one command that kicks off a standard docs-plan or code-plan-build flow),
- introduce minimal `.opencode\gstack\` config **only** if there is strong value in having per-project GStack defaults beyond what skills already provide.

Any such changes must:

- respect AGENTS and permission rules,
- be reflected in `opencode-setup.md`, `workspace-description.md`, and `workspace-docmap.md`.