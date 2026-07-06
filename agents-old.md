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

---

## Definitions

- **OpenCode** — the host agent/orchestrator operating in this workspace. Encompasses both the Plan agent and Build agent modes described under Access Model.
- **Skill** — any of Gstack, Gbrain, Matt Pocock skills, Claude Wireframe, or a custom skill, once invoked to carry out a task.
- **Task** — a discrete unit of work with a defined completion condition, whether given directly by the user or delegated to a skill.
- **Gate** — a checkpoint in a workflow that requires explicit user approval before the next step may proceed.
- **Artifact** — any file produced during a session: reports, reviews, wireframes, screens, checklists, docs, ADRs, etc.
- **project-library** — this repo (planning/documentation/design). **mb-3-cs** — the separate live application code repo.

---

## Precedence order (highest to lowest)

When instructions conflict, resolve in this order:

1. Explicit user instruction given in the current session
2. Hard Rules in this file
3. ADRs (`adr/`)
4. Specifications and durable docs (`docs/`)
5. A skill's own default behavior

A conflict at any level is a trigger for the **Default-to-Ask Rule** below — never a silent resolution in either direction.

---

## Hard rules (always apply)

### 1. anvil.yaml protection
Never write to `anvil.yaml`.
- Treat it as Anvil-managed metadata/configuration.
- Do not create, edit, reformat, regenerate, normalize, merge-edit, or patch it.
- Do not include it in bulk refactors or automated file updates.
- If a task appears to require changing it, stop and ask the user for explicit approval.
- Assume Anvil is the authoritative writer for `anvil.yaml`.

### 2. No time-based references in planning docs
Never use time-based references (weeks, days, hours) in planning documents.
- Use the Phase > Stage > Task > Sub-task hierarchy instead.
- Write "Phase 2, Stage 2.1" or "Task T2.1-003" — not "2 weeks" or "3 days."
- Applies to all planning docs, build plans, roadmaps, and implementation schedules.

### 3. No hardcoded colors
No hardcoded colors permitted in the application.
- Every color must resolve through a CSS custom property from the palette system.
- No literal hex, `rgb()`, `rgba()`, or `hsl()` values — except MUI card-shadow `hsla()` values as defined in the MUI aesthetic overlay, and the status semantic exceptions: `#00b958` (success), `#ff9469` (warning), `#ff000c` (error).
- See `docs/DESIGN_palette_section.md` for authoritative token definitions.

### 4. No writing to the code repo (mb-3-cs) without permission
- No agent or skill may write to `mb-3-cs` under any circumstance until the project has explicitly entered its coding phase.
- Even once coding has started, no agent may decide unilaterally to write to `mb-3-cs`. Every attempted write must first be posed to the developer for explicit permission — one-time blanket approval does not apply.
- This applies equally to OpenCode acting directly and to any skill acting on OpenCode's behalf.

### 5. Skill ownership — no usurping
- When a task is delegated to a skill, that skill owns the task until it explicitly reports completion.
- If a skill reports the task is incomplete or pauses partway through, control must return to that same skill to continue. OpenCode must NOT silently take over and finish the remaining work itself.
- Before starting any new task, OpenCode must stop and ask the user which skill (if any) should be used, or propose a skill and wait for confirmation, rather than defaulting to its own general-purpose execution.

### 6. No batching or templatizing without permission
- Never batch, templatize, or mass-produce work — including repeated tasks like generating multiple wireframes — without asking first.
- Batching or templatizing without explicit developer permission is an immediate failure condition, not a minor deviation. Flag it loudly if it's about to happen; don't do it quietly.

### 7. No deviation from the given task
- Execute the task as specified. Do not substitute your own approach, add unrequested scope, or reinterpret the goal.
- If you believe a deviation would produce a better outcome, propose it and get explicit approval before proceeding. Judgment is not authorization to act.

### 8. Binding documents — ADRs, specs, rule files
- ADRs, specification files, and rule files are binding constraints, not suggestions.
- Before starting a task, check for applicable ADRs/specs/rules and confirm compliance.
- If a request conflicts with an existing ADR or spec, flag the conflict to the user — do not silently override the document, and do not silently override the request.

### 9. Guardrails and gates
- Defined checkpoints, approval gates, and guardrails are hard stops.
- Do not skip, auto-approve, or work around a gate — including when you judge it unnecessary for a given task.
- If a gate seems to be blocking legitimate progress, say so and ask. Do not bypass it.

### 10. Checklist integrity — no rubber-stamping
- When a checklist is supplied for a task (e.g. per-wireframe or per-screen compliance checklists), it is a mandatory verification instrument, not a formality.
- Every item must be checked against the actual produced artifact, individually and honestly, before the task can be marked complete.
- Do NOT mark an item "passed" unless you have genuinely verified the artifact satisfies it. Marking a whole checklist "passed on all points" as a default/summary action is a critical failure.
- If an artifact fails an item, report that item as failed explicitly, state which specification was not met, and stop — do not proceed to the next artifact or quietly record a pass.
- When work repeats across multiple artifacts (e.g. 50 wireframes), apply the checklist fresh to *each one*. A pass on one artifact is never evidence of a pass on another.
- If you find yourself about to record a pass while suspecting non-compliance, stop and flag it to the developer rather than reconciling the discrepancy in the artifact's favor.
- Checklist verification happens within the owning skill's process — it is subject to Rule 5 (Skill Ownership) and must not be taken over by OpenCode.

### 11. Default-to-ask (mandatory, applies globally)
Whenever you are uncertain — about which skill applies, whether a gate is satisfied, whether an ADR/spec applies, whether scope has been exceeded, or whether an action would count as batching/templatizing — stop and ask the developer rather than proceeding on your own judgment. Uncertainty is a trigger to ask, never a license to act. This overrides any instinct toward "helpful" autonomous completion.

### 12. Zero Gate-Jumping / Strict Scope Adherence Clause
No unauthorized scope expansion: A task's scope is defined by what was explicitly requested — nothing more. Relatedness is not authorization. If something adjacent, upstream, downstream, or "obviously connected" to the current task comes into view, that is not license to act on it, fix it, extend into it, or fold it into the current task, no matter how minor or how clearly beneficial it appears.
Raise, don't run: When OpenCode or a skill identifies an out-of-scope issue, dependency, or opportunity, the correct action is to flag it to the developer and stop — not to act on it, not to silently queue it as a "next step," and not to treat the flag itself as approval to proceed. A flag is an observation, not a green light.
No self-authorized gate-jumping: Approval gates, checkpoints, and scope boundaries may not be crossed on the basis of OpenCode's own judgment that doing so is reasonable, efficient, or clearly correct. "It was going to be needed anyway" is not a valid justification after the fact. Every gate requires an explicit developer decision made before the boundary is crossed, not a rationalization offered after.
Silence is not consent: If the developer has not responded to a flagged item, that is not consent to proceed. Absence of a "no" is not a "yes." Work stops at the gate until an explicit "yes" is received.
---

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
| `matt-skills-output/` | Matt Pocock skill outputs (copies — originals may also go to `%TEMP%`) |
| `workspace/gbrain/` | GBrain working files and configuration |
| `workspace/gstack/` | GStack working files and configuration |

---

## Artifact governance

**Principle:** Every session-produced artifact gets saved. The destination depends on who produced it and what kind of file it is. Never ask whether to save — always save. Confirm the save location in the on-screen response.

- GStack often saves documents to a system-defined location. That behavior stays as-is, but a copy of every artifact or report GStack produces must **also** be saved to `gstack-outputs/`.
- Matt Pocock skills often save documents to a system-defined location. That behavior stays as-is, but a copy of every artifact or report a Matt Pocock skill produces must **also** be saved to `matt-skills-output/` (subject to the per-skill rules below).
- OpenCode-produced reports, reviews, and artifacts (i.e. produced directly, not via a skill) must be saved to `opencode-outputs/`.

### Routing by producer

| Producer | Destination |
|---|---|
| GStack (any gstack-* skill, including OpenClaw variants) | `gstack-outputs/` |
| OpenCode (direct, not via a skill) | `opencode-outputs/` |
| Matt Pocock skills | `matt-skills-output/` (see per-skill rules below) |

### Routing by file type (overrides producer routing)

These file types have fixed homes regardless of producer, and do not belong in a producer's output folder:

| File type | Destination |
|---|---|
| Durable application documentation | `docs/` |
| Architecture Decision Records | `adr/` |
| Wireframes and visual mockups | `wireframes/` |
| Screen HTML mockups | `screens/` |
| Wireframe/screen compliance reports | Same folder as the wireframe or screen it belongs to |
| DESIGN.md | `docs/` |

Every task, regardless of who performs it, must produce a task completion report stating what was done. The report is saved in the producer's output folder.

### Exclusions

This routing applies to reports, reviews, analyses, task lists, and similar session-produced artifacts. It does **not** apply to:
- Durable docs (`docs/`), ADRs (`adr/`), wireframes (`wireframes/`), screens (`screens/`)
- Files in `wip/` (own lifecycle)
- GBrain working files (`workspace/gbrain/`)
- GStack working files (`workspace/gstack/`)

### Confirmation rule

The on-screen response must confirm the save location explicitly:
`"Saved to: gstack-outputs/<filename>."` or `"Saved to: opencode-outputs/<filename>."`
If the save fails, state why — do not silently report success.

### Promotion rules

- Long-lived app specifications must not stay in `gstack-outputs/` — promote to `docs/`.
- Transient planning reports must not go in `docs/` unless explicitly promoted.
- ADRs must not go in `docs/` or `gstack-outputs/` — they belong in `adr/`.

---

## GStack binary path override

When any GStack skill needs to call `gstack-slug`, `gstack-config`, or any other `gstack-*` binary, always use `/mnt/c/mybizz/gstack/bin/<binary-name>` directly. Do not attempt host-detection logic (`~/.codex/skills/gstack`, `.agents/skills/gstack`, or similar) to locate these binaries — that detection has been confirmed to fail in this environment. The literal path above is correct and verified.

---

## Matt Pocock skills artifact governance

> ⚠️ **UNVERIFIED — pre-emptive rules only.** These were written before any of these skills had actually been run. Do not treat as fully binding until confirmed against real output; verify against real output the first time each skill runs, and correct this section if behavior differs.

- `domain-modeling`, `grill-with-docs` → context output goes to `CONTEXT.md` at the project root. ADR output goes to `adr/` (overriding these skills' own documented default of `docs/adr/`).
- `to-prd`, `to-issues`, `triage` → leave at their existing default location, `.scratch/`. No redirect.
- `triage` rejection records → leave at their existing default, `.out-of-scope/`. No redirect.
- `setup-matt-pocock-skills` → leave its config output at its existing defaults, `docs/agents/` and `CLAUDE.md`. No redirect.
- `handoff`, `improve-codebase-architecture` → keep the default save to `%TEMP%` intact. ADDITIONALLY save a copy to `matt-skills-output/`. Never skip the `%TEMP%` save — only add to it.
- `teach` → redirect entirely to `C:\mybizz\matt-skills-teach` (outside `project-library`), not the skill's own default of the invoking directory.
- Any Matt Pocock skill not named above → no rule yet. Do not guess; ask the user before saving anywhere.

---

## Source of truth for this project

### Authoritative sources

- `project-library/adr/` — architectural decisions
- `project-library/docs/` — application documentation and specifications
- `project-library/wireframes/` — visual and layout references
- `mb-3-cs/` — live application code and `anvil.yaml` (read-only context — see Hard Rule 4)

### Fact rule

Do not use general training data or prior memory to assert project-specific facts. If a fact is not supported by the allowed files above, treat it as inferred or missing, not as known truth.

Research from GBrain must be validated before use — it cannot be depended on as a source of truth by itself.

### Exception handling

If the user explicitly instructs you to read a file outside the authoritative sources, you may use it only for that request and must state that its content is advisory unless the user explicitly promotes it.

### Mandatory reading

Any content given to you as added context with specified files must be treated as mandatory reading before proceeding.

---

## Certainty levels (must be explicit)

When assessing how well something is defined, every statement must fall into one of:

- `Clearly defined:` an explicit statement in an authoritative file directly answers the point.
- `Partially defined:` files give some constraints but important details are missing or implied.
- `Inferred only:` connecting dots not explicitly stated; depends on interpretation.
- `Missing:` the authoritative files do not provide enough information to answer reliably.

Do not upgrade inferred material to clearly defined.

---

## Language restrictions

### Schema and alignment claims
Do not claim the database schema "matches" or "fully implements" any document unless you were explicitly asked to compare them in this session and produced a concrete comparison output. Use cautious language: "broadly aligned with," "appears consistent with," "implements many of the patterns described in."

### Flow completeness claims
Do not use phrases like "fully specified" or "complete user flows" unless the files actually contain step-by-step or screen-by-screen flow descriptions.

---

## Access model

There are two agents native to OpenCode. All skills (Gstack, Gbrain, Matt Pocock, Claude Wireframe, custom skills) operate as tools invoked by one of these two agents, under the rules above — the agents may use tools as needed without necessarily surfacing that to the user.

### Plan agent
- May read and write the entire `project-library` repo (subject to user approval per the Hard Rules above).
- May read `mb-3-cs` for implementation context.
- Must not make live implementation changes in `mb-3-cs` (see Hard Rule 4).

### Build agent
- May write explicitly requested non-live-code files in `project-library`.
- Must not write in `mb-3-cs` except where Hard Rule 4 has been satisfied (coding phase active, and explicit per-instance permission granted).

---

## Safety rules

- Do not create, edit, move, or delete files unless the user clearly asks for that operation.
- Never run or suggest destructive operations without a clear warning and explicit confirmation.
- Prefer proposing diffs or patches in chat over auto-applying changes.
- If a requested change affects many files, propose a small, reviewable subset first.
- Git operations are permission-only.
- Do not treat generated summaries as canonical unless validated by the user.
- Do not push, publish, sync, or export anything unless the user explicitly asks.

---

## Communication style

- Keep answers concise, structured, and practical.
- Prefer stepwise guidance over long explanations.
- Distinguish clearly between facts from the repo, inferences, and proposed revisions.
- Flag uncertainty, contradictions, or missing provenance explicitly.
- No emojis or side topics unless the user asks.

---

## Prescribed standards

- Anvil.works working methods and standards are hard standards throughout. No deviation may ever be made without requesting authority and providing full justification for the deviation.
- Anvil.works uses the Material 3 theme, the M3 dependency, and the MUI overlay. These must be strictly adhered to at all times. No deviation is permitted.
- Color usage under this standard is governed by Hard Rule 3 (No hardcoded colors) — see `docs/DESIGN_palette_section.md` for token definitions.

---

## Change log

- Consolidated all "Hard Rule" style entries (skill ownership, anti-batching, scope lock, binding documents, guardrails, checklist integrity, default-to-ask, code-repo write protection) into a single Hard Rules section.
- Added Definitions and Precedence Order sections.
- Corrected a routing error: Matt Pocock skill artifact copies belong in `matt-skills-output/`, not `gstack-outputs/`.
- Clarified Access Model: only Plan agent and Build agent are native OpenCode agents; all skills act as tools under one of the two.
- Consolidated scattered standards references (colors, M3/MUI) under Prescribed Standards with cross-reference to Hard Rule 3.
