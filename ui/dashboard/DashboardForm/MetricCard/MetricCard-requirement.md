---
title: "MetricCard-requirement"
status: draft
doc-id: "mb-10ce61ba"
updated: "2026-11-25"
---
# MetricCard — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `dashboard.DashboardForm.MetricCard` |
| Purpose | One dashboard metric: current value, percentage change and trend (ticket 25, F-04; architecture reference F-04) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | A local component of `dashboard.DashboardForm`; moved to this module path when DashboardForm is built (David's Step 22 rule) |
| Route | None — a local component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Card container | `m3.Card` (`card_metric`, elevated) | A KPI and dashboard metric card is elevated (DESIGN.md Cards: `metric-card` — elevated) | `metric-card` | mobile | — |
| Metric label | `m3.Text` (`text_metric_label`) | The metric's name (Revenue, Bookings, New customers) | `body-medium` | — | set by the host (`dashboard.metric.*`) |
| Metric value | `m3.Heading` (`hdg_metric_value`) | The current value | `headline-large` | — | — |
| Percentage change | `m3.Text` (`text_metric_change`) | The percentage change against the comparison period, with the direction stated in words as well (no state by colour alone) | `body-small` | — | `dashboard.change.up`, `dashboard.change.down`, `dashboard.change.flat` |
| Mini trend | Anvil `Plot` (`plot_metric_trend`) | The trend line for the period | — | never | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Value, change and trend | `daily_summary`, `service_performance`, `provider_performance` | the summary tables' figures for the period |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None | — | — | The card is presentation; the host passes the figures in |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the card shows its value, or 0 |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Same as the dashboard; the host shows the metric only to roles that may see it.

## 7. Text

| Phrase key | English text |
|---|---|
| `dashboard.change.up` | Up {pct}% |
| `dashboard.change.down` | Down {pct}% |
| `dashboard.change.flat` | No change |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The dashboard's video covers the metrics |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The dashboard's metric cards |
