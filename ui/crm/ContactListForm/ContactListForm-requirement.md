---
title: "ContactListForm-requirement"
status: draft
doc-id: "mb-3c4b5263"
updated: "2026-11-25"
---
# ContactListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.ContactListForm` |
| Purpose | The CRM's contact overview: totals, status breakdown, prebuilt segments, search and the contact grid; bulk actions and CSV import/export (ticket 20, F-17; UF-21) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_contacts`) | "Contacts", `headline-large` | `page-title` | — | `crm.contacts.title` |
| Overview stat cards | `m3.Card` (`card_stat_total`, `card_stat_leads`, `card_stat_customers`, `card_stat_inactive`, elevated) | Metric cards: total contacts and the status breakdown (UF-21: total contacts, status breakdown) | `metric-card` | mobile | — |
| Segments row | Anvil `FlowPanel` (`flow_segments`) | The prebuilt segments (New Leads, Active Customers, Inactive Customers, High Value) as filter chips that wrap | — | mobile | — |
| Segment chip | `m3.Chip` (`chip_segment_*`, type `filter`) | A choice the user switches on and off (Chip.md: filter chips) | — | — | `crm.segments.<name>` |
| Search field | `m3.SearchBar` (`srch_contacts`) | Search contacts by name, email, phone, tags | — | — | `crm.contacts.search` |
| Add Contact button | `m3.Button` (`btn_add_contact`, filled) | The primary action; opens ContactEditorForm in create mode | `primary-button` | — | `crm.contacts.add` |
| Import button | `m3.Button` (`btn_import`, outlined) | Opens `crm.ContactImportForm` | `secondary-button` | — | `crm.contacts.import` |
| Export button | `m3.Button` (`btn_export`, text) | CSV export of the current view | `tertiary-button` | — | `crm.contacts.export` |
| Contact grid | Anvil `DataGrid` (`dg_contacts`) | The contact grid (UF-21) | — | mobile | — |
| Contact row | Anvil `DataRowPanel` (`drp_contact_*`) | One contact per row: name, email, phone, status, tags, total spent, last contact | — | — | — | — | — |
| Bulk action bar | `m3.Toolbar` (`tbar_bulk`, docked) | Appears when rows are selected: add tag, enrol in a campaign, CSV export (UF-21 bulk actions) | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.contacts`) | The feature's tooltip and help video | — | — | `help.crm.contacts.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Stat cards | `contacts` | `status` counts (via the contact service's own read) |
| Segment chips | `segments` | `segment_name`, `contact_count` (prebuilt, `segment_type = prebuilt`) |
| Grid | `contacts` | `first_name`, `last_name`, `email`, `phone`, `status`, `tags`, `total_spent`, `last_contact_date` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_contacts` | `contact_service` list read (`server_customers/contact_service`) | The grid, stats and segments arrive (server-side filtering per adr-data-access-patterns) |
| Search | `srch_contacts_search` | the same read | The grid reloads with the matches |
| Filter by segment | `chip_segment_*_change` | the same read | The grid reloads with the segment's contacts |
| Open a contact | the row click | None | `crm.ContactViewerForm` opens for the contact |
| Add a contact | `btn_add_contact_click` | None | `crm.ContactEditorForm` opens in create mode |
| Bulk add tag | `tbar_bulk` action | `contact_service` bulk write | The tag is added to the selected contacts |
| Bulk enrol | `tbar_bulk` action | the campaigns' enrol function (ticket 29's server side) | The selected contacts are enrolled in the chosen sequence |
| Import | `btn_import_click` | None | `crm.ContactImportForm` opens |
| Export | `btn_export_click` | `contact_import_export` export (`server_customers/contact_import_export`) | A CSV of the current view is downloaded |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | People icon (`mi:group`) |
| Heading | `crm.contacts.empty.title` |
| Body text | `crm.contacts.empty.body` |
| Primary action | `crm.contacts.add` (+ Add Contact) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; Admin may edit only the listed writable fields (`writable_fields.contacts`).

## 7. Text

| Phrase key | English text |
|---|---|
| `crm.contacts.title` | Contacts |
| `crm.contacts.search` | Search contacts |
| `crm.contacts.add` | + Add Contact |
| `crm.contacts.import` | Import contacts |
| `crm.contacts.export` | Export CSV |
| `crm.segments.new_leads` | New Leads |
| `crm.segments.active_customers` | Active Customers |
| `crm.segments.inactive_customers` | Inactive Customers |
| `crm.segments.high_value` | High Value |
| `crm.status.lead` | Lead |
| `crm.status.customer` | Customer |
| `crm.status.inactive` | Inactive |
| `crm.bulk.tag` | Add tag |
| `crm.bulk.enrol` | Enrol in campaign |
| `crm.contacts.empty.title` | No contacts yet |
| `crm.contacts.empty.body` | Contacts are created automatically from bookings and enquiries, or you can add one now. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Finding and managing contacts | `master_template` help entry `crm.contacts` — played in the app language |
| Importing contacts | `master_template` help entry `crm.contacts.import` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-21 Contact management | The overview and the contact grid; bulk actions; import and export |
| UF-20 Lead capture | Captured leads appear here with status `lead` |
| UF-23 / UF-24 Campaigns | The bulk enrol action |
