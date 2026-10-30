---
title: "issue-tracker"
doc-id: "mb-d3d40a41"
updated: "2026-10-30"
---
# Issue tracker: local markdown

Tickets for this business live as markdown files in `pdlf-tickets/`, in this project library. They are never placed in an app repository: tickets are documentation, and code and documentation are never mixed. Every repository is public, so a ticket never holds a secret, a key or client data.

## Conventions

- **Create a ticket:** create `pdlf-tickets/<run>/<ticket-slug>.md`, where `<run>` is `app{n}` for the whole application or the feature's short name. Use the frontmatter below.
- **Read a ticket:** read the file.
- **List tickets:** list the markdown files in `pdlf-tickets/` and filter by their frontmatter.
- **Comment:** append to the file under `## Comments`, with the date.
- **Labels:** edit the `labels` field. The labels are in `docs/agents/triage-labels.md`.
- **Close:** set `status: closed`.

## File template

```yaml
---
title: "Ticket title"
status: open          # open | closed
labels: []
created: YYYY-MM-DD
---

## Description

## Comments
```

## When a skill says "publish to the issue tracker"

Create a file in `pdlf-tickets/`.

## When a skill says "fetch the relevant ticket"

Read the file from `pdlf-tickets/`.
