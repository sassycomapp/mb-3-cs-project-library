---
title: "flow-30-landing-scope"
doc-id: "mb-2482d5dc"
updated: "2026-11-17"
---
# Scope Card — flow-30-landing

**Intent.** How the Owner builds a campaign page in one of three templates and publishes it to a shareable address. Load-bearing claim: a landing page is outside the public website's own pages, and its only job is to capture a lead.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Landing list | Owner / Manager | Landing list |
| Editor | Owner / Manager | 3 templates |
| Edit mode | Owner / Manager | on the page |
| Publish | Server | validated + address |
| /landing/:slug | Campaign visitor | blank layout |
| Analytics | Owner / Manager | views + conversions |

**Main path.** Landing list → Editor → Edit mode → Publish → /landing/:slug

## Deferred to cards

- Template content schemas
- The share targets (email, social, embed)

**Corridor plan.** Owner→server crossings at cols 2–3; server→visitor at cols 3–4, through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
