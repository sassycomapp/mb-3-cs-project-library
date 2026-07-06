# OpenCode Setup & Status Tracker

**What this document is:**  
This is the current OpenCode setup, policy, and status tracker for the dual-project workspace under `C:\mybizz`. It is aligned with OpenCode Desktop on Windows, the current WSL bridge, and the current 10-file config set now in use. [web:1083][web:1098]

---

# 0. MGrep / WSL / OpenCode have also been setup and are working

This heading is a status note only. Detailed workspace usage guidance belongs in the workspace description document, while this file remains the authoritative setup/status tracker. [web:1083]

---

# A. Global Behavior & Rules

1. (AGENTS.md)  
`C:\Users\dev-p\.config\opencode\AGENTS.md`

Task/s
- [DONE] Maintain global behavior rules for OpenCode sessions.
- [DONE] Enforce concise, stepwise, validation-first responses with no emojis or side topics.
- [DONE] Distinguish clearly between facts, inferences, and proposed revisions.
- [DONE] Add a global numbering rule: never use `§` for numbering; always use period-style numbering when a symbol is needed (for example: `1.`, `2.`, `3.`).

2. (Repository AGENTS.md – docs)  
`C:\mybizz\project-library\AGENTS.md`

Task/s
- [DONE] Define the documentation workspace trust model and document provenance rules.
- [DONE] Enforce required workflow order (refine ADRs, validate `am1/mm1` and `am2/mm2` against baselines, then improve working docs).
- [DONE] Keep this file focused on documentation and planning behavior, not live code execution rules.
- [DONE] Update canonical shared tracking references to:
  - `C:\mybizz\devlog\README.md`
  - `C:\mybizz\devlog\index.md`
  - `C:\mybizz\todo\todo.md`
  - `C:\mybizz\todo\todo-archive.md`

3. (Repository AGENTS.md – code)  
`C:\mybizz\mb-3-cs\AGENTS.md`

Task/s
- [DONE] Define live code repo behavior: conservative, minimal, implementation-focused.
- [DONE] Protect:
  - `anvil.yaml`
  - `standard-page.html`
  - `theme.css`
  - `parameters.yaml`
  - `templates.yaml`
  - all `form_template.yaml` files  
  from change without explicit permission.
- [DONE] Require backups in `working-backup` mirroring the original path, with incremented `(NN)` filenames when duplicates exist.
- [DONE] Require `Created:` / `Updated:` timestamps in UTC+2 at the top of touched files where safe.
- [DONE] Keep hard protections and backup/timestamp rules in sync with how you actually work.
- [DONE] Align canonical shared tracking references with the shared root-level `devlog` and `todo` folders.

---

# B. Global Config & Core Platform — Status

Status: Core provider and model config implemented and aligned with the current global config set for OpenCode Desktop on Windows. [web:1083][web:1098]

4. (opencode.json – global)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Create the global OpenCode config file for the desktop app.
- [DONE] Use it as the central place for providers, default models, default agents, tools, permissions, keybinds, and commands.
- [DONE] Keep this file as the baseline global layer; project configs override it where needed. [web:1083][web:1098]
- Future: extend this file with any additional agents, tools, and keybinds once the basics are stable.

5. (providers – global)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Configure a single provider: `openrouter`.
- [DONE] Set `api_key` to `env:OPENROUTER_API_KEY` and confirm the environment variable is set.
- [DONE] Restrict the provider to the single Qwen model actually in use:
  - `openrouter/qwen/qwen3.6-plus`

6. (default models – global)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Set the main default model to:
  - `model`: `openrouter/qwen/qwen3.6-plus`
- [DONE] Set the lightweight default model to the same configured model:
  - `small_model`: `openrouter/qwen/qwen3.6-plus`
- [DONE] Confirm the model is visible and usable in OpenCode Desktop.
- [DONE] Keep the current policy as a single-model setup using Qwen 3.6 Plus everywhere unless intentionally changed later.

---

# C. Agents, Skills, Tools, Permissions — Status (agents + base permissions)

Status: Global plan/build agents, project-local plan/build agents, and conservative permissions are now implemented. [web:1089][web:1098]

7. (global agents)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Define two core global agents:
  - `plan`: planning, analysis, and refactor-thinking agent.
  - `build`: implementation and coding agent.
- [DONE] Assign per-agent models:
  - `plan` → `openrouter/qwen/qwen3.6-plus`
  - `build` → `openrouter/qwen/qwen3.6-plus`
- [DONE] Align these roles with the refactor-heavy docs project vs the implementation-heavy mb-3-cs project.

8. (default_agent – global)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Set `default_agent` to `plan` so new sessions favour high-quality reasoning and planning.
- [DONE] Confirm that new Desktop sessions start with the `plan` agent active when no project-level default overrides it. [web:1083][web:1098]

9. (project-local agents – docs)  
`C:\mybizz\project-library\.opencode\agents\`

Task/s
- [DONE] Create project-local `plan` and `build` agent files for `project-library`.
- [DONE] Keep `plan` as the primary documentation/planning agent.
- [DONE] Keep `build` as a tightly scoped execution/helper agent for explicitly requested non-live-code tasks.
- [DONE] Align both with canonical shared tracking ownership:
  - Plan may update canonical shared devlog/todo files.
  - Build may read but must hand off changes.

10. (project-local agents – code)  
`C:\mybizz\mb-3-cs\.opencode\agents\`

Task/s
- [DONE] Create project-local `plan` and `build` agent files for `mb-3-cs`.
- [DONE] Keep `build` as the primary implementation agent for the live repo.
- [DONE] Keep `plan` as the planning/architecture agent.
- [DONE] Align both with canonical shared tracking ownership:
  - Plan may update canonical shared devlog/todo files.
  - Build may read but must hand off changes.

11. (tools – global defaults)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Enable tools in a conservative way by default via permissions.
- [DONE] Keep edits, web access, and shell access approval-based by default.
- Future: explicitly list additional tools (for example: `mgrep`, `webfetch`) as your workflow stabilises.

12. (tool permissions – per agent)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Set conservative global permissions so edits and commands require explicit approval.
- [DONE] Keep:
  - `edit`: `"ask"`
  - `webfetch`: `"ask"`
  - `bash`: `"*": "ask"`
- [DONE] Ensure agents never write or execute freely; they must act only on your explicit prompts and with your confirmation.

13. (skills – global)  
`C:\Users\dev-p\.config\opencode\skills\`

Task/s
- [NOT DONE] No explicit custom global skill configuration has been implemented yet.
- Current state: relying on OpenCode/Desktop defaults and built-in behavior.

14. (skills – per project)  
Project skill locations (for example: `C:\mybizz\project-library\skills\` and `C:\mybizz\mb-3-cs\skills\` if used)

Task/s
- [NOT DONE] No project-specific skills have been created or configured yet. (61 skills symlinked into `~/.config/opencode/skills/` from `/mnt/c/mybizz/skills/`.)
- Decision pending: determine whether either repo needs local skills.

---

# D. Project Configs (Docs & Code) — Status

Status: Project-level `opencode.json` files are implemented for both main workspaces, and project configs are the highest-precedence standard config layer. [web:1083][web:1098]

15. (opencode.json – docs project)  
`C:\mybizz\project-library\opencode.json`

Task/s
- [DONE] Create the project config for the documentation workspace.
- [DONE] Set `default_agent` to `plan`.
- [DONE] Keep permissions conservative:
  - `edit`: `"ask"`
  - `webfetch`: `"ask"`
  - `bash`: `"*": "ask"`
- [DONE] Set project agent intent:
  - `plan` = primary agent for document analysis, validation, provenance checks, architectural reasoning, and refactor planning.
  - `build` = secondary agent for tightly scoped document/support tasks only when explicitly instructed.
- [DONE] Allow cross-repo read access to:
  - `C:\mybizz\mb-3-cs\`
  - `C:\mybizz\devlog\`
  - `C:\mybizz\todo\`

16. (opencode.json – code project)  
`C:\mybizz\mb-3-cs\opencode.json`

Task/s
- [DONE] Create the project config for the live code repository.
- [DONE] Set `default_agent` to `build`.
- [DONE] Keep permissions conservative:
  - `edit`: `"ask"`
  - `webfetch`: `"ask"`
  - `bash`: `"*": "ask"`
- [DONE] Set project agent intent:
  - `plan` = planning, architectural reasoning, and implementation analysis agent.
  - `build` = primary coding and test-oriented agent for explicit user-directed implementation tasks only.
- [DONE] Add path-based edit protection for critical files and allow cross-repo read access to:
  - `C:\mybizz\project-library\`
  - `C:\mybizz\devlog\`
  - `C:\mybizz\todo\`

---

# E. Interaction, Commands, and Workflow Comfort — Status

Status: OpenCode Desktop keybind behavior has been checked against the installed app on this machine. [web:1083]

17. (keybinds & shortcuts – global)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [NOT DONE] No custom keybinds configured yet.
- [DONE] Verify actual built-in OpenCode Desktop shortcut behavior from Settings > Keyboard Shortcuts.
- [DONE] Confirm that `Ctrl+P` is active in this Desktop install and opens an action list.
- [DONE] Confirm that `Ctrl+X` is not active in this Desktop install.
- [DONE] Confirm that `F2` is not active in this Desktop install.
- Current state: rely on the Desktop app’s actual shortcut list, not assumed defaults from external references.

18. (modes / commands – global)  
`C:\Users\dev-p\.config\opencode\opencode.json`

Task/s
- [DONE] Commands: 49 command files in `.opencode/commands/` route to GStack skills. See workspace-description.md Section 10.
- [DONE] Confirm that OpenCode Desktop already exposes built-in actions through the current shortcut/action system.
- Decision pending: add custom commands only after repeated workflow needs are identified.

19. (modes / commands – project-specific)  
Project `opencode.json` files / `.opencode` directories

Task/s
- [DONE] Project commands: 49 command files in `.opencode/commands/` route to GStack skills. Cleaned 2026-07-06.
- Decision pending after more real usage in `project-library` and `mb-3-cs`.

---

# F. Quality, Formatting, and Tooling

20. (formatters & linters – global and per project)  
Language- and tool-specific config files, plus references in `opencode.json` if needed

Task/s
- [DONE] Confirm Anvil.works formatter direction.
- [DONE] Confirm that Anvil.works currently uses Ruff in the Anvil IDE for formatting/linting, not Black.
- [DONE] Confirm that this local codebase currently contains neither `pyproject.toml` nor `ruff.toml`.
- [DECIDED] Do not enable OpenCode formatter or linter integration yet.
- [DECIDED] Do not introduce Ruff configuration into the repo at this stage.
- [DECIDED] Treat Ruff as the leading future candidate if formatter/linter standardisation is later adopted for this codebase.
- Rationale: automatic formatting should not be introduced into this project until formatting rules are deliberately defined and accepted for the local repo.

21. (protected-file edit controls for mb-3-cs)  
`C:\mybizz\mb-3-cs\opencode.json`

Task/s
- [DONE] Confirm that `.gitignore` is not a sufficient protection against OpenCode editing tracked files.
- [DONE] Confirm that file edit protection must be enforced primarily through `AGENTS.md` rules and OpenCode permission rules.
- [DONE] Add explicit path-based `edit` protection in `mb-3-cs\opencode.json`.
- [DONE] Deny edits to:
  - `anvil.yaml`
  - `parameters.yaml`
  - `templates.yaml`
  - all `form_template.yaml` files
- [DONE] Keep `standard-page.html` and `theme.css` protected by default with `ask` permissions, so they may only be edited on explicit user request in the active session.
- Rationale: this repository is pushed back to Anvil via GitHub, so protected-file controls must prevent edits before staging and commit concerns arise.

22. (package.json)  
`C:\Users\dev-p\.config\opencode\package.json`

Task/s
- [DONE] Determine whether `package.json` is part of the active OpenCode Desktop configuration strategy.
- [DECIDED] `package.json` is not part of the active setup at this stage.
- [DECIDED] No node-based helper tooling, formatter package dependencies, or plugin package management will be introduced through this file at this time.
- [DECIDED] Leave `package.json` unchanged unless a later OpenCode setup step explicitly requires package-managed tooling.
- Rationale: the current OpenCode Desktop setup is driven by `AGENTS.md`, `opencode.json`, providers, models, and permissions, not by node package management.

23. (package-lock.json)  
`C:\Users\dev-p\.config\opencode\package-lock.json`

Task/s
- [DONE] Determine whether `package-lock.json` is part of the active OpenCode Desktop configuration strategy.
- [DECIDED] `package-lock.json` is not part of the active setup at this stage.
- [DECIDED] Leave `package-lock.json` unchanged.
- [DECIDED] Review this file only if `package.json` later becomes part of the active configuration strategy through npm-managed tooling.
- Rationale: `package-lock.json` is only relevant when npm-managed dependencies are part of the actual configuration workflow.

---

# G. MCP Servers and External Integrations — Status

Status: Documentation access requirements are identified. Built-in web research is the first-line approach; MCP remains a future option if real workflow gaps justify it. [web:1098]

24. (documentation access requirement)  
OpenCode built-in tools and possible future MCP support

Task/s
- [DONE] Confirm that authoritative documentation access is a required capability for this OpenCode setup.
- [DONE] Confirm current documentation targets:
  - Anvil.works
  - Paystack
  - PayPal
  - Stripe
  - Brevo
- [DONE] Confirm that built-in OpenCode web research tools can be used for live official documentation lookup and retrieval.
- [DECIDED] Use built-in documentation/web research first for official docs lookup.
- [DECIDED] Evaluate docs-focused MCP only if built-in web research proves insufficient for structured, repeatable, or version-aware documentation work.

25. (MCP / external servers – future documentation and integration layer)  
MCP-specific config locations as required by future integrations

Task/s
- [DONE] GBrain MCP server configured via global `opencode.jsonc` — `gbrain serve` running (v0.42.44.0, PGLite engine).
- [IN REVIEW] Determine whether a docs-focused MCP server is needed for indexed, persistent, or multi-source documentation retrieval.
- [IN REVIEW] Future MCP candidates should be justified by real workflow gaps, especially around integration documentation accuracy and repeatability.
- Possible future scope:
  - documentation indexing/search across official vendor docs
  - GitHub or repository integration
  - other external system integrations only when clearly needed

---

# H. WSL server bridge — Status

Status: OpenCode Desktop ↔ WSL server bridge validated.

26. (WSL OpenCode server)  
WSL terminal / runtime command

Task/s
- [DONE] Run OpenCode server in WSL with:
  - `opencode serve --hostname 0.0.0.0 --port 4096`
- [DONE] Confirm the server starts and listens successfully.

27. (Windows health check)  
Windows browser → `http://localhost:4096/global/health`

Task/s
- [DONE] Verify health endpoint from Windows.
- [DONE] Confirm healthy response:
  - `{"healthy":true,"version":"1.14.30"}`

28. (Desktop server entry)  
OpenCode Desktop → Servers

Task/s
- [DONE] Add Desktop server entry:
  - Name: `wsl-opencode`
  - Address: `http://localhost:4096`
- [DONE] Confirm the Desktop app can register and retain this server entry.

29. (WSL-backed workspace access)  
OpenCode Desktop session via `wsl-opencode`

Task/s
- [DONE] Confirm Desktop sessions can operate against WSL/Linux-side paths.
- [DONE] Confirm a session can inspect `/mnt/c/mybizz/project-library`.
- [DONE] Treat the Windows Desktop UI + WSL server combination as a validated working mode for this workspace.

---

# I. GStack status record

# I. GStack status record

Status: GStack is locally installed, WSL-compatible, and actively used as the primary workflow/orchestration layer via skills (slash commands) in OpenCode. It remains subordinate to OpenCode’s permissions, AGENTS rules, and protected-file policies. [web:212][web:560]

30. (GStack repo location)  
`C:\\mybizz\\gstack`  
WSL path: `/mnt/c/mybizz/gstack`

Task/s  
- [DONE] Clone or place the GStack repo locally under `C:\\mybizz\\gstack`.  
- [DONE] Confirm repo contents include `bin/`, `node_modules/`, and the expected support files.  
- [DONE] Keep this repo as the authoritative source for GStack code and documentation on this machine.

31. (WSL shell compatibility for GStack scripts)  
`/mnt/c/mybizz/gstack/bin/*`

Task/s  
- [DONE] Identify CRLF line-ending issues causing `bash\r` execution failures in WSL.  
- [DONE] Install `dos2unix` in WSL.  
- [DONE] Convert the relevant `bin/*` scripts from CRLF to LF.  
- [DONE] Re-test script execution successfully from WSL.  
- [DONE] Confirm that core GStack scripts run cleanly under the WSL shell (no `^M` / CRLF artefacts).

32. (GStack command validation – base CLI)  
`/mnt/c/mybizz/gstack`

Task/s  
- [DONE] Confirm:  
  - `./bin/gstack-config --help`  
- [DONE] Verify the command returns normal help output:  
  - `Usage: gstack-config {get|set|list|defaults} [key] [value]`  
- [DONE] Validate that other core helper scripts under `bin/` run without path or shell errors in WSL.

33. (Current GStack integration status)  
Workspace integration decision

Task/s  
- [DONE] Confirm GStack is locally present and runnable in WSL.  
- [DONE] Use GStack as the primary workflow/orchestration layer around OpenCode via skills (slash commands) exposed to the OpenCode agents (for example: `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`, `/build`, `/investigate`, `/review`, `/ship`, `/setup-gbrain`, etc.). [web:212][web:560][web:568]  
- [DONE] Confirm that GStack skills can be invoked from OpenCode Desktop sessions attached to the WSL server and that they operate against the correct WSL paths (`/mnt/c/mybizz/...`).  
- - [DONE] GStack integrated via skillpack and 49 project-local commands. No separate per-project GStack config path needed. under `.config\\opencode\\gstack\\` or project-local `.opencode\\gstack\\` paths; integration is currently through the agent’s skills/skillpack and workspace-level docs, not through `.opencode` command wrappers.  
- [DECIDED] Treat GStack as a workflow/orchestration layer that:  
  - **does not** override OpenCode global/project config, AGENTS rules, or protected-file behavior,  
  - **does** provide structured workflows (plan/build/review/ship, docs-plan, code-plan-build, status-handover) for both `project-library` and `mb-3-cs`.

---

---

# J. GStack & GBrain — Status

# J. GStack & GBrain — Status

Status: Core operating models and workspace scaffolding are in place for both GStack and GBrain. GStack is actively used as an orchestration layer via skills in OpenCode, and GBrain is fully implemented as a Supabase-backed brain with OpenRouter-based embeddings and a passing smoke test. Some workspace docs and optional `.opencode` command wrappers remain to be populated. [file:574][web:239][web:532][web:533]

34. (GStack / GBrain workspace scaffold)  
`C:\\mybizz\\project-library\\workspace\\`

Task/s  
- [DONE] Create a dedicated workspace-level area under `project-library\\workspace\\` for GStack, GBrain, and workspace meta-docs.  
- [DONE] Establish the following structure on Windows (WSL uses `/mnt/c/mybizz/project-library/workspace`):

  - `workspace\\gstack\\`  
    - `gstack-operating-model.md` (Populated)  
    - `gstack-workflows.md` (Populated)  
    - `gstack-handoff-contract.md` (Populated)  
    - `gstack-command-map.md` (populated)
    - `gstack-install-plan.md` (populated)
    - `gstack-open-adaption.md` (populated)
    - `gstack-overview.md` (populated)
    - `gstack-role-map.md` (populated)
    - `README.md` (populated)

  - `workspace\\gbrain\\`  
    - `gbrain-operating-model.md` (Populated)  
    - `gbrain-source-tiers.md` (Populated)  
    - `gbrain-ingestion-plan.md` (populated)
    - `gbrain-install-plan.md` (populated)
    - `gbrain-overview.md` (populated)
    - `gbrain-repo-strategy.md` (populated)
    - `gbrain-sources.md` (populated)
    - `gbrain-sync-policy.md` (populated)
    - `README.md` (populated)

  - `workspace\\opencode-setup.md` (this document)  
  - `workspace\\workspace-description.md`  
  - `workspace\\workspace-docmap.md`

- [DECIDED] Use `workspace\\gstack\\` for workflow and handoff standards, not for live code or config.  
- [DECIDED] Use `workspace\\gbrain\\` for memory and provenance rules, not for runtime state.

35. (GStack – operating model and workflows)  
`C:\\mybizz\\project-library\\workspace\\gstack\\`

Task/s  
- [DONE] Define GStack’s operating role for the dual-project CS workspace in `gstack-operating-model.md` — OpenCode remains the runtime host and authority for permissions and protected files; GStack is a workflow layer around it. [file:574][web:568]  
- [DONE] Document the three primary workflow families in `gstack-workflows.md`:  
  - `docs-plan` (primary in `project-library`),  
  - `code-plan-build` (primary in `mb-3-cs`),  
  - `status-handover` (cross-workspace status and handoffs).  
- [DONE] Define a minimum Plan↔Build handoff contract in `gstack-handoff-contract.md`.  
  - [DONE] All GStack workspace docs populated: `gstack-command-map.md`, `gstack-install-plan.md`, `gstack-open-adaption.md`, `gstack-overview.md`, `gstack-role-map.md`, `README.md`.
- [DECIDED] Treat GStack as a workflow and orchestration layer around OpenCode; GStack does not override OpenCode permissions, project rules, or protected-file behavior.

36. (GBrain – operating model, engine, and source tiers)  
`C:\\mybizz\\project-library\\workspace\\gbrain\\`

Task/s  
- [DONE] Define GBrain’s operating role as an advisory memory layer, not a truth source, in `gbrain-operating-model.md`.  
- [DONE] Define source tiers and ingestion boundaries in `gbrain-source-tiers.md`, including:  
  - Tier A: Authoritative CS specification sources (ADRs and key docs),  
  - Tier B: Canonical shared tracking (`devlog` and `todo`),  
  - Tier C: Validated workflow outputs,  
  - Tier D: Contextual summaries,  
  - Tier E: External advisory sources.  
- [DONE] Implement a running GBrain instance with the following **runtime configuration**:  
  - `gbrain` version: `v0.42.44.0` at `~/.bun/bin/gbrain`.  
  - Engine: **Supabase (PostgreSQL)** project named `project-library` (`fnjcssmqljshckdksvam`). [web:532][web:541]  
  - Embeddings: `text-embedding-3-large` via **OpenRouter**, using `OPENAI_API_BASE` pointing at `https://openrouter.ai/api/v1`. [web:533][web:538]  
  - Smoke test: **Passed** — write + embed + search round-trip confirmed from `/setup-gbrain` / GStack. [web:239]  
- [DONE] Apply a local patch to GBrain so that the embedding client respects `OPENAI_API_BASE`:  
  - Modified `~/gbrain/src/core/embedding.ts` to use the `OPENAI_API_BASE` environment variable instead of a hardcoded `https://api.openai.com`.  
  - [DECIDED] Treat this as a **local fork change** that will be lost on `gbrain` upgrade unless upstreamed; any future upgrade must re-apply or retire this patch based on upstream support.  
  - [DONE] All GBrain workspace docs populated: `gbrain-ingestion-plan.md`, `gbrain-install-plan.md`, `gbrain-overview.md`, `gbrain-repo-strategy.md`, `gbrain-sources.md`, `gbrain-sync-policy.md`, `README.md`. PGLite engine (migrated from Supabase), v0.42.44.0.
- [DECIDED] GBrain memory must always defer to current on-disk sources; memory recall is advisory, must carry provenance, and must be checked against the current repo/ADRs when in doubt.

37. (GStack / GBrain integration status)  
Workspace integration decision

Task/s  
- [DONE] Establish initial GStack and GBrain operating models and file scaffolding under `project-library\\workspace`.  
- [DONE] Keep all GStack and GBrain docs behind OpenCode’s existing authority model:  
  - user choice,  
  - repo files,  
  - project/global OpenCode config,  
  - then GStack workflows,  
  - then GBrain memory.  
- [DONE] Wire GBrain into GStack via the `/setup-gbrain` skill, selecting the Supabase engine and OpenRouter-based embedding model, and confirming a passing smoke test (write + embed + search). [web:239][web:466]  
- [DONE] Allow GStack-aware workflows (for example: `/gstack-learn`, context save/restore flows, and other memory-heavy skills) to read/write to this GBrain instance while still treating it as an advisory memory layer under the defined source-tier rules. [web:466][web:542]  
- [DONE] GStack workflows available via 49 project-local commands in `.opencode/commands/`. Web UI only.  
- [DECIDED] Proceed with small, reviewable steps so that each new command, wrapper, or integration (for example, project-local GStack/GBrain commands) can be tested in isolation against this status tracker.
---

---

# K. Startup & Operational Commands — Status

Status: WSL server startup and project access are working; Desktop “Open project” buttons are unreliable on this install, but direct path-based opening via the chat interface works consistently.

38. (WSL OpenCode server startup)  
WSL terminal (Ubuntu)

Task/s  
- [DONE] Confirm that OpenCode is installed in WSL.  
- [DONE] Use a dedicated WSL terminal window to run the OpenCode server:

  ```bash
  opencode serve --hostname 0.0.0.0 --port 4096
  ```

- [DONE] Confirm the server starts and listens successfully:  
  - Expected output (example):  
    `Warning: OPENCODE_SERVER_PASSWORD is not set; server is unsecured.`  
    `opencode server listening on http://0.0.0.0:4096`
- [DONE] Leave this terminal window running while using OpenCode Desktop.  
- [DECIDED] Do not set `OPENCODE_SERVER_PASSWORD` yet; treat this as a trusted local dev environment.

39. (WSL project path validation)  
WSL terminal (Ubuntu)

Task/s  
- [DONE] Confirm that the Windows project directories are reachable via `/mnt/c`:

  ```bash
  ls /mnt/c/mybizz/project-library
  ls /mnt/c/mybizz/mb-3-cs
  ```

- [DONE] Confirm that `project-library` contains the expected workspace structure:

  - `README.md`
  - `adr\`
  - `agents.md`
  - `brain-resources\`
  - `docs\`
  - `handovers\`
  - `logs\`
  - `open-tasks\`
  - `opencode.json`
  - `prompts\`
  - `references\`
  - `workspace\`

40. (Desktop server entry – reminder)  
OpenCode Desktop → Servers

Task/s  
- [DONE] Use the existing server entry:
  - Name: `wsl-opencode`
  - Address: `http://localhost:4096`
- [DONE] Confirm that the Desktop app can reconnect to this server when it is running in WSL.

41. (Project access from Desktop – current working pattern)  
OpenCode Desktop + WSL server

Task/s  
- [DONE] Confirm that the Desktop “Open project” buttons are currently unreliable on this install.  
- [DONE] Confirm that projects can be opened reliably by sending the WSL path through the Desktop chat interface after the server is running.  
- [DONE] Use:

  - For `project-library`:

    ```text
    /mnt/c/mybizz/project-library
    ```

  - For `mb-3-cs`:

    ```text
    /mnt/c/mybizz/mb-3-cs
    ```

  as direct path messages to open the corresponding workspace when connected to `wsl-opencode`.  
- [DECIDED] Treat “paste WSL path into the chat input” as the primary way to open projects from Desktop until the UI buttons are fixed or updated.

42. (Quick startup checklist – this machine)  

Task/s  
- [DONE] Define a minimal startup sequence for this specific Windows + WSL setup:

  1. Open WSL (Ubuntu) and start the OpenCode server:

     ```bash
     opencode serve --hostname 0.0.0.0 --port 4096
     ```

  2. Start OpenCode Desktop and ensure it is connected to the `wsl-opencode` server.  
  3. To work in `project-library`, send:

     ```text
     /mnt/c/mybizz/project-library
     ```

     in the Desktop chat input to open the workspace.  
  4. To work in `mb-3-cs`, send:

     ```text
     /mnt/c/mybizz/mb-3-cs
     ```

     in the Desktop chat input.  
  5. Use the OpenCode terminal inside each workspace to confirm location:

     ```bash
     pwd
     ls
     ```

- [NOT DONE] Investigate and resolve the Desktop “Open project” button behaviour; deferred until a future troubleshooting session.

43. (Diagnostic commands – environment sanity checks)  

Task/s  
- [DONE] Confirm the Windows-side environment variable for custom config (even though we later chose project-local commands instead of relying on it):

  ```powershell
  [System.Environment]::GetEnvironmentVariable("OPENCODE_CONFIG_DIR", "User")
  [System.Environment]::GetEnvironmentVariable("OPENCODE_CONFIG_DIR", "Machine")
  $env:OPENCODE_CONFIG_DIR
  ```

- [DONE] Confirm visibility from a fresh PowerShell:

  - Expected User value (example):  
    `C:\mybizz\opencode-integration`

- [DONE] Confirm that the WSL environment does **not** automatically see this Windows variable, and that project-local `.opencode\commands\` is therefore the more reliable pattern:

  ```bash
  echo "$OPENCODE_CONFIG_DIR"
  ```

- [DONE] Confirm that the WSL paths and repo contents are consistent:

  ```bash
  ls /mnt/c/mybizz/project-library/workspace/gstack
  ls /mnt/c/mybizz/project-library/workspace/gbrain
  ```

- [DECIDED] Use project-local `.opencode\commands\` rather than cross-OS `OPENCODE_CONFIG_DIR` for this machine.

44. (Project-local commands – verification)  
`C:\mybizz\project-library\.opencode\commands\`

Task/s  
- [DONE] Create `.opencode\commands\` for `project-library`.  
- [DONE] Add `test-config-dir.md` and verify it appears in the command palette as `test-config-dir sandbox: project-library`.  
- [DONE] Use this pattern as the baseline for future project-local commands (including GStack/GBrain-related helpers) rather than relying on the global custom config directory.  
- [NOT DONE] Finalise and stabilise a set of real project-local commands for:
  - standard docs workflows,
  - standard code workflows,
  - GStack workflow entry points,
  - GBrain ingestion/recall helpers.

This structure gives you:

- broad chapters (A–I) to reason about strategy,
- numbered items with filename + path + tasks,
- explicit coverage of tools, skills, agents, WSL bridging, GStack status, and MCPs,
- and clear separation between global vs project concerns.