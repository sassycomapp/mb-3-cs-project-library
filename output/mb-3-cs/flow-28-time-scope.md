---
title: "flow-28-time-scope"
doc-id: "mb-e0f4feb8"
updated: "2026-11-17"
---
# Scope Card — flow-28-time

**Intent.** How providers record time and how billable hours become invoice lines exactly once. Load-bearing claim: an entry is marked invoiced in the same call that creates its invoice line, so it can never be invoiced twice.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Timer | Staff | start / stop |
| Manual entry | Staff | rate from the service |
| Billable | Staff | uninvoiced |
| Create invoice | Server | lines + marked |
| Reports | Staff | per provider |
| time_entries | Server | duration |

**Main path.** Timer → Manual entry → Billable → Create invoice → Reports

## Deferred to cards

- The timer's rounding rule
- Non-billable entries' reporting

**Corridor plan.** Staff→server crossings at cols 2–3; server→data at cols 4–5, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
