---
title: "opencode-config-optimization-20260706"
doc-id: "mb-3f9780e8"
updated: "2026-10-01"
---
# OpenCode Config Optimization — Completion Report

**Date:** 2026-07-06
**Agent:** Build (project-library)
**Scope:** Multi-project OpenCode configuration audit, fix, and documentation refresh

---

## Summary

Optimized OpenCode configuration for a multi-project environment (4 projects: mb-3-cs, mb4ecom, mb5pdlf, scratch). Fixed the Plan-write permission conflict in mb-3-cs, updated all agent files, created project configs for previously unconfigured projects, cleaned up 52 command files, and rewrote 3 workspace reference documents.

---

## What changed

### Global config (`~/.config/opencode/opencode.jsonc`)
- Removed hardcoded model block (`xiaomi/mimo-v2.5-pro`) — model now selected from UI picker only
- Updated `external_directory` to cover `C:\projects\**` and `C:\pdlf\**` in addition to `C:\mybizz\**`
- API key retained (deliberate decision — env var approach previously unreliable)

### mb-3-cs/AGENTS.md — Plan-write conflict fix
- Removed Plan write permissions to `prompts/`, `docs/`, `tests/` (violated global "Plan never writes" rule)
- Consolidated all write access under Build agent only
- Added explicit-approval gates for Build writing to `prompts/`, `docs/`, `tests/`, `theme/`

### Agent files (all 4 updated)
Added to each `plan.md` and `build.md` (both project-library and mb-3-cs):
- `## Task completion` section — devlog-index.md, judgement-trail.md, housekeeping questions
- `## Environment` section — Web UI only, WSL paths, no Desktop bridge
- mb-3-cs plan.md: fixed "unless explicitly asked" → "Plan does not write. Ever."

### Project configs created
- `mb4ecom/mb4ecom-project-library/opencode.json` — plan agent, deny anvil.yaml, read code repo
- `mb4ecom/mb4ecom/opencode.json` — build agent, deny anvil.yaml + form_template.yaml, read project-library
- `mb5pdlf/mb5pdlf-project-library/opencode.json` — same pattern
- `mb5pdlf/mb5pdlf/opencode.json` — same pattern

### Command files (52 → 49, cleaned)
- Removed 3 duplicates: `gstack-context-save.md`, `gstack-learn.md`, `gstack-upgrade.md`
- Fixed headers: `build.md` (`/build-gstack` → `/build`), `review.md` (`/review-gstack` → `/review`)
- Standardized format across 5 outliers: `office-hours.md`, `plan-ceo-review.md`, `plan-eng-review.md`, `qa.md`, `setup-gbrain.md`, `setup-gbrain-doctor.md`, `sync-gbrain.md`
- Fixed `test-config-dir.md` — stale `C:\mybizz\` paths replaced with current

### Registry
- `project-inventory.txt` — fixed mb-3-cs paths, removed non-existent `pdlf-project-library` entry

### Workspace docs (3 files rewritten)
- `workspace-docmap.md` — multi-project path register The use of registers has been discontinued. Please review, all paths current, GBrain section deduplicated
- `workspace-description.md` — Web UI only usage guide, Desktop bridge removed, multi-project layout
- `opencode-10-config-files.md` — all 10 config file contents updated to current state, API key redacted, new project configs documented

---

## Files changed

| File | Action |
|---|---|
| `~/.config/opencode/opencode.jsonc` | Updated (models removed, external_directory expanded) |
| `mb-3-cs/agents.md` | Fixed (Plan write conflict resolved) |
| `project-library/.opencode/agents/plan.md` | Updated (task completion + environment sections) |
| `project-library/.opencode/agents/build.md` | Updated (task completion + environment sections) |
| `mb-3-cs/.opencode/agents/plan.md` | Updated (Plan-never-writes fix + new sections) |
| `mb-3-cs/.opencode/agents/build.md` | Updated (task completion + environment sections) |
| `mb4ecom/mb4ecom-project-library/opencode.json` | Created |
| `mb4ecom/mb4ecom/opencode.json` | Created |
| `mb5pdlf/mb5pdlf-project-library/opencode.json` | Created |
| `mb5pdlf/mb5pdlf/opencode.json` | Created |
| `.opencode/commands/test-config-dir.md` | Updated (stale paths) |
| `.opencode/commands/gstack-context-save.md` | Deleted (duplicate) |
| `.opencode/commands/gstack-learn.md` | Deleted (duplicate) |
| `.opencode/commands/gstack-upgrade.md` | Deleted (duplicate) |
| `.opencode/commands/build.md` | Fixed (header) |
| `.opencode/commands/review.md` | Fixed (header) |
| `.opencode/commands/office-hours.md` | Standardized |
| `.opencode/commands/plan-ceo-review.md` | Standardized |
| `.opencode/commands/plan-eng-review.md` | Standardized |
| `.opencode/commands/qa.md` | Standardized |
| `.opencode/commands/setup-gbrain.md` | Standardized |
| `.opencode/commands/setup-gbrain-doctor.md` | Fixed (header) |
| `.opencode/commands/sync-gbrain.md` | Standardized |
| `_ projects-registry/project-inventory.txt` | Fixed (paths) |
| `workspace/.../workspace-docmap.md` | Rewritten |
| `workspace/.../workspace-description.md` | Rewritten |
| `workspace/.../opencode-10-config-files.md` | Rewritten |
| `devlog-index.md` | Updated (this task) |
| `judgement-trail.md` | Updated (flagged issues) |
| `opencode-outputs/opencode-config-optimization-20260706.md` | Created (this report) |

**Total: 30 files changed (4 created, 13 updated, 3 deleted, 4 rewritten, 6 standardized/fixed)**

---

## Verification

- Global `opencode.jsonc`: no model block, external_directory covers all 3 roots
- `mb-3-cs/agents.md`: Plan-write permissions removed, Build covers all approved areas
- All 4 agent files: task completion and environment sections present
- mb4ecom/mb5pdlf: both repos have `opencode.json` with correct paths and protections
- 49 command files: no duplicates, all headers match filenames, standardized format
- `test-config-dir.md`: no stale `mybizz` paths
- `project-inventory.txt`: all paths current, no non-existent entries
- All 3 workspace docs: current paths, Web UI only, no model references, no duplicated content
- `devlog-index.md` and `judgement-trail.md`: this task logged

---

## State at completion

| Component | Status |
|---|---|
| Global config | Optimized (no models, multi-project external_directory) |
| Agent files (4) | Updated (consistent with global AGENTS.md) |
| mb-3-cs AGENTS.md | Fixed (Plan-write conflict resolved) |
| Project configs (mb4ecom, mb5pdlf) | Created (both repos) |
| Command files (49) | Cleaned and standardized |
| Workspace docs (3) | Current and accurate |
| Devlog index | Updated |
| Judgement trail | Updated |

---

## Next steps

- GStack config optimization — Build (when directed)
- GBrain config optimization — Build (when directed)
- Matt Pocock skills config — Build (when directed)
- PDLF rollout across all projects — Build (when directed)
