# GBrain Log

Audit trail of GBrain operations, migrations, and notable events.

---

## 2026-07-06 — Engine migration complete + config optimization

**Engine:** PGLite (embedded PostgreSQL 17)
**Version:** v0.42.44.0 (upgraded from v0.18.2)
**Change:** Migrated from Supabase to PGLite. Data directory at `~/.gbrain/brain.pglite/` (323 MB).

**Sources registered (10):**
- `project-library` — 318 pages (mb-3-cs docs)
- `mb-3-cs` — 165 pages (mb-3-cs code)
- `mb4ecom` / `mb4ecom-project-library` — 3 pages combined
- `mb5pdlf` / `mb5pdlf-project-library` — 51 pages combined
- `pdlf` — 18 pages
- `scratch` / `scratch-project-library` — 2 pages combined
- `default` — 143 pages (federated)

**Total:** 700 pages, 2901 chunks, all embedded.

**Sync:** All sources synced successfully. Edges extracted (692 pages, 0 links, 0 timeline entries).

**Reranker:** ZeroEntropy (`zerank-2`) enabled. API key configured via `ZEROENTROPY_API_KEY`.

**Resolution:** 12 historical sync failures (from `default` federated source, 2026-07-04) auto-resolved. All sources healthy.

**GStack detection:** `gbrain_local_status: ok`

**Note:** PGLite is single-writer. `gbrain serve` must be stopped before running CLI sync commands.
