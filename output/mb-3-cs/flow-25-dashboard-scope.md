---
title: "flow-25-dashboard-scope"
doc-id: "mb-733c427e"
updated: "2026-11-17"
---
# Scope Card — flow-25-dashboard

**Intent.** How the team reads its day and its numbers, and how the summary tasks keep the figures honest. Load-bearing claim: dashboards read pre-computed summary tables and never aggregate source tables on load.

**Type.** workflow — user and system paths through forms, gates and lanes.
**Quality profile.** standard

## Node inventory

| Node | Lane | Role |
|---|---|---|
| Dashboard | Team | metrics |
| Today's calendar | Team | Today's calendar |
| Analytics | Team | revenue + marketing |
| Drill + export | Team | PDF |
| Summary tasks | Summary tasks | period-keyed upsert |
| One row/period | Summaries | re-run replaces |
| Sample banner | Team | Owner clears |
| Clear sample | Summary tasks | Owner only, audited |

**Main path.** Dashboard → Analytics → Drill + export → Summary tasks → One row/period

## Deferred to cards

- The at-risk alert thresholds (App 5's side)
- Marketing metric formulas

**Corridor plan.** Team→tasks and tasks→data crossings cross at the empty bands at cols 3–5.

**Layout budget source.** `renderers/workflow/README.md` (readable v2 contract).

**Probe declaration.** Expected corridors as in the corridor plan; abort on a failed probe after one geometry repair — return to this card, reshape, re-present.

## Decisions

Approved by David at the Step 19 gate, 2026-11-17 — one approval for the set of 28, with the amendment to card 28 (business profile carries the AI first website text).
