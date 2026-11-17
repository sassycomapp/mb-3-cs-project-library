---
title: "flow-36-blog-scope"
doc-id: "mb-159a96c9"
updated: "2026-11-17"
---
# Scope Card — flow-36-blog

**Intent.** How the client publishes posts, takes comments, and shares them with AI captions. Load-bearing claim: only approved comments are public, and every share carries the `?ref=` source.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Blog admin | Owner / Manager | the list |
| Editor | Owner / Manager | layouts |
| Publish | Server | captions written |
| Share panel | Owner / Manager | 4 platforms |
| Read | Visitor | 2 list |
| Comment | Visitor | awaiting approval |
| Moderate | Owner / Manager | approve |
| ?ref= tracking | Server | visits + contacts |
| blog-image endpoint | Server | published posts only |

**Main path.** Blog admin → Editor → Publish → Share panel → Read

## Deferred to cards

- The three post templates' layouts
- AI caption regeneration rules

**Corridor plan.** Owner→server crossings at cols 1–2; server→visitor at cols 2–4, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
