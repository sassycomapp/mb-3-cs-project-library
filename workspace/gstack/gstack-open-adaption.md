# GStack OpenCode Adaption Notes

## 1. Purpose

This document captures how **stock GStack ideas** are adapted to:

- OpenCode’s agent/permissions model,
- this Windows + WSL setup,
- your personal preferences (Plan vs Build, devlog/todo, ADRs, etc.).

---

## 2. Permissions and authority

- OpenCode remains the **only** system that:
  - defines which files can be edited,
  - enforces protected-file rules,
  - controls shell/web/tool permissions.

- GStack is deliberately kept:
  - as a workflow/structuring layer,
  - never as a permissions or config authority.

- Any GStack workflow that conflicts with:
  - AGENTS rules,
  - `opencode.json` permissions,
  - or explicit user instructions,
  is overridden by those higher-level rules.

---

## 3. Plan vs Build vs GStack

- Plan and Build are defined in:
  - global `AGENTS.md` and `opencode.json`,
  - project-local agent files.

- GStack workflows are written to **match** that split:
  - `docs-plan` → Plan leads, Build optional for mechanical edits.
  - `code-plan-build` → Build leads, Plan used for complex/risky work.
  - `status-handover` → Plan leads.

- The GStack role map (`gstack-role-map.md`) exists specifically to keep those aligned.

---

## 4. Protected files and backups

Stock GStack might assume more freedom over file edits. Here:

- All code edits go through `mb-3-cs` rules:
  - backups in `working-backup`,
  - `Created:` / `Updated:` timestamps where safe,
  - strict protected-file behavior (Anvil/parameters/templates/theme/standard-page).

- Any GStack workflow that involves code must:
  - treat these rules as non-negotiable,
  - explicitly confirm backups/protection in Build→Plan handoffs.

---

## 5. Devlog, todo, and ADR integration

Instead of letting GStack manage its own tracking, this workspace:

- uses the shared devlog/todo at:
  - `C:\mybizz\devlog\*`
  - `C:\mybizz\todo\*`

- requires:
  - Plan → allowed to update devlog/todo.
  - Build → read-only; must hand off logging/todo updates.

- integrates ADRs as Tier A GBrain sources, which means:
  - workflows that change architecture should update ADRs,
  - memory respects those ADRs as authoritative.

GStack workflows are written to assume this pattern.

---

## 6. Skills vs CLI

Rather than relying on direct `gstack` CLI calls from the user:

- this workspace leans on **skills** inside OpenCode for GStack behaviour,
- the agents are encouraged (by your opening prompt) to choose and chain those skills automatically.

CLI usage in WSL is kept for:

- debugging,
- low-level inspection,
- experimentation that is not yet codified as a workflow.

---

## 7. Personal style notes

This adaption also encodes your preferences:

- concise, numbered responses,
- explicit separation of:
  - facts,
  - inferences,
  - proposed actions,
- heavy emphasis on:
  - devlog/todo/ADR discipline,
  - minimal repeated explanation once the system is stable.

Any future GStack changes should keep these preferences in mind.