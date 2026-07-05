# GBrain Install Plan (Mybizz CS)

## 1. Purpose

This document records **how GBrain is installed and configured** for this workspace and what must be preserved on upgrades.

It is not a general tutorial; it is a concrete record of the local setup.

---

## 2. Components

- **GBrain CLI**
  - Source: installed globally via bun (`~/.bun/bin/gbrain`).
  - Version at current: `v0.42.44.0`.

- **Database engine**
  - PGLite (embedded PostgreSQL 17)
  - Data directory: `~/.gbrain/brain.pglite/` (323 MB on disk)
  - **Note:** PGLite is single-writer. Stop `gbrain serve` before running CLI commands like `gbrain sync` or `gbrain sources list`.

- **Embeddings**
  - Model: `text-embedding-3-large`
  - Provider: OpenAI native (no OpenRouter proxy needed in v0.42+)
  - API key: `OPENAI_API_KEY` environment variable.

- **Reranker**
  - ZeroEntropy (`zerank-2`), configured via `gbrain config set search.reranker.enabled true`
  - API key: `ZEROENTROPY_API_KEY` environment variable

---

## 3. Install / setup steps (completed)

1. **Install GBrain CLI in WSL**
   - Clone the GBrain repository into `~/gbrain`.
   - Install dependencies (Node, pnpm/npm, etc. as required).
   - Build or install the CLI per the GBrain README.

2. **Create Supabase project for this workspace**
   - Create a project named `project-library`.
   - Obtain the connection/pooler string.
   - Store it securely for use by GBrain.

3. **Configure embeddings via OpenRouter**
   - Create an OpenRouter API key.
   - Set environment variables (for the shell/user that runs GBrain):
     - `OPENROUTER_API_KEY`
     - `OPENAI_API_KEY` (if required by client)
     - `OPENAI_API_BASE=https://openrouter.ai/api/v1`

4. **Patch GBrain embedding client for OpenRouter**
   - File: `~/gbrain/src/core/embedding.ts`
   - Change: use `OPENAI_API_BASE` instead of hardcoding `https://api.openai.com`.
   - Result: GBrain’s embedding calls go to OpenRouter’s API while still using an OpenAI-compatible shape.

5. **Run initial doctor / smoke tests**
   - `gbrain doctor` to verify DB + embedding reachability.
   - A small write + embed + search test to confirm end-to-end behavior.

---

## 4. Upgrade / maintenance considerations

- The local patch in `embedding.ts` is **not** upstream; upgrades may overwrite it.
  - Before upgrading GBrain:
    - Save the exact diff of the patch.
    - Re-apply or replace the patch after upgrade.
    - Re-run `gbrain doctor` and a smoke test.

- Supabase project:
  - Ensure long-term retention and backups are configured on the Supabase side.
  - Rotate keys and update environment variables as needed.

---

## 5. Future changes

If any of the following change, update this file:

- Supabase project name/ID or connection string.
- Embedding model or provider.
- Any new local patches to GBrain (paths, purpose, and rationale).
- New validation/doctor steps you rely on for routine checks.