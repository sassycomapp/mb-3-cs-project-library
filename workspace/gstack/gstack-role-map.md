# GStack Role Map (Plan, Build, Repos)

## 1. Purpose

This document maps the GStack workflows and roles onto:

- OpenCode agents (`plan`, `build`),
- the two repos (`project-library`, `mb-3-cs`),
- and your responsibilities.

---

## 2. Agents and repos

### 2.1 project-library

- Default agent: `plan`
- Primary workflow family: `docs-plan`
- Main responsibilities:
  - ADRs and architecture decisions,
  - MM/AM and spec validation,
  - refactor/feature planning,
  - prompt and build-rules authoring,
  - canonical devlog/todo writes (Plan only).

### 2.2 mb-3-cs

- Default agent: `build`
- Primary workflow family: `code-plan-build`
- Main responsibilities:
  - explicit, user-directed code changes,
  - refactors and migrations under clear rules,
  - backups and timestamps,
  - respecting protected-file rules,
  - providing tracking handoff data back to Plan or you.

---

## 3. Roles inside each workflow

### 3.1 docs-plan

- **Plan (lead role):**
  - Reads ADRs, MM/AM docs, and current devlog/todo.
  - Clarifies objectives and scope.
  - Produces structured plans, analysis, and handoff templates.
  - Writes canonical tracking updates where rules allow.

- **Build (supporting role in docs):**
  - Used only when non-live-code mechanical work is needed (e.g. structured document edits).
  - Must not bypass Plan’s ownership of canonical tracking.

### 3.2 code-plan-build

- **Plan:**
  - Only used when:
    - the task is architectural/complex,
    - risk is high,
    - multiple files or modules are involved.
  - Produces:
    - change plans,
    - risk notes,
    - handoff content for Build.

- **Build (lead role):**
  - Performs the actual edits in `mb-3-cs` under repo rules.
  - Ensures:
    - backups in `working-backup`,
    - timestamps where safe,
    - protected-file approvals.
  - Produces:
    - implementation summaries,
    - validation results,
    - proposed devlog/todo handoffs.

### 3.3 status-handover

- **Plan (lead role):**
  - Summarises current state at:
    - session close,
    - checkpoint points,
    - cross-repo handover moments.
  - Produces:
    - status snapshot,
    - unresolved items,
    - suggested next steps,
    - proposed devlog/todo entries.

- **Build (supporting role):**
  - Mainly a data source:
    - what was changed,
    - where,
    - with which results.

---

## 4. Your responsibilities vs agent responsibilities

- **You:**
  - choose the workspace (`project-library` vs `mb-3-cs`),
  - decide when something is too risky and needs extra confirmation,
  - accept/reject plans and handoffs,
  - can override or refine workflows when needed.

- **Agents (via GStack workflows):**
  - propose plans and handoffs,
  - keep devlog/todo/ADRs in sync with agreed rules,
  - respect permissions and protected-file rules,
  - use GBrain for recall but defer to current files.

---

## 5. Changes and drift

If you find that:

- Build is doing too much planning,
- Plan is drifting into uncontrolled edits,
- or GStack workflows feel out of sync with how you actually work,

you should:

1. update this role map,
2. update the relevant AGENTS.md files,
3. adjust `gstack-workflows.md` to match reality.

This file is the “who does what where” map; it should always represent your *current* intent.