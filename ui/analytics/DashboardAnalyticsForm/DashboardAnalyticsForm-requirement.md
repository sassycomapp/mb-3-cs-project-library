---
title: "DashboardAnalyticsForm-requirement"
status: draft
doc-id: "mb-7118c47e"
updated: "2026-11-25"
---
# DashboardAnalyticsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `analytics.DashboardAnalyticsForm` |
| Purpose | Revenue by service and by provider (ticket 25, F-04; UF-25's detailed reports) |
| Screen type and width | Dashboard — 1200 px, full width: metric cards |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Analytics) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_analytics`) | "Analytics", `headline-large` | `page-title` | — | `analytics.title` |
| Period selector | `m3.ButtonGroup` (`bgrp_period`, connected, single) + custom `m3.DatePicker` pair | The period the figures cover | — | mobile | `dashboard.period.week`, `dashboard.period.month`, `dashboard.period.custom` |
| Revenue by service card | `m3.Card` (`card_by_service`, outlined) | Revenue and booking count per service from `service_performance` | `surface-card` | mobile | — |
| Service performance grid | Anvil `DataGrid` (`dg_service_performance`) | Service, revenue, bookings, completed | — | mobile | — |
| Revenue by provider card | `m3.Card` (`card_by_provider`, outlined) | Revenue and booking count per provider from `provider_performance` | `surface-card` | mobile | — |
| Provider performance grid | Anvil `DataGrid` (`dg_provider_performance`) | Provider, revenue, bookings | — | mobile | — |
| Trend chart | Anvil `Plot` (`plot_revenue_trend`) | The period's revenue trend | — | mobile | — |
| Help icon | `components.HelpTooltipComponent` (help entry `analytics.dashboard`) | The feature's tooltip and help video | — | — | `help.analytics.dashboard.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Service figures | `service_performance` | `service_id`, `date`, `revenue`, `booking_count`, `completed_count` |
| Provider figures | `provider_performance` | `provider_id`, `date`, `revenue`, `booking_count` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the figures | `form_show` → `load_analytics` | `reporting_service` read (`server_analytics/reporting_service`) | The period's figures arrive |
| Change the period | `bgrp_period_change` | the same read | The figures reload |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Chart icon (`mi:monitoring`) |
| Heading | `analytics.empty.title` |
| Body text | `analytics.empty.body` |
| Primary action | None — the figures arrive as the business operates |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix rows added at this step.

## 7. Text

| Phrase key | English text |
|---|---|
| `analytics.title` | Analytics |
| `analytics.by_service.title` | Revenue by service |
| `analytics.by_provider.title` | Revenue by provider |
| `analytics.empty.title` | No data yet |
| `analytics.empty.body` | Your figures appear here as your business operates. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading revenue by service and provider | `master_template` help entry `analytics.dashboard` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The analytics half of the daily routine |
| UF-25 Marketing dashboard | The marketing figures live in the marketing dashboard |
