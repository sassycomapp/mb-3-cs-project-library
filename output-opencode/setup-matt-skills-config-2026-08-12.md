# Setup Report: Matt Pocock Engineering Skills Configuration

**Date:** 2026-08-12T212422+0200
**Operation:** Re-run of `setup-matt-pocock-skills` — issue tracker switched to GitHub, agent config moved to AGENTS.md, domain docs consumer rules corrected, stale copies archived.
**Result:** 6 files changed, 3 files moved (nothing deleted).

---

## Decisions (developer-approved)

| Section | Decision |
|---|---|
| A — Issue tracker | **GitHub Issues** (`gh` CLI) — switched from the previous Local markdown (`.scratch/`) convention |
| B — Triage labels | Defaults kept: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix` |
| C — Domain docs | **Single-context**, consumer rules corrected to actual on-disk layout |
| File target | **AGENTS.md** (developer override — project uses AGENTS.md, not CLAUDE.md) |
| Stale copies | Moved to `obsolete/` — **not deleted** (developer override) |

## Changes

| File | Change |
|---|---|
| `AGENTS.md` | Added `## Agent skills` section: GitHub issue tracker, default triage labels, single-context domain docs (`docs-local/CONTEXT.md` + `adr-local/`) |
| `CLAUDE.md` | Replaced its `## Agent skills` block with a pointer to AGENTS.md as canonical source (avoids two conflicting sources) |
| `docs/agents/issue-tracker.md` | Rewritten for GitHub (mb-3-cs-0038, `date-created` refreshed): `gh` conventions, PRs-as-request-surface **off**, wayfinder operations |
| `docs/agents/domain.md` | Consumer rules corrected to real layout (mb-3-cs-0037, refreshed): reads `docs-local/CONTEXT.md` + `adr-local/`; file-structure tree now matches disk |
| `docs/agents/triage-labels.md` | Unchanged — already the five defaults |
| `docs-local/CONTEXT.md` | Dropped stale "47 ADRs / `docs/adr-index.md`" claim → "ADRs live in `adr-local/` (currently 3)" |
| `agents/{issue-tracker,triage-labels,domain}.md` | `git mv` → `obsolete/` (renames recorded in git, nothing deleted) |

## Verification

- `git status`: 3 renames (`agents/*.md` → `obsolete/*.md`), no deletions.
- Repo-wide grep: no live `docs/adr-index.md` references remain (only the archived `obsolete/domain.md` copy, intentionally preserved).
- `AGENTS.md:88` `## Agent skills` present; `CLAUDE.md:59` defers to AGENTS.md.
- Pre-existing uncommitted changes (`adr-local/*` ×3, `opencode.json`, prior AGENTS.md edits) left untouched.

---

## Next Steps

- **Plan:** none — configuration complete. Future changes to tracker/labels/domain rules are edits to `docs/agents/*.md` (or a fresh re-run to switch trackers).
- **Build:** none outstanding.
- Housekeeping (asked separately): commit/push approval; GBrain sync approval.
