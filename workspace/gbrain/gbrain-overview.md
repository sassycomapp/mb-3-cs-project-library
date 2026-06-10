# GBrain Overview

## 1. What GBrain is in this workspace

GBrain is the **advisory memory and recall layer** for the Mybizz CS workspace.

It exists to:
- remember validated decisions and workflows across sessions,
- surface relevant past context quickly,
- help reconstruct “why” and “how” something was done,
- support GStack workflows with historical context.

GBrain does **not**:
- define project truth,
- override current repo files or ADRs,
- bypass OpenCode rules or protected-file controls,
- apply changes automatically.

Authoritative order always remains:
1. User decision in the current session.
2. Authoritative repo files and canonical tracking.
3. OpenCode rules and validated current outputs.
4. GBrain memory (advisory only).

---

## 2. Main use cases

Typical uses for GBrain in this workspace:

- **Recall ADR decisions**  
  “What did we decide about X in the ADRs, and which files implement that?”

- **Recall workflow conventions**  
  “What is the agreed procedure for backups, timestamps, or protected files?”

- **Recall past work segments**  
  “What did we do the last time we touched module Y or feature Z?”

- **Bridge Plan ↔ Build sessions**  
  Support GStack handoffs by remembering key decisions, risks, and validation notes.

---

## 3. Boundaries and safety rules

- GBrain may **suggest** context; OpenCode agents must still read the actual files.
- When GBrain recall conflicts with:
  - ADRs,
  - repo files,
  those sources win, and conflicting memory should be treated as stale.

- No ingestion from:
  - scratchpads,
  - speculative drafts,
  - deleted or superseded material,
  unless explicitly promoted after review.

These rules are enforced conceptually by:
- the operating model (`gbrain-operating-model.md`),
- the source tiers (`gbrain-source-tiers.md`),
- and the ingestion/sync policies (this folder).

---

## 4. Relationship to OpenCode and GStack

- **OpenCode** is the runtime and permission system.
- **GStack** orchestrates workflows and handoffs.
- **GBrain** stores and recalls advisory context for those workflows.

A typical pattern:
- GStack workflow runs in OpenCode.
- Validated outputs (for example, a Plan→Build handoff + ADR updates) are ingested into GBrain at an appropriate tier.
- Future sessions can ask GStack/OpenCode workflows to recall that context from GBrain, but always re-check against current files.

---

## 5. Current status (high level)

- Operating model and source tiers are defined.
- A live GBrain instance is running (Supabase + OpenRouter) and has passed a smoke test.
- Ingestion and sync are governed by the companion documents in this folder; automation should not be added until those rules are followed.