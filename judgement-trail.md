# Judgement Trail
Flagged issues, developer decisions, and approved deviations, extracted per task.

## 2026-07-06 — ADR reorganization

**Issue flagged:** Initial classification was wrong — treated many ADRs as local when the architecture, technology, and standards are shared across all Mybizz apps. Only the business context changes per app.
**Developer decision:** Reclassified: 39 global, 1 local. Key corrections: lead-capture, CRM, payment gateways, invoices, onboarding, free-trial-abandoned are all global (company-wide decisions). Tiers-model is global (Mybizz-centric). Schema-scope-reduction is the sole local ADR (CS-specific table set).
**Deletions:** 9 (multi-vertical — historical, no longer relevant), 12 (superseded build sequence), 13 (superseded by 16), 25 (onboarding finality, fully reversed by 26). ADR-37 kept as global despite partial supersession (still contains valid M3/CSS customization approach).

## 2026-07-06 — VS Code remnants cleanup

**Issue flagged:** `rm -rf` via WSL on `/mnt/c/Users/dev-p/AppData/Roaming/Code/` failed with "Permission denied" on 15 Git `askpass` scripts — Windows filesystem permission locks prevented deletion even after `chmod +w`.
**Developer decision:** Used `cmd.exe /c rmdir /s /q` — Windows-native deletion that bypasses the WSL/Windows permission boundary. Successful on first attempt.
**Scope note:** No `.code` directory found anywhere on the system; user can safely ignore that concern.

## 2026-07-06 — GStack + GBrain config optimization

**Issue flagged:** GBrain CLI commands (`sources list`, `sync`, `doctor`, `stats`) all failed silently — `broken-config` status.
**Root cause:** PGLite is single-writer. `gbrain serve` owned the exclusive database connection. CLI commands couldn't open the database.
**Developer decision:** Established the PGLite operational pattern: stop serve → run CLI commands → restart serve. Documented in gbrain-config-reference.md §7.

**Issue flagged:** GBrain at v0.42.44.0, PGLite engine. All workspace docs said v0.18.2, Supabase engine, OpenRouter proxy, local `embedding.ts` patch.
**Developer decision:** Updated 6 files to reflect current reality: `gbrain-install-plan.md`, `gbrain-repo-strategy.md`, `gbrain-log.md`, `gbrain-reference.md`, `gbrain-operating-guide.md`, `opencode-setup.md`. Removed stale references to OpenRouter patch and Supabase project ID.

**Issue flagged:** ZeroEntropy reranker failing (230 audit log entries over 3 weeks) — `ZEROENTROPY_API_KEY` not set.
**Developer decision:** API key provided by developer. Configured in `~/.bashrc` (persistent) and OpenCode MCP environment (for serve process). Reranker enabled via `gbrain config set search.reranker.enabled true`.

**Issue flagged:** 12 historical sync failures from `default` federated source — `createVersion failed: page not found`.
**Developer decision:** Auto-resolved. All 10 isolated sources synced successfully. Federation picks up current pages. Failures were from pre-migration path changes.

**Issue flagged:** 2 oversized pages (>50KB: `docs/mybizz-application-overview`, `docs/design`).
**Developer decision:** Flagged for future maintenance. Pages are embedded and searchable. Chunking would improve retrieval quality but requires re-indexing.

**Issue flagged:** `mb5pdlf/pdlf-state.md` had old pre-migration paths (`/mnt/c/_project-mb5pdlf/`).
**Developer decision:** Fixed to current paths (`/mnt/c/projects/mb5pdlf/`). Orphaned `~/.gstack/projects/unknown/` state removed.

## 2026-07-06 — OpenCode config optimization

**Issue flagged:** `mb-3-cs/AGENTS.md` directly violated global "Plan never writes" rule — granted Plan write access to `prompts/`, `docs/`, `tests/`.
**Developer decision:** Fixed. Plan write permissions removed. Build agent now covers all writes with explicit-approval gates on `prompts/`, `docs/`, `tests/`, `theme/`.

**Issue flagged:** Global `opencode.jsonc` had hardcoded model block (`xiaomi/mimo-v2.5-pro`). User wants model selected from UI picker.
**Developer decision:** Model block removed entirely. Provider block retained (OpenRouter API key needed for auth — env var approach previously unreliable).

**Issue flagged:** Global `external_directory` only covered `C:\mybizz\**` — didn't cover `C:\projects\**` or `C:\pdlf\**`.
**Developer decision:** Added both. All projects now accessible from any workspace without per-project config changes.

**Issue flagged:** 4 agent files missing task-completion reporting, devlog/judgement-trail awareness, and environment (Web UI only).
**Developer decision:** Added `## Task completion` and `## Environment` sections to all 4 agent files.

**Issue flagged:** 52 command files in `.opencode/commands/` — 3 duplicates, 2 broken headers, 5 formatting outliers.
**Developer decision:** Removed duplicates (`gstack-context-save.md`, `gstack-learn.md`, `gstack-upgrade.md`). Fixed headers (`build.md`, `review.md`). Standardized 5 outliers.

**Issue flagged:** mb-3-cs project library agent files (plan/build) now part of PDLF transition. All projects are PDLF projects.
**Developer decision:** Project configs (`opencode.json`) created for mb4ecom and mb5pdlf for proper OpenCode operation. PDLF-specific config handled in later pass.

**Issue flagged:** API key in plain text in `opencode.jsonc` and `opencode-10-config-files.md`.
**Developer decision:** Key redacted from workspace doc. Kept in config file (env var approach was unreliable — this is a known, deliberate trade-off).

**Issue flagged:** Three workspace reference docs had stale paths, Desktop bridge text, model references, and duplicated content blocks.
**Developer decision:** All three rewritten: `workspace-docmap.md`, `workspace-description.md`, `opencode-10-config-files.md`.

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
