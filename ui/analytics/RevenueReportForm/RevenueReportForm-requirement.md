---
title: "RevenueReportForm-requirement"
status: draft
doc-id: "mb-06b5dc16"
updated: "2026-11-25"
---
# RevenueReportForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `analytics.RevenueReportForm` |
| Purpose | Reports, PDF and CSV export, scheduled reports (ticket 25, F-04) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Reports) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_revenue_report`) | "Reports", `headline-large` | `page-title` | — | `reports.title` |
| Period fields | `m3.DatePicker` (`dp_report_from`), `m3.DatePicker` (`dp_report_to`) | The report's period (last month by default, as the accounts export does) | `outlined-date` | mobile | `data.history.from`, `data.history.to` |
| Report grid | Anvil `DataGrid` (`dg_report`) | The period's revenue by service, provider and month | — | mobile | — |
| Export PDF button | `m3.Button` (`btn_export_pdf`, outlined) | The PDF export (UF-25) | `secondary-button` | — | `analytics.export.pdf` |
| Export CSV button | `m3.Button` (`btn_export_csv`, outlined) | The CSV export (UF-25) | `secondary-button` | — | `analytics.export.csv` |
| Schedule fields | `m3.DropdownMenu` (`dd_schedule_freq`), `m3.DropdownMenu` (`dd_schedule_recipient`) | The scheduled report's frequency and email recipient | `outlined-select` | mobile | `reports.schedule.freq`, `reports.schedule.recipient` |
| Schedule button | `m3.Button` (`btn_schedule_save`, filled) | Saves the scheduled report; emailed by background task | `primary-button` | — | `reports.schedule.save` |
| Export progress | `m3.LoadingIndicator` (`load_report`, hidden) | The export runs as a background task | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `analytics.reports`) | The feature's tooltip and help video | — | — | `help.analytics.reports.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Report figures | `daily_summary`, `service_performance`, `provider_performance` | the period's summary rows |
| Schedules | `config` | the scheduled-report settings |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the report | `form_show` → `load_report` | `reporting_service` read (`server_analytics/reporting_service`) | The period's figures arrive |
| Export PDF / CSV | `btn_export_pdf_click` / `btn_export_csv_click` | `report_export_service` (`server_analytics/report_export_service`) | The file is produced and downloaded; the export is audited (AC-S16) |
| Schedule a report | `btn_schedule_save_click` | `reporting_service` schedule | The report is emailed by a background task at its schedule |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Report icon (`mi:summarize`) |
| Heading | `reports.empty.title` |
| Body text | `reports.empty.body` |
| Primary action | `analytics.export.csv` |

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
| `reports.title` | Reports |
| `reports.schedule.freq` | How often |
| `reports.schedule.recipient` | Email to |
| `reports.schedule.save` | Schedule report |
| `reports.schedule.saved` | Report scheduled. |
| `reports.empty.title` | Nothing to report yet |
| `reports.empty.body` | Your figures appear here as your business operates. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Exporting and scheduling reports | `master_template` help entry `analytics.reports` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-25 Marketing dashboard | The report actions UF-25 lists |
| UF-32 Expenses and accounts | Not this form — the accounts export is AccountsForm's |
