# GStack Overview (Mybizz CS Workspace)

## 1. What GStack is here

In this workspace, GStack is a **workflow and orchestration layer wrapped around OpenCode**, not a separate runtime or permission system.

GStack’s job is to:

- standardise how work flows (Plan ↔ Build, docs ↔ code, status/handover),
- structure tasks into clear workflows (`docs-plan`, `code-plan-build`, `status-handover`),
- improve handoff quality between sessions and repos.

GStack does **not**:

- override OpenCode permissions or protected-file rules,
- define project truth,
- write to repos or tracking files outside the rules in `AGENTS.md` and `opencode.json`.

Authoritative order for this workspace:

1. User decisions in the current session.
2. Repo files and canonical tracking (devlog/todo).
3. OpenCode rules (global + project).
4. GStack workflows and conventions.
5. GBrain advisory memory.

---

## 2. How GStack is actually used

GStack is surfaced to you through **skills inside OpenCode** (slash commands) rather than you running `gstack` manually most of the time.

Typical pattern:

- You start OpenCode Desktop, connected to the WSL server.
- You open `project-library` or `mb-3-cs` using the `/mnt/c/mybizz/...` path.
- You give the agent an opening instruction that allows it to call GStack skills (e.g. `/office-hours`, `/plan-ceo-review`, `/build`, `/investigate`, `/review`, `/ship`, `/setup-gbrain`) when needed.

The agent then chooses which workflows to run:

- `docs-plan` in `project-library` for planning/ADR/validation/prompt work.
- `code-plan-build` in `mb-3-cs` for safe implementation under repo rules.
- `status-handover` across both when closing a session or doing check‑ins.

Internally, these workflows:

- follow the rules in `gstack-operating-model.md`,
- use the handoff templates in `gstack-handoff-contract.md`,
- respect canonical devlog/todo ownership and protected-file policies.

---

## 3. Relationship to OpenCode and GBrain

- **OpenCode** is the runtime:
  - provides agents (`plan`, `build`),
  - enforces permissions and protected files,
  - owns canonical tracking behavior.

- **GStack**:
  - wraps workflows around OpenCode,
  - helps the agents structure their work into phases and handoffs,
  - can call out to GBrain where helpful.

- **GBrain**:
  - stores advisory memory (Supabase + OpenRouter),
  - can be called by GStack-aware skills for recall,
  - never outranks repo files, ADRs, or current decisions.

In short:
- OpenCode = “what is allowed and how edits happen”.
- GStack = “how work should flow”.
- GBrain = “what we remember across sessions”.

---

## 4. Main workflows (summary)

The three core workflows defined in `gstack-workflows.md` are:

1. **`docs-plan`** (project-library)
   - Use for ADR/architecture, MM/AM validation, docs/prompt/build‑rules.
   - Primary agent: `plan`.
   - Produces: scoped recommendations, document changes, handoffs, devlog/todo updates.

2. **`code-plan-build`** (mb-3-cs)
   - Use for implementation and refactors in the live code repo.
   - Flow: `plan` (for risky/complex work) → `build` (for concrete edits).
   - Produces: code changes, validation results, devlog/todo handoff data.

3. **`status-handover`** (cross-workspace)
   - Use for session close, checkpoints, and handoff to future sessions.
   - Primary agent: `plan`.
   - Produces: status summaries, unresolved items, next steps, tracking proposals.

Each of these can be driven manually (by naming the workflow) or indirectly via a GStack skill the agent chooses.

---

## 5. What this folder is for

The `workspace\gstack\` folder contains **policy and design docs** for GStack in this workspace. It does not:

- contain the GStack code (that is in `C:\mybizz\gstack`),
- act as a live config directory.

Instead it:

- defines how GStack should behave here,
- records decisions that constrain how skills and workflows are used,
- acts as a reference for both you and the agents when interpreting or extending workflows.