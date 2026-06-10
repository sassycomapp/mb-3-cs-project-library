# GBrain Ingestion Plan

## 1. Purpose

This document defines **what may be ingested into GBrain**, in what order, and under which conditions.

The aim is to start small with high-trust sources and expand only when behavior is well understood.

---

## 2. Initial ingestion scope

### 2.1 Phase 1 — High-trust only

Start with:

- **Tier A** — Authoritative CS specification sources:
  - `C:\_mb2-cs-app\project-library\adr\`
  - `C:\_mb2-cs-app\project-library\docs\`
  - `C:\_mb2-cs-app\project-library\developer-rules`

- **Tier B** — Canonical tracking:
  - `C:\_mb2-cs-app\project-library\implementation`

Rules for Phase 1:
- Ingest only content that is:
  - stable,
  - validated,
  - clearly within current architecture/policy.
- Do **not** ingest legacy or partially-migrated content.

### 2.2 Phase 2 — Validated workflow outputs
- **Tier C** — Validated workflow outputs:
  - `C:\_mb2-cs-app\project-library\gstack-outputs`
  

Once Phase 1 behavior is understood:

- Ingest specific, named outputs from:
  - Plan→Build handoffs,
  - completed GStack workflows,
  - ADR decisions that reference new implementation work.

Conditions:
- Only ingest Tier C outputs **after** they are:
  - accepted by you,
  - reflected in ADRs and/or repo changes.

### 2.3 Later phases — Tier D and Tier E (optional)

- Tier D (contextual summaries) may be ingested cautiously once:
  - tooling for generating summaries is stable,
  - you have a policy for marking them as lower-trust.
- Tier E (external docs) should be ingested sparingly and always carry:
  - version or date,
  - vendor/source information.

---

## 3. Ingestion triggers

For now, ingestion is **explicit and manual** (or via explicit workflows), not automatic.

Good triggers:

- After writing a new ADR, ingest that ADR (Tier A).
- After closing a feature and logging it in the devlog, ingest:
  - the relevant devlog entry (Tier B),
  - any associated ADR (Tier A),
  - and optionally a short validated summary (Tier C).
- After a successful Plan→Build cycle with a handoff document, ingest the final handoff (Tier C).

---

## 4. Ingestion metadata requirements

Each ingested item must carry at least:

- source path,
- source tier (A–E),
- authority label (e.g. “Authoritative ADR”, “Canonical tracking”, “Advisory summary”),
- ingested timestamp,
- freshness / TTL policy (for example “re-check every 30 days”),
- source version marker (hash, commit, or ADR ID where possible).

If metadata cannot be attached or persisted, the item should not be ingested until this can be fixed.

---

## 5. Exclusions

Do **not** ingest:

- scratchpad files,
- speculative or unvalidated drafts,
- deleted/archived legacy docs,
- conflicting or ambiguous summaries,
- files with unclear status.

These may be considered later on a case-by-case basis once clearly labelled and curated.

---

## 6. Review and adjustment

This plan should be reviewed after:

- an initial ingestion of Tier A and Tier B,
- at least one successful end-to-end recall workflow.

Adjustments should be made only after observing actual behavior, and changes must remain consistent with:
- `gbrain-operating-model.md`,
- `gbrain-source-tiers.md`,
- `gbrain-sync-policy.md`.