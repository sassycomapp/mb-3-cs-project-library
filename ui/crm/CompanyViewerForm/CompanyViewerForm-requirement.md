---
title: "CompanyViewerForm-requirement"
status: draft
doc-id: "mb-4b868ccd"
updated: "2026-11-25"
---
# CompanyViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.CompanyViewerForm` |
| Purpose | A company's details and its contacts, deals, quotes and invoices (ticket 20, F-35; UF-21) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `crm.CompanyListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_company_viewer`) | The company's name, `headline-large` | `page-title` | — | — |
| Details panel | `m3.Card` (`card_company_details`, outlined) | Website, phone, email, address, tax number, industry | `surface-card` | mobile | — |
| Edit button | `m3.Button` (`btn_edit_company`, outlined) | Opens the company's edit panel | `secondary-button` | — | `users.row.edit` |
| Contacts panel | `m3.Card` (`card_company_contacts`, outlined) | The company's contacts | `surface-card` | mobile | — |
| Contact row | `m3.ListItem` (`li_company_contact_*`, clickable) | One contact per row; the click opens `crm.ContactViewerForm` | — | — | — |
| Deals panel | `m3.Card` (`card_company_deals`, outlined) | The company's deals | `surface-card` | mobile | — |
| Quotes panel | `m3.Card` (`card_company_quotes`, outlined) | The company's quotes | `surface-card` | mobile | — |
| Invoices panel | `m3.Card` (`card_company_invoices`, outlined) | The company's invoices (through its contacts) | `surface-card` | mobile | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.company_viewer`) | The feature's tooltip and help video | — | — | `help.crm.company_viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Details | `companies` | `name`, `website`, `phone`, `email`, `address`, `tax_number`, `industry`, `notes` |
| Contacts | `contacts` | rows linked by `company_id` |
| Deals | `deals` | rows linked by `company_id` |
| Quotes | `quotes` | rows linked by `company_id` |
| Invoices | `invoice` | rows linked to the company's contacts |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the company | `form_show` → `load_company` | `company_service` read (`server_customers/company_service`) | The company and its records arrive |
| Edit | `btn_edit_company_click` | `company_service` save | The details panel switches to its fields and saves |
| Open a contact | `li_company_contact_*_click` | None | `crm.ContactViewerForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Building icon (`mi:business`) |
| Heading | `crm.company.empty.title` |
| Body text | `crm.company.empty.body` |
| Primary action | `users.row.edit` (Edit) |

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
| `crm.company.website` | Website |
| `crm.company.phone` | Phone |
| `crm.company.email` | Email |
| `crm.company.tax_number` | Tax number |
| `crm.company.industry` | Industry |
| `crm.company.contacts.title` | Contacts |
| `crm.company.deals.title` | Deals |
| `crm.company.quotes.title` | Quotes |
| `crm.company.invoices.title` | Invoices |
| `crm.company.empty.title` | Nothing here yet |
| `crm.company.empty.body` | Add the company's details and link its contacts to see everything here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading a company's record | `master_template` help entry `crm.company_viewer` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-21 Contact management | The Companies section of the CRM |
| UF-40 Deals, companies and quotes | The company's deals and quotes |
