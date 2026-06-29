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
| `screens/` | Screen HTML mockups and their compliance checklists |
| `wip/` | Active work-in-progress: current plans, prompts, and files being prepared for delivery |
| `gstack-outputs/` | GStack-produced reports, reviews, task lists, and artifacts |
| `opencode-outputs/` | OpenCode-produced reports, reviews, and artifacts |
| `matt-skills-output/` | Matt Pocock skill outputs (copies — originals may also go to %TEMP%) |
| `workspace/gbrain/` | GBrain working files and configuration |
| `workspace/gstack/` | GStack working files and configuration |

## Artifact governance

**Principle:** Every session-produced artifact gets saved. The destination depends on
who produced it and what kind of file it is. Never ask whether to save — always save.
Confirm the save location in the on-screen response.

### Routing by producer

| Producer | Destination |
|---|---|
| GStack (any gstack-* skill, including OpenClaw variants) | `gstack-outputs/` |
| OpenCode (direct, not via a skill) | `opencode-outputs/` |
| Matt Pocock skills | `matt-skills-output/` (see per-skill rules below) |

### Routing by file type (overrides producer routing)

These file types have fixed homes regardless of producer:

| File type | Destination |
|---|---|
| Durable application documentation | `docs/` |
| Architecture Decision Records | `adr/` |
| Wireframes and visual mockups | `wireframes/` |
| Screen HTML mockups | `screens/` |
| Wireframe/screen compliance reports | Same folder as the wireframe or screen it belongs to |
| DESIGN.md | `docs/` |

### Exclusions

This routing applies to reports, reviews, analyses, task lists, and similar
session-produced artifacts. It does **not** apply to:
- Durable docs (`docs/`), ADRs (`adr/`), wireframes (`wireframes/`), screens (`screens/`)
- Files in `wip/` (those have their own lifecycle)
- GBrain working files (`workspace/gbrain/`)
- GStack working files (`workspace/gstack/`)

### Confirmation rule

The on-screen response must confirm the save location explicitly:
`"Saved to: gstack-outputs/<filename>."` or `"Saved to: opencode-outputs/<filename>."`
If the save fails, state why — do not silently report success.

### Promotion rules

- Long-lived app specifications must not stay in `gstack-outputs/` — promote to `docs/`
- Transient planning reports must not go in `docs/` unless explicitly promoted
- ADRs must not go in `docs/` or `gstack-outputs/` — they belong in `adr/`

## GStack binary path override
When any GStack skill needs to call gstack-slug, gstack-config, or any other
gstack-* binary, always use /mnt/c/_mb2-cs-app/gstack/bin/<binary-name> directly.
Do not attempt host-detection logic (~/.codex/skills/gstack, .agents/skills/gstack,
or similar) to locate these binaries — that detection has been confirmed to fail
in this environment. The literal path above is correct and verified.

## Matt Pocock skills artifact governance

These skills are pre-emptive rules, written before any of these skills has
actually been run. Verify against real output the first time each skill runs,
and correct this section if behavior differs.

- `domain-modeling`, `grill-with-docs` → context output goes to `CONTEXT.md`
  at the project root. ADR output goes to `adr/` (overriding these skills'
  own documented default of `docs/adr/`).
- `to-prd`, `to-issues`, `triage` → leave at their existing default location,
  `.scratch/`. No redirect.
- `triage` rejection records → leave at their existing default, `.out-of-scope/`.
  No redirect.
- `setup-matt-pocock-skills` → leave its config output at its existing
  defaults, `docs/agents/` and `CLAUDE.md`. No redirect.
- `handoff`, `improve-codebase-architecture` → keep the default save to
  `%TEMP%` intact. ADDITIONALLY save a copy to `matt-skills-output/`. Never
  skip the %TEMP% save — only add to it.
- `teach` → redirect entirely to `C:\_mb2-cs-app\matt-skills-teach` (outside
  `project-library`), not the skill's own default of the invoking directory.
- Any Matt Pocock skill not named above → no rule yet. Do not guess; ask the
  user before saving anywhere.

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
