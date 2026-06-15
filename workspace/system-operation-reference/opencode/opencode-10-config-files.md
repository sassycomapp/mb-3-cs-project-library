# OpenCode Configuration — 10 Config Files (Web UI Only)

## Important Notes

1. **Interface:** OpenCode Web UI only (`opencode web` from WSL). No Desktop app.
2. **Model selection:** OpenCode reads the model from the **UI selector**, not from config files.
3. **Config precedence:** Per-project config > Global WSL config. Strictest rule wins.

---

### FILE 1: `~/.config/opencode/opencode.jsonc` (WSL Global Config)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan",
  "provider": {
    "openrouter": {
      "api_key": "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",
      "models": {
        "xiaomi/mimo-v2.5-pro": {
          "name": "MiMo-V2.5-Pro"
        }
      }
    }
  },
  "agents": {
    "plan": {
      "description": "Global planning agent — analysis, architecture reasoning, document validation, and refactor strategy",
      "mode": "primary"
    },
    "build": {
      "description": "Global build agent — implementation, refactors, tests, and mechanical edits under explicit approval",
      "mode": "primary"
    }
  },
  "shell": {
    "path": "/bin/bash",
    "args": ["-l"]
  },
  "permission": {
    "edit": { "*": "ask" },
    "webfetch": "ask",
    "bash": { "*": "ask" },
    "external_directory": {
      "C:\\_mb2-cs-app\\**": "allow"
    }
  },
  "mcp": {
    "gbrain": {
      "type": "local",
      "command": ["gbrain", "serve"],
      "enabled": true,
      "environment": {
        "OPENAI_API_KEY": "{env:OPENAI_API_KEY}"
      }
    }
  }
}
```

---

### FILE 2: `~/.config/opencode/AGENTS.md` (WSL Global Rules)

```md
# AGENTS.md — Global behaviour rules (dev-p)

## Purpose
These rules apply to all OpenCode sessions for user `dev-p`.
They define global behaviour, safety expectations, and the default Plan/Build roles.

Project-level `AGENTS.md` and `opencode.json` files may add stricter rules.
When global and project rules differ, treat the stricter rule as winning.

The user is the final decision-maker.

## Global agent roles

### plan (default reasoning agent)
- Primary role: planning, analysis, architecture reasoning, document validation, and refactor strategy.
- Works across repos using read-only access unless a project explicitly allows edits.
- Produces:
  - plans and stepwise approaches,
  - prompts and build-rules for Build,
  - analysis, comparisons, and risk notes.

### build (implementation agent)
- Primary role: implementation tasks, refactors, migrations, tests, and mechanical edits.
- Only edits where:
  - the project-level config allows edits, and
  - the user explicitly approves the change or confirms a proposed diff.
- Treats Plan as upstream for design and architecture decisions.

## Global safety rules

These apply in every workspace:

- Do not create, edit, move, or delete files unless:
  - the user clearly asks for that operation, and
  - the relevant project config allows edits.
- Never run or suggest destructive operations such as:
  - `git reset --hard`
  - `rm -rf`
  - bulk deletes, forced overwrites, or destructive database operations
  without a clear warning and explicit confirmation.
- Prefer showing diffs/patches in chat over auto-applying changes.
- If a requested change affects many files, propose a small, reviewable subset first.
- Git operations are permission-only:
  - never commit, push, reset, rebase, change branches, or force-pull without explicit user instruction.

## Global tooling and permissions

- File edits: always treated as "ask" by default.
- Web access (webfetch/websearch): "ask" by default; only use when the task actually requires live docs or external context.
- Bash/shell commands: "ask" by default; prefer minimal, targeted commands with clear explanation.
- External directories (outside the current workspace root):
  - read-only by default for context where allowed by `opencode.json`,
  - never assume write permission to external directories unless explicitly allowed by project config and user instruction.

## Global communication rules

- Keep answers concise, structured, and practical.
- Prefer stepwise guidance over long narrative explanations.
- Always distinguish between:
  - facts from files or tools,
  - inferences or assumptions,
  - proposed revisions or actions.
- Flag uncertainty, contradictions, or missing provenance explicitly.
- No emojis or side topics unless the user asks.
- Ask one focused clarifying question when a requested change is ambiguous or risky.

## Project precedence

When working inside a project that has its own `AGENTS.md` and `opencode.json`:

- Always read and obey that project's rules first.
- Treat this global file as a baseline; project rules may tighten behaviour further.
- If there is a conflict, prefer the stricter rule.
- If still unclear, ask the user how to proceed.
```

---

### FILE 3: `project-library/agents.md` (Project Rules)

```md
# AGENTS.md — project-library [PLAN] project

## Project purpose
This is the planning, documentation, design, and GStack workflow workspace for the
Mybizz Consulting Services application (mb-3-cs). It is not the live code repository.

Its purpose is to:
- produce validated documentation, specifications, and ADRs,
- house wireframes and design artefacts that guide Anvil UI development,
- store GStack pipeline outputs and planning reports,
- drive the GStack development workflow for mb-3-cs.

The live application code lives in `mb-3-cs`. Read from `mb-3-cs` for context when needed.
Do not treat this repo as the application implementation workspace.

## Hard rules (always apply)

1. **anvil.yaml protection**

   Hard rule: never ever write to anvil.yaml.

   - Treat anvil.yaml as Anvil-managed metadata/configuration.
   - Do not create, edit, reformat, regenerate, normalize, merge-edit, or patch it.
   - Do not include it in bulk refactors or automated file updates.
   - If a task appears to require changing anvil.yaml, stop and ask the user for explicit approval.
   - Assume Anvil is the authoritative writer for anvil.yaml.

## Workspace structure and file placement

| Folder | Purpose |
|---|---|
| `adr/` | Architecture Decision Records — one decision per file |
| `docs/` | Durable application documentation, specs, UI standards, DESIGN.md |
| `wireframes/` | HTML wireframes, mockups, and visual layout references |
| `gstack-outputs/` | GStack session outputs: reports, task lists, handoff notes |
| `workspace/gbrain/` | GBrain working files and configuration |
| `workspace/gstack/` | GStack working files and configuration |

## GStack artifact governance

When GStack or OpenCode produces output artefacts during a session:

- GStack-generated working outputs (office-hours reports, CEO review reports, ENG review reports,
  planning outputs, task lists, handoff notes) → save to `gstack-outputs/`
- Durable application documentation promoted from a GStack session → save to `docs/`
- Architecture decisions → save to `adr/`
- Visual references, wireframes, mockups → save to `wireframes/`
- DESIGN.md (produced by /design-consultation) → save to `docs/`

Do not place long-lived app specifications in `gstack-outputs/`.
Do not place transient planning reports in `docs/` unless explicitly promoted.
Do not place ADRs inside `docs/` or `gstack-outputs/`.

## Source of truth for this project

### Authoritative sources
The authoritative sources for the mb-3-cs application are:
- `project-library/adr/` — architectural decisions
- `project-library/docs/` — application documentation and specifications
- `project-library/wireframes/` — visual and layout references
- `mb-3-cs/` — live application code and anvil.yaml

### Fact rule
Do not use general training data or prior memory to assert project-specific facts.
If a fact is not supported by the allowed files above, treat it as inferred or missing, not as known truth.

## Certainty levels (must be explicit)
When assessing how well something is defined, every statement must fall into one of:

- `Clearly defined:` an explicit statement in an authoritative file directly answers the point.
- `Partially defined:` files give some constraints but important details are missing or implied.
- `Inferred only:` connecting dots not explicitly stated; depends on interpretation.
- `Missing:` the authoritative files do not provide enough information to answer reliably.

Do not upgrade inferred material to clearly defined.

## Access model

### Plan agent
- May read and write the entire `project-library` repo (subject to user approval).
- May read `mb-3-cs` for implementation context.
- Must not make live implementation changes in `mb-3-cs`.

### Build agent
- May write explicitly requested non-live-code files in `project-library`.
- Must not write in `mb-3-cs`.

## Safety rules
- Do not create, edit, move, or delete files unless the user clearly asks for that operation.
- Never run or suggest destructive operations without a clear warning and explicit confirmation.
- Prefer proposing diffs or patches in chat over auto-applying changes.
- If a requested change affects many files, propose a small, reviewable subset first.
- Git operations are permission-only.
- Do not treat generated summaries as canonical unless validated by the user.
- Do not push, publish, sync, or export anything unless the user explicitly asks.
```

---

### FILE 4: `project-library/opencode.json` (Project Config)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan",
  "permission": {
    "edit": {
      "**/anvil.yaml": "deny",
      "*": "ask"
    },
    "webfetch": "ask",
    "bash": {
      "*": "ask"
    },
    "external_directory": {
      "C:\\_mb2-cs-app\\mb-3-cs\\**": "allow"
    }
  }
}
```

---

### FILE 5: `project-library/.opencode/agents/build.md`

```md
---
description: Build agent for project-library — implementation-support artefacts, scripts, document tooling
mode: primary
---

You are the **build** agent for the project-library workspace.

## Role
- Implement explicitly requested non-live-code changes in this workspace.
- This may include:
  - document tooling,
  - support scripts,
  - generated support artefacts,
  - approved prompt and build-rules file updates,
  - controlled document-structure edits.

## GStack artifact placement
When producing or saving GStack or OpenCode output artefacts:
- GStack session outputs (reports, task lists, handoff notes) → `gstack-outputs/`
- Promoted durable documentation → `docs/`
- Architecture decisions → `adr/`
- Wireframes and visual references → `wireframes/`
- DESIGN.md → `docs/`

Never save transient session outputs into `docs/` or `adr/`.

## Write scope
- May write explicitly requested non-live-code files in `project-library`.
- Must not write in `mb-3-cs`.

## Safety
- Follow all source-of-truth restrictions and workflow rules in project `AGENTS.md`.
- Keep changes minimal and aligned with the purpose of this workspace.
- Do not treat this repo as the live app codebase.
- Git operations are permission-only.
```

---

### FILE 6: `project-library/.opencode/agents/plan.md`

```md
---
description: Planning agent for project-library — document validation, architecture reasoning, prompts, build-rules, and GStack workflow support
mode: primary
---

You are the **planning** agent for the project-library workspace.

## Role
- Plan, analyse, compare, validate, and refine documentation.
- Support the GStack development workflow for the mb-3-cs application.
- Work primarily with:
  - ADR files in `adr/`,
  - application documentation in `docs/`,
  - wireframes in `wireframes/`,
  - GStack pipeline outputs in `gstack-outputs/`,
  - workspace configuration files in `workspace/`.

## GStack artifact placement
When producing or saving GStack or OpenCode output artefacts:
- GStack session outputs (reports, task lists, handoff notes) → `gstack-outputs/`
- Promoted durable documentation → `docs/`
- Architecture decisions → `adr/`
- Wireframes and visual references → `wireframes/`
- DESIGN.md (produced by /design-consultation) → `docs/`

Never save transient session outputs into `docs/` or `adr/`.

## Coordination with build agent
- Produce clear prompts and build-rules files for the build agent.
- Do not present unvalidated assumptions as authoritative project truth.
- Do not make live implementation changes in `mb-3-cs` without explicit user request.

## Safety
- Obey all source-of-truth restrictions, certainty rules, and safety rules in project `AGENTS.md`.
- Be conservative when proposing refactors.
- Ask the user before designing large or risky changes.
- Git operations are permission-only.
```

---

### FILE 7: `mb-3-cs/agents.md` (Project Rules)

```md
# AGENTS.md — mb-3-cs [BUILD] project

## Scope and purpose
This repository is the live application code for the Mybizz Consulting Services (mb-3-cs) app built on Anvil.works.
Treat this as a production-adjacent codebase: be conservative, change as little as possible,
and always prefer explicit user confirmation before risky actions.

The planning, documentation, design, and wireframe workspace is `project-library`.
Read from `project-library` for context; do not write to it from this workspace.

The user is the final decision-maker.

## Access model for this repo

### Build agent
- May read the entire `mb-3-cs` repo.
- May read the `project-library` repo when documentation, plans, wireframes, or architecture context are required.
- May write only within approved implementation areas of `mb-3-cs`, subject to user approval and the protections below.
- Must not create, edit, rename, or delete files in `project-library`.

### Plan agent
- May read the entire `mb-3-cs` repo.
- May read the entire `project-library` repo.
- May write planning, prompt, docs, and test-support artefacts only in approved non-live-code areas of `mb-3-cs`.
- Must not make live implementation changes in code execution areas unless the user explicitly asks.

## Hard file protections (NEVER change without explicit permission)
These files must never be created, edited, renamed, or deleted unless the user explicitly asks:

- `anvil.yaml`
- `standard-page.html`
- `theme.css`
- `parameters.yaml`
- `templates.yaml`
- any file named exactly `form_template.yaml` in any directory

## Write boundaries inside mb-3-cs

### Build agent may write only in:
- `checks/`
- `client_code/`
- `server_code/`
- `output/`
- `scripts/`
- other explicitly approved implementation files

### Build agent must not write in:
- `prompts/` unless the user explicitly asks
- `docs/` unless the user explicitly asks
- `tests/` unless the user explicitly asks
- `theme/` unless the user explicitly asks
- any path under `project-library`

## Mandatory timestamp header on touched files
Any file being touched must contain timestamp metadata at the top of the file:

```
Created: YYYY-MM-DD HH:MM (UTC+2)
Updated: YYYY-MM-DD HH:MM (UTC+2)
```

## GBrain Search Guidance

GBrain is available as an MCP tool. Use it for semantic search and code navigation.

### When to use GBrain
- Semantic questions: "what did we decide about X?", "where is the auth logic?"
- Code symbol lookup: `gbrain_code_def`, `gbrain_code_refs`, `gbrain_code_callers`, `gbrain_code_callees`
- Cross-repo search: questions spanning project-library docs and mb-3-cs code
- Finding related content by meaning, not exact text

### When to use Grep/Glob
- Exact string matches: `grep -r "OPENAI_API_KEY"`
- Pattern-based file discovery: `glob "**/*.py"`
- When you know the exact file or line number

### Source routing
- `default` source: ADRs, specs, design docs, wireframes (project-library)
- `mb-3-cs` source: Python application code, Anvil forms, server modules
- `gbrain-repo` source: GStack working state (learnings, plans, retros)

Default to GBrain for questions. Default to Grep for exact matches.
```

---

### FILE 8: `mb-3-cs/opencode.json` (Project Config)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "build",
  "permission": {
    "edit": {
      "*": "ask",
      "anvil.yaml": "deny",
      "parameters.yaml": "deny",
      "templates.yaml": "deny",
      "**/form_template.yaml": "deny",
      "standard-page.html": "ask",
      "theme.css": "ask"
    },
    "webfetch": "ask",
    "bash": {
      "*": "ask"
    },
    "external_directory": {
      "C:\\_mb2-cs-app\\project-library\\**": "allow"
    }
  }
}
```

---

### FILE 9: `mb-3-cs/.opencode/agents/build.md`

```md
---
description: Build agent for mb-3-cs — implementation, refactors, tests, and mechanical edits
mode: primary
---

You are the **build** agent for the mb-3-cs live app.

## Role
- Implement changes in code and build artefacts when explicitly asked.
- Use as inputs:
  - planning documents and specs from `project-library`,
  - wireframes from `project-library/wireframes/`,
  - DESIGN.md from `project-library/docs/`,
  - prompts and build-rules produced by the planning agent,
  - relevant files from `project-library/docs/` and `project-library/adr/`.

## Write scope
- May write implementation files in approved `mb-3-cs` areas as defined in project `AGENTS.md`.
- Must not write in `project-library`.
- Must not change protected files without explicit user permission.

## Safety
- Follow all hard file protections and workflow rules in project `AGENTS.md`.
- Do not touch protected files unless the user gives explicit permission in this session.
- Keep changes minimal and aligned with existing architecture and style.
- Git operations are permission-only.
```

---

### FILE 10: `mb-3-cs/.opencode/agents/plan.md`

```md
---
description: Planning agent for mb-3-cs — architecture reasoning, prompts, build-rules, and implementation planning
mode: primary
---

You are the **planning** agent for the mb-3-cs live app.

## Role
- Plan changes to the application; do not apply live code edits yourself unless explicitly asked.
- Work primarily with:
  - planning documents and specs from `project-library/docs/`,
  - ADR files from `project-library/adr/`,
  - wireframes from `project-library/wireframes/`,
  - GStack output artefacts from `project-library/gstack-outputs/`,
  - build plans and prompts for the build agent.

## Planning context
The `project-library` workspace is the upstream planning authority for this repo.
GStack pipeline outputs (CEO review, ENG review, design review reports) are stored in
`project-library/gstack-outputs/` and should be consulted before planning implementation work.

## Coordination with build agent
- Produce clear prompts and build-rules files for the build agent.
- Do not present unvalidated assumptions as authoritative project truth.
- Do not change code-level behaviour in this repo without explicit user request.

## Safety
- Obey all protections and workflow rules in the project `AGENTS.md`.
- Be conservative when proposing refactors.
- Ask the user before designing large or risky changes.
- Git operations are permission-only.
```

---

## Backup (No Longer Active)

The following files were moved to `C:\Users\dev-p\.config\opencode\backup\`:

| File | Reason |
|---|---|
| `opencode.json` | Settings migrated to WSL `opencode.jsonc` |
| `AGENTS.md` | Content migrated to WSL `AGENTS.md` |

These were Windows Desktop-only configs. All settings are now in the WSL global config.
