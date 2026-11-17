---
title: "flow-32-tasks-scope"
doc-id: "mb-22966d46"
updated: "2026-11-17"
---
# Scope Card — flow-32-tasks

**Intent.** How work for a contact is tracked by hand or created by an automation, and how every automation run is recorded. Load-bearing claim: one run per rule and triggering event — recorded as success, skipped or failed.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Tasks | Team | by due date |
| Complete | Team | timestamped |
| New automation | Team | trigger |
| Actions | Team | 4 action kinds |
| Evaluation | Automation engine | background task |
| automation_runs | Data | one per rule |

**Main path.** Tasks → Complete → New automation → Actions → Evaluation

## Deferred to cards

- The five trigger events' detail
- Condition evaluation order

**Corridor plan.** Team→engine crossings at cols 3–4; engine→data at cols 4–5, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
