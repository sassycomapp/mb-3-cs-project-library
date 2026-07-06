# Matt Pocock Skills — Configuration Reference

## 1. Installation

| Property | Value |
|---|---|
| **Source** | `github.com/mattpocock/skills` (MIT licensed) |
| **Install method** | `npx skills@latest add mattpocock/skills` targeting OpenCode |
| **Install location** | `~/.agents/skills/<skill-name>/SKILL.md` (WSL: `/home/dev-p/.agents/skills/`) |
| **Lock file** | `~/.agents/.skill-lock.json` (18 skills tracked) |
| **Install date** | 2026-06-24 |
| **Update source** | `npx skills@latest update` |
| **Global access** | Paths are absolute (`file:///home/dev-p/.agents/skills/...`) — available across all projects |

## 2. Skills Inventory (18 installed)

### Engineering — User-invoked

| Skill | Description | OpenCode command |
|---|---|---|
| `ask-matt` | Router — which skill fits your situation | `/ask-matt` |
| `grill-with-docs` | Relentless interview + domain model + ADRs | `/grill-with-docs` |
| `triage` | Issue state machine (needs-triage → closed) | `/triage` |
| `improve-codebase-architecture` | Codebase architecture scan + HTML report | `/improve-codebase-architecture` |
| `setup-matt-pocock-skills` | Per-repo config scaffold (run once per repo) | `/setup-matt-pocock-skills` |
| `to-issues` | Break PRD/spec into independent issues | `/to-issues` |
| `to-prd` | Conversation → PRD, no interview | `/to-prd` |

### Engineering — Model-invoked

| Skill | Description |
|---|---|
| `prototype` | Throwaway terminal/UI prototype |
| `diagnosing-bugs` | Reproduce → minimise → hypothesise → instrument → fix |
| `tdd` | Red-green-refactor test-driven development |
| `domain-modeling` | Build/sharpen domain glossary + ADRs (called by grill-with-docs) |
| `codebase-design` | Deep module design vocabulary and patterns |

### Productivity — User-invoked

| Skill | Description | OpenCode command |
|---|---|---|
| `grill-me` | Relentless interview (no docs) | `/grill-me` |
| `handoff` | Compact conversation for another agent | `/handoff` |
| `teach` | Multi-session teaching workspace | `/teach` |
| `writing-great-skills` | Skill authoring reference | `/writing-great-skills` |

### Productivity — Model-invoked

| Skill | Description |
|---|---|
| `grilling` | The interview loop behind grill-me and grill-with-docs |

### Utility

| Skill | Description |
|---|---|
| `find-skills` | Discover/install skills (from `vercel-labs/skills`) |

## 3. Invocation types

Per the source repo's categorization:

- **User-invoked** — only reachable when typed explicitly (e.g. `/grill-me`). They orchestrate. May call model-invoked skills internally, never other user-invoked ones.
- **Model-invoked** — reachable when typed OR automatically by the agent when the task fits. Hold reusable discipline.

## 4. Output Routing — Governance Rules

Applied per `AGENTS.md` § Matt Pocock skills artifact governance. These override skill defaults where indicated.

| Skill | Artifact | Destination | Override? |
|---|---|---|---|
| `domain-modeling`, `grill-with-docs` | Domain glossary | `CONTEXT.md` at project root | No — default |
| `domain-modeling`, `grill-with-docs` | Architecture decisions | `adr/` at project root | **Yes** — default is `docs/adr/` |
| `to-prd`, `to-issues`, `triage` | Issues/PRDs | `.scratch/<feature>/` | No — default |
| `triage` | Rejection records | `.out-of-scope/<topic>.md` | No — default |
| `setup-matt-pocock-skills` | Agent config | `docs/agents/` + `CLAUDE.md`/`AGENTS.md` | No — default |
| `handoff`, `improve-codebase-architecture` | Primary output | `%TEMP%` | No — default kept |
| `handoff`, `improve-codebase-architecture` | Additional copy | `matt-skills-output/` | **Yes** — copy added |
| `teach` | All teaching artefacts | `C:\_mb2-cs-app\matt-skills-teach` | **Yes** — redirected from invoking directory |
| Other skills | Varies | — | **No rule yet** — ask before saving |

## 5. Per-Project Setup State

### mb-3-cs

| Artifact | Path | Status |
|---|---|---|
| `docs/agents/domain.md` | project-library | Created by setup skill |
| `docs/agents/issue-tracker.md` | project-library | Created by setup skill — local markdown |
| `docs/agents/triage-labels.md` | project-library | Created by setup skill — defaults |
| `CLAUDE.md` | project-library | Edited by setup skill — Agent skills block present |
| `CONTEXT.md` | project-library | Bootstrapped 2026-07-06 |
| `.scratch/` | project-library | Created — empty |
| `matt-skills-output/` | project-library | Created — empty |
| `.out-of-scope/` | project-library | Created 2026-07-06 |
| `adr/` | project-library | 47 ADRs |
| `C:\_mb2-cs-app\matt-skills-teach` | shared | Created — empty (shared across all projects) |

### mb4ecom

| Artifact | Path | Status |
|---|---|---|
| `docs/agents/domain.md` | project-library | Created 2026-07-06 |
| `docs/agents/issue-tracker.md` | project-library | Created 2026-07-06 — local markdown |
| `docs/agents/triage-labels.md` | project-library | Created 2026-07-06 — defaults |
| `AGENTS.md` | project-library | Agent skills block appended 2026-07-06 |
| `CONTEXT.md` | project-library | Not yet created — skills create lazily |
| `.scratch/` | project-library | Created 2026-07-06 |
| `matt-skills-output/` | project-library | Created 2026-07-06 |
| `.out-of-scope/` | project-library | Created 2026-07-06 |

### mb5pdlf

| Artifact | Path | Status |
|---|---|---|
| `docs/agents/domain.md` | project-library | Created 2026-07-06 |
| `docs/agents/issue-tracker.md` | project-library | Created 2026-07-06 — local markdown |
| `docs/agents/triage-labels.md` | project-library | Created 2026-07-06 — defaults |
| `AGENTS.md` | project-library | Agent skills block appended 2026-07-06 |
| `CONTEXT.md` | project-library | Not yet created — skills create lazily |
| `.scratch/` | project-library | Created 2026-07-06 |
| `matt-skills-output/` | project-library | Created 2026-07-06 |
| `.out-of-scope/` | project-library | Created 2026-07-06 |

## 6. Intentionally excluded skills

| Skill | Reason |
|---|---|
| `code-review` | Would clash with GStack `/review` |
| `QA` / `review` skills | Would clash with GStack `/qa` / `/review` |
| `research` | Not installed — upstream addition after initial install |

## 7. Operational notes

- **Issue tracker**: All three projects use local markdown issues in `.scratch/`. No GitHub Issues integration.
- **Triage labels**: All three projects use the same 5 default labels.
- **`teach` redirect**: `C:\_mb2-cs-app\matt-skills-teach` is shared across all projects — only one teaching workspace for the whole system.
- **`handoff` output**: Saved to `%TEMP%` AND copied to `matt-skills-output/`. The `%TEMP%` save is the skill's native behavior; the copy is added by AGENTS.md governance.
- **CONTEXT.md per project**: Only mb-3-cs has a bootstrapped CONTEXT.md. The other projects' CONTEXT.md files will be created lazily by the skills when first needed.
