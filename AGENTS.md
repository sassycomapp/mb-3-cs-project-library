---
title: "AGENTS"
doc-id: "mb-2d2c3f8f"
updated: "2026-11-10"
---
# AGENTS.md — mb-3-cs project library

This file adds the rules for this business's project library. It sits under the global `AGENTS.md`, which applies in full.

## What this library is

The project library of Mybizz Consulting Services (`mb-3-cs`). It holds every document, decision and PDLF record for the business's apps. It holds no application code. It is a public repository.

The business has five apps (`spec-five-app-architecture-model.md` in `project-library-global`):

- `mb-3-cs-app1`, App 1, the development workspace. All feature work happens here.
- `mb-3-cs-app2`, App 2, `master_template`, a clone of App 1's approved code.
- `mb-3-cs-app3`, App 3, `blank_client_template`, the client app every client instance runs.
- `mb-3-cs-app4`, the release image definition. A GitHub repository only, never an Anvil app.
- `mb-3-cs-app5`, App 5, the management app.
- App 4, each client's own instance: a container running the release image, with its own database, on DigitalOcean.

## Rules

1. **Never write `anvil.yaml`.** Anvil manages it. `opencode.json` enforces this.
2. **Code and documents are never mixed.** Documents go here. Code goes in the app folders, and only during building, with David's permission each time.
3. **No application is ever multi-tenant.** One app and one database per client, always.
4. **Never add forms or server modules to App 3.** Every client instance runs App 3; a change for one client is made in App 1 and released through App 2.
5. **The agent never commits or pushes.** Only David commits, with `commit-all`. Every PDLF step ends by asking David to run `/mb-close-out`.
6. **Search, read, apply.** Search GBrain first, then read the actual file each result points to before applying it.
7. **Plan reads, Build writes.** Plan mode never writes. Build mode writes only with David's approval.
8. **Nothing secret is committed.** This repository is public. No key, password, token or client data is ever written here.
9. **Every document goes in its folder.** Each folder's `README.md` says what belongs in it. A document with no folder goes in `wip\` until it has one.
10. **A skill's output goes to the skill's own place.** Inside a PDLF step, a copy also goes to `pdlf-outputs\{step}\`. Outside a PDLF step, a skill's report goes to `reports-local\{family}\`.
11. **Reference material is never committed.** A reference platform's source and reference material stay in `reference-platform-local\`, which git and GBrain ignore.

## Where things go

| What | Where |
|---|---|
| The domain glossary | `docs-local\GLOSSARY.md` |
| A run's design doc, test plan, access matrix and threat model | `docs-local\design\{run}\`, where `{run}` is `app{n}` or the feature's short name |
| Research on a reference platform | `docs-local\reference-platform-research.md` |
| Standard documents | `docs-standard-local\` |
| This business's decisions | `adr-local\adr-{kebab-case-name}.md` |
| Feature tickets | `pdlf-tickets\{run}\{ticket-slug}.md` |
| Everything a PDLF step produces | `pdlf-outputs\{step}\`, named `mb-3-cs-{step}-{run}-{name}` |
| Reports from skills outside PDLF | `reports-local\{family}\{skill}-{topic}-{yyyy-mm-dd}.md` |
| The design system | `DESIGN.md` |
| Wireframes | `wireframe\` |
| Screens | `ui\{package}\{FormName}\` |

## Agent skills

### Issue tracker

Tickets are local markdown files in `pdlf-tickets\`. See `docs\agents\issue-tracker.md`.

### Triage labels

The five standard labels: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs\agents\triage-labels.md`.

### Domain docs

Single context: the glossary is `docs-local\GLOSSARY.md` and the decisions are in `adr-local\`. See `docs\agents\domain.md`.
