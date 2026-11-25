---
title: "QuoteEditorForm-requirement"
status: draft
doc-id: "mb-cc159a04"
updated: "2026-11-25"
---
# QuoteEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `sales.QuoteEditorForm` |
| Purpose | As InvoiceEditorForm, with the validity date in place of payment terms; lines typed or from services, the client's tax rate; actions Save Draft and Send Quote (ticket 38, F-35; DESIGN.md Sales Design) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `sales.QuoteListForm` and the deal's Create Quote |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_quote_editor`) | "New quote" / the quote number, `headline-large` | `page-title` | — | `quotes.new` |
| Customer selector | `m3.DropdownMenu` (`dd_quote_customer`) | The contact | `outlined-select` | — | `invoices.customer` |
| Company selector | `m3.DropdownMenu` (`dd_quote_company`, `allow_none=True`) | The optional company | `outlined-select` | — | `crm.companies.title` |
| Issue date field | `m3.DatePicker` (`dp_quote_issue`, outlined) | The issue date | `outlined-date` | — | `invoices.issue_date` |
| Valid until field | `m3.DatePicker` (`dp_quote_valid`, outlined) | The validity date, in place of payment terms (DESIGN.md Sales Design); the default from Settings → Invoicing | `outlined-date` | — | `quotes.valid_until` |
| Currency display | `m3.Text` (`text_quote_currency`) | The system currency | `body-medium` | — | `invoices.currency` |
| Lines panel | `m3.Card` (`card_quote_lines`, outlined) with the lines grid | Lines typed or from services (`quote_items`) | `surface-card` | mobile | — |
| Lines grid | Anvil `DataGrid` (`dg_quote_items`) | Description, service, quantity, unit price, amount | — | mobile | — |
| Add from service selector | `m3.DropdownMenu` (`dd_quote_service`, `allow_none=True`) | The service a line prices (`quote_items.service_id`) | `outlined-select` | — | `services.name` |
| Add line button | `m3.Button` (`btn_add_quote_line`, outlined) | Adds a typed line | `secondary-button` | — | `invoices.line.add` |
| Tax summary | `m3.Text` (`text_quote_tax_summary`) | Subtotal, tax at the client's rate, total (fixed at issue) | `body-medium` | — | `invoices.subtotal` |
| Notes field | `m3.TextArea` (`txt_quote_notes`, outlined) | The quote's notes | `outlined-input` | — | `invoices.notes` |
| Save Draft button | `m3.Button` (`btn_quote_save_draft`, filled) | The primary action (DESIGN.md Sales Design: actions Save Draft and Send Quote) | `primary-button` | — | `invoices.save_draft` |
| Send Quote button | `m3.Button` (`btn_send_quote`, outlined) | Sends; the PDF is emailed to the customer with a link to accept or decline | `secondary-button` | — | `quotes.send` |
| Cancel button | `m3.Button` (`btn_cancel_quote`, text) | Returns to the list | `tertiary-button` | — | `editmode.bar.cancel` |
| AI helper trigger | `components.AIHelperComponent` (trigger) | The proposal generator assist drafts the quote (F-26) | — | — | `ai.open` |
| Help icon | `components.HelpTooltipComponent` (help entry `sales.quote_editor`) | The feature's tooltip and help video | — | — | `help.sales.quote_editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Quote | `quotes` | `contact_id`, `company_id`, `deal_id`, `issue_date`, `valid_until`, `subtotal`, `tax_rate`, `tax_amount`, `total_amount`, `currency`, `notes` |
| Lines | `quote_items` | `service_id`, `description`, `quantity`, `unit_price`, `amount`, `sort_order` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the quote | `form_show` → `load_quote` | `quote_service` read (`server_sales/quote_service`) | Edit works on drafts (and re-sends for sent quotes) |
| Save Draft | `btn_quote_save_draft_click` | `quote_service` save | Status `draft`; numbering through the `config` sequence keys |
| Send Quote | `btn_send_quote_click` | `quote_service` send | The PDF is generated (`quote_pdf`) and emailed with the `/quote/{token}` link; the quote and a linked deal show on the contact's timeline (UF-40 step 5) |
| AI draft | the AI trigger | `request(assist="proposal", ...)` (`server_ai/ai_request_layer`) | The proposal draft appears for acceptance |

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
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `quotes.valid_until` | Valid until |
| `quotes.send` | Send Quote |
| `quotes.sent` | Your quote is with the customer. They accept or decline at their own link. |
| `quotes.line.service` | Service |

## 8. Help Videos

| Process | Help video |
|---|---|
| Creating and sending a quote | `master_template` help entry `sales.quote_editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-40 Deals, companies and quotes | The quote's build and send |
| UF-41 Customer accepts a quote | The sent quote's acceptance link |
