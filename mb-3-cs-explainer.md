---
title: "mb-3-cs-explainer"
doc-id: "mb-a9484d68"
updated: "2026-11-12"
---
# mb-3-cs-explainer

## What it is

The project library of Mybizz Consulting Services: every document, decision and PDLF record for the business's apps, in one public repository. It holds no application code — the code lives in the app folders beside it, and this library holds everything that is true of the apps rather than inside them: the glossary, the decisions, each run's design folder, the standard documents, and one folder per PDLF step of outputs.

## What it can do

- Fix the business's vocabulary in one place (`docs-local/GLOSSARY.md`), so a term means the same thing in every later step and every app.
- Hold the business's own decisions (`adr-local/`) beside the global ones it inherits by reference.
- Keep each PDLF run's working documents (design doc, test plan, access matrix, threat model) in `docs-local/design/{run}/`, and every step's evidence in `pdlf-outputs/{step}/`.
- Hold the standard documents every Mybizz project keeps (`docs-standard-local/`), the tickets, the screen requirements, the sessions record, and the Archify Scope Cards and run learnings.

## Suitability

Best for anything that is true of this business's apps: vocabulary, decisions, designs, run records, client-facing guides. Not for application code (the app folders), not for standards shared by every Mybizz application (`project-library-global` holds those; this library references them, never copies them), and not for PDLF's own machinery (`C:\pdlf`).

## Why it is built this way

- **One glossary**, because all the business's apps serve one domain — App 1, App 5 and every feature share the same words, so a later run extends the glossary and never rewrites it.
- **Code and documents are never mixed**, because a document that lives with code rots with it and a public repository must never carry a secret; the split is enforced by the folders and by `AGENTS.md`.
- **Public by design**, because nothing secret belongs here — keys and credentials live in the apps' Vault, never in a document.
- **PDLF writes here only what belongs to this business**, and the developer commits — agents never do — so the record of what happened is always the developer's own.

## What has to happen

David commits with `commit-all`, which also updates GBrain and pushes. Agents follow `AGENTS.md` and never commit, push or write secrets. PDLF runs from `C:\pdlf` and ends every step with a receipt here. The glossary is extended as the business learns, never rewritten. When a decision changes, every document that states the fact is updated in the same task.
