# GStack Workflows

## Purpose
This document defines the active GStack workflows for the Mybizz CS workspace.

These workflows standardise how work is initiated, executed, handed off, and closed.  
They are usually invoked via **OpenCode skills** (slash commands) chosen by the agents, not by direct `gstack` CLI usage.

---

## Workflow 1 — docs-plan

### Use when
Use this workflow when the primary task is in `project-library` and the work is mainly:

- analysis,
- validation,
- architecture reasoning,
- prompt authoring,
- build-rules authoring,
- documentation refinement,
- planning,
- canonical devlog/todo maintenance.

### Default workspace
- `C:\projects\mb-3-cs\project-library`

### Default agent
- `plan`

### Mandatory initial steps
At the start of any docs-plan workflow:

1. **Consult `todo.md`**
   - Open ``wip/todo.md` at project root`.
   - Identify any entries relevant to the current task.
   - Incorporate them into the current work where appropriate.
   - When such tasks are completed, mark them as `[DONE]` and move them to `todo-archive.md`.

2. **Consult ADRs**
   - Scan the ADR folder for entries relevant to the current task.
   - Treat ADRs as authoritative.
   - If new architectural decisions are made, record them as new ADR files.

### Typical inputs
- ADR files
- AM1/MM1 documents
- approved reference files
- canonical devlog/todo files
- prior validated planning outputs

### Typical outputs
- analysis summary
- clearly scoped recommendation
- document change plan
- prompt/build-rules draft
- validated document edits
- canonical devlog/todo updates when requested and permitted
- new or updated ADRs when decisions change architecture

### How invoked (examples)
- Agent-driven skills such as:
  - `/office-hours`
  - `/plan-ceo-review`
  - `/plan-eng-review`
- Optionally via project-local commands that wrap these skills.

### Exit condition
This workflow is complete when one of the following is true:

- the planning/document objective is complete,
- a build-ready handoff has been prepared,
- a canonical tracking update has been prepared or applied by Plan where requested,
- any out-of-scope items have been handed off into the canonical todo process.

---

## Workflow 2 — code-plan-build

### Use when
Use this workflow when the primary task is in `mb-3-cs` and the task requires:

- implementation,
- code refactoring,
- test support,
- migration,
- code validation,
- controlled file edits under repo rules.

### Default workspace
- `C:\projects\mb-3-cs\mb-3-cs`

### Default flow
1. Start with `plan` if the task is architectural, risky, unclear, or affects multiple files.
2. Move to `build` for approved implementation work.
3. Use Build→Plan handoff to propose devlog/todo updates and any ADR changes.

### Mandatory initial steps
At the start of any code-plan-build workflow:

1. **Confirm repo and path**
   - Ensure the workspace is `mb-3-cs` (via `pwd` or equivalent).

2. **Consult `todo.md`**
   - Check ``wip/todo.md` at project root` for items related to the current code area.
   - Fold them into the current task where appropriate.
   - Completed items are marked `[DONE]` and moved to `todo-archive.md`.

3. **Consult ADRs**
   - Read relevant ADRs in `project-library\adr\`.
   - Follow those decisions as the architectural baseline.
   - Plan to add a new ADR if you make new architectural decisions.

### Mandatory checks before edits
- repo path confirmed,
- protected files checked (Anvil/parameters/templates/form_template, `standard-page.html`, `theme.css`),
- backup rule applied before edit (mirrored path under `working-backup`),
- timestamp rule applied where safe (`Created:` / `Updated:` UTC+2),
- write scope checked against repo rules.

### Typical outputs
- approved implementation plan (if Plan was needed),
- code changes under backup + protection rules,
- validation result (tests / checks),
- proposed devlog handoff (type, slug, summary, files, outcome),
- proposed todo handoff (for out-of-scope items),
- ADR references and suggested new ADRs when architecture changed.

### How invoked (examples)
- Agent-driven skills such as:
  - `/build`
  - `/investigate`
  - `/review`
  - `/qa`
  - `/ship`
- Optionally via project-local “Code workflow” commands that wrap these.

### Exit condition
This workflow is complete when:

- approved implementation work is complete,
- validation result is recorded,
- any canonical tracking requirements have been handed off to Plan or to you,
- no protected-file or out-of-scope action remains unresolved.

---

## Workflow 3 — status-handover

### Use when
Use this workflow when the task is:

- session close,
- checkpoint summary,
- handover for later continuation,
- current-state consolidation,
- unresolved decision summary.

### Default workspace
- cross-workspace (docs and code as needed)

### Default agent
- `plan`

### Typical inputs
- current session notes,
- recent devlog entries,
- updated todo items,
- ADRs touched in this session,
- outputs from docs-plan or code-plan-build workflows.

### Typical outputs
- current status summary,
- list of completed items (with devlog confirmation),
- list of unresolved items,
- next recommended steps,
- proposed devlog status entry,
- proposed todo updates with pointers to future work.

### How invoked (examples)
- As the “last step” in a work block, either:
  - via a dedicated status skill, or
  - as part of a `/office-hours`/wrap-up flow.

### Exit condition
This workflow is complete when:

- the current state is captured clearly enough for a fresh session to resume without reconstructing context from scratch,
- any necessary devlog/todo updates have either been applied (by Plan) or clearly handed off.

---

## Workflow selection rule
If the user does not specify a workflow:

- use `docs-plan` in `project-library`,
- use `code-plan-build` in `mb-3-cs`,
- use `status-handover` for cross-session or session-close work.

If uncertain, ask one focused question before starting a large task.