# OpenCode System Docmap

**What this docmap is / how to use it:**  
This document registers the relevant OpenCode system, workspace, and integration files with their paths and roles. It is intended to be both human‑readable and agent‑scannable, so it stays path‑focused, explicit, and free of speculative implementation detail.

---

## 1. Global config & rules

- **Global AGENTS rules**  
  `C:\Users\dev-p\.config\opencode\AGENTS.md`  
  Global behavior rules for all OpenCode sessions (Desktop, TUI, IDE). Defines the default interaction style (concise, stepwise, validation‑first) and operating constraints that apply across workspaces.

- **Global OpenCode config**  
  `C:\Users\dev-p\.config\opencode\opencode.json`  
  Global OpenCode configuration file. Holds provider configuration, global models, default agents (`plan` / `build`), tools, permissions, and any future shared keybinds/commands.

- **Global skills directory (no custom skills yet)**  
  `C:\Users\dev-p\.config\opencode\skills\`  
  Placeholder for any future global skills. No custom global skills are defined; the system currently relies on OpenCode defaults.

---

## 2. Project rules & config — project-library

- **Project AGENTS rules (docs)**  
  `C:\_mb2-cs-app\project-library\AGENTS.md`  
  Documentation‑workspace rules file. Defines:
  - provenance expectations and trust model,
  - required workflow order (refine ADRs → validate AM1/MM1 / AM2/MM2 → improve working docs),
  - documentation‑specific behavior and how canonical shared devlog/todo files are used.

- **Project config (docs)**  
  `C:\_mb2-cs-app\project-library\opencode.json`  
  Project‑level OpenCode configuration for `project-library`. Sets:
  - `default_agent` = `plan`,
  - conservative permissions (`edit` / `webfetch` / `bash` all `ask`),
  - read access to:
    - `C:\_mb2-cs-app\mb-3-cs\`
    - `C:\_mb2-cs-app\devlog\`
    - `C:\_mb2-cs-app\todo\`

- **Project-local agents (docs)**  
  `C:\_mb2-cs-app\project-library\.opencode\agents\`  
  Defines `plan` and `build` for the docs workspace:
  - `plan` = primary documentation/planning agent (analysis, validation, provenance checks, architectural reasoning, refactor planning).
  - `build` = tightly scoped helper for non‑live‑code document tasks on explicit request.
  - `plan` may update canonical devlog/todo files where rules allow; `build` may read but must hand off changes.

- **Project-local commands (docs)**  
  `C:\_mb2-cs-app\project-library\.opencode\commands\`  
  Holds project‑local commands (for example, the current `test-config-dir` sandbox command). This is the preferred pattern for commands on this machine, rather than relying on `OPENCODE_CONFIG_DIR`, because Windows environment variables are not automatically visible inside WSL.

---

## 3. Project rules & config — mb-3-cs

- **Project AGENTS rules (code)**  
  `C:\_mb2-cs-app\mb-3-cs\AGENTS.md`  
  Live‑code repository rules file. Defines:
  - conservative, implementation‑focused behavior,
  - protected‑file rules,
  - backup and timestamp expectations,
  - how this repo uses the canonical shared devlog/todo files.

- **Project config (code)**  
  `C:\_mb2-cs-app\mb-3-cs\opencode.json`  
  Project‑level OpenCode configuration for `mb-3-cs`. Sets:
  - `default_agent` = `build`,
  - conservative permissions (`edit` / `webfetch` / `bash` all `ask`),
  - path‑based edit protection for critical files:
    - `anvil.yaml`
    - `parameters.yaml`
    - `templates.yaml`
    - all `form_template.yaml` files
  - `standard-page.html` and `theme.css` protected with `ask` permissions only,
  - read access to:
    - `C:\_mb2-cs-app\project-library\`
    - `C:\_mb2-cs-app\devlog\`
    - `C:\_mb2-cs-app\todo\`

- **Project-local agents (code)**  
  `C:\_mb2-cs-app\mb-3-cs\.opencode\agents\`  
  Defines `plan` and `build` for the live repo:
  - `build` = primary implementation agent for explicit user‑directed code tasks.
  - `plan` = planning/architecture/analysis agent.
  - `plan` may update canonical devlog/todo where allowed; `build` may read but must hand off tracking changes.

---

## 4. Workspace reference docs (project-library /workspace)

All paths below are relative to:

- Windows: `C:\_mb2-cs-app\project-library\workspace\`
- WSL: `/mnt/c/_mb2-cs-app/project-library/workspace/`

- **Workspace description**  
  `workspace-description.md`  
  Human‑facing description of:
  - how the dual‑project OpenCode workspace is structured,
  - how to start sessions on this specific Windows + WSL setup,
  - how `project-library` and `mb-3-cs` are intended to be used,
  - where GStack and GBrain fit in the overall picture.

- **Workspace docmap**  
  `workspace-docmap.md`  
  This file. Path‑centric register for setup, config, orchestration, and memory artifacts. Intended to be both human‑readable and agent‑scannable.

- **OpenCode setup & status**  
  `opencode-setup.md`  
  Detailed setup log and policy/status tracker for:
  - OpenCode configuration (global + project),
  - permissions and protected files,
  - agents and tools,
  - WSL server bridge,
  - GStack and GBrain status and future integration decisions.

---

## 5. Shared devlog & todo locations (canonical)

These paths are shared across `project-library` and `mb-3-cs` and are treated as canonical:

- **Devlog (canonical):**
  - `C:\_mb2-cs-app\devlog\README.md`
  - `C:\_mb2-cs-app\devlog\index.md`

- **Todos (canonical):**
  - Active backlog: `C:\_mb2-cs-app\todo\todo.md`
  - Archive: `C:\_mb2-cs-app\todo\todo-archive.md`

Ownership and behavior for these files are defined in each project’s `AGENTS.md` and in the workspace‑level GStack/GBrain docs. In particular:
- Plan is allowed to write canonical tracking where rules allow.
- Build may read but must hand off tracking updates.

---

## 6. Tools & integrations (system-level)

- **MGrep / WSL / OpenCode Desktop integration**  
  MGrep, WSL, and OpenCode Desktop are all operational and form the current base environment. MGrep is used from WSL against `/mnt/c/_mb2-cs-app/...` paths so it sees the same repositories as Windows.

- **WSL OpenCode server**  
  WSL runs the OpenCode server with:

  ```bash
  opencode serve --hostname 0.0.0.0 --port 4096
  ```

  Windows can reach that server at:

  ```text
  http://localhost:4096
  ```

- **Desktop server entry (wsl-opencode)**  
  OpenCode Desktop includes a server entry:

  - Name: `wsl-opencode`
  - Address: `http://localhost:4096`

  When connected to this server, Desktop sessions operate against WSL paths (e.g. `/mnt/c/_mb2-cs-app/project-library`) while still using the Windows UI.

- **Project opening pattern (current reality)**  
  On this machine, the Desktop “Open project” buttons are unreliable. Projects are opened reliably by sending the WSL path directly in the chat input after connecting to `wsl-opencode`, for example:

  - `project-library`:

    ```text
    /mnt/c/_mb2-cs-app/project-library
    ```

  - `mb-3-cs`:

    ```text
    /mnt/c/_mb2-cs-app/mb-3-cs
    ```

---

## 7. GStack — repo, workspace docs, integration state

### 7.1 Actual GStack repo

- **Local GStack repo (Windows)**  
  `C:\\_mb2-cs-app\\gstack`  
  Actual local clone of the GStack repository.

- **Local GStack repo (WSL)**  
  `/mnt/c/_mb2-cs-app/gstack`  
  Linux‑side path used when running GStack from WSL.

- **Runnable status (WSL)**  
  Core GStack shell scripts in `bin\\` have been normalized for WSL (CRLF → LF), and commands such as:

  ```bash
  ./bin/gstack-config --help
  ```

  are confirmed runnable from within WSL. This validates that the underlying GStack CLI/tooling works in the same environment that OpenCode uses for code execution. [file:575][web:560]

### 7.2 Workspace‑level GStack docs (project-library)

- **Location (Windows)**  
  `C:\\_mb2-cs-app\\project-library\\workspace\\gstack\\`

- **Key populated docs:**
  - `gstack-operating-model.md`  
    Defines the GStack operating model for the dual‑project CS workspace. Clarifies that:
    - OpenCode remains the runtime host and authority for permissions and protected files.
    - GStack is a workflow/orchestration layer wrapped around OpenCode.
    - GBrain is an advisory memory layer with provenance, not a truth source. [file:575][web:568]

  - `gstack-workflows.md`  
    Defines three main workflows:
    - `docs-plan` — primary in `project-library` for planning, validation, documentation, and prompt/build‑rules work.
    - `code-plan-build` — primary in `mb-3-cs` for safe implementation (Plan → Build).
    - `status-handover` — cross‑workspace status, checkpoint, and handoff workflow.

  - `gstack-handoff-contract.md`  
    Defines Plan↔Build handoff structure and templates:
    - Plan→Build handoff (task summary, scope, source context, target change set, risks, instructions, validation expectations, tracking expectations).
    - Build→Plan handoff (completion status, files changed, backup/protection confirmations, validation result, implementation summary, devlog/todo handoff proposals).

- **Additional GStack docs (scaffolded, awaiting content):**
  - `gstack-command-map.md`
  - `gstack-install-plan.md`
  - `gstack-open-adaption.md`
  - `gstack-overview.md`
  - `gstack-role-map.md`
  - `README.md`

### 7.3 Integration state for GStack

- GStack is **actively used** as the primary workflow/orchestration layer by the OpenCode agents via skills (slash commands), for example: `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`, `/build`, `/investigate`, `/review`, `/ship`, `/setup-gbrain`, and related flows. These skills are available to the agents when sessions are attached to the WSL server. [file:575][web:212][web:560][web:568]

- There is **no GStack‑specific config** yet under:

  - `C:\\Users\\dev-p\\.config\\opencode\\gstack\\` (global), or  
  - `C:\\_mb2-cs-app\\project-library\\.opencode\\gstack\\` / `C:\\_mb2-cs-app\\mb-3-cs\\.opencode\\gstack\\` (project‑local),

  so GStack is currently integrated **via its skillpack and workspace docs**, not via `.opencode` command wrappers.

- GStack currently functions as:
  - a documented workflow layer via the workspace docs above, and  
  - a local tool/skillpack under `/mnt/c/_mb2-cs-app/gstack` that the agents can call when appropriate.

- **Future work:**
  - Populate the remaining GStack docs with concrete, practiced workflows and examples.
  - Add project‑local commands in `.opencode\\commands\\` that map directly onto:
    - `docs-plan`,
    - `code-plan-build`,
    - `status-handover`,
    while preserving OpenCode’s permission model and protected‑file behavior.
---

## 8. GBrain — workspace docs and operating model

### 8.1 Workspace‑level GBrain docs (project-library)

- **Location (Windows)**  
  `C:\\_mb2-cs-app\\project-library\\workspace\\gbrain\\`

- **Key populated docs:**
  - `gbrain-operating-model.md`  
    Defines GBrain as:
    - an advisory memory and recall layer, not an authoritative truth source,
    - subordinate in authority to:
      1. user decisions,
      2. repo files and canonical tracking,
      3. OpenCode rules,
      4. validated current outputs,
      5. GBrain memory,
    - requiring provenance, freshness, and explicit conflict handling. [file:575]

  - `gbrain-source-tiers.md`  
    Defines source tiers for memory ingestion and recall:
    - Tier A — Authoritative CS specification sources (ADRs, key AM/MM docs, baseline reference files).
    - Tier B — Canonical shared tracking (`devlog` and `todo` files).
    - Tier C — Validated workflow outputs (final handoffs, approved summaries).
    - Tier D — Contextual repo summaries.
    - Tier E — External advisory sources (vendor docs, web research, etc.).
    - Off‑limits by default: speculative notes, scratchpads, deleted/legacy material unless explicitly approved.

- **Additional GBrain docs (scaffolded, awaiting content):**
  - `gbrain-ingestion-plan.md`
  - `gbrain-install-plan.md`
  - `gbrain-overview.md`
  - `gbrain-repo-strategy.md`
  - `gbrain-sources.md`
  - `gbrain-sync-policy.md`
  - `README.md`

### 8.2 Integration state for GBrain

- **Running GBrain instance (runtime configuration):**
  - `gbrain` version: `v0.18.2` at `~/gbrain/`.  
  - Engine: **Supabase (PostgreSQL)** project named `project-library` (`fnjcssmqljshckdksvam`), accessed via the Supabase connection/pooler URL configured during `/setup-gbrain`. [web:532][web:541]  
  - Embeddings: `text-embedding-3-large` via **OpenRouter**, using `OPENAI_API_BASE` set to `https://openrouter.ai/api/v1` and an OpenRouter API key. [web:533][web:538]  
  - Smoke test: **Passed** — write + embed + search round‑trip confirmed from the GStack `/setup-gbrain` flow. [web:239]

- **Local patch for OpenRouter compatibility:**
  - The file `~/gbrain/src/core/embedding.ts` has been modified so that the embedding client respects the `OPENAI_API_BASE` environment variable instead of hardcoding `https://api.openai.com`.  
  - This allows GBrain to talk to OpenRouter’s `text-embedding-3-large` endpoint transparently while still using the OpenAI‑compatible client interface. [web:533][web:538]  
  - This is a **local fork change** and will be lost on a `gbrain` upgrade unless upstreamed or re‑applied. Any future upgrade must account for this patch explicitly.

- **Integration with GStack and OpenCode:**
  - GBrain is wired into GStack via the `/setup-gbrain` skill, which configured:
    - the Supabase engine and connection details,
    - the embedding model and OpenRouter base URL,
    - and performed a doctor/smoke‑test run to validate write + embed + search. [web:239][web:466]
  - GStack-aware workflows (for example, memory‑heavy skills like `/gstack-learn`, context save/restore flows, and other long‑term memory features) can read from and write to this GBrain instance automatically, within the source‑tier and provenance rules defined in the GBrain workspace docs. [web:466][web:542]
  - GBrain remains an **advisory** layer: recalled content must be treated as context that can inform decisions, but final authority always rests with current repo content, ADRs, and user decisions.

- **Future work:**
  - Fill in the ingestion and sync plans in:
    - `gbrain-ingestion-plan.md`
    - `gbrain-sync-policy.md`
    - `gbrain-sources.md`
    - `gbrain-repo-strategy.md`
  - Add project‑local commands for safe GBrain ingestion/recall (under `.opencode\\commands\\`), aligned with GStack workflows and OpenCode rules, so that common memory workflows can be triggered via explicit commands in addition to GStack skills.

### 8.2 Integration state for GBrain

- GBrain is not yet wired into any live ingestion or recall commands.
- GBrain docs currently define:
  - what may be ingested (and at what tier),
  - how memory must expose provenance and authority,
  - how recalled memory must defer to current repo content.
- Future work:
  - Fill in ingestion and sync plans.
  - Add project‑local commands for safe GBrain ingestion/recall, aligned with GStack workflows and OpenCode rules.

---

## 9. Future placeholders

- **Possible future GStack config root (global):**  
  `C:\Users\dev-p\.config\opencode\gstack\`  
  Reserved as a possible future global GStack config location. Not used at present.

- **Possible future project-local GStack config paths:**  
  - `C:\_mb2-cs-app\project-library\.opencode\gstack\`  
  - `C:\_mb2-cs-app\mb-3-cs\.opencode\gstack\`  
  Reserved for potential future per‑project GStack config. No active configs live here yet.

- **Additional workspace‑level docs:**  
  As new orchestration or memory layers are introduced (for example, docs‑focused MCP servers or additional integration tools), new entries should be added here with:
  - full Windows and WSL paths,
  - clear role descriptions,
  - current status (NOT DONE / IN REVIEW / DONE).