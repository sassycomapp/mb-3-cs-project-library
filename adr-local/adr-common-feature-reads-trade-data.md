---
title: "adr-common-feature-reads-trade-data"
status: Accepted
date: 2026-11-12
doc-id: "mb-efe1a942"
updated: "2026-11-12"
---
# Common features read trade data

Mybizz applications are a common part and a trade part, and the common part never uses the trade part (`adr-common-and-trade-modules`). We decided that the one-direction rule binds code packages and data — no common package imports a trade package, and no common table holds a link to a trade table — while a feature is marked common when every Mybizz business needs it, and such a common feature may read trade tables through its own server functions to display them: the dashboard (F-04) reads service performance, the accounts reports (F-28) read invoices and expenses, and the customer portal (F-24) reads bookings, quotes, invoices, packages and reviews. A review still reports common code reaching trade code, and any common table linking a trade table, as a finding. The alternative — marking those features trade — would have moved features every business needs out of the project template.

Marked **common**.

## Considered options

- **Features marked by need; the prohibition on packages and table links** (chosen).
- **Mark the dashboard, the accounts reports and the customer portal trade** — rejected: they are common in every Mybizz business, and marking them trade would remove them from the template.

## Consequences

- At the template stage, the common feature generalises and the trade tables it read become the trade part's plug-in point.
- Every review reports a common item using a trade item as a finding, within this rule.

*End of `adr-common-feature-reads-trade-data` ADR*
