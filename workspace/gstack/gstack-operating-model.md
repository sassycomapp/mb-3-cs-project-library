# GStack Operating Model

## Purpose
This document defines how GStack operates in the Mybizz CS workspace.

GStack is the workflow and orchestration layer around the existing OpenCode setup.  
It does not replace OpenCode permissions, project rules, or repo-local authority.

## Authority model
The authority order for this workspace is:

1. User decision in the active session.
2. Authoritative repo files and canonical tracking (`devlog` / `todo`).
3. Project `AGENTS.md` and project `opencode.json`.
4. Global OpenCode `AGENTS.md` and `opencode.json`.
5. GStack workflow conventions.
6. GBrain advisory memory.

If a GStack workflow expectation conflicts with OpenCode config, project rules, or explicit user instruction, the stricter or more authoritative layer wins.

## Role of GStack
GStack is responsible for:

- standardising repeatable workflows (docs-plan, code-plan-build, status-handover),
- improving task handoff quality,
- reducing ambiguity between Plan and Build phases,
- making docs, code, and handover work more consistent,
- coordinating with GBrain when advisory memory is useful.

GStack is not responsible for:

- overriding file permissions,
- changing protected-file rules,
- making canonical devlog/todo writes directly unless the active OpenCode agent and project rules already allow it,
- redefining project truth.

## Workspace scope
This operating model applies to:

- `C:\_mb2-cs-app\project-library`
- `C:\_mb2-cs-app\mb-3-cs`
- `C:\_mb2-cs-app\devlog\`
- `C:\_mb2-cs-app\todo\`
- `C:\_mb2-cs-app\gstack`
- the running GBrain instance (Supabase + OpenRouter) configured for this workspace

## Core principle
OpenCode remains the runtime host.

- OpenCode global and project configs define models, agents, permissions, and protected-file behavior.
- GStack defines how work should flow (plan, build, review, handoff).
- GBrain stores and recalls approved memory with provenance and authority labels.

## Workflow families
This workspace uses three GStack workflow families (see `gstack-workflows.md`):

1. `docs-plan`
   - used primarily in `project-library`
   - planning-first
   - documentation, validation, architecture, prompt, and build-rules work

2. `code-plan-build`
   - used primarily in `mb-3-cs`
   - plan before build when the task is architectural, risky, or unclear
   - build executes approved implementation work inside repo rules

3. `status-handover`
   - cross-workspace
   - status, checkpoint, session-close, and handoff preparation work

These workflows are usually invoked through **OpenCode skills** (slash commands) chosen by the agents, not by you running the `gstack` CLI directly.

## Plan and Build coordination

### Plan
Plan is responsible for:

- reasoning,
- architecture and workflow analysis,
- implementation guidance,
- prompt/build-rules preparation,
- canonical devlog/todo updates where allowed,
- ADR review and creation when new architectural decisions are made.

### Build
Build is responsible for:

- implementation under repo rules,
- controlled mechanical edits,
- validation output,
- providing handoff data for Plan when tracking updates or ADR updates are required.

### Shared tracking rule
Canonical shared tracking remains:

- `C:\_mb2-cs-app\devlog\README.md`
- `C:\_mb2-cs-app\devlog\index.md`
- `C:\_mb2-cs-app\todo\todo.md`
- `C:\_mb2-cs-app\todo\todo-archive.md`

Rules:

- Plan may update canonical tracking where rules allow.
- Build may read for context but must not write directly.
- At the start of any task, agents must:
  - consult `todo.md` for relevant items and incorporate them if appropriate,
  - consult the ADR folder for relevant decisions and treat those ADRs as authoritative.

Any new architectural decisions must be recorded as new ADRs.

## GBrain defaults for this workspace
- GBrain is a running Supabase-backed brain with OpenRouter embeddings.
- GStack workflows may use GBrain for:
  - recalling past ADR/decision/devlog/todo context,
  - re-surfacing prior handoffs and validated summaries.
- GBrain recall is always advisory:
  - repo files, ADRs, and canonical tracking win on conflict,
  - stale memory must be re-ingested or invalidated per the GBrain sync policy.

## GStack defaults for this workspace
- Checkpoint mode: off by default.
- Checkpoint push: false by default.
- Telemetry: off by default unless explicitly re-enabled later.
- Cross-repo work: read access only unless OpenCode rules and the active repo both allow writes.
- Handoffs: mandatory when work crosses Plan/Build, docs/code, or session boundaries.

## Success criteria
GStack is operational in this workspace when:

- workflows are repeatable and predictable,
- handoffs are consistent and follow the contract,
- OpenCode permissions remain authoritative,
- canonical tracking and ADR ownership is preserved,
- docs and code workflows can be executed with minimal ambiguity,
- GBrain memory is used for recall without undermining repo truth.