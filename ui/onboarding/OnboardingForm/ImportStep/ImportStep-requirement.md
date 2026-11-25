---
title: "ImportStep-requirement"
status: draft
doc-id: "mb-77c634ff"
updated: "2026-11-25"
---
# ImportStep — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `onboarding.OnboardingForm.ImportStep` |
| Purpose | Onboarding step 9: CSV upload, source tool selector (HubSpot, Mailchimp, Acuity, Other), column mapping table, preview of the first rows, Import. Skippable (ticket 51, F-03; UF-02 step 9) |
| Screen type and width | Onboarding — 600 px centred card (the mapping table widens to the card's content) |
| Layout | Hosted by `onboarding.OnboardingForm`'s step card |
| Route | None — a local step of the wizard |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Step container | Anvil `ColumnPanel` (`col_import_step`) | The step's one column | — | mobile | — |
| File upload | `m3.FileLoader` (`fu_ob_csv`, `file_types=".csv"`, `show_state=True`) | The CSV file exported from the old tool | — | — | `crm.import.file` |
| Source tool selector | `m3.DropdownMenu` (`dd_ob_source_tool`) | HubSpot, Mailchimp, Acuity, Other (UF-02 step 9) | `outlined-select` | — | `crm.import.source` |
| Mapping table | Anvil `DataGrid` (`dg_ob_mapping`) | The column mapping; the matching columns are filled in for the known tools | — | mobile | — |
| Mapping row | Anvil `DataRowPanel` (`drp_ob_mapping_*`) | One CSV column per row with its field selector | — | — | — |
| Preview grid | Anvil `DataGrid` (`dg_ob_preview`) | The first rows as they will import | — | mobile | — |
| Import button | `m3.Button` (`btn_ob_import`, filled) | Runs the import (a heavy job; runs as a background task) | `primary-button` | — | `crm.import.run` |
| Import progress | `m3.LoadingIndicator` (`load_ob_import`, hidden) | Shown while the import runs | — | — | — |
| Skip note | `m3.Text` (`text_ob_import_skip`) | The step can be skipped | `body-small` | — | `onboarding.skip` |

The same mapping and the same `contact_import_export` functions as `crm.ContactImportForm` run here (ticket 51).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Imported rows | `contacts` | `first_name`, `last_name`, `email`, `phone`, `status`, `tags`, `source` (`import`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the file | `fu_ob_csv_change` | None | The columns are read and the known mappings pre-filled |
| Import | `btn_ob_import_click` | `import_contacts` (`server_customers/contact_import_export`) | Existing contacts with the same email are updated, not duplicated; the counts return |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Upload icon (`mi:upload_file`) |
| Heading | `crm.import.empty.title` |
| Body text | `crm.import.empty.body` |
| Primary action | The file loader's own upload action |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

## 7. Text

| Phrase key | English text |
|---|---|
| `onboarding.import.title` | Bring your contacts |
| `onboarding.import.hint` | Export your contacts from your old tool and bring them in. Existing contacts with the same email are updated, not duplicated. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Importing your contacts | `master_template` help entry `onboarding.import` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | Step 9 of the wizard, skippable |
| UF-21 Contact management | The same import continues from the CRM after onboarding |
