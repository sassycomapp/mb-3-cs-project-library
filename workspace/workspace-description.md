# OpenCode Workspace Description & Usage Guide

**What this workspace is:**  
This is your dual-project OpenCode environment on Windows 11, configured for both documentation/refinement work (`project-library`) and live application development (`mb-2-cs`). It runs OpenCode Desktop on Windows, with a WSL-hosted OpenCode server that the desktop client can connect to for Linux-side tools and paths.

---

## 0. Current state (2026‑05‑02)

- **Desktop client:** OpenCode Desktop v1.14.30 on Windows 11.

- **WSL server:** OpenCode CLI v1.14.30 running in WSL with:
  - Command: `opencode serve --hostname 0.0.0.0 --port 4096`
  - Health check from Windows browser: `http://localhost:4096/global/health` → `{"healthy":true,"version":"1.14.30"}`

- **Desktop ↔ WSL bridge:**
  - Desktop has a server entry named `wsl-opencode` pointing at `http://localhost:4096`.
  - When the WSL server is running, the Desktop client can operate against Linux paths such as `/mnt/c/_mb2-cs-app/project-library`.

- **Project access on this machine (current behaviour):**
  - The “Open project” buttons in OpenCode Desktop are unreliable on this install.
  - Projects open reliably by sending the WSL path in the chat input once the Desktop is connected to `wsl-opencode`, for example:
    - `/mnt/c/_mb2-cs-app/project-library`
    - `/mnt/c/_mb2-cs-app/mb-2-cs`

- **GStack repo status:**
  - Local clone at `C:\_mb2-cs-app\gstack`.
  - WSL path: `/mnt/c/_mb2-cs-app/gstack`.
  - Core scripts in `bin/` converted to Unix line endings and confirmed runnable (for example: `./bin/gstack-config --help` works inside WSL).

- **Workspace-level docs for orchestration and memory:**
  - `C:\_mb2-cs-app\project-library\workspace\gstack\` — GStack operating model and workflow docs.
  - `C:\_mb2-cs-app\project-library\workspace\gbrain\` — GBrain operating model and source-tier docs.
  - `C:\_mb2-cs-app\project-library\workspace\opencode-setup.md` — setup/status tracker.
  - `C:\_mb2-cs-app\project-library\workspace\workspace-description.md` — this document.
  - `C:\_mb2-cs-app\project-library\workspace\workspace-docmap.md` — structure/ownership map.
  
This section is a status snapshot only; the detailed configuration policy and task tracking live in `opencode-setup-2.md`.

---

## 1. Global layer

- **Global config location:**  
  `C:\Users\dev-p\.config\opencode\`

- **Global rules:**  
  `C:\Users\dev-p\.config\opencode\AGENTS.md`  
  Applies to all OpenCode sessions (desktop, TUI, IDE). Enforces concise, stepwise, validation‑first responses with conservative edit/command behavior.

- **Global config:**  
  `C:\Users\dev-p\.config\opencode\opencode.json`  
  Contains provider configuration, default models, global agents, tools, permissions, and any future keybinds/commands.

- **Model policy (current):**
  - Provider: `openrouter`
  - Primary model: `openrouter/qwen/qwen3.6-plus` (planning/analysis)
  - Lightweight model: `openrouter/qwen/qwen3.6-plus` (implementation / quick tasks)

  Project-level configs may override which agent is default, but both projects currently rely on the same provider/model set defined globally.

---

## 2. Project-library (MM1/AM1 planning & docs)

- **Path (Windows):**  
  `C:\_mb2-cs-app\project-library`

- **Purpose:**  
  Documentation workspace for analyzing, validating, and refining the multi‑vertical → single‑vertical conversion, maintaining ADRs, handovers, open tasks, and refactor notes.

- **Default agent:**  
  `plan` (configured to use `qwen3.6-plus` via the global provider setup) for high‑quality reasoning, validation, and architectural analysis.

- **How to run (Desktop + WSL on this machine):**

  - **Startup:**
    1. In WSL (Ubuntu), start the OpenCode server:

       ```bash
       opencode serve --hostname 0.0.0.0 --port 4096
       ```

       Leave this terminal running.

    2. Start OpenCode Desktop on Windows and ensure it is connected to the `wsl-opencode` server entry (`http://localhost:4096`).

    3. In the Desktop chat input, send:

       ```text
       /mnt/c/_mb2-cs-app/project-library
       ```

       to open the `project-library` workspace (this is more reliable than using the “Open project” buttons on this install).

  - **Planning / analysis tasks:**
    1. Confirm the active workspace is `C:\_mb2-cs-app\project-library` (or `/mnt/c/_mb2-cs-app/project-library` in the shell) using `pwd` in the OpenCode terminal if needed.
    2. Use the `plan` agent for document analysis, validation of MM/AM documents, ADR review, and architecture/refactor reasoning.
    3. Let the agent read ADRs, `docs\am1-*/mm1-*` materials, and canonical devlog/todo files as needed, but keep file edits guarded; agents must ask for approval before editing or running commands.

  - **Build / mechanical document tasks:**
    1. Explicitly switch to the `build` agent only when you want tightly scoped, non-live-code document work (for example: restructuring a section, applying an agreed mapping, adjusting wording).
    2. Treat `build` as an execution/helper agent; keep higher-level reasoning, provenance checks, and cross-repo thinking in `plan`.

  - **GStack/GBrain awareness:**
    - For workflow expectations, consult the workspace-level GStack docs:
      - `workspace\gstack\gstack-operating-model.md`
      - `workspace\gstack\gstack-workflows.md`
      - `workspace\gstack\gstack-handoff-contract.md`
    - For memory and provenance rules, consult:
      - `workspace\gbrain\gbrain-operating-model.md`
      - `workspace\gbrain\gbrain-source-tiers.md`
	  
- **Devlog & todos (canonical, shared with mb-2-cs):**
  - Devlog:
    - `C:\_mb2-cs-app\devlog\README.md`
    - `C:\_mb2-cs-app\devlog\index.md`
  - Active backlog: `C:\_mb2-cs-app\todo\todo.md`
  - Archive: `C:\_mb2-cs-app\todo\todo-archive.md`

---

## 3. mb-2-cs (live app build)

- **Path (Windows):**  
  `C:\_mb2-cs-app\mb-2-cs`

- **Purpose:**  
  Production‑adjacent live application code repository with conservative change policies and explicit protections for Anvil‑related files.

- **Default agent:**  
  `build` (using `qwen3.6-plus`) for implementation tasks, with `plan` available for design and architecture reasoning.

- **How to run (Desktop + WSL on this machine):**

  - **Startup:**
    1. In WSL (Ubuntu), start the OpenCode server (if not already running):

       ```bash
       opencode serve --hostname 0.0.0.0 --port 4096
       ```

    2. Start OpenCode Desktop on Windows and connect to the `wsl-opencode` server entry.

    3. In the Desktop chat input, send:

       ```text
       /mnt/c/_mb2-cs-app/mb-2-cs
       ```

       to open the `mb-2-cs` workspace.

  - **Implementation tasks:**
    1. Confirm the active workspace is `C:\_mb2-cs-app\mb-2-cs` (or `/mnt/c/_mb2-cs-app/mb-2-cs` in the shell) using `pwd` if needed.
    2. Use the default `build` agent for explicit user-directed coding tasks that you have already approved (for example: implementing a specific change described in a Plan handoff).
    3. Respond to permission prompts for edits/commands explicitly; there should be no free-running changes without your confirmation.

  - **Planning / architecture tasks:**
    1. Switch to the `plan` agent when you need structural decisions, refactor planning, or impact analysis that spans multiple files or touches protected areas.
    2. Once the plan is stable (ideally in the form of a clear handoff using the GStack handoff contract), return to `build` for concrete edits.

  - **GStack awareness:**
    - For code work, the relevant GStack workflow is typically `code-plan-build` as described in:
      - `workspace\gstack\gstack-operating-model.md`
      - `workspace\gstack\gstack-workflows.md`
    - Use the handoff templates from `workspace\gstack\gstack-handoff-contract.md` when moving between Plan and Build or between repos.

- **Critical protections (enforced via AGENTS/opencode configs):**
  - Hard‑denied edits to:
    - `anvil.yaml`
    - `parameters.yaml`
    - `templates.yaml`
    - All `form_template.yaml` files
  - `standard-page.html` and `theme.css` are protected by default and may only be edited on explicit request.

- **Devlog & todos (canonical, shared with project-library):**
  - Devlog:
    - `C:\_mb2-cs-app\devlog\README.md`
    - `C:\_mb2-cs-app\devlog\index.md`
  - Active backlog: `C:\_mb2-cs-app\todo\todo.md`
  - Archive: `C:\_mb2-cs-app\todo\todo-archive.md`
  
- **Critical protections (enforced via AGENTS/opencode configs):**
  - Hard‑denied edits to:
    - `anvil.yaml`
    - `parameters.yaml`
    - `templates.yaml`
    - All `form_template.yaml` files
  - `standard-page.html` and `theme.css` are protected by default and may only be edited on explicit request.

- **Devlog & todos (canonical, shared with project-library):**
  - Devlog:
    - `C:\_mb2-cs-app\devlog\README.md`
    - `C:\_mb2-cs-app\devlog\index.md`
  - Active backlog: `C:\_mb2-cs-app\todo\todo.md`
  - Archive: `C:\_mb2-cs-app\todo\todo-archive.md`

---

## 4. Tools and integration

- **MGrep in WSL:**  
  Available for codebase searching and analysis in the WSL environment. Intended to be run against `/mnt/c/_mb2-cs-app/...` paths so it sees the same repositories the Windows side uses.

- **OpenCode Desktop on Windows 11:**  
  - Primary day‑to‑day interface.
  - Verified shortcut behavior:
    - `Ctrl+P` opens the action list / command palette.
    - `Ctrl+X` and `F2` are not bound and should not be assumed to exist.

- **OpenCode server in WSL:**
  - The WSL instance runs the OpenCode server that Desktop can attach to:
    - `opencode serve --hostname 0.0.0.0 --port 4096`
  - Desktop server entry:
    - Name: `wsl-opencode`
    - Address: `http://localhost:4096`
  - This lets Desktop sessions operate against Linux paths (for example `/mnt/c/_mb2-cs-app/project-library`) while still using the Windows UI.

---

## 5. GStack (current and future)

- **Actual repo:**
  - Location (Windows): `C:\_mb2-cs-app\gstack`
  - Location (WSL): `/mnt/c/_mb2-cs-app/gstack`
  - Status:
    - Repository cloned.
    - `node_modules` present.
    - Shell scripts in `bin/` converted to LF line endings for WSL.
    - `./bin/gstack-config --help` and related commands are runnable.

- **Planned integration:**
  - GStack is intended to become an additional “meta‑layer” around this workspace, providing:
    - Higher‑level workflows for doc and code changes.
    - Possible GBrain integration for long‑term learning/logging.
  - Detailed GStack usage, command maps, and role maps live in the `gstack` documentation set:
    - `workspace/gstack/gstack-overview.md`
    - `workspace/gstack/gstack-install-plan.md`
    - `workspace/gstack/gstack-command-map.md`
    - `workspace/gstack/gstack-open-adaption.md`
    - `workspace/gstack/gstack-role-map.md`

- **Not yet implemented:**
  - No GStack‑specific config has been added under `.config\opencode` or `.opencode\gstack` in the repos.
  - GStack is currently scoped to local experimentation from `/mnt/c/_mb2-cs-app/gstack`, not wired into OpenCode or the IDE as an always‑on layer.

---

## 6. How to start a session

**Planning task in project-library:**

1. Open OpenCode Desktop with the workspace root set to `C:\_mb2-cs-app\project-library`.
2. Ensure the session is using the `plan` agent (default for this project).
3. Begin with document analysis, validation, or architectural reasoning tasks; let the agent read ADRs, MM/AM docs, and current todos.

**Implementation task in mb-2-cs:**

1. Open OpenCode Desktop with the workspace root set to `C:\_mb2-cs-app\mb-2-cs`.
2. Use the default `build` agent for explicit user‑directed coding tasks.
3. Switch to `plan` when you need higher‑level reasoning before making changes, then return to `build` for the concrete edits.