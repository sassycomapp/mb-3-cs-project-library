# GBrain Sync and Freshness Policy

## 1. Purpose

This document defines **when and how GBrain memory is refreshed, re-embedded, or invalidated** so that it does not drift away from current repo reality.

---

## 2. Freshness rules

### 2.1 Always-fresh sources (Tier A and Tier B)

For Tier A and Tier B:

- Treat repo files as the real-time source of truth.
- Memory is **allowed to lag**, but:
  - if a file changes significantly,
  - or if an ADR is superseded,
  memory connected to that file should be:
  - re-ingested, or
  - marked as stale/invalid.

Suggested policy:

- For ADRs:
  - Re-check or re-ingest when:
    - ADR is edited,
    - ADR status changes (e.g., “Accepted” → “Superseded”).
- For devlog and todo:
  - Re-ingest only finalized entries (for example, devlog entries that describe completed work).

### 2.2 Advisory sources (Tier C–E)

For Tier C–E:

- Set a default review window (for example, 60–90 days).
- After the window:
  - either re-validate and refresh, or
  - mark the memory as “stale” and lower its priority in recall.

---

## 3. Sync events and triggers

Avoid continuous automatic sync. Use **explicit triggers**:

- **After structural changes**:
  - major refactor,
  - schema or architecture updates recorded in ADRs,
  - big shifts in workflow policy.

- **After closing significant work items**:
  - feature completion that leads to new ADRs and devlog entries,
  - large Plan→Build handoffs.

- **Periodic maintenance**:
  - occasional “memory audit” session, during which:
    - you sample recalled items,
    - mark obviously stale items for re-ingestion or invalidation.

---

## 4. Conflict handling

When a conflict is detected between GBrain memory and current source files:

1. Treat the current source (ADR, repo file, devlog, todo) as authoritative.
2. Mark the conflicting memory as:
   - “stale” or “invalidated”, and
   - if possible, attach a pointer to the new authoritative source.
3. Prefer re-ingestion from the corrected source if the information is still valuable.

This should be reflected in how recall results are ranked and presented.

---

## 5. Operational notes

- Sync / refresh must **never** run in a way that can:
  - overwrite repo files,
  - auto-apply code changes,
  - bypass OpenCode’s protections.

- Any automation or scripts that operate on GBrain should:
  - log what they ingest or invalidate,
  - keep a small audit trail (for example, in devlog or a dedicated GBrain log).

---

## 6. Review schedule

Revisit this policy whenever:

- you change the set of ingested sources,
- you notice repeated stale/incorrect recall,
- you upgrade GBrain or change embedding providers.

Adjust rules incrementally and keep them aligned with:
- the operating model,
- the source tiers,
- and actual working practice.