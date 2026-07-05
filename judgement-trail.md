# Judgement Trail
Flagged issues, developer decisions, and approved deviations, extracted per task.

## 2026-07-04 — Project registration

**Issue flagged:** pdlf-project-library path (`C:\pdlf\pdlf-project-library`) nested inside pdlf source (`C:\pdlf`) — gbrain blocks overlapping source paths.
**Developer decision:** Skip pdlf-project-library as a separate gbrain source. Content under that path will be included in the `pdlf` source.

**Issue flagged:** MCP `gbrain_sources_add` does not store `local_path` — sources created via MCP show null paths.
**Agent action:** Switched to gbrain CLI for source registration. Required stopping `gbrain serve` to free PGLite lock.

**Issue flagged:** Three repos (`pdlf`, `scratch`, `scratch-project-library`) not git-initialized. GBrain requires git repos.
**Agent action:** Initialized with `git init` and placeholder commits (allowed structural bootstrap).
