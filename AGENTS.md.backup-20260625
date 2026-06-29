# AGENTS.md — project-library [PLAN] project

## Project purpose
This is the planning, documentation, design, and GStack workflow workspace for the
Mybizz Consulting Services application (mb-3-cs). It is not the live code repository.

Its purpose is to:
- produce validated documentation, specifications, and ADRs,
- house wireframes and design artefacts that guide Anvil UI development,
- store GStack pipeline outputs and planning reports,
- drive the GStack development workflow for mb-3-cs.

The live application code lives in `mb-3-cs`. Read from `mb-3-cs` for context when needed.
Do not treat this repo as the application implementation workspace.

## Hard rules (always apply)

1. **anvil.yaml protection**

   Hard rule: never ever write to anvil.yaml.

   - Treat anvil.yaml as Anvil-managed metadata/configuration.
   - Do not create, edit, reformat, regenerate, normalize, merge-edit, or patch it.
   - Do not include it in bulk refactors or automated file updates.
   - If a task appears to require changing anvil.yaml, stop and ask the user for explicit approval.
   - Assume Anvil is the authoritative writer for anvil.yaml.

## Workspace structure and file placement

| Folder | Purpose |
|---|---|
| `adr/` | Architecture Decision Records — one decision per file |
| `docs/` | Durable application documentation, specs, UI standards, DESIGN.md |
| `wireframes/` | HTML wireframes, mockups, and visual layout references |
| `gstack-outputs/` | GStack session outputs: reports, task lists, handoff notes |
| `workspace/gbrain/` | GBrain working files and configuration |
| `workspace/gstack/` | GStack working files and configuration |

## GStack artifact governance

When GStack or OpenCode produces output artefacts during a session:

- GStack-generated working outputs (office-hours reports, CEO review reports, ENG review reports,
  planning outputs, task lists, handoff notes) → save to `gstack-outputs/`
- Durable application documentation promoted from a GStack session → save to `docs/`
- Architecture decisions → save to `adr/`
- Visual references, wireframes, mockups → save to `wireframes/`
- DESIGN.md (produced by /design-consultation) → save to `docs/`

Do not place long-lived app specifications in `gstack-outputs/`.
Do not place transient planning reports in `docs/` unless explicitly promoted.
Do not place ADRs inside `docs/` or `gstack-outputs/`.

## Source of truth for this project

### Authoritative sources
The authoritative sources for the mb-3-cs application are:
- `project-library/adr/` — architectural decisions
- `project-library/docs/` — application documentation and specifications
- `project-library/wireframes/` — visual and layout references
- `mb-3-cs/` — live application code and anvil.yaml

### Fact rule
Do not use general training data or prior memory to assert project-specific facts.
If a fact is not supported by the allowed files above, treat it as inferred or missing, not as known truth.

### Exception handling
If the user explicitly instructs you to read a file outside the authoritative sources,
you may use it only for that request and must state that its content is advisory unless the user explicitly promotes it.

## Certainty levels (must be explicit)
When assessing how well something is defined, every statement must fall into one of:

- `Clearly defined:` an explicit statement in an authoritative file directly answers the point.
- `Partially defined:` files give some constraints but important details are missing or implied.
- `Inferred only:` connecting dots not explicitly stated; depends on interpretation.
- `Missing:` the authoritative files do not provide enough information to answer reliably.

Do not upgrade inferred material to clearly defined.

## Language restrictions

### Schema and alignment claims
Do not claim that the database schema "matches" or "fully implements" any document unless
you were explicitly asked to compare them in this session and produced a concrete comparison output.
Use cautious language: "broadly aligned with", "appears consistent with", "implements many of the patterns described in".

### Flow completeness claims
Do not use phrases like "fully specified" or "complete user flows" unless the files actually
contain step-by-step or screen-by-screen flow descriptions.

## Access model

### Plan agent
- May read and write the entire `project-library` repo (subject to user approval).
- May read `mb-3-cs` for implementation context.
- Must not make live implementation changes in `mb-3-cs`.

### Build agent
- May write explicitly requested non-live-code files in `project-library`.
- Must not write in `mb-3-cs`.

## Safety rules
- Do not create, edit, move, or delete files unless the user clearly asks for that operation.
- Never run or suggest destructive operations without a clear warning and explicit confirmation.
- Prefer proposing diffs or patches in chat over auto-applying changes.
- If a requested change affects many files, propose a small, reviewable subset first.
- Git operations are permission-only.
- Do not treat generated summaries as canonical unless validated by the user.
- Do not push, publish, sync, or export anything unless the user explicitly asks.

## Communication style
- Keep answers concise, structured, and practical.
- Prefer stepwise guidance over long explanations.
- Distinguish clearly between facts from the repo, inferences, and proposed revisions.
- Flag uncertainty, contradictions, or missing provenance explicitly.
- No emojis or side topics unless the user asks.