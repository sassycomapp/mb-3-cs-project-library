# OpenCode Workspace Description & Usage Guide

**What this workspace is:**  
This is your dual-project OpenCode environment on Windows 11, configured for both documentation/refinement work (`project-library`) and live application development (`mb-3-cs`). It runs OpenCode Desktop on Windows, with a WSL-hosted OpenCode server that the desktop client can connect to for Linux-side tools and paths.

---

## 0. Current state (2026‑05‑03)

- **Desktop client:** OpenCode Desktop v1.14.30 on Windows 11.

- **WSL server:** OpenCode CLI v1.14.30 running in WSL with:
  - Command: `opencode serve --hostname 0.0.0.0 --port 4096`
  - Health check from Windows browser: `http://localhost:4096/global/health` → `{"healthy":true,"version":"1.14.30"}`

- **Desktop ↔ WSL bridge:**
  - Desktop has a server entry named `wsl-opencode` pointing at `http://localhost:4096`.
  - When the WSL server is running, the Desktop client can operate against Linux paths such as `/mnt/c/_mb2-cs-app/project-library` and `/mnt/c/_mb2-cs-app/mb-3-cs`.

- **Project access on this machine (current behaviour):**
  - The “Open project” buttons in OpenCode Desktop are unreliable on this install.
  - Projects open reliably by sending the WSL path in the chat input once the Desktop is connected to `wsl-opencode`, for example:
    - `/mnt/c/_mb2-cs-app/project-library`
    - `/mnt/c/_mb2-cs-app/mb-3-cs`

- **Model / provider stack (global):**
  - Provider: `openrouter`
  - Primary model: `openrouter/qwen/qwen3.6-plus` (planning/analysis)
  - Lightweight model: `openrouter/qwen/qwen3.6-plus` (implementation / quick tasks)
  - Both projects use this same model set via global and project-level OpenCode config. [file:576]

- **GStack status (runtime):**
  - Local repo: `C:\\_mb2-cs-app\\gstack` (Windows) / `/mnt/c/_mb2-cs-app/gstack` (WSL).
  - Core scripts in `bin/` converted to Unix line endings and confirmed runnable (for example: `./bin/gstack-config --help` works inside WSL).
  - GStack skills (slash commands) are available to the OpenCode agents and are actively used as the primary workflow/orchestration layer (plan, build, investigate, review, ship, `/setup-gbrain`, etc.), while OpenCode remains the authority for permissions and protected-file rules. [file:576][web:212][web:560][web:568]

- **GBrain status (runtime):**
  - GBrain installed at `~/gbrain` (version `v0.18.2`).
  - Engine: **Supabase (PostgreSQL)** project `project-library` (`fnjcssmqljshckdksvam`) with a verified connection via the Supabase pooler URL.
  - Embeddings: `text-embedding-3-large` via **OpenRouter**, using `OPENAI_API_BASE=https://openrouter.ai/api/v1` and an OpenRouter key. [web:532][web:533]
  - Local patch applied to `~/gbrain/src/core/embedding.ts` so the embedding client respects `OPENAI_API_BASE` instead of hardcoding `https://api.openai.com` (this is a local fork change and will be lost on `gbrain` upgrade unless re-applied).
  - Smoke test from `/setup-gbrain`: **Passed** — write + embed + search round-trip confirmed. [web:239][web:466]

- **Workspace-level docs for orchestration and memory:**
  - `C:\\_mb2-cs-app\\project-library\\workspace\\gstack\\` — GStack operating model and workflow docs.
  - `C:\\_mb2-cs-app\\project-library\\workspace\\gbrain\\` — GBrain operating model and source-tier docs.
  - `C:\\_mb2-cs-app\\project-library\\workspace\\opencode-setup.md` — setup/status tracker.
  - `C:\\_mb2-cs-app\\project-library\\workspace\\workspace-description.md` — this document.
  - `C:\\_mb2-cs-app\\project-library\\workspace\\workspace-docmap.md` — structure/ownership map.

This section is a status snapshot only; the detailed configuration policy and task tracking live in `opencode-setup.md`.

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
      - `workspace\\gstack\\gstack-operating-model.md`
      - `workspace\\gstack\\gstack-workflows.md`
      - `workspace\\gstack\\gstack-handoff-contract.md`
    - In practice, the `plan` and `build` agents may invoke GStack skills (slash commands) to run `docs-plan`, `code-plan-build`, and `status-handover` workflows while respecting OpenCode permissions and protected-file rules. [file:576][web:568]
    - For memory and provenance rules, consult:
      - `workspace\\gbrain\\gbrain-operating-model.md`
      - `workspace\\gbrain\\gbrain-source-tiers.md`
    - GBrain is configured as a Supabase-backed brain with OpenRouter embeddings; agents can use GStack-aware skills to read/write advisory memory there, but recalled content must always defer to current repo files and ADRs.
	  
- **Devlog & todos (canonical, shared with mb-3-cs):**
  - Devlog:
    - `C:\_mb2-cs-app\devlog\README.md`
    - `C:\_mb2-cs-app\devlog\index.md`
  - Active backlog: `C:\_mb2-cs-app\todo\todo.md`
  - Archive: `C:\_mb2-cs-app\todo\todo-archive.md`

---

## 3. mb-3-cs (live app build)

- **Path (Windows):**  
  `C:\_mb2-cs-app\mb-3-cs`

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
       /mnt/c/_mb2-cs-app/mb-3-cs
       ```

       to open the `mb-3-cs` workspace.

  - **Implementation tasks:**
    1. Confirm the active workspace is `C:\_mb2-cs-app\mb-3-cs` (or `/mnt/c/_mb2-cs-app/mb-3-cs` in the shell) using `pwd` if needed.
    2. Use the default `build` agent for explicit user-directed coding tasks that you have already approved (for example: implementing a specific change described in a Plan handoff).
    3. Respond to permission prompts for edits/commands explicitly; there should be no free-running changes without your confirmation.

  - **Planning / architecture tasks:**
    1. Switch to the `plan` agent when you need structural decisions, refactor planning, or impact analysis that spans multiple files or touches protected areas.
    2. Once the plan is stable (ideally in the form of a clear handoff using the GStack handoff contract), return to `build` for concrete edits.

- **GStack awareness:**
  - For code work, the relevant GStack workflow is typically `code-plan-build` as described in:
    - `workspace\\gstack\\gstack-operating-model.md`
    - `workspace\\gstack\\gstack-workflows.md`
  - The `plan` and `build` agents may call GStack skills (slash commands) to structure implementation work into:
    - planning (`/office-hours`, `/plan-ceo-review`, `/plan-eng-review`),
    - execution (`/build`, `/investigate`),
    - review and release (`/review`, `/qa`, `/ship`),
    while still obeying OpenCode’s conservative permissions and protected-file policies. [file:576][web:212][web:560]
  - Use the handoff templates from `workspace\\gstack\\gstack-handoff-contract.md` when moving between Plan and Build or between repos, so that GStack workflows and the OpenCode agents share the same task context.

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

## 5. GStack (current runtime role)

- **Actual repo:**
  - Location (Windows): `C:\\_mb2-cs-app\\gstack`
  - Location (WSL): `/mnt/c/_mb2-cs-app/gstack`
  - Status:
    - Repository cloned.
    - `node_modules` present.
    - Shell scripts in `bin/` converted to LF line endings for WSL.
    - `./bin/gstack-config --help` and related commands are runnable from WSL.

- **Runtime integration:**
  - GStack acts as a workflow/orchestration layer around OpenCode, not as a replacement for OpenCode’s rules:
    - OpenCode (AGENTS, `opencode.json`, protected-file rules) remains the authority for what can be edited or executed.
    - GStack provides structured workflows (docs-plan, code-plan-build, status-handover) that the agents can invoke via skills (slash commands) in chat. [file:576][web:568]
  - In day-to-day use, you interact with the OpenCode `plan` / `build` agents; they in turn may call GStack skills such as:
    - planning: `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`,
    - implementation: `/build`, `/investigate`,
    - review/release: `/review`, `/qa`, `/ship`,
    - memory setup: `/setup-gbrain`.
  - These skills run against the WSL-side repos (`/mnt/c/_mb2-cs-app/project-library`, `/mnt/c/_mb2-cs-app/mb-3-cs`) and are bound by OpenCode’s permission prompts and protected-file configuration. [web:212][web:560][web:568]

- **Docs and maps:**
  - Detailed GStack usage, command maps, and role maps live in the workspace `gstack` documentation set:
    - `workspace/gstack/gstack-overview.md`
    - `workspace/gstack/gstack-install-plan.md`
    - `workspace/gstack/gstack-command-map.md`
    - `workspace/gstack/gstack-open-adaption.md`
    - `workspace/gstack/gstack-role-map.md`
    - plus the already-populated:
      - `workspace/gstack/gstack-operating-model.md`
      - `workspace/gstack/gstack-workflows.md`
      - `workspace/gstack/gstack-handoff-contract.md`

- **Config integration (current state):**
  - No GStack-specific config has been added yet under:
    - `C:\\Users\\dev-p\\.config\\opencode\\gstack\\` (global), or
    - `.opencode\\gstack\\` in `project-library` or `mb-3-cs` (project-local).
  - GStack is currently wired in via:
    - its skillpack / skills exposed to the OpenCode agents, and
    - the workspace documentation, not via `.opencode` command wrappers.
  - Future work (optional):
    - Add project-local OpenCode commands in `.opencode\\commands\\` that directly trigger common GStack workflows (docs-plan, code-plan-build, status-handover) if/when needed.

---

## 6. How to start a session (current, WSL‑backed workflow)

On this machine, every OpenCode session goes through the WSL server first, then the Desktop client, then a path‑based “open project” message. This sequence ensures that GStack skills and GBrain access are available inside both projects.

### 6.1 Start the WSL OpenCode server (Ubuntu terminal)

1. Open **Ubuntu 22.04.5 LTS** (WSL) and get a bash prompt.

2. In that terminal, start the OpenCode server:

   ```bash
   opencode serve --hostname 0.0.0.0 --port 4096
   ```

3. Leave this terminal running while you work.  
   - Expected output (example):

     ```text
     Warning: OPENCODE_SERVER_PASSWORD is not set; server is unsecured.
     opencode server listening on http://0.0.0.0:4096
     ```

4. Do **not** run OpenCode commands such as `opencode plan` or `opencode build` directly in this terminal for normal work; its job is to host the server that the Desktop app connects to.

### 6.2 Attach OpenCode Desktop to the WSL server

1. Start **OpenCode Desktop** on Windows 11.

2. In Desktop, ensure you are connected to the `wsl-opencode` server entry:

   - Name: `wsl-opencode`  
   - Address: `http://localhost:4096`

3. Once connected, Desktop sessions will operate against **WSL paths** (for example `/mnt/c/_mb2-cs-app/project-library`) but still use the Windows UI.

### 6.3 Open a project from Desktop (via WSL path in chat)

On this install, the “Open project” buttons are unreliable. The dependable way to open each project is to paste the WSL path into the chat input **after** the server connection is active.

**To work in `project-library` (planning & docs):**

1. In an OpenCode Desktop chat (connected to `wsl-opencode`), send:

   ```text
   /mnt/c/_mb2-cs-app/project-library
   ```

2. Wait for Desktop to switch the workspace to `C:\\_mb2-cs-app\\project-library` (you can confirm via the OpenCode terminal in that session by running `pwd`).

3. The default agent for this project is `plan`. Use this workspace for:
   - document analysis and validation (MM/AM, ADRs),
   - architecture/refactor reasoning,
   - prompt and workflow documentation.

4. The `plan` and `build` agents in this workspace may call GStack skills (for example, `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`) and can use GBrain-backed memory via those skills, subject to GBrain’s source-tier and provenance rules.

**To work in `mb-3-cs` (live application code):**

1. In an OpenCode Desktop chat (connected to `wsl-opencode`), send:

   ```text
   /mnt/c/_mb2-cs-app/mb-3-cs
   ```

2. Wait for Desktop to switch the workspace to `C:\\_mb2-cs-app\\mb-3-cs` (again, confirm via `pwd` in the OpenCode terminal).

3. The default agent for this project is `build`. Use this workspace for:
   - explicit, user-directed coding tasks,
   - implementing agreed plans from `project-library`,
   - running tests and applying small, clearly scoped changes.

4. Switch to the `plan` agent when you need:
   - structural/architectural decisions,
   - refactor planning,
   - impact analysis across multiple files.

5. In this workspace, GStack skills (for example, `/build`, `/investigate`, `/review`, `/ship`) can be used by the agents to structure implementation, debugging, and release work. These skills can also read from and write to GBrain via the configured Supabase-backed brain, but they always remain subject to:
   - AGENTS rules,
   - project `opencode.json` permissions,
   - and the protected-file configuration (Anvil and template files).
   
## 7. GBrain (runtime summary)

This section summarizes the current running GBrain setup so both you and the agents know what is backing long‑term memory.

- **Location and version:**
  - GBrain repo: `~/gbrain` (inside WSL).
  - GBrain version: `v0.18.2`.

- **Engine (database):**
  - Engine: **Supabase (PostgreSQL)**.
  - Project name: `project-library`.
  - Project ID: `fnjcssmqljshckdksvam`.
  - Connection: uses the Supabase pooler URL configured during the `/setup-gbrain` flow.

- **Embeddings:**
  - Model: `text-embedding-3-large`.
  - Provider: **OpenRouter**.
  - Base URL: `https://openrouter.ai/api/v1` via the `OPENAI_API_BASE` environment variable.
  - OpenRouter key: stored as an environment variable and used by GBrain’s embedding client.

- **Local patch for OpenRouter:**
  - File patched: `~/gbrain/src/core/embedding.ts`.
  - Change: GBrain’s embedding client now reads `OPENAI_API_BASE` instead of hardcoding `https://api.openai.com`, allowing it to talk to OpenRouter’s embedding endpoint via an OpenAI‑compatible interface.
  - Impact: this is a **local fork change**. Upgrading GBrain may overwrite it; any upgrade must re‑apply or retire this patch explicitly.

- **Integration with GStack and OpenCode:**
  - GBrain was wired into GStack via the `/setup-gbrain` skill, which:
    - detected/installed the GBrain CLI,
    - configured the Supabase engine and connection,
    - set the embedding model and OpenRouter base URL,
    - ran `gbrain doctor` (warnings expected on a fresh brain),
    - and performed a smoke test (write + embed + search) that **passed**.
  - When you interact with the `plan` and `build` agents in OpenCode and they use GStack skills (for example, `/gstack-learn`, context save/restore flows), those skills can:
    - write advisory memory into this GBrain instance, and
    - search/recall past context via hybrid (vector + keyword) queries.
  - GBrain is bound by:
    - the GBrain operating model (`workspace\\gbrain\\gbrain-operating-model.md`),
    - the source tiers defined in `gbrain-source-tiers.md`,
    - and the rule that memory recall must always defer to current repo files and ADRs when there is a conflict or uncertainty.