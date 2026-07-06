# AGENTS.md — project-library [PLAN] project

> This file inherits the Global `AGENTS.md` (dev-p) in full. Where this file is silent, the
> global rules apply. Where this file adds detail, it may only tighten global rules, never loosen
> them — this applies in particular to the Plan/Build write boundary below.

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

- **OpenCode** — the host agent/orchestrator operating in this workspace, encompassing the Plan agent and Build agent (see Global `AGENTS.md`).
- **Skill** — any of Gstack, Gbrain, Matt Pocock skills, Claude Wireframe, or a custom skill, once invoked to carry out a task.
- **Task** — a discrete unit of work with a defined completion condition, whether given directly by the user or delegated to a skill.
- **Gate** — a checkpoint in a workflow that requires explicit user approval before the next step may proceed.
- **Artifact** — any file produced during a session: reports, reviews, wireframes, screens, checklists, docs, ADRs, etc.
- **project-library** — this repo (planning/documentation/design). **mb-3-cs** — the separate live application code repo.

---

## Precedence order (highest to lowest)

1. Explicit user instruction given in the current session
2. Global `AGENTS.md` rules that this file may not loosen (e.g. Plan never writes)
3. Hard Rules in this file
4. ADRs (`adr/`)
5. Specifications and durable docs (`docs/`)
6. A skill's own default behavior

A conflict at any level triggers Default-to-Ask (see Global `AGENTS.md`) — never a silent resolution in either direction.

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

### 4. Plan reads, Build writes — no exceptions, applies everywhere in this repo
This project follows the Global `AGENTS.md` write boundary at full strength, with no local loosening:
- The Plan agent never writes anywhere in `project-library` — not docs, not wireframes, not reports, not checklists. It reads and proposes only.
- The Build agent is the only agent that writes in `project-library`, and only where the user has approved the specific change.
- Any exception for Plan to write must be confirmed explicitly, case by case, for that one write — never as a standing permission, never as a per-task or per-project approval.

### 5. No writing to the code repo (mb-3-cs)
- No agent or skill may write to `mb-3-cs` under any circumstance until the project has explicitly entered its coding phase.
- Even once coding has started, no agent may decide unilaterally to write to `mb-3-cs`. Every attempted write must first be posed to the developer for explicit, case-by-case permission — one-time blanket approval does not apply, and this cannot be satisfied by a standing project-level setting.
- This applies equally to Build acting directly and to any skill acting on Build's behalf. Plan never writes to `mb-3-cs` under any circumstance (see Rule 4).

### 6. Skill ownership — no usurping
Inherits the Global "Skill / tool continuity" principle. Project-specific addition:
- The skills in scope for this project are Gstack, Gbrain, Matt Pocock skills, Claude Wireframe, and custom skills.
- Before starting any new task, OpenCode must stop and ask the user which skill (if any) should be used, or propose a skill and wait for confirmation, rather than defaulting to its own general-purpose execution.

### 7. No batching or templatizing without permission
Inherits the Global "No batching or templatizing" principle. Project-specific addition:
- This applies with particular force to repeated wireframe/screen generation — never generate multiple wireframes or screens as a batch without asking first, regardless of how repetitive or mechanical the pattern looks.

### 8. No deviation from the given task
- Execute the task as specified. Do not substitute your own approach, add unrequested scope, or reinterpret the goal.
- If you believe a deviation would produce a better outcome, propose it and get explicit approval before proceeding. Judgment is not authorization to act.

### 9. Binding documents — ADRs, specs, rule files
- ADRs, specification files, and rule files are binding constraints, not suggestions.
- Before starting a task, check for applicable ADRs/specs/rules and confirm compliance.
- If a request conflicts with an existing ADR or spec, flag the conflict to the user — do not silently override the document, and do not silently override the request.

### 10. Guardrails, gates, and zero gate-jumping
Inherits the Global "Zero gate-jumping / strict scope adherence" principle in full. Project-specific addition:
- Defined checkpoints in the GStack pipeline, wireframe/screen approval steps, and any other project workflow gate are hard stops.
- Do not skip, auto-approve, or work around a gate — including when you judge it unnecessary for a given task.

### 11. Checklist integrity — no rubber-stamping
Inherits the Global "Checklist integrity" principle in full. Project-specific addition:
- Applies specifically to per-wireframe and per-screen compliance checklists in `wireframes/` and `screens/`.
- When work repeats across multiple artifacts (e.g. 50 wireframes), apply the checklist fresh to each one — a pass on one wireframe is never evidence of a pass on another, and checklist results may not be copied, assumed, or bulk-applied across artifacts.
- Checklist verification happens within the owning skill's process — it is subject to Rule 6 (Skill Ownership) and must not be taken over by OpenCode.

### 12. Default-to-ask
Inherits the Global "Default-to-ask" principle in full — applies to skill selection, gate satisfaction, ADR/spec applicability, scope, and batching/templatizing decisions in this project.

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

**Principle:** Every session-produced artifact gets saved. The destination depends on who produced it and what kind of file it is. Never ask whether to save — always save (all such saves are Build actions; see Rule 4). Confirm the save location in the on-screen response.

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
- `mb-3-cs/` — live application code and `anvil.yaml` (read-only context — see Hard Rule 5)

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

Governed by Global `AGENTS.md` at full strength — see Hard Rule 4 above. Summary as applied to this project:

### Plan agent
- May read the entire `project-library` repo and `mb-3-cs` for context.
- Does not write anywhere, in either repo, under any project-level setting.
- May propose plans, drafts, and content for Build to write, but does not write them itself.

### Build agent
- Is the only agent that writes in `project-library`, and only where the user has approved the specific change.
- Must not write in `mb-3-cs` except where Hard Rule 5 has been satisfied (coding phase active, and explicit case-by-case permission granted for that specific write).

---

## Safety rules, communication style

This project inherits the Global `AGENTS.md` Safety Rules and Communication Rules in full, with no local additions or exceptions. See Global `AGENTS.md` for the authoritative text — do not maintain a duplicate copy here.

---

## Prescribed standards

- Anvil.works working methods and standards are hard standards throughout. No deviation may ever be made without requesting authority and providing full justification for the deviation.
- Anvil.works uses the Material 3 theme, the M3 dependency, and the MUI overlay. These must be strictly adhered to at all times. No deviation is permitted.
- Color usage under this standard is governed by Hard Rule 3 (No hardcoded colors) — see `docs/DESIGN_palette_section.md` for token definitions.

---

## Task completion reporting and audit trail

Inherits the Global "Task completion reporting," "Task completion prompts," and "Audit trail"
rules in full. Project-specific mechanics:

- Every task completion report is saved as its own `.md` file in the producer's output folder
  (`gstack-outputs/`, `opencode-outputs/`, or `matt-skills-output/` per Artifact Governance above).
- `devlog-index.md` (project root) references every task completion report by filename, in
  order, as the single index into the full history.
- `judgement-trail.md` (project root) collects, per task: issues flagged, developer decisions
  given, and any deviations approved. Extracted from each completion report at the time it is
  written — not reconstructed after the fact.

### Scaffold initialization

If `devlog-index.md` or `judgement-trail.md` does not exist when first needed, create it with
only the following header, then proceed:

`devlog-index.md`:
```
# Devlog Index
Reverse-chronological list of task completion reports.
```

`judgement-trail.md`:
```
# Judgement Trail
Flagged issues, developer decisions, and approved deviations, extracted per task.
```

This is a one-time structural creation, not a content decision — the agent does not invent
format beyond this header. Creating these two files is the one exception to the Plan/Build write
lock (Hard Rule 4) and the no-unrequested-action rules: this AGENTS.md rule is itself the standing
approval for that specific bootstrap action only. It does not authorize scaffolding anything else
unprompted.

---

## Change log

- Merged all prose "other Rules" into the Hard Rules section, in a consistent terse style.
- Added Definitions and Precedence Order sections.
- Corrected a routing error: Matt Pocock skill artifact copies belong in `matt-skills-output/`, not `gstack-outputs/`.
- Reworked Access Model: Plan never writes, anywhere in this project, under any circumstance short of explicit case-by-case permission. Build is the sole writer. This mirrors the Global `AGENTS.md` write boundary at full strength — no local loosening.
- Added Hard Rule 5 (mb-3-cs write protection): no writes pre-coding-phase; post-coding-phase writes require explicit case-by-case permission, never blanket or standing approval.
- Trimmed Safety Rules and Communication Style to a pointer at the Global file, removing duplicated text that risked drifting out of sync.
- Five Hard Rules (Skill Ownership, Anti-Batching, Guardrails/Zero-Gate-Jumping, Checklist Integrity, Default-to-Ask) now state the principle by reference to Global `AGENTS.md` and retain only project-specific detail locally.
- Added project-specific mechanics for Task Completion Reporting and Audit Trail (`devlog-index.md`, `judgement-trail.md`), matching new Global rules on rule re-read, fact verification, error transparency, consultation availability, single session, abort conditions, rule file self-integrity, credential exposure, and build verification.
- Added a Scaffold Initialization clause: agent may create `devlog-index.md` and `judgement-trail.md` from a fixed minimal header if missing, as a one-time, explicitly pre-authorized exception to the Plan/Build write lock — no broader scaffolding license implied.
