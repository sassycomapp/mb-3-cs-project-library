---
title: "mb-3-cs-config"
doc-id: "mb-00ff9b58"
updated: "2026-11-12"
---
# mb-3-cs-config — state

| Field | Value |
|---|---|
| Issuer | MyBizz |
| Project | Mybizz Consulting Services (`mb-3-cs`) |
| Repository type | Documentation (the business's project library) |
| Status | PDLF Phase B closed; `domain-locked` passed 2026-11-12; run `app1` paused before Phase C (Steps 17–46 not built) |

## Repositories

The one table of every repository, local path, GitHub repository, GBrain source and hook coverage is `C:\mybizz\mybizz-os-docs\os-reference-docs\repo-organization.md` — this file never repeats it. This library is `sassycomapp/mb-3-cs-project-library`. The code repositories are `mb-3-cs-app1` (development workspace), `mb-3-cs-app3` and `mb-3-cs-app5`; `mb-3-cs-app2` is the release of App 1; `mb-3-cs-app4` exists only once created in the App 3 run.

## Agent setup

| Item | Value |
|---|---|
| `opencode.json` | present at the repository root |
| `AGENTS.md` | present at the repository root (published from `mb-3-cs-reference.md` at each close-out) |
| Role files | `docs/agents/` — `issue-tracker.md`, `triage-labels.md`, `domain.md` |
| Commands folder | none |

## Installed tools

| Item | Value |
|---|---|
| GBrain source | `mb-3-cs-project-library` (federated) |
| PDLF | runs from `C:\pdlf`; the ledger's `projects` row `mb-3-cs` exists |
| Test suite tools | `~/mybizz-test-tools` (versions per `ts-method` §9) |

## Key documents

| Document | State |
|---|---|
| `docs-local/GLOSSARY.md` | the business's glossary: 127 terms, written at Step 16, marked common/trade |
| `adr-local/` | five accepted ADRs — `adr-blank-client-template`, `adr-crm-package-replaces-marketing`, `adr-schema-scope-reduction`, `adr-common-feature-reads-trade-data`, `adr-plan-summary-display-text` (the last two from Step 16) |
| `docs-local/design/app1/` | `design-doc.md`, `test-plan.md`, `access-matrix.yaml`, `threat-model.md` — current through Step 16 |
| `docs-standard-local/authoritative-schema.md` | 71 tables, current (mirrors the 2026-11-06 amendment and F-37's `plan_summary`) |
| `docs-local/schema-migration-log.md` | prompts P1–P14 recorded; unapplied (App 1 holds 44 tables); applied by David through Resolve |
| `pdlf-outputs/` | `a01`–`b16` hold this run's outputs; `b13` holds no `app1` rows (the step is not used) |
| `output/mb-3-cs/` | the Archify Scope Cards, machine headers, candidate specs and run learnings |

## Open items

- AC-6 (Anvil hosting conditions) and AC-14 (SES production access) — David's business actions, both before the first paying client.
- The three proving-run findings on the b16 receipt — PDLF improvements, not app work.
- The other-document alignments David makes outside the run: "delivery mode" → meeting type; "staff"/"user" wording; "newsletter" → broadcast; three schema descriptions.
- The schema migration prompts are recorded and unapplied; David applies them through Resolve when the build needs them.
