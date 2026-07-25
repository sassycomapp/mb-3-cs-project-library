# AGENTS.md — mb-3-cs-project-library [mb-3-cs]

> Inherits Global `AGENTS.md` in full. This file adds project-specific rules only.

## Project purpose

Planning, documentation, design, and GStack workflow workspace for the Mybizz Consulting Services application (mb-3-cs). Not the live code repository. Live code lives in `mb-3-cs`.

---

## Definitions

- **mb-3-cs-project-library** — this repo (planning/documentation/design). **mb-3-cs** — the separate live application code repo.
- Other definitions as per Global `AGENTS.md`.

---

## Precedence order (highest to lowest)

1. Explicit user instruction in the current session
2. Global `AGENTS.md` rules
3. Hard Rules in this file
4. ADRs (`adr/`)
5. Specifications (`docs/`)
6. A skill's own default behavior

---

## Hard rules

### 1. anvil.yaml protection (enforced by opencode.json)
Never write to `anvil.yaml`.

### 2. Two-repo separation
- `mb-3-cs-project-library/` = planning, docs, ADRs, wireframes. Does NOT go to Anvil.
- `mb-3-cs/` = live Anvil code + `anvil.yaml`. Goes to Anvil via GitHub VC.
- Write to code repo only during coding phase, only for code files, never `anvil.yaml`.

### 3. GBrain cross-contamination discipline
Tag every GBrain result with its source project. Cross-project decisions require explicit human confirmation. Scope queries with `--source mb-3-cs`.

### 4. No time-based references
Use Phase > Stage > Task > Sub-task hierarchy. Not "2 weeks" or "3 days."

### 5. No hardcoded colors
Every color through CSS custom properties. See `docs-local/DESIGN_palette_section.md`.

### 6. Plan reads, Build writes (enforced by opencode.json)
Plan never writes. Build writes only with user approval.

### 7. Skill ownership
Ask which skill to use before starting any task.

### 8. No deviation from the given task
Execute as specified. Propose deviations before acting.

### 9. Binding documents
ADRs, specs, rule files are binding. Check before starting. Flag conflicts.

### 10. Mandatory project context reading
Read `README.md` before every task.

---

## Workspace structure

| Folder | Purpose |
|---|---|
| `adr-local/` | Architecture Decision Records |
| `docs-local/` | Durable documentation, specs, UI standards |
| `wireframes/` | HTML wireframes and mockups |
| `screens/` | Screen HTML mockups and compliance checklists |
| `wip/` | Active work-in-progress |
| `output-gstack/` | GStack-produced reports |
| `output-opencode/` | OpenCode-produced reports |
| `output-matt-skills/` | Matt Pocock skill outputs |

---

## Artifact governance

Every artifact gets saved. Build actions only. Confirm save location in response.

| Producer | Destination |
|---|---|
| GStack | `output-gstack/` |
| OpenCode | `output-opencode/` |
| Matt Pocock skills | `output-matt-skills/` |

File type overrides: docs → `docs/`, ADRs → `adr/`, wireframes → `wireframes/`, screens → `screens/`.

Promotion: long-lived specs from `output-gstack/` → `docs-local/`. ADRs belong in `adr-local/`.

---

## GStack binary path override

Use `/mnt/c/mybizz/gstack/bin/<binary-name>` directly.

---

## Matt Pocock skills artifact governance

- `domain-modeling`, `grill-with-docs` → `CONTEXT.md` + `adr/`.
- `to-tickets`, `triage` → `.scratch/`.
- `setup-matt-pocock-skills` → `docs/agents/` + `AGENTS.md`.
- `handoff`, `improve-codebase-architecture` → `%TEMP%` + `output-matt-skills/`.
- `teach` → `C:\mybizz\matt-skills-teach`.
- Others → ask the user.

---

## Source of truth

- `mb-3-cs-project-library/adr-local/` — architectural decisions
- `mb-3-cs-project-library/docs-local/` — documentation and specs
- `mb-3-cs-project-library/wireframes/` — visual references
- `mb-3-cs/` — live code (read-only context)

Fact rule: do not use training data for project-specific facts. Validate GBrain research before use.

---

## Access model

- Plan: may read both repos. Never writes.
- Build: writes in `mb-3-cs-project-library` with user approval. Writes in `mb-3-cs` only during coding phase with case-by-case permission.

---

## Task completion reporting

- Reports saved as `.md` in producer's output folder.
- `devlog-index.md` — reverse-chronological index of all reports.
- `judgement-trail.md` — flagged issues and developer decisions per task.
- If `devlog-index.md` or `judgement-trail.md` doesn't exist, create with header only (one-time scaffold exception).
