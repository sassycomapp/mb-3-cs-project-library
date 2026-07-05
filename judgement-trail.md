# Judgement Trail
Flagged issues, developer decisions, and approved deviations, extracted per task.

## 2026-07-05 — Repo migration

**Issue flagged:** GBrain CLI v0.42.44.0 argument parser rejects `--id` flag syntax. "Unknown flag" errors on positional values.
**Agent action:** Read CLI source to discover correct syntax: `gbrain sources add <id> --path <path>` (positional, not `--id` flag).

**Issue flagged:** GBrain daemon unstartable from OpenCode bash tool — `gbrain serve` is MCP-stdio-only and exits on stdin close.
**Agent action:** Noted as manual step. CLI operations (sync, embed, search, doctor) all work without daemon.

**Previously (2026-07-04):**

**Issue flagged:** pdlf-project-library path (`C:\pdlf\pdlf-project-library`) nested inside pdlf source (`C:\pdlf`) — gbrain blocks overlapping source paths.
**Developer decision:** Skip pdlf-project-library as a separate gbrain source. Content under that path will be included in the `pdlf` source.

**Issue flagged:** MCP `gbrain_sources_add` does not store `local_path` — sources created via MCP show null paths.
**Agent action:** Switched to gbrain CLI for source registration. Required stopping `gbrain serve` to free PGLite lock.

**Issue flagged:** Three repos (`pdlf`, `scratch`, `scratch-project-library`) not git-initialized. GBrain requires git repos.
**Agent action:** Initialized with `git init` and placeholder commits (allowed structural bootstrap).
