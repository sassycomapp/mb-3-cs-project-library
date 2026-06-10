# GBrain Repo Strategy

## 1. Purpose

This document describes **how GBrain interacts with the repos and workspace structure** in the Mybizz CS setup.

---

## 2. Repos and roots

Workspace structure (Windows):

- `C:\_mb2-cs-app\project-library` — docs/ADR/refactor planning repo.
- `C:\_mb2-cs-app\mb-3-cs` — live / production-adjacent code repo.
- Shared tracking:
  - To be confirmed

GBrain’s primary focus:

- authoritative documents and ADRs in `project-library`,
- canonical tracking in project-library,
- selected validated outputs referencing `mb-3-cs`.

---

## 3. In-scope vs out-of-scope areas

### 3.1 In-scope (high priority)

- ADRs and architecture docs:
  - `project-library\adr\`
  - specific architecture/specification subfolders under `project-library\docs\...`

- Canonical tracking:
  - `project-library\

- Validated workflow outputs:
  - handoffs and summaries explicitly marked as final.

### 3.2 In-scope (lower priority, advisory)

- Summaries or notes that:
  - are clearly tied to specific ADRs or code modules,
  - have been validated in a Plan or Build session.

These should be ingested as Tier C or Tier D with conservative freshness rules.

### 3.3 Out-of-scope (by default)

- Raw code files in `mb-3-cs`:
  - GBrain should not wholesale ingest all code; summaries are preferable.
- Scratchpads, unvalidated notes, and legacy directories where status is unclear.
- Intermediate or abandoned branches of design.

These can be pulled into scope only after explicit review and promotion.

---

## 4. Cross-repo considerations

- **Cross-repo reads**:
  - GBrain recall may reference both repos as long as:
    - the underlying files are allowed to be read by OpenCode,
    - any cross-repo relationships are explained (e.g., “ADR 012 describes `module_x` in mb-3-cs”).

- **Repo authority**:
  - For architecture and policy:
    - ADRs and published docs in `project-library` win.
  - For current implementation details:
    - code and config in `mb-3-cs` win.

GBrain should help link these two, not blur them.

---

## 5. Practical examples

Good uses of GBrain in repo context:

- Remembering “ADR 012 + devlog entry 2026-03-01” as the decision + implementation point for a feature.
- Remembering which `mb-3-cs` modules implement a specific ADR.
- Remembering which TODO items were created because of a particular repo change.

Bad uses:

- Treating a code summary from six months ago as authoritative without checking current files.
- Ingesting an entire code tree without structure or metadata.

---

## 6. Future adjustments

Adjust this strategy when:

- the repo structure changes,
- new long-lived repos are added,
- or you decide to bring more areas in-scope for memory.

Any change here must remain consistent with:
- `gbrain-operating-model.md`,
- `gbrain-source-tiers.md`,
- and `gbrain-ingestion-plan.md`.