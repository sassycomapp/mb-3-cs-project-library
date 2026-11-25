---
title: "DealEditorForm-requirement"
status: draft
doc-id: "mb-7e6392df"
updated: "2026-11-25"
---
# DealEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `sales.DealEditorForm` |
| Purpose | Create or edit a deal: title, contact or company, owner, value, expected close date, linked quote, lost reason (ticket 38, F-35; UF-40) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `sales.DealBoardForm` and the quick actions |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_deal_editor`) | "New deal" / the deal's title, `headline-large` | `page-title` | — | `deals.new` |
| Title field | `m3.TextBox` (`txt_deal_title`, outlined) | The deal's title | `outlined-input` | — | `deals.title` |
| Contact selector | `m3.DropdownMenu` (`dd_deal_contact`) | The contact (or the company below) | `outlined-select` | — | `invoices.customer` |
| Company selector | `m3.DropdownMenu` (`dd_deal_company`, `allow_none=True`) | The optional company | `outlined-select` | — | `crm.companies.title` |
| Owner selector | `m3.DropdownMenu` (`dd_deal_owner`) | The salesperson | `outlined-select` | — | `tasks.filter.owner` |
| Stage selector | `m3.DropdownMenu` (`dd_deal_stage`) | The stage, starting at `lead` (UF-40 step 1) | `outlined-select` | — | `deals.stage.lead` |
| Value field | `m3.TextBox` (`txt_deal_value`, type `number`, outlined) | The deal's value | `outlined-input` | — | `services.price` |
| Expected close date field | `m3.DatePicker` (`dp_deal_close`, outlined) | The expected close date | `outlined-date` | — | `deals.expected_close` |
| Quote selector | `m3.DropdownMenu` (`dd_deal_quote`, `allow_none=True`) | The optional linked quote (Create Quote on a deal, UF-40) | `outlined-select` | — | `quotes.title` |
| Lost reason field | `m3.TextBox` (`txt_deal_lost_reason`, outlined) | Shown for `lost` | `outlined-input` | mobile | `deals.lost_reason` |
| Create Quote button | `m3.Button` (`btn_deal_create_quote`, outlined) | Create Quote on a deal (UF-40) | `secondary-button` | — | `quotes.new` |
| Save button | `m3.Button` (`btn_save_deal`, filled) | Saves the deal | `primary-button` | — | `services.session.save` |
| Cancel button | `m3.Button` (`btn_cancel_deal`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `sales.editor`) | The feature's tooltip and help video | — | — | `help.sales.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Deal | `deals` | `title`, `contact_id`, `company_id`, `owner_id`, `stage`, `value`, `expected_close_date`, `quote_id`, `lost_reason` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the deal | `form_show` → `load_deal` | `deal_service` read (`server_sales/deal_service`) | The fields fill |
| Save | `btn_save_deal_click` | `deal_service` save | The deal is saved; the timeline records it |
| Create Quote | `btn_deal_create_quote_click` | None | `sales.QuoteEditorForm` opens with the deal linked |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the editor always shows its fields |
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

Staff own only.

## 7. Text

| Phrase key | English text |
|---|---|
| `deals.expected_close` | Expected close date |

## 8. Help Videos

| Process | Help video |
|---|---|
| Creating a deal | `master_template` help entry `sales.editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-40 Deals, companies and quotes | The deal's record |
