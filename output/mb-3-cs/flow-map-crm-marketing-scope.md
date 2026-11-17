---
title: "flow-map-crm-marketing-scope"
doc-id: "mb-cfadff82"
updated: "2026-11-17"
---
# Scope Card — flow-map-crm-marketing

**Intent.** How enquiries and captures become contacts and leads, how campaigns and broadcasts go out through SES, and how the Team Inbox reads and replies. Load-bearing claim: a lead and its contact are created together in one step, and one SES tenant carries every email.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Contact form | Visitor | ≤ 1,000 chars |
| Landing page | Visitor | capture form |
| Enquiry | Server | contact + task |
| Lead capture | Server | one call |
| Welcome sequence | Server | enrolled |
| Add as lead | Server | unknown sender |
| Contacts | CRM (team) | timeline |
| Segments | CRM (team) | rules |
| Campaign | CRM (team) | through SES |
| Reviews | CRM (team) | approve |
| Team Inbox | Inbox (team) | addresses |
| Reply | Inbox (team) | AI draft |

**Main path.** Landing page → Lead capture → Welcome sequence → Campaign

## Deferred to cards

- Suppression scope detail
- Automation rules (ticket 20's drawing)
- Review lifecycle detail

**Corridor plan.** The visitor→server crossings (contactform→enquiry, landing→capture) cross at the empty band at cols 0–2.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
