# GBrain Source Tiers

## Purpose
This document defines the approved source tiers for GBrain ingestion in the Mybizz CS workspace.

The source tier determines how much trust may be placed in recalled information.

## Tier A — Authoritative CS specification sources
These are the highest-trust GBrain inputs for the current CS application.

Approved Tier A sources are:
- `C:\_mb2-cs-app\project-library\adr\`
- `C:\_mb2-cs-app\project-library\docs\am1-anvil-methods\`
- `C:\_mb2-cs-app\project-library\docs\mm1-mybizz-methods\`
- `C:\_mb2-cs-app\project-library\docs\reference-anvil.yaml\anvil.yaml`

Tier A recall may be treated as high-confidence advisory memory, but the underlying files still remain authoritative.

## Tier B — Canonical shared workflow tracking
These are authoritative tracking sources for shared work state.

Approved Tier B sources are:
- `C:\_mb2-cs-app\devlog\README.md`
- `C:\_mb2-cs-app\devlog\index.md`
- `C:\_mb2-cs-app\todo\todo.md`
- `C:\_mb2-cs-app\todo\todo-archive.md`

Tier B recall may be used for work history, task state, and handoff context.

## Tier C — Validated workflow outputs
These are validated outputs produced by approved workflows.

Examples:
- final Plan handoffs,
- final Build handoffs,
- final GStack workflow summaries,
- approved implementation support artefacts.

Tier C is advisory but useful for operational continuity.

## Tier D — Contextual repo summaries
These are lower-trust operational summaries derived from repo content.

Examples:
- codebase summaries,
- directory-level notes,
- repo metadata snapshots.

Tier D must never outrank current repo files.

## Tier E — External advisory sources
These are non-repo, non-canonical external references.

Examples:
- vendor documentation,
- web research notes,
- external technical references.

Tier E is advisory only and must always carry freshness/version caution.

## Off-limits for ingestion by default
Do not ingest by default:
- deleted or archive content,
- speculative notes,
- scratchpad content,
- incomplete or unvalidated summaries,
- outdated legacy material unless explicitly approved.

## Retrieval rule
Every recalled memory item must state its source tier.
If multiple memories conflict, the higher-trust current source wins.