---
title: "ContactImportForm-requirement"
status: draft
doc-id: "mb-2e696513"
updated: "2026-11-25"
---
# ContactImportForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.ContactImportForm` |
| Purpose | CSV import with column mapping — the same mapping as onboarding step 9 (ticket 20, F-17, F-03; SR-08a) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `crm.ContactListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_import`) | "Import contacts", `headline-large` | `page-title` | — | `crm.import.title` |
| File upload | `m3.FileLoader` (`fu_csv`, `file_types=".csv"`, `show_state=True`) | The CSV file (UF-02 step 9: upload a CSV exported from any tool) | — | — | `crm.import.file` |
| Source selector | `m3.DropdownMenu` (`dd_source_tool`) | HubSpot, Mailchimp, Acuity, Other — the ready-made column mappings (UF-02 step 9) | `outlined-select` | — | `crm.import.source` |
| Mapping table | Anvil `DataGrid` (`dg_mapping`) | CSV column → contact field; the matching columns are filled in for the known tools (UF-02 step 9) | — | mobile | — |
| Mapping row | Anvil `DataRowPanel` (`drp_mapping_*`) | One CSV column per row with its field selector | — | — | — |
| Preview grid | Anvil `DataGrid` (`dg_preview`) | The first rows as they will import (UF-02 step 9: preview of the first rows) | — | mobile | — |
| Import button | `m3.Button` (`btn_import_csv`, filled) | Runs the import (a heavy job; runs as a background task) | `primary-button` | — | `crm.import.run` |
| Import progress | `m3.LoadingIndicator` (`load_import`, hidden) | Shown while the import runs | — | — | — |
| Result text | `m3.Text` (`text_import_result`, hidden) | Imported and updated counts (the `import_contacts` return) | `body-medium` | — | `crm.import.result` |
| Cancel button | `m3.Button` (`btn_cancel_import`, text) | Back to the list | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.import`) | The feature's tooltip and help video | — | — | `help.crm.import.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Imported rows | `contacts` | `first_name`, `last_name`, `email`, `phone`, `status`, `tags`, `source` (`import`) |
| The source record | `contact_events` | the import event on each contact's timeline |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the file | `fu_csv_change` | None | The CSV's columns are read and the known tools' mappings are pre-filled |
| Map columns | `dg_mapping` field selectors | None | Each CSV column is mapped to a contact field |
| Preview | automatic after mapping | `contact_import_export` preview (`server_customers/contact_import_export`) | The first rows show as they will import |
| Import | `btn_import_csv_click` | `import_contacts` (`server_customers/contact_import_export`) | Existing contacts with the same email are updated, not duplicated (ticket 20); the counts return; the import is audited |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:upload_file` |
| Heading | `crm.import.empty.title` |
| Body text | `crm.import.empty.body` |
| Primary action | The file loader's own upload action |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix (`import_contacts`: Owner, Manager).

## 7. Text

| Phrase key | English text |
|---|---|
| `crm.import.title` | Import contacts |
| `crm.import.file` | Choose a CSV file |
| `crm.import.source` | Where the file comes from |
| `crm.import.run` | Import |
| `crm.import.result` | {imported} contacts added, {updated} updated. |
| `crm.import.empty.title` | No file yet |
| `crm.import.empty.body` | Choose a CSV exported from your old tool. We fill in the mapping for the well-known ones. |
| `crm.import.mapping.csv_column` | CSV column |
| `crm.import.mapping.contact_field` | Contact field |

## 8. Help Videos

| Process | Help video |
|---|---|
| Importing contacts from a CSV | `master_template` help entry `crm.import` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-21 Contact management | The CSV import with column mapping, as in onboarding step 9 |
| UF-02 Owner onboarding | The onboarding wizard's import step runs the same mapping through the same server function |
