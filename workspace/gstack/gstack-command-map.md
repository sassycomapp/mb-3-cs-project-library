# GStack Command and Workflow Map

## 1. Purpose

This document maps **desired workflows** onto:

- GStack-related skills inside OpenCode,
- (optionally) `.opencode\commands\` entries,
- and the repos/agents they should be used with.

It is meant to help you and the agents choose the right mechanism without guessing.

---

## 2. High-level rule

When in doubt:

- In `project-library` → prefer `docs-plan` workflows.
- In `mb-3-cs` → prefer `code-plan-build` workflows.
- For session close or cross-session continuation → prefer `status-handover`.

The actual “commands” are usually **skills** (slash commands) the agents choose, not `gstack` CLI calls you run manually.

---

## 3. Typical workflows and their commands

> Note: The exact skill names may vary slightly; this map is conceptual and should be adjusted once you settle on the exact skillpack names.

### 3.1 Docs planning (`docs-plan`)

Use in: `project-library` with `plan` as primary agent.

- Goal: refine ADRs, validate MM/AM docs, design plans, update tracking.
- Likely skills / commands:
  - `/office-hours` — for general planning/triage.
  - `/plan-ceo-review` — for high-level strategic / CEO-facing planning.
  - `/plan-eng-review` — for technical/engineering planning.
- Outcomes:
  - updated ADRs and docs,
  - structured handoffs to `mb-3-cs`,
  - devlog/todo updates.

If you later add `.opencode\commands\` entries, you might define:

- `Docs workflow` → start a standard `docs-plan` flow with a small prompt.

### 3.2 Code implementation (`code-plan-build`)

Use in: `mb-3-cs` with `build` as primary agent (and `plan` when needed).

- Goal: implement a well-scoped change safely.
- Likely skills / commands:
  - `/build` — for the main implementation cycle.
  - `/investigate` — to gather context or debug before changes.
  - `/review` — to review the change set.
  - `/qa` — to run or plan tests where applicable.
  - `/ship` — to finalise a change and prepare it for deployment (as far as this workspace allows).
- Outcomes:
  - code changes under backup + protected-file rules,
  - validation results,
  - devlog/todo handoff proposals.

Potential `.opencode\commands\` (optional):

- `Code workflow` → standard “plan (if needed) → build → review → handoff” flow.

### 3.3 Status and handover (`status-handover`)

Use when: ending a session, checkpointing, or leaving a handoff for future work.

- Goal: capture current state clearly so a new session can resume easily.
- Likely skills / commands:
  - a generic `status-handover` flow (may be part of `/office-hours` or a specific status skill).
- Outcomes:
  - status summary,
  - unresolved items,
  - next steps,
  - proposed devlog/todo entries.

Optional future `.opencode\commands\`:

- `Status snapshot` → run a short `status-handover` routine.

---

## 4. GBrain-related commands (conceptual)

As GBrain usage matures, you may add:

- ingestion commands:
  - “Ingest ADR + devlog summary for completed feature”.
- recall commands:
  - “Recall decisions and context for module X from ADR & devlog”.

For now, these are expected to be:

- GStack-aware skills that know how to talk to GBrain,
- governed by the policies in `workspace\gbrain\`.

---

## 5. Maintenance

Whenever you:

- add a new frequently used skill,
- create an `.opencode\commands\` wrapper,
- or retire a workflow,

update this map so that:

- you don’t have to remember names from scratch every time,
- the agents have a clear reference for what you consider “normal” workflows.