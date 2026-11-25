---
title: "CompanyListForm-requirement"
status: draft
doc-id: "mb-8d898bfc"
updated: "2026-11-25"
---
# CompanyListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.CompanyListForm` |
| Purpose | The list of business customers: their details and their contacts, deals, quotes and invoices (ticket 20, F-35; UF-21 Companies) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_companies`) | "Companies", `headline-large` | `page-title` | — | `crm.companies.title` |
| Search field | `m3.SearchBar` (`srch_companies`) | Search companies by name | — | — | `crm.companies.search` |
| Add Company button | `m3.Button` (`btn_add_company`, filled) | The primary action; opens the company editor (DESIGN.md empty states: "+ Add Company") | `primary-button` | — | `crm.companies.add` |
| Company grid | Anvil `DataGrid` (`dg_companies`) | Name, industry, phone, email, contacts count | — | mobile | — |
| Company row | Anvil `DataRowPanel` (`drp_company_*`) | One company per row | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.companies`) | The feature's tooltip and help video | — | — | `help.crm.companies.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `companies` | `name`, `industry`, `phone`, `email` |
| Contacts count | `contacts` | rows linked by `company_id` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_companies` | `company_service` list read (`server_customers/company_service`) | The grid arrives |
| Search | `srch_companies_search` | the same read | The grid reloads |
| Open a company | the row click | None | `crm.CompanyViewerForm` opens |
| Add a company | `btn_add_company_click` | `company_service` create | A new company is created; its viewer opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Building icon (`mi:business`) |
| Heading | `crm.companies.empty.title` (DESIGN.md: "No companies yet") |
| Body text | `crm.companies.empty.body` (DESIGN.md: "Companies group the contacts of a business customer.") |
| Primary action | `crm.companies.add` (+ Add Company) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `crm.companies.title` | Companies |
| `crm.companies.search` | Search companies |
| `crm.companies.add` | + Add Company |
| `crm.companies.empty.title` | No companies yet |
| `crm.companies.empty.body` | Companies group the contacts of a business customer. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Working with companies | `master_template` help entry `crm.companies` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-21 Contact management | The Companies section of the CRM |
| UF-40 Deals, companies and quotes | The companies half of the sales feature |
