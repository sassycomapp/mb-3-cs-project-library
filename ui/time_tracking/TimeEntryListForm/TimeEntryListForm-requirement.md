---
title: "TimeEntryListForm-requirement"
status: draft
doc-id: "mb-6af2a3c0"
updated: "2026-11-25"
---
# TimeEntryListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `time_tracking.TimeEntryListForm` |
| Purpose | Time entries: filter, select billable entries, create invoice (ticket 28, F-14; UF-13) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Time Entries) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_time_entries`) | "Time Entries", `headline-large` | `page-title` | — | `time.title` |
| Add Entry button | `m3.Button` (`btn_add_entry`, filled) | Opens TimeEntryEditorForm in create mode | `primary-button` | — | `time.add` |
| Customer filter | `m3.DropdownMenu` (`dd_time_customer`, `allow_none=True`) | Filter by customer (UF-13 step 4) | `outlined-select` | — | `time.filter.customer` |
| Billable filter | `m3.ButtonGroup` (`bgrp_time_filter`, connected, single: All/Billable/Uninvoiced) | Filter billable, uninvoiced entries (UF-13 step 4) | — | mobile | `time.filter.all`, `time.filter.billable`, `time.filter.uninvoiced` |
| Provider filter | `m3.DropdownMenu` (`dd_time_provider`, `allow_none=True`) | Filter by provider | `outlined-select` | — | `calendar.filter.provider` |
| Entries grid | Anvil `DataGrid` (`dg_time_entries`) | Date, customer, service, provider, hours, rate, total, billable, invoiced | — | mobile | — |
| Entry row | Anvil `DataRowPanel` (`drp_entry_*`) | One entry per row, selectable (UF-13 step 5) | — | — | — |
| Create Invoice button | `m3.Button` (`btn_invoice_entries`, filled) | Select entries → Create Invoice (UF-13 step 5); visible when uninvoiced billable rows are selected | `primary-button` | — | `time.create_invoice` |
| Reports link | `m3.Link` (`lnk_time_reports`) | Hours and value per provider, per customer and per service (UF-13 reports) | `tertiary-button` | — | `time.reports` |
| Help icon | `components.HelpTooltipComponent` (help entry `time.list`) | The feature's tooltip and help video | — | — | `help.time.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `time_entries` | `start_time`, `end_time`, `duration_hours`, `hourly_rate`, `total_amount`, `description`, `is_billable`, `is_invoiced`, `contact_id`, `provider_id`, `service_id` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_entries` | `time_entry_service` list read (`server_time/time_entry_service`) | The grid arrives (Staff own rows only) |
| Filter | `dd_time_customer_change`, `bgrp_time_filter_change`, `dd_time_provider_change` | the same read | The grid reloads |
| Create an invoice | `btn_invoice_entries_click` | `time_entry_service` invoicing | The selected entries become invoice lines and are marked invoiced in the same call that creates the lines, so they cannot be invoiced twice (ticket 28) |
| Add an entry | `btn_add_entry_click` | None | `time_tracking.TimeEntryEditorForm` opens |
| Reports | `lnk_time_reports_click` | None | The reports open |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Timer icon (`mi:timer`) |
| Heading | `time.empty.title` |
| Body text | `time.empty.body` |
| Primary action | `time.add` (+ Add Entry) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only (access matrix; `writable_fields` pattern).

## 7. Text

| Phrase key | English text |
|---|---|
| `time.title` | Time Entries |
| `time.add` | + Add Entry |
| `time.filter.customer` | Customer |
| `time.filter.all` | All |
| `time.filter.billable` | Billable |
| `time.filter.uninvoiced` | Uninvoiced |
| `time.create_invoice` | Create Invoice |
| `time.reports` | Reports |
| `time.empty.title` | No time logged yet |
| `time.empty.body` | Log time worked for a customer, then turn billable hours into an invoice. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Invoicing billable hours | `master_template` help entry `time.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-13 Time tracking and invoicing hours | The filter, selection and invoice step |
| UF-14 Invoices | The lines created here appear on the invoice |
