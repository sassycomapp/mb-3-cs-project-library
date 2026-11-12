---
title: "README"
doc-id: "mb-4aef6c9c"
updated: "2026-11-12"
---
# Mybizz Consulting Services — Project Library

The project library of Mybizz Consulting Services (`mb-3-cs`): every document, decision and PDLF record for all of the business's apps. It is the business's GBrain source `mb-3-cs-project-library`. It holds no application code; the code is in the app folders beside it.

The repository is public. No secret, key or client data is ever committed to it.

## Folders

| Folder | What it holds |
|---|---|
| `pre-project\` | The three pre-project files David fills in for a new application with no context. |
| `docs-local\` | The business's own working documents: the glossary, the design folder of every run, strategy, releases and the schema migration log. |
| `docs-standard-local\` | The standard documents every Mybizz project keeps, modelled on `project-library-global\docs-standard-global\`. |
| `adr-local\` | Decisions that belong to this business only. |
| `policy-local\` | Policies that belong to this business only. |
| `specifications-local\` | Specifications that belong to this business only, such as a feature's specification. |
| `standard-operating-procedures-local\` | Procedures that belong to this business only. |
| `guides-local\` | The client-facing guides and the video scripts. |
| `legal-local\` | The default clauses the clause builder offers the client, and Mybizz's agreements with the client. |
| `assets-local\` | The source files of the brand: logos, images and fonts. |
| `prompts-local\` | Prompts that belong to this business only. |
| `templates-local\` | Templates that belong to this business only. |
| `ui\` | The screen requirements, one folder per Anvil package and one per form. |
| `docs\` | Folders owned by skills: `docs\agents\` for the Matt Pocock skills. |
| `pdlf-tickets\` | The feature tickets, one markdown file each. |
| `pdlf-outputs\` | One folder per PDLF step (`a01` to `f46`): the Step Receipt, the test evidence and a copy of every output. |
| `reports-local\` | Reports from skills run outside a PDLF step. |
| `reference-platform-local\` | A reference platform's source and research material. Never committed and never indexed by GBrain. |
| `.out-of-scope\` | Ideas the triage skill set aside as out of scope. |
| `V2\` | What has been deferred to version 2. |
| `wip\` | The project's to-do list and work not yet in its permanent place. |

## Files at the root

| File | What it is |
|---|---|
| `AGENTS.md` | The rules every agent follows in this library. |
| `opencode.json` | OpenCode's permissions for this library. |
| `DESIGN.md` | The application's design system, written at Step 20 by GStack `/design-consultation`, in its own place. Not present until then. |

## How this library is worked on

Agents follow `AGENTS.md`. PDLF runs from `C:\pdlf` and writes here only what belongs to this business. Only David commits, with `commit-all`.

## Required reading

- [[[mb-3-cs-project-library:mb-3-cs-config]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/mb-3-cs-config.md) — the state of the project and this repository
- [[[mb-3-cs-project-library:agents]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/AGENTS.md) — the rules every agent follows here
- [[[mb-3-cs-project-library:mb-3-cs-explainer]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/mb-3-cs-explainer.md) — what the library is and why it is built this way
