# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Two-repo architecture

This application spans two repos:

- **`project-library`** (this repo) — planning, documentation, ADRs, wireframes, design artefacts
- **`mb-3-cs`** — live application code (Python/Anvil)

Both repos apply to the same application. When a skill needs implementation context, it should read from `mb-3-cs`. When it needs architectural decisions or design context, it reads from this repo.

## Before exploring, read these

- **`CONTEXT.md`** at the repo root — domain glossary and key concepts (create lazily via `/domain-modeling` when needed).
- **`adr/`** at the repo root — 40+ Architecture Decision Records. Use `docs/adr-index.md` to find relevant ADRs by topic.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## File structure

Single-context repo:

```
/
├── CONTEXT.md                ← domain glossary (created lazily)
├── adr/                      ← Architecture Decision Records
│   ├── 1-brevo-replaces-zoho-email.md
│   ├── ...
│   └── 41-dependency-based-not-multi-tenant.md
├── docs/
│   └── adr-index.md          ← ADR index with status and notes
└── ...
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal — either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts `client-instance-architecture` ADR (client instance architecture) — but worth reopening because…_
