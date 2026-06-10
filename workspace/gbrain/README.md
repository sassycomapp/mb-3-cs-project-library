# GBrain (Workspace Docs)

Working notes and operating docs for the GBrain-compatible memory layer in the Mybizz CS workspace.

In this workspace, GBrain is an **advisory memory and recall layer**. It helps with continuity, structured recall, and source‑aware summarisation, but it does **not** override current repo files, canonical tracking, OpenCode rules, or direct user decisions.

---

## 1. Role of this folder

This folder contains the **design and operating docs** for how GBrain works in this workspace, plus the policy that governs the live GBrain instance (Supabase + OpenRouter).

Use these docs to answer:

- “What kinds of information should be remembered?”
- “Which sources are authoritative and which are only advisory?”
- “How should memory be ingested, refreshed, and recalled safely?”
- “How should GBrain interact with OpenCode and GStack without becoming a second source of truth?”
- “Which concrete paths are in scope for memory, and under what tiers?”

Operational wiring (Supabase project, embeddings, patch, smoke tests) is recorded in `gbrain-install-plan.md` and in `workspace\opencode-setup.md`. Ingestion and sync behavior are governed by `gbrain-ingestion-plan.md`, `gbrain-sources.md`, and `gbrain-sync-policy.md`.

---

## 2. Files in this folder

### 2.1 Core populated docs (policy)

- `gbrain-operating-model.md`  
  Defines the GBrain operating model for this workspace, including:

  - GBrain as an advisory memory layer, not a decision maker.
  - Required provenance and freshness expectations.
  - Conflict handling rules when memory disagrees with current repo content.
  - Authority order, with memory deliberately ranked below:
    1. user decisions,
    2. authoritative repo files and canonical tracking,
    3. OpenCode rules and validated current outputs,
    4. GBrain memory.

- `gbrain-source-tiers.md`  
  Defines the source hierarchy for ingestion and recall:

  - **Tier A** — Authoritative CS specification sources  
    (ADRs, baseline documents, critical AM/MM files, validated policy docs).

  - **Tier B** — Canonical shared tracking  
    (`devlog`, `todo`, approved status records).

  - **Tier C** — Validated workflow outputs  
    (approved handoffs, confirmed implementation summaries).

  - **Tier D** — Contextual summaries and supporting internal material.

  - **Tier E** — External advisory sources  
    (official vendor docs, research notes, external references).

  It also defines which sources are off‑limits or low‑trust by default.

- `gbrain-overview.md`  
  High‑level explanation of what GBrain is for in this workspace, typical use cases, and how it differs from simple notes or raw search.

- `gbrain-install-plan.md`  
  Records the concrete installation/configuration steps for this workspace:

  - GBrain CLI at `~/gbrain` (version `v0.18.2`),
  - Supabase `project-library` project and connection details,
  - OpenRouter embedding configuration and `OPENAI_API_BASE` patch,
  - doctor/smoke tests and upgrade considerations.

- `gbrain-ingestion-plan.md`  
  Defines what should be ingested first, in what order, and with what exclusions, starting with:

  - Tier A: ADRs and core spec docs,
  - Tier B: devlog/todo,
  - then selected Tier C outputs once validated.

- `gbrain-repo-strategy.md`  
  Describes how repositories and workspace docs are treated as memory sources, including:

  - which areas of `project-library` and `mb-3-cs` are in scope,
  - cross‑repo boundaries and authority rules,
  - good and bad patterns for code-related memory.

- `gbrain-sources.md`  
  Practical source inventory for potential ingestion targets, grouped by tier with actual paths (ADRs, AM/MM docs, devlog/todo, handoffs, summaries, external sources).

- `gbrain-sync-policy.md`  
  Defines when sync/refresh should happen, how staleness is handled, and how to prevent memory drift from current repo reality:

  - explicit triggers (ADR changes, major features, periodic audits),
  - freshness windows for advisory tiers,
  - conflict resolution (files beat memory, stale items are re-ingested or invalidated).

- `README.md` (this file)  
  Index and high‑level orientation for the GBrain docs in this workspace.

- `gbrain-log.md` (live)
Together, these documents describe **what GBrain may remember, from where, and under which rules**, and provide enough detail to keep the running GBrain instance safe and predictable.

---

## 3. Relationship to the rest of the workspace

### 3.1 OpenCode

OpenCode remains the main runtime environment. Its authority comes from:

- global rules in:
  - `C:\Users\dev-p\.config\opencode\AGENTS.md`
  - `C:\Users\dev-p\.config\opencode\opencode.json`

- project rules in:
  - `C:\_mb2-cs-app\project-library\AGENTS.md`
  - `C:\_mb2-cs-app\project-library\opencode.json`
  - `C:\_mb2-cs-app\mb-3-cs\AGENTS.md`
  - `C:\_mb2-cs-app\mb-3-cs\opencode.json`

GBrain must always operate **under** those rules and never bypass protected-file or permission policies.

### 3.2 GStack

GStack is the workflow/orchestration layer.  
GBrain is the advisory memory layer.

In simple terms:

- **GStack** helps decide the shape of work:
  - docs planning,
  - code plan/build flow,
  - status handover.

- **GBrain** helps preserve and retrieve context:
  - what was previously decided,
  - what sources matter,
  - what prior work is relevant.

GStack workflows may call on GBrain for recall via the configured Supabase + OpenRouter brain, but memory should always be checked against current files before acting.

Relevant GStack docs live in:

- `workspace\gstack\gstack-operating-model.md`
- `workspace\gstack\gstack-workflows.md`
- `workspace\gstack\gstack-handoff-contract.md`

### 3.3 Canonical repo and tracking sources

GBrain should defer to current source material, especially:

- `project-library` docs and ADRs,
- `mb-3-cs` current repo state,
- canonical shared tracking:
  - `C:\_mb2-cs-app\project-library\`
 
Memory is useful for recall and continuity, but the current source files remain authoritative. When conflicts appear, the policy in `gbrain-operating-model.md` and `gbrain-sync-policy.md` explains how to resolve them (files and ADRs win; stale memory is re‑ingested or invalidated).

---

## 4. What GBrain should and should not remember

### 4.1 Good candidates for memory

GBrain is most useful for preserving high‑value context such as:

- stable architectural decisions (ADRs),
- agreed workflow rules and policies,
- source relationships between docs and code,
- validated implementation history (devlog/todo + handoffs),
- canonical handoff summaries,
- trusted external reference pointers,
- recurring repo patterns and terminology.

These should be ingested at appropriate tiers (A–C) with clear provenance and freshness metadata.

### 4.2 Poor or dangerous memory candidates

GBrain should not casually ingest or elevate:

- scratch notes,
- speculative drafts,
- contradictory intermediate summaries,
- stale task fragments,
- deleted or superseded policy files,
- low‑trust external summaries,
- bulk unstructured code dumps.

If a source has unclear status, it should stay out until explicitly reviewed and, if appropriate, promoted via ADRs or devlog/todo.

---

## 5. Current status and next steps

Current status:

- GBrain is **installed and running** (Supabase engine + OpenRouter embeddings with a local client patch).
- The operating model and source‑tier guardrails are defined.
- Ingestion, source inventory, repo strategy, overview, and sync rules have first-pass content.
- GBrain is recognised and documented in:
  - `workspace\opencode-setup.md`
  - `workspace\workspace-description.md`
  - `workspace\workspace-docmap.md`

Next steps (operational focus):

1. Run initial, small-scope ingestion based on `gbrain-ingestion-plan.md` (Tier A and Tier B only).
2. Use GStack/OpenCode workflows to perform at least one end-to-end memory recall test and confirm that:
   - provenance is visible,
   - conflicts are resolved in favour of current files/ADRs.
3. Adjust ingestion and sync rules in this folder based on observed behaviour, keeping them aligned with the operating model and source tiers.

This folder should now be treated as the **active policy layer** for GBrain in this workspace, not just a planning surface.