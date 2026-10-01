---
title: "project-registration-20260704"
doc-id: "mb-6099df04"
updated: "2026-10-01"
---
# Project Registration — Completion Report

**Date:** 2026-07-04
**Agent:** Build

## 1. Purpose

Register new projects with gbrain and update the PDLF registry to reflect the new naming convention and folder structure under `C:\projects\`.

## 2. Inputs

- User's project list and path specifications
- PDLF SKILL.md (registry location, conventions)
- Existing gbrain sources and PDLF registry state

## 3. Skills Invoked

None. This was a direct OpenCode task — no skills were invoked.

## 4. Work Performed

### Phase 1: Remove stale gbrain sources

Removed 4 sources that pointed to the old `_project-*` paths (now deleted):

- `mb4ecom` (was project-library at `_project-mb4ecom/`)
- `mb4ecom-code` (was code at `_project-mb4ecom/`)
- `mb5pdlf` (was project-library at `_project-mb5pdlf/`, 35 pages deleted)
- `mb5pdlf-code` (was code at `_project-mb5pdlf/`)

### Phase 2: Register 8 new gbrain sources

Naming convention: `<name>` = code, `<name>-project-library` = docs.

| Source | Path | Pages imported |
|---|---|---|
| `mb4ecom` | `/mnt/c/projects/mb4ecom/mb4ecom` | 1 |
| `mb4ecom-project-library` | `/mnt/c/projects/mb4ecom/mb4ecom-project-library` | 2 |
| `mb5pdlf` | `/mnt/c/projects/mb5pdlf/mb5pdlf` | 1 |
| `mb5pdlf-project-library` | `/mnt/c/projects/mb5pdlf/mb5pdlf-project-library` | 50 |
| `pdlf` | `/mnt/c/pdlf` | 18 |
| `scratch` | `/mnt/c/projects/scratch/scratch` | 1 |
| `scratch-project-library` | `/mnt/c/projects/scratch/scratch-project-library` | 1 |

**pdlf-project-library** was skipped — the path overlaps with the `pdlf` source (user confirmed skip).

### Phase 3: Initialize git repos and sync

`pdlf`, `scratch`, and `scratch-project-library` were not git repos. Initialized with `git init`, created initial commits, then synced with `gbrain sync --source <name>`.

### Phase 4: Update PDLF registry

Updated `/mnt/c/pdlf/registry/projects.json`:
- `mb4ecom`: updated paths and gbrain source IDs (`mb4ecom-project-library` for docs, `mb4ecom` for code)
- `mb5pdlf`: updated paths and gbrain source IDs (`mb5pdlf-project-library` for docs, `mb5pdlf` for code)
- Added `pdlf` entry
- Added `scratch` entry
- `mb-3-cs` unchanged

## 5. Outputs

| Artifact | Path |
|---|---|
| Updated registry | `/mnt/c/pdlf/registry/projects.json` |
| Completion report | `opencode-outputs/project-registration-20260704.md` |
| Bootstrapped devlog-index | `/mnt/c/mybizz/project-library/devlog-index.md` |
| Bootstrapped judgement-trail | `/mnt/c/mybizz/project-library/judgement-trail.md` |

## 6. Decisions Made

- **User**: Skip pdlf-project-library as a separate gbrain source (path overlap with pdlf)
- **User**: New naming convention confirmed (`<name>` = code, `<name>-project-library` = docs)
- **Agent**: Used gbrain CLI (not MCP) for source add/sync — MCP `gbrain_sources_add` didn't store `local_path`

## 7. Issues Encountered

1. **MCP `gbrain_sources_add` doesn't store `local_path`**: Sources created via MCP showed `local_path: null`. CLI is required for proper path storage.
2. **PGLite lock timeouts**: CLI commands failed while MCP server held the database lock. Resolved by stopping `gbrain serve` before running CLI operations.
3. **Overlapping source paths**: `pdlf-project-library` nested inside `/mnt/c/pdlf/` (already registered as `pdlf`). User chose to skip.
4. **Non-git repos**: `pdlf`, `scratch`, and `scratch-project-library` weren't git repos. Initialized with initial commits (required for gbrain sync).
5. **Git identity missing**: New repos had no committer identity. Used `-c user.name/user.email` per-command.

## 8. Next Steps

- Embed job was cancelled during the PGLite lock issue. Submit a new `gbrain submit_job embed` for the new sources once `gbrain serve` is restarted — **Build**
- Verify GitHub remotes for `mb4ecom` and `mb5pdlf` repos at their new paths — **Build**
- Consider adding GitHub remotes for `pdlf` and `scratch` repos — **Build**
