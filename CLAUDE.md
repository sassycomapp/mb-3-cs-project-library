---
document: CLAUDE.md
doc-id: mb-3-cs-0002
state: Live
date-created: 2026-06-25T073307+0200
---

# CLAUDE.md

## GBrain Configuration (configured by /setup-gbrain)
- Engine: pglite
- Config file: ~/.gbrain/config.json (mode 0600)
- Setup date: 2026-05-19
- MCP registered: yes (OpenCode mcpServers)
- Artifacts sync: full
- Current repo policy: read-write

## GBrain Search Guidance

GBrain is available as an MCP tool. Use it for semantic search and code navigation.

### When to use GBrain
- Semantic questions: "what did we decide about X?", "where is the auth logic?"
- Code symbol lookup: `gbrain_code_def`, `gbrain_code_refs`, `gbrain_code_callers`, `gbrain_code_callees`
- Cross-repo search: questions spanning project-library docs and mb-3-cs code
- Finding related content by meaning, not exact text

### When to use Grep/Glob
- Exact string matches: `grep -r "OPENAI_API_KEY"`
- Pattern-based file discovery: `glob "**/*.py"`
- When you know the exact file or line number

### Source routing
- `default` source: ADRs, specs, design docs, wireframes (project-library)
- `mb-3-cs` source: Python application code, Anvil forms, server modules
- `gbrain-repo` source: GStack working state (learnings, plans, retros)

Default to GBrain for questions. Default to Grep for exact matches.

## Skill routing

When the user's request matches an available skill, invoke it via the Skill tool. When in doubt, invoke the skill.

Key routing rules:
- Product ideas/brainstorming → invoke /office-hours
- Strategy/scope → invoke /plan-ceo-review
- Architecture → invoke /plan-eng-review
- Design system/plan review → invoke /design-consultation or /plan-design-review
- Full review pipeline → invoke /autoplan
- Bugs/errors → invoke /investigate
- QA/testing site behavior → invoke /qa or /qa-only
- Code review/diff check → invoke /review
- Visual polish → invoke /design-review
- Ship/deploy/PR → invoke /ship or /land-and-deploy
- Save progress → invoke /context-save
- Resume context → invoke /context-restore
- Author a backlog-ready spec/issue → invoke /spec

## Agent skills

This project's canonical agent configuration lives in `AGENTS.md` (see its `## Agent skills` section) and in `docs/agents/*.md`. Keep this section in sync with those.
