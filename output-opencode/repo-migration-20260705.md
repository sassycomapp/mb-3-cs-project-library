---
title: "repo-migration-20260705"
doc-id: "mb-886dcfbe"
updated: "2026-10-05"
---
# Repo Migration — Completion Report

**Date:** 2026-07-05
**Agent:** Build

## Purpose

Migrate two repos from `C:\mybizz\` to `C:\projects\mb-3-cs\`:
1. **project-library** (docs repo): move in-place after committing unpushed work
2. **mb-3-cs** (code repo): clone from GitHub + transfer gitignored local files

## Phases Completed

### Phase 1: Commit & push project-library
- Committed 206 files (14,705 insertions, 22,881 deletions) — bulk cleanup, reorg, new docs, screens rework
- Pushed to `origin/main`: `68c3947..947fb8b`

### Phase 2: Move project-library in-place
- Copied from `C:\mybizz\project-library` to `C:\projects\mb-3-cs\project-library`
- WSL-to-Windows `cp -r` timed out on node_modules; completed with `rsync`
- Verified: 52 commands, 2 agents, 24 node_modules packages, `.git` intact

### Phase 3: Clone mb-3-cs
- Cloned `https://github.com/sassycomapp/mb-3-cs` to `C:\projects\mb-3-cs\mb-3-cs` (259 files)

### Phase 4: Transfer mb-3-cs local-only files
All gitignored items copied from old to new location:
- `.opencode/` (agents: plan.md, build.md)
- `agents.md`
- `opencode.json`
- `pytest.ini`
- `tests/` (conftest.py, test_vault_service.py)

### Phase 5: Update path references
Updated in 3 files + 5 operational documents:
- `mb-3-cs/opencode.json`: `C:\mybizz\project-library\**` → `C:\projects\mb-3-cs\project-library\**`
- `project-library/opencode.json`: `C:\mybizz\mb-3-cs\**` → `C:\projects\mb-3-cs\mb-3-cs\**`
- `pdlf/registry/projects.json`: updated `library_path` and `code_path`
- `prompts/screen-production-prompt.md`: 19 path references updated
- `standard-operating-procedures/internal-standards-anvil-wireframe-conversion.md`: 1 ref updated
- `workspace/system-operation-reference/workflow reference/Daily operations.md`: 4 refs updated
- `workspace/gstack/gstack-install-plan.md`: 1 ref updated

### Phase 6: Re-register GBrain sources
- Killed stale gbrain daemon (PID 837, pts/6)
- Removed old `mb-3-cs` source (240 pages) and `project-library` source (212 pages)
- Created new `mb-3-cs` source → `/mnt/c/projects/mb-3-cs/mb-3-cs` (2 pages markdown-only)
- Created new `project-library` source → `/mnt/c/projects/mb-3-cs/project-library` (318 pages)
- Synced and embedded both sources
- GBrain doctor: embed score 35/35 (perfect). Overall brain score lower due to features not yet configured (links, timeline, takes).

### Phase 7: Verification
- Git remotes confirmed correct for both repos
- `.opencode/` commands: 52 in project-library, agents present in both
- All 5 mb-3-cs local files present
- GBrain search from both new sources returns results
- All path reference edits confirmed

## Issues Encountered

1. **GBrain CLI syntax**: v0.42.44.0 uses positional syntax `gbrain sources add <id> --path <path>`, not `--id` flag. The earlier plan used incorrect `--id` flag, causing "Unknown flag" errors. Resolved by reading CLI source.
2. **GBrain daemon can't start from bash tool**: `gbrain serve` is MCP-stdio-only and exits when stdin closes. Must be started from a persistent terminal.
3. **WSL filesystem performance**: `cp -r` of large `.opencode/node_modules/` (24 packages, many small files) timed out. Resolved with `rsync`.

## MB-3-CS code pages

The old gbrain `mb-3-cs` source had 240 pages (including Python code files). The new sync only imports markdown files (2 pages). The Python code import script from `workspace/system-operation-reference/gbrain reference/gbrain-reference.md` needs to be re-run to re-import the code surface. This is a separate task.

## Next Steps

- Start gbrain daemon from a persistent terminal: `gbrain serve` — **User**
- Re-run mb-3-cs Python code import for full code search — **Build**
- After verification, delete old `C:\mybizz\project-library` and `C:\mybizz\mb-3-cs` — **User**
- Consider running `gbrain extract --stale` for graph edges if link coverage is needed — **Build**

Saved to: `opencode-outputs/repo-migration-20260705.md`
