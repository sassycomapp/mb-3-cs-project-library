# OpenCode Configuration — 10 Config Files (Web UI Only)

## Important Notes

1. **Interface:** OpenCode Web UI only (`opencode web` from WSL). No Desktop app.
2. **Model selection:** OpenCode reads the model from the **UI selector**, not from config files. No models hardcoded.
3. **Config precedence:** Per-project config > Global WSL config. Strictest rule wins.
4. **Multi-project:** Global config covers `C:\projects\**` for all projects. Per-project configs add tighter protections.
5. **Plan/Build boundary:** Plan reads, Build writes. Plan does not write. Ever. Applies in every workspace without exception.

---

### FILE 1: `~/.config/opencode/opencode.jsonc` (WSL Global Config)

Path: `/home/dev-p/.config/opencode/opencode.jsonc`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan",
  "provider": {
    "openrouter": {
      "options": {
        "apiKey": "<redacted — present in file>"
      }
    }
  },
  "permission": {
    "edit": { "*": "ask" },
    "webfetch": "ask",
    "bash": { "*": "ask" },
    "external_directory": {
      "C:\\mybizz\\**": "allow",
      "C:\\projects\\**": "allow",
      "C:\\pdlf\\**": "allow"
    }
  },
  "shell": "/bin/bash",
  "mcp": {
    "gbrain": {
      "type": "local",
      "command": ["/home/dev-p/.bun/bin/gbrain", "serve"],
      "enabled": true,
      "environment": {
        "OPENAI_API_KEY": "{env:OPENAI_API_KEY}"
      }
    }
  }
}
```

Key points:
- **No models block** — model selected from UI picker
- **external_directory** covers shared infrastructure (`mybizz`), all projects (`projects`), and PDLF framework (`pdlf`)
- API key hardcoded (deliberate decision — env var approach was unreliable with OpenRouter in prior attempts)
- GBrain MCP enabled via bun binary at full path
- All permissions: conservative (`ask`)

---

### FILE 2: `~/.config/opencode/AGENTS.md` (WSL Global Rules)

Path: `/home/dev-p/.config/opencode/AGENTS.md`

**214 lines.** Loaded as mandatory context in every OpenCode session. Section structure:

| Section | Content |
|---|---|
| Purpose | Applies to all sessions for user `dev-p`. Project-agnostic. Defines Plan/Build roles and global defaults. |
| Global agent roles | Plan = primary reasoning agent. Plan does not write. Ever. Build = sole writer, only on explicit approval. |
| Session boundary | Rule re-read before every task. Single session only. Abort on compliance failure or runaway loops. |
| Behavioral principles | Fact verification, error transparency, default-to-ask, zero gate-jumping, skill/tool continuity, no batching, checklist integrity. |
| Safety rules | Destructive operation warnings, diff-first approach, git permission-only, no credential exposure. |
| Tooling permissions | Edits/webfetch/bash all `ask` by default. External directories read-only unless explicitly allowed. |
| Build verification | No code complete until tested against applicable test standard. |
| Task completion reporting | Every task has a report. Separate housekeeping questions (commit/push, GBrain sync). Audit trail required. |
| Communication rules | Concise, structured, fact-vs-inference distinction, no emojis. |
| Project precedence | Project AGENTS.md inherits global; can only tighten, never loosen. Conflict → stricter wins → ask. |

---

### FILE 3: `project-library/AGENTS.md` (Project Rules — PLAN workspace)

Path: `/mnt/c/projects/mb-3-cs/project-library/AGENTS.md`

**329 lines.** Full project rules for the planning/documentation workspace. Key sections:

- **12 Hard Rules:** anvil.yaml protection, no time-based references, no hardcoded colors (CSS custom properties only, MUI palette), Plan reads/Build writes boundary, no writes to mb-3-cs, skill ownership, no batching, no deviation from task, binding documents, zero gate-jumping, checklist integrity, default-to-ask
- **Workspace structure:** 10 folders — `adr/`, `docs/`, `wireframes/`, `screens/`, `wip/`, `gstack-outputs/`, `opencode-outputs/`, `matt-skills-output/`, `workspace/gbrain/`, `workspace/gstack/`
- **Artifact governance:** Routing by producer (GStack/OpenCode/Matt Pocock) and by file type. Promotion rules. Exclusions.
- **GStack binary path override:** `/mnt/c/mybizz/gstack/bin/<binary-name>` (always use this literal path)
- **Matt Pocock skills artifact governance:** Per-skill save location rules (pre-emptive, unverified for some skills)
- **Source of truth:** `adr/`, `docs/`, `wireframes/` in project-library; `mb-3-cs/` for code (read-only context)
- **Prescribed standards:** Anvil.works, Material 3 theme, M3 dependency, MUI overlay
- **Task completion reporting:** Reports to producer's output folder. `devlog-index.md` and `judgement-trail.md` at project-library root. Scaffold initialization for these files if missing.

---

### FILE 4: `project-library/opencode.json` (Project Config — PLAN workspace)

Path: `/mnt/c/projects/mb-3-cs/project-library/opencode.json`

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
      "C:\\projects\\mb-3-cs\\mb-3-cs\\**": "allow"
    }
  }
}
```

Key points:
- Default agent: `plan`
- anvil.yaml explicitly denied
- Read access to sibling code repo: `C:\projects\mb-3-cs\mb-3-cs\**`

---

### FILE 5: `project-library/.opencode/agents/build.md`

Path: `/mnt/c/projects/mb-3-cs/project-library/.opencode/agents/build.md`

```markdown
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

## Task completion
- Every task produces a completion report saved to `opencode-outputs/` per project AGENTS.md artifact governance.
- Update `devlog-index.md` and `judgement-trail.md` per project AGENTS.md.
- Ask the two housekeeping questions (commit/push, GBrain sync) separately at every task completion.

## Environment
- Web UI only: `opencode web` from WSL. No Desktop app, no Desktop bridge.
- Workspace root: `/mnt/c/projects/mb-3-cs/project-library/`.
- Code repo (read-only): `/mnt/c/projects/mb-3-cs/mb-3-cs/`.
```

---

### FILE 6: `project-library/.opencode/agents/plan.md`

Path: `/mnt/c/projects/mb-3-cs/project-library/.opencode/agents/plan.md`

```markdown
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

## Design artefact awareness
- Wireframes in `wireframes/` are structural HTML layout references for Anvil UI development.
- DESIGN.md in `docs/` is the authoritative design system document produced by /design-consultation.
- Production HTML outputs from /design-html serve as the Anvil build reference.

## Coordination with build agent
- Produce clear prompts and build-rules files for the build agent.
- Do not present unvalidated assumptions as authoritative project truth.
- Do not make live implementation changes in `mb-3-cs` without explicit user request.

## Safety
- Obey all source-of-truth restrictions, certainty rules, and safety rules in project `AGENTS.md`.
- Be conservative when proposing refactors.
- Ask the user before designing large or risky changes.
- Git operations are permission-only.

## Task completion
- Every task produces a completion report saved to `opencode-outputs/` per project AGENTS.md artifact governance.
- Update `devlog-index.md` and `judgement-trail.md` per project AGENTS.md.
- Ask the two housekeeping questions (commit/push, GBrain sync) separately at every task completion.

## Environment
- Web UI only: `opencode web` from WSL. No Desktop app, no Desktop bridge.
- Workspace root: `/mnt/c/projects/mb-3-cs/project-library/`.
- Code repo (read-only context): `/mnt/c/projects/mb-3-cs/mb-3-cs/`.
```

---

### FILE 7: `mb-3-cs/agents.md` (Project Rules — BUILD workspace)

Path: `/mnt/c/projects/mb-3-cs/mb-3-cs/agents.md`

**130 lines.** Key sections (updated 2026-07-06 — Plan write conflict resolved):

- **Access model:** Build agent may read both repos, write only in approved mb-3-cs areas. Plan agent reads both repos for analysis. Plan does not write. Ever. (Matches global AGENTS.md Hard Rule 4.)
- **Hard file protections:** `anvil.yaml`, `standard-page.html`, `theme.css`, `parameters.yaml`, `templates.yaml`, `**/form_template.yaml` — never change without explicit permission.
- **Write boundaries:** Only Build agent writes. Build may write in: `checks/`, `client_code/`, `server_code/`, `output/`, `scripts/`, `prompts/` (when approved), `docs/` (when approved), `tests/` (when approved), `theme/` (when approved). Build must not write in `project-library`.
- **Timestamp header:** `Created:` / `Updated:` in UTC+2 on all touched files.
- **GBrain search guidance:** Source routing for `default`, `mb-3-cs`, `gbrain-repo` sources.

---

### FILE 8: `mb-3-cs/opencode.json` (Project Config — BUILD workspace)

Path: `/mnt/c/projects/mb-3-cs/mb-3-cs/opencode.json`

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
      "C:\\projects\\mb-3-cs\\project-library\\**": "allow"
    }
  }
}
```

Key points:
- Default agent: `build`
- 4 files denied (`anvil.yaml`, `parameters.yaml`, `templates.yaml`, `**/form_template.yaml`)
- 2 files explicitly `ask` (`standard-page.html`, `theme.css`)
- Read access to sibling project-library: `C:\projects\mb-3-cs\project-library\**`

---

### FILE 9: `mb-3-cs/.opencode/agents/build.md`

Path: `/mnt/c/projects/mb-3-cs/mb-3-cs/.opencode/agents/build.md`

```markdown
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

## Task completion
- Every task produces a completion report saved to `opencode-outputs/` in `project-library`.
- Update `project-library/devlog-index.md` and `project-library/judgement-trail.md` per project AGENTS.md.
- Ask the two housekeeping questions (commit/push, GBrain sync) separately at every task completion.

## Environment
- Web UI only: `opencode web` from WSL. No Desktop app, no Desktop bridge.
- All paths are WSL paths: `/mnt/c/projects/mb-3-cs/mb-3-cs/` (code), `/mnt/c/projects/mb-3-cs/project-library/` (docs).
```

---

### FILE 10: `mb-3-cs/.opencode/agents/plan.md`

Path: `/mnt/c/projects/mb-3-cs/mb-3-cs/.opencode/agents/plan.md`

```markdown
---
description: Planning agent for mb-3-cs — architecture reasoning, prompts, build-rules, and implementation planning
mode: primary
---

You are the **planning** agent for the mb-3-cs live app.

## Role
- Plan changes to the application. Plan does not write. Ever. This matches global AGENTS.md Hard Rule 4.
- Produce plans, prompts, and build-rules — the Build agent handles all writes on explicit user approval.
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

## Task completion
- Every task produces a completion report saved to `opencode-outputs/` in `project-library`.
- Update `project-library/devlog-index.md` and `project-library/judgement-trail.md` per project AGENTS.md.
- Ask the two housekeeping questions (commit/push, GBrain sync) separately at every task completion.

## Environment
- Web UI only: `opencode web` from WSL. No Desktop app, no Desktop bridge.
- All paths are WSL paths: `/mnt/c/projects/mb-3-cs/mb-3-cs/` (code), `/mnt/c/projects/mb-3-cs/project-library/` (docs).
```

---

## Additional project configs (mb4ecom, mb5pdlf)

Both PDLF-managed projects now have `opencode.json` files created 2026-07-06:

**mb4ecom project-library** (`C:\projects\mb4ecom\mb4ecom-project-library\opencode.json`):
- default_agent: `plan`
- deny: `**/anvil.yaml`
- external_directory: `C:\projects\mb4ecom\mb4ecom\**`

**mb4ecom code** (`C:\projects\mb4ecom\mb4ecom\opencode.json`):
- default_agent: `build`
- deny: `anvil.yaml`, `**/form_template.yaml`
- external_directory: `C:\projects\mb4ecom\mb4ecom-project-library\**`

**mb5pdlf project-library** (`C:\projects\mb5pdlf\mb5pdlf-project-library\opencode.json`):
- Same pattern as mb4ecom with paths adjusted

**mb5pdlf code** (`C:\projects\mb5pdlf\mb5pdlf\opencode.json`):
- Same pattern as mb4ecom with paths adjusted

These projects use their own `AGENTS.md` for governance and do not have `.opencode/agents/` definitions (use global plan/build agent defaults).

---

## Command files

`project-library/.opencode/commands/` contains 49 `.md` command files — project-local slash-command wrappers that delegate to GStack skills. Each follows the standard pattern:

```md
# /command-name

Run the gstack-command-name workflow.

When you run this command, you should:
- Load and use the `gstack-command-name` skill.
- Follow the skill instructions exactly.
- Respect the current workspace rules, AGENTS.md, and OpenCode permissions.
- Keep changes minimal and fit for purpose.
```

Cleaned up 2026-07-06: removed 3 duplicate files (`gstack-context-save.md`, `gstack-learn.md`, `gstack-upgrade.md`), fixed 2 incorrect headers (`build.md`, `review.md`), standardized 5 formatting outliers.

No global commands directory exists. These project-level commands are the only command definitions.
