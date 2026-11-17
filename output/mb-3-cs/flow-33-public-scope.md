---
title: "flow-33-public-scope"
doc-id: "mb-2b9a0931"
updated: "2026-11-17"
---
# Scope Card — flow-33-public

**Intent.** How a visitor reads the client's public pages, how the Owner edits them in place, and how an enquiry becomes a contact. Load-bearing claim: every public route carries its SEO tags, and an enquiry creates or updates exactly one contact.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Public pages | Visitor | home |
| Service detail | Visitor | start of booking |
| Contact form | Visitor | ≤ 1,000 chars |
| Enquiry | Server | contact |
| Edit mode | Owner / Manager | Owner / Manager |
| Save once | Server | audited |
| Media library | Owner / Manager | images + files |
| Pages list | Owner / Manager | name, SEO, address |
| Legal pages | Visitor | published versions |
| Route.meta | Server | title |

**Main path.** Public pages → Service detail → Contact form → Enquiry → Legal pages

## Deferred to cards

- The cookie banner's placement (ticket 28's ticket — 41)
- Edit-mode element registration

**Corridor plan.** Visitor→server crossings (contact→enquiry) at cols 2–3; owner→server at col 3; both through empty bands.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
