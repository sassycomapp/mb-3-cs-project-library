---
title: "2026-11-12-domain-map"
doc-id: "mb-00f6a720"
updated: "2026-11-12"
---
# Archify run learnings — mb-3-cs-b16-app1-domain-map (2026-11-12)

Recorded with David's approval at the close of the run, per mb-archify-scope Phase 9.

| Learning | Right action | Destination |
|---|---|---|
| With explicit sides, a routed edge is pinned to the **centre port** of its target's side; the final segment must run perpendicular into that centre, and only one edge can occupy a side | Feed a hub node from **different sides** (left, right, top, bottom), or move the surplus feeder to the row its edge naturally enters and drop it straight in | PDLF Phase B/C steps that draw architecture maps; mb-archify-scope Phase 5 corridor plan |
| The auto-inferred sides go **horizontal whenever dx ≠ 0** — any horizontal offset produces a right/left inference and a Z-route that violates the inferred side | For a straight vertical edge, **centre-align the two nodes exactly** (dx = 0); then no `fromSide`/`via` is needed at all | Same |
| Via points do not snap perpendicular — the route runs straight from the port to `via[0]` and from `via[-1]` to the centre port, so `via[0]` must sit on the port's axis and `via[-1]` on the target port's axis | Compose via routes as: port-aligned first point, free middle waypoints, port-aligned last point | Same |
| When validation fails on routing geometry, two rounds of hand-repair still failed; the recovery (move the node to the row its edge naturally enters) fixed it in one pass | **After one failed geometry repair, run mb-archify-scope's recovery and present a revised Scope Card** — reshape the rows, do not hand-route around them (David's ruling; PDLF to gain this rule) | PDLF steps 15/16 diagram work; mb-archify-scope abort conditions |
| `renderers/architecture/` has no README with a layout budget — only `grid.mjs` and `render-architecture.mjs` | Read the **renderer source's own constants** for the architecture type; treat the README pointer in the Scope Card as type-conditional | mb-archify-scope Phase 5; the scope card's layout-budget field |
| A label on a same-row horizontal can sit 0px from an unrelated vertical segment (the "billed as"/"settled by" warnings) | Prefer layouts where verticals do not share the spine's band; where they must, `labelAt` the label into the empty band (the "invoiced" fix) | Archify repair order; corridor plan |
| Node sublabels carry the common/trade mark ("common · …" / "trade · …") so the marking survives the render, not only the card | Start every domain-map sublabel with its mark (David's visual-check ruling) | Domain-map Scope Cards; mb-archify-scope card conventions |

*End of run learnings*
