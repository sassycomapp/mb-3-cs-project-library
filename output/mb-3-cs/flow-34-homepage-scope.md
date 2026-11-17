---
title: "flow-34-homepage-scope"
doc-id: "mb-14f63238"
updated: "2026-11-17"
---
# Scope Card — flow-34-homepage

**Intent.** How the Owner shapes the public home page from four ready-made templates and a section editor. Load-bearing claim: the configuration is one `home_config` row — the template renders it, and an empty section hides.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Homepage editor | Owner | 8 steps of UF-17 |
| Auto-save | Owner | every 30 seconds |
| home_config | Server | one row |
| Template chosen | Server | classic |
| Testimonials | Owner | approved reviews or typed |
| Live home page | Visitor | sections render or hide |

**Main path.** Homepage editor → Auto-save → home_config → Template chosen → Live home page

## Deferred to cards

- The shared section inventory (DESIGN.md)
- The differentiation matrix

**Corridor plan.** Owner→server crossings at cols 1–2; server→visitor at cols 3–5, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
