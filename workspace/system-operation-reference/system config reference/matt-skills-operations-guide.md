# Matt Pocock Skills — Operations Guide & Cheatsheet

## What is this?

Matt Pocock's skills are a set of 18 engineering and productivity tools designed for real software development with AI agents — not vibe coding. They enforce software engineering fundamentals: domain language, architecture discipline, test-first development, and controlled handoffs.

## When to use which skill

```
                          START HERE
                              │
                    ┌─────────┴─────────┐
                    │  I have an idea   │
                    └─────────┬─────────┘
                              │
                    /ask-matt (router — which skill?)
                              │
              ┌───────────────┼───────────────┐
              │               │               │
         New Feature     Bug/Request     Codebase Health
              │               │               │
    /grill-with-docs    /triage       /improve-codebase
         (domain         (state        -architecture
         model +         machine)       (deep module
         ADRs)                           scan + report)
              │
    /prototype (optional: test the design)
              │
         /to-prd (publish specification)
              │
        /to-issues (break into tasks)
              │
         /tdd (build test-first)
              │
    /diagnosing-bugs (if something breaks)
              │
         /handoff (pass to another agent)
```

### Quick decision guide

| You want to... | Use | Invocation |
|---|---|---|
| Figure out which skill to use | `/ask-matt` | Type it |
| Stress-test an idea with hard questions | `/grill-me` | Type it |
| Same + build domain glossary + ADRs | `/grill-with-docs` | Type it |
| Turn conversation into a formal spec | `/to-prd` | Type it |
| Break a spec into independent tasks | `/to-issues` | Type it |
| Process incoming bugs/issues through states | `/triage` | Type it |
| Build features test-first | `/tdd` | Automatic or type |
| Debug a failing test or bug | `/diagnosing-bugs` | Automatic or type |
| Test a design idea with throwaway code | `/prototype` | Automatic or type |
| Scan codebase for architecture problems | `/improve-codebase-architecture` | Type it |
| Save session for another agent | `/handoff` | Type it |
| Learn a new concept interactively | `/teach` | Type it |

## Skill Invocation Rules

### User-invoked (type the command)
- `/ask-matt`, `/grill-me`, `/grill-with-docs`, `/to-prd`, `/to-issues`, `/triage`, `/improve-codebase-architecture`, `/handoff`, `/teach`, `/setup-matt-pocock-skills`, `/writing-great-skills`

### Model-invoked (agent reaches for them automatically)
- `/grilling`, `/domain-modeling`, `/tdd`, `/diagnosing-bugs`, `/prototype`, `/codebase-design`, `/find-skills`

**Important:** User-invoked skills orchestrate. Model-invoked skills hold reusable discipline. A user-invoked skill may call model-invoked ones internally but never another user-invoked one.

## Where Skills Write Output

| Skill | Writes to | Notes |
|---|---|---|
| `domain-modeling` | `CONTEXT.md` + `adr/` | Creates files lazily |
| `grill-with-docs` | Same as domain-modeling | Calls domain-modeling internally |
| `to-prd`, `to-issues`, `triage` | `.scratch/<feature>/` | Local markdown issue tracker |
| `triage` (rejected) | `.out-of-scope/<topic>.md` | Rejection records |
| `setup-matt-pocock-skills` | `docs/agents/` + `CLAUDE.md` | Run once per repo |
| `handoff` | `%TEMP%` + copy to `matt-skills-output/` | Temp save + permanent copy |
| `improve-codebase-architecture` | `%TEMP%` + copy to `matt-skills-output/` | HTML report |
| `teach` | `C:\mybizz\matt-skills-teach` | Shared teaching workspace |
| `tdd`, `diagnosing-bugs`, `prototype` | In-place in codebase | Modify code directly |

## The Domain Model System

The most powerful technique in the Matt Pocock toolkit is the **domain model** — a shared language between you and the agent.

### How it works
1. Run `/grill-with-docs` when starting a new feature
2. The agent asks you detailed questions about your design
3. As you answer, it builds a glossary in `CONTEXT.md` and writes ADRs for architectural decisions
4. Future sessions read this context — the agent already knows your jargon

### Example
- **Before:** "There's a problem when a lesson inside a section of a course is given a spot in the file system"
- **After:** "There's a problem with the materialization cascade"

This concision pays off every session.

## Per-Project Setup State

| Project | CONTEXT.md | docs/agents | .scratch | .out-of-scope | matt-skills-output |
|---|---|---|---|---|---|
| mb-3-cs | Bootstrapped | 3 files | ✓ | ✓ | ✓ |
| mb4ecom | Lazy create | 3 files | ✓ | ✓ | ✓ |
| mb5pdlf | Lazy create | 3 files | ✓ | ✓ | ✓ |

All three use local markdown issue trackers and default triage labels.

## Common Workflows

### Workflow 1: New Feature (the full pipeline)
```
/grill-with-docs    → Align on design, build domain model, write ADRs
/prototype          → Test the design (optional, throwaway)
/to-prd             → Publish formal specification
/to-issues          → Break into independent tasks
/tdd                → Build each issue test-first
/diagnosing-bugs    → If anything breaks during /tdd
/handoff            → Save for another agent session
```

### Workflow 2: Bug Fix
```
/triage             → Process the bug through state machine
/diagnosing-bugs    → Reproduce → minimise → hypothesise → fix
```

### Workflow 3: Architecture Health Check
```
/improve-codebase-architecture   → Scan for deepening opportunities
                                 → Generates HTML report with diagrams
                                 → Grills you on chosen candidate
/handoff                         → Save analysis for implementation
```

### Workflow 4: Learning Something New
```
/teach              → Creates a teaching workspace in matt-skills-teach/
                   → Multi-session: lessons, references, learning records
                   → Uses retrieval practice + spacing + interleaving
```

## Relationship with GStack and PDLF

Matt Pocock skills complement GStack and PDLF — they don't replace them:

| Layer | Tool | Role |
|---|---|---|
| **Orchestration** | PDLF | Full project lifecycle, stage-gated, 25 steps |
| **Workflow** | GStack | Planning (CEO/ENG/design review), QA, review, ship |
| **Engineering discipline** | Matt Pocock | Domain modeling, TDD, architecture, debugging, handoffs |

### Integration points
- PDLF Step 2 calls `/to-prd` (Matt) to create the PRD
- PDLF Step 5 calls `/domain-modeling` (Matt) to lock the domain model
- PDLF Step 7 calls `/to-issues` (Matt) to break into tasks
- PDLF Step 16 (build loop) uses `/tdd` (Matt) for test-first builds
- PDLF Step 21 optionally calls `/improve-codebase-architecture` (Matt)

## Files to Know

| File | Purpose |
|---|---|
| `CONTEXT.md` (project root) | Domain glossary — the agent's shared vocabulary |
| `adr/` (project root) | Architecture Decision Records |
| `docs/agents/domain.md` | Rules for how skills consume domain docs |
| `docs/agents/issue-tracker.md` | Which issue tracker and how to use it |
| `docs/agents/triage-labels.md` | Label vocabulary for `/triage` |
| `CLAUDE.md` / `AGENTS.md` | Contains `## Agent skills` block |
| `.scratch/` | Local markdown issue tracker |
| `.out-of-scope/` | Rejection records |
| `matt-skills-output/` | Copies of handoff + architecture reports |

## Troubleshooting

| Problem | Fix |
|---|---|
| Skill not appearing in picker | Verify `~/.agents/skills/<name>/SKILL.md` exists |
| Skill can't find issue tracker | Check `docs/agents/issue-tracker.md` |
| Agent doesn't know domain terms | `CONTEXT.md` missing or empty — run `/grill-with-docs` |
| `/triage` errors about .out-of-scope/ | Create directory: `mkdir .out-of-scope` |
| Handoff output not saved | Check `matt-skills-output/` exists |
| `/teach` writes to wrong directory | Check AGENTS.md governance: should redirect to `C:\mybizz\matt-skills-teach` |
