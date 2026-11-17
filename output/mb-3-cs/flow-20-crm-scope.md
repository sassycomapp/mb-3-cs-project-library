---
title: "flow-20-crm-scope"
doc-id: "mb-8992c39f"
updated: "2026-11-17"
---
# Scope Card — flow-20-crm

**Intent.** How a client's team manages every person it knows as one contact record, grouped by company, filed by segment. Load-bearing claim: contacts are created or matched, never duplicated, and internal notes are projected only to the roles allowed.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Contacts list | Contacts (team) | search |
| Viewer | Contacts (team) | timeline, stats |
| Editor | Contacts (team) | add or edit |
| CSV import | Contacts (team) | column mapping |
| Match email | Server | never duplicate |
| Companies | Sales (team) | grouped |
| Segments | Sales (team) | all/any rules |
| Nightly count | Server | recalculated |
| Timeline | Data | every touch |
| Counter | Data | in transaction |

**Main path.** Contacts list → Viewer → Editor → Match email → Timeline

## Deferred to cards

- The prebuilt segments
- The writable-fields table per role

**Corridor plan.** Team→server crossings (editor→match) cross at the empty band at col 2–3; server→data at cols 4–5.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
