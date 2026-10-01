---
title: "rename-report-project-library-to-mb-3-cs-2026-07-25"
doc-id: "mb-08dedf3b"
updated: "2026-10-01"
---
# Rename Report: project-library → mb-3-cs-project-library

**Date:** 2026-07-25
**Operation:** System-wide rename of the mb-3-cs documentation repo from `project-library` to `mb-3-cs-project-library`
**Result:** 19 files updated across 4 repos, 45+ references changed

---

## Summary by Repository

### 1. dev-root-hub (`C:\dev\dev-root\`) — 6 references in 3 files

| File | Line(s) | Content |
|------|---------|---------|
| `docmap.md` | 45 | `├── mb-3-cs-project-library/ ← Docs repo` |
| `project-inventory.md` | 16 | Documentation repo (Local): `/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library` |
| `project-inventory.md` | 17 | Documentation repo (GitHub): `sassycomapp/mb-3-cs-project-library` |
| `project-inventory.md` | 18 | Documentation repo (GBrain): `mb-3-cs-project-library` |
| `project-inventory.md` | 88 | Active Repo table entry |
| `mb-align-docs-README.md` | 127 | Register The use of registers has been discontinued. Please review + Quarantine paths (2 occurrences on one line) |

### 2. projects-reference (`C:\projects-reference\`) — 11 references in 6 files

| File | Line(s) | Content |
|------|---------|---------|
| `custom-skills-store/docs-manager-v8/SKILL.md` | 171 | Docs repo exclusion rule path |
| `custom-skills-store/docs-manager-v8/docmap.md` | 60 | Directory tree entry |
| `workspace-reference/opencode reference/opencode-reference.md` | 28 | Shell command path |
| `workspace-reference/opencode reference/opencode-reference.md` | 150 | opencode.json path |
| `workspace-reference/system config reference/workspace-docmap.md` | 28 | Workspace docs plan entry |
| `workspace-reference/system config reference/workspace-docmap.md` | 126 | GStack artifact name |
| `workspace-reference/gstack-reference/gstack-reference.md` | 27 | Heading |
| `workspace-reference/gstack-reference/gstack-reference.md` | 162 | Workspace table entry |
| `COMPLIANCE-SYSTEM.md` | 94 | AGENTS.md size tracking |
| `COMPLIANCE-SYSTEM.md` | 132 | OpenCode startup sequence (step 2) |
| `COMPLIANCE-SYSTEM.md` | 134 | OpenCode startup sequence (step 4) |

### 3. mb-3-cs code repo (`C:\dev\dev-mb-3-cs\mb-3-cs\`) — 9 references in 1 file

| File | Line(s) | Content |
|------|---------|---------|
| `agents.md` | 8 | Workspace description |
| `agents.md` | 9 | Read-only boundary |
| `agents.md` | 17 | Build agent read permission |
| `agents.md` | 19 | Plan agent restriction |
| `agents.md` | 23 | Plan agent read scope |
| `agents.md` | 62 | Blocked write path |
| `agents.md` | 93 | Build agent read context |
| `agents.md` | 117 | Cross-repo search |
| `agents.md` | 126 | GBrain default source |

### 4. mb-3-cs-project-library (`C:\dev\dev-mb-3-cs\mb-3-cs-project-library\`) — 19+ references in 9 files

| File | Line(s) | Content |
|------|---------|---------|
| `AGENTS.md` | 1, 13, 35, 115, 116, 117, 127 | Repo identity + directory references (7 instances) |
| `CONTEXT.md` | 9 | Repo definition |
| `agents/domain.md` | 9 | Domain model identity |
| `docs/agents/domain.md` | 9 | Domain model identity |
| `screens/analytics/DashboardAnalyticsForm/checklist-...md` | 3, 4 | Screen + wireframe paths (also fixed stale `/mnt/c/mybizz/` paths) |
| `screens/analytics/RevenueReportForm/checklist-...md` | 3, 4 | Screen + wireframe paths |
| `screens/blog/BlogPostForm/checklist-...md` | 3, 4 | Screen + wireframe paths |
| `standard-operating-procedures-local/internal-standards-anvil-wireframe-conversion.md` | 4, 5, 45, 97, 146, 366 | Shorthand name + absolute path references (also fixed stale `/mnt/c/projects/mb-3-cs/` paths) |
| `prompts-local/screen-production-prompt.md` | 24, 25, 28-33, 36-38, 41-45, 153, 156, 206 | 20+ absolute path references (also fixed stale `/mnt/c/projects/mb-3-cs/` paths) |

---

## Git Commits

| Repo | Commit | Status |
|------|--------|--------|
| `dev-root-hub` | `2782622` | Pushed |
| `projects-reference` | `a64a89c` | Pushed |
| `mb-3-cs` | Already committed | In sync with remote |
| `mb-3-cs-project-library` | `fd15eb2` | Pushed |

---

## Exclusions (not renamed, correct as-is)

- `project-library-global` — separate shared-standards repo at `C:\dev\project-library-global\`
- `mb4ecom-project-library`, `mb5pdlf-project-library`, `makepdlf-project-library` — already follow the `{slug}-project-library` convention
- Generic template references (`{slug}-project-library`, `<project-library>`) — represent the convention, not a specific project
- Historical/log files under `output-opencode/`, `output-gstack/`, `judgement-trail.md`, `devlog-index.md` — contain valid historical records of previous states

---

## Filesystem Verification

```
C:\dev\dev-mb-3-cs\
├── mb-3-cs/                    ← Code repo
├── mb-3-cs-project-library/    ← Docs repo (RENAMED)
├── WIP/
```

The folder on disk is named `mb-3-cs-project-library`. All 19 authoritative documents now match.
