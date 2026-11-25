---
title: "DataTab-requirement"
status: draft
doc-id: "mb-3ce19390"
updated: "2026-11-25"
---
# DataTab — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.DataTab` |
| Purpose | The Settings → Data tab: history view from the client's R2 bucket, export all data as CSV, Clear sample data (tickets 18 and 46, F-29; UF-33) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm` |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_data_tab`) | The tab's one column | — | mobile | — |
| History heading | `m3.Heading` (`hdg_history`) | "History from your storage", `headline-small` | `headline-small` | — | `data.history.title` |
| History kind selector | `m3.DropdownMenu` (`dd_history_kind`) | What to view — audit history older than 30 days, webhook records, past backups (UF-33 step 1) | `outlined-select` | — | `data.history.kind` |
| History period selector | `m3.DatePicker` (`dp_history_from`) + `m3.DatePicker` (`dp_history_to`) | The period the records are read from | `outlined-date` | — | `data.history.from`, `data.history.to` |
| View button | `m3.Button` (`btn_view_history`, outlined) | Reads the chosen records from the client's R2 bucket and shows them | `secondary-button` | — | `data.history.view` |
| History list | Anvil `ColumnPanel` (`col_history_rows`) | The records in time order | — | mobile | — |
| History row | `m3.ListItem` (`li_history_*`) | One record row: headline, supporting text (details), trailing text (when) | — | — | — |
| Export heading | `m3.Heading` (`hdg_export`) | "Export all data", `headline-small` | `headline-small` | — | `data.export.title` |
| Export hint | `m3.Text` (`text_export_hint`) | Every table as CSV, at any time | `body-medium` | — | `data.export.hint` |
| Export button | `m3.Button` (`btn_export_csv`, filled) | The Owner's export action; step-up (access matrix `export_all_data`) | `primary-button` | — | `data.export.submit` |
| Export progress | `m3.LoadingIndicator` (`load_export`, hidden) | The export runs as a background task; the indicator shows it is working | — | — | — |
| Sample data heading | `m3.Heading` (`hdg_sample`) | "Sample data", `headline-small` | `headline-small` | — | `data.sample.title` |
| Sample hint | `m3.Text` (`text_sample_hint`) | The sample data's presence and what Clear does | `body-medium` | — | `data.sample.hint` |
| Clear sample data button | `m3.Button` (`btn_clear_sample_data`, outlined) | Owner only; the confirmation dialog states the step cannot be undone (UF-43) | `secondary-button` | — | `admin.banner.sample.clear` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.data`) | The tab's tooltip and help video | — | — | `help.settings.data.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| History records | the client's R2 bucket (index rows only in the instance) | `activity_log` rows older than 30 days at `audit/{year}/{month}/`; webhook records via `webhook_log.r2_key`; backups at `backups/daily/`, `backups/weekly/` |
| Export | every data table | all columns, as CSV |
| Sample data | `sample_rows` | row presence |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| View history | `btn_view_history_click` | `history_service` read (`server_storage/history_service`) | The chosen records are read from the client's R2 bucket and shown (ticket 46) |
| Export all data | `btn_export_csv_click` | `export_all_data` (`server_shared/export_service`; step-up, background task) | The full CSV export is produced and kept in the client's bucket; the Owner is told when it is ready (ticket 46; AC-S16) |
| Clear sample data | `btn_clear_sample_data_click` | `clear_sample_data` (`server_settings/sample_data_service`) | The confirmation dialog, then the background task removes every sample record, empties the links from real rows and recomputes the summaries (UF-43) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:history` |
| Heading | `data.history.empty.title` |
| Body text | `data.history.empty.body` |
| Primary action | `data.history.view` (View history) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.Data`.

## 7. Text

| Phrase key | English text |
|---|---|
| `data.history.title` | History from your storage |
| `data.history.kind` | What to view |
| `data.history.from` | From |
| `data.history.to` | To |
| `data.history.view` | View history |
| `data.export.title` | Export all data |
| `data.export.hint` | Every table of your business as CSV files, at any time. The export is kept in your own storage too. |
| `data.export.submit` | Export all data as CSV |
| `data.sample.title` | Sample data |
| `data.sample.hint` | You are viewing sample data. Clearing it removes every sample record; your own records stay. The step cannot be undone. |
| `data.history.empty.title` | Nothing here yet |
| `data.history.empty.body` | Records older than 30 days arrive here from your own storage. |
| `data.export.started` | Your export is running. We'll email you when it's ready. |
| `data.sample.cleared` | Sample data cleared. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Viewing history and exporting data | `master_template` help entry `settings.data` — played in the app language |
| Clearing the sample data | `master_template` help entry `data.clear_sample` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-33 Restoring or viewing data | The history view and the export (UF-33) |
| UF-43 Clearing the sample data | The Clear sample data action |
| UF-32 Expenses and accounts | Not this tab — the accounting export is AccountsForm's |
