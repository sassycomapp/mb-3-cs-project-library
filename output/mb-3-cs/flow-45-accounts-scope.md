---
title: "flow-45-accounts-scope"
doc-id: "mb-4b0f2bb6"
updated: "2026-11-17"
---
# Scope Card — flow-45-accounts

**Intent.** How the client records expenses with their receipts and produces the accounts reports and the package exports. Load-bearing claim: a void expense is left out of every report and export, and every export writes an audit row.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| New expense | Owner / Manager | supplier |
| Receipt photo | Owner / Manager | to R2 |
| Void | Owner / Manager | never deleted |
| Reports | Owner / Manager | P&L |
| Export | Server | 3 formats |
| exports/ | Data | one ZIP of the period |

**Main path.** New expense → Receipt photo → Void → Reports → Export

## Deferred to cards

- The three export formats' column maps
- Customer statements' PDF layout

**Corridor plan.** Finance→server crossings at cols 3–4; server→data at cols 4–5, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
