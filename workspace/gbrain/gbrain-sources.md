# GBrain Source Inventory

## 1. Purpose

This file is a **practical inventory** of paths and files that GBrain may ingest, grouped by tier.

It is a companion to `gbrain-source-tiers.md`.

---

## 2. Tier A — Authoritative CS specification sources

Primary high-trust inputs:

- ADRs:
  - `C:\mybizz\project-library\adr\**\*.md`

- Anvil and Mybizz method docs:
  - `C:\mybizz\project-library\docs\am1-anvil-methods\**\*.md`
  - `C:\mybizz\project-library\docs\mm1-mybizz-methods\**\*.md`

- Reference Anvil file:
  - `C:\mybizz\project-library\docs\reference-anvil.yaml\anvil.yaml`

Ingestion guideline:
- Prefer well-structured sections and decisions, not whole-directory dumps.

---

## 3. Tier B — Canonical shared tracking

Devlog:

- `C:\mybizz\devlog\README.md`
- `C:\mybizz\devlog\index.md`
- Any additional structured devlog entries as they are added.

Todo:

- `C:\mybizz\todo\todo.md`
- `C:\mybizz\todo\todo-archive.md`

Ingestion guideline:
- Ingest completed entries or stable summaries, not half-finished drafts.

---

## 4. Tier C — Validated workflow outputs

Examples (paths may vary):

- Plan→Build handoff documents stored under `project-library\docs\` or dedicated handoff folders.
- Final GStack workflow summaries for major tasks.
- Confirmed implementation notes explicitly marked as final.

When you create such outputs, add them here with their actual paths, for example:

- `C:\mybizz\project-library\docs\handoffs\2026-05-03-feature-X.md`

---

## 5. Tier D — Contextual repo summaries

Potential future candidates:

- Summaries under a dedicated folder, for example:
  - `C:\mybizz\project-library\docs\code-summaries\**\*.md`

These should:
- clearly state which modules/paths they refer to,
- be regenerated when major refactors happen.

Do not treat them as authoritative; they are only context.

---

## 6. Tier E — External advisory sources

Examples (to be added as they are used):

- Vendor docs:
  - URLs or stored excerpts with clear version/date.
- External blog posts or articles, if captured as notes.

Each item should include:
- source (URL or citation),
- date,
- relevance.

---

## 7. Off-limits / special cases

Explicitly excluded unless promoted:

- Any `project-library\logs\devlog` legacy paths.
- Old `project-library\todo*.md` files superseded by the new root-level todo.
- Temporary notes folders, scratchpads, or “draft” directories.

If a file moves from “off-limits” to “in-scope”, update:
- this file,
- `gbrain-source-tiers.md`,
- and the ingestion plan if necessary.