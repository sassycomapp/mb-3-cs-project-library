# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Two-repo architecture

This application spans two repos:

- **`project-library`** (this repo) — planning, documentation, ADRs, wireframes, design artefacts.
- **`mb-3-cs`** — live application code (Python/Anvil). Built on Anvil.works.

Both repos apply to the same application. When a skill needs implementation context, read from `mb-3-cs`. When it needs architectural decisions or design context, read from this repo.

## Before exploring, read these

- **`CONTEXT.md`** at the repo root — domain glossary and key concepts (create lazily via `/domain-modeling` when needed).
- **`adr/`** at the repo root — Architecture Decision Records.

If any of these files don't exist, **proceed silently**. The `/domain-modeling` skill creates them lazily when terms or decisions actually get resolved.

## File structure

```
/
├── CONTEXT.md                ← domain glossary (created lazily)
├── adr/                      ← Architecture Decision Records
├── docs/
│   └── agents/               ← Agent configuration
├── gstack-outputs/           ← GStack-produced reports
├── matt-skills-output/       ← Matt Pocock skill outputs
├── .scratch/                 ← Issue tracker
├── .out-of-scope/            ← Rejected issues
├── wireframes/               ← HTML wireframes
├── screens/                  ← Screen HTML mockups
└── wip/                      ← Work in progress
```

## Use the glossary's vocabulary

When your output names a domain concept, use the term as defined in `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding.
