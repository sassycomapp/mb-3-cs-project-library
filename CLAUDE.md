# CLAUDE.md

## GBrain Configuration (configured by /setup-gbrain)
- Engine: pglite
- Config file: ~/.gbrain/config.json (mode 0600)
- Setup date: 2026-05-19
- MCP registered: yes (OpenCode mcpServers)
- Memory sync: full
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
