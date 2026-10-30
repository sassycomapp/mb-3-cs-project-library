---
title: "domain"
doc-id: "mb-62fff0db"
updated: "2026-10-30"
---
# Domain docs

How the engineering skills read this business's domain documents.

## Layout

Single context. The business has one vocabulary and one set of decisions, shared by all its apps.

- **The glossary:** `docs-local/GLOSSARY.md`.
- **The decisions:** `adr-local/`. Global decisions are in `project-library-global` and are referenced, never copied. Each decision is one file, `adr-{kebab-case-name}.md`, as `spec-nomenclature` names it, written in the format of the `domain-modeling` skill's `ADR-FORMAT.md`.
- **The code:** in the app folders beside this library, `mb-3-cs-app1`, `mb-3-cs-app2`, `mb-3-cs-app3` and `mb-3-cs-app5`. Read code there directly.

## Before exploring, read these

- `docs-local/GLOSSARY.md`
- the decisions in `adr-local/` that concern the task

If either does not exist yet, carry on without it. Step 16 creates the glossary and the decisions as terms and decisions are settled.

## Use the glossary's words

When anything you write names a domain concept (a ticket title, a proposal, a test name), use the term exactly as `GLOSSARY.md` defines it, never a synonym.
