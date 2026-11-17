---
title: "flow-map-content-scope"
doc-id: "mb-08c4effe"
updated: "2026-11-17"
---
# Scope Card — flow-map-content

**Intent.** How the Owner edits the public website in place, configures the homepage, publishes landing pages and legal documents, and shares posts. Load-bearing claim: every edit saves in one server call and is audited; a legal publish raises the version and keeps its copy in R2.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Edit mode | Editors | in place |
| Pages list | Editors | SEO + address |
| Homepage editor | Editors | auto-save |
| Landing page | Editors | 3 templates |
| Clause Builder | Legal | 3 documents |
| Blog editor | Blog | schedule |
| Share panel | Blog | AI captions |
| Save once | Server | audited |
| home_config | Server | one row |
| Publish+version | Server | copy to R2 |
| Public page | Visitor | visitors see it |

**Main path.** Edit mode → Save once → Homepage editor → home_config → Public page

## Deferred to cards

- The 12 page templates' inventory (DESIGN.md)
- Consent interplay with embeds
- Syndication `?ref=` detail

**Corridor plan.** Owner→server crossings (editmode→save, homepage→homeconfig) cross at the empty band at cols 0–3.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
