---
title: "InvoicingTab-requirement"
status: draft
doc-id: "mb-e96e73ca"
updated: "2026-11-25"
---
# InvoicingTab — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.InvoicingTab` |
| Purpose | The Settings → Invoicing tab: default payment terms, tax rate and label, tax registration number, invoice numbering, default quote validity (ticket 18, F-05; architecture reference F-05) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm` |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_invoicing_tab`) | The tab's one column | — | mobile | — |
| Payment terms field | `m3.TextBox` (`txt_payment_terms`, type `number`, outlined) | The default days until an invoice is due; 0 = due on receipt | `outlined-input` | — | `invoicing.payment_terms` |
| Tax rate field | `m3.TextBox` (`txt_tax_rate`, type `number`, outlined) | A percentage, default 0 (no tax) | `outlined-input` | — | `invoicing.tax_rate` |
| Tax label field | `m3.TextBox` (`txt_tax_label`, outlined) | The name shown on invoices, for example VAT, GST or Sales Tax | `outlined-input` | — | `invoicing.tax_label` |
| Tax registration field | `m3.TextBox` (`txt_tax_registration`, outlined) | Shown on invoices when set | `outlined-input` | — | `invoicing.tax_registration` |
| Numbering display | `m3.Text` (`text_numbering`) | The next invoice and quote numbers (`INV-{year}-{sequence}`, `QT-{year}-{sequence}`); numbering is automatic and not editable (architecture reference F-15) | `body-small` | — | `invoicing.numbering` |
| Quote validity field | `m3.TextBox` (`txt_quote_validity`, type `number`, outlined) | The default quote validity in days (F-35: validity date default from Settings → Invoicing) | `outlined-input` | — | `invoicing.quote_validity` |
| Save button | `m3.Button` (`btn_save_invoicing`, filled) | The tab's one filled action | `primary-button` | — | `invoicing.save` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms the save | — | — | `invoicing.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.invoicing`) | The tab's tooltip and help video | — | — | `help.settings.invoicing.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Default payment terms | `business_profile` | `default_payment_terms_days` |
| Tax rate | `business_profile` | `tax_rate` |
| Tax label | `business_profile` | `tax_label` |
| Tax registration number | `business_profile` | `tax_registration_number` |
| Next numbers | `config` | `value` of `invoice_sequence` and `quote_sequence` (read-only display) |
| Quote validity | `config` | the quote-validity default key |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_invoicing` | `settings_service` read (`server_settings/settings_service`) | The values fill the fields |
| Save | `btn_save_invoicing_click` | `settings_service` save (`server_settings/settings_service`) | The defaults are saved; the tax rate is a percentage (default 0); the snackbar confirms |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the tab always shows its fields |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.Invoicing`.

## 7. Text

| Phrase key | English text |
|---|---|
| `invoicing.payment_terms` | Default payment terms (days; 0 = due on receipt) |
| `invoicing.tax_rate` | Tax rate (%) |
| `invoicing.tax_label` | Tax label |
| `invoicing.tax_registration` | Tax registration number |
| `invoicing.numbering` | Invoice and quote numbers are issued automatically: {invoice_number}, {quote_number} |
| `invoicing.quote_validity` | Default quote validity (days) |
| `invoicing.save` | Save |
| `invoicing.saved` | Invoicing defaults saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting the invoicing defaults | `master_template` help entry `settings.invoicing` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-14 Invoices | The default terms and tax the invoice editor starts from |
| UF-40 Deals and quotes | The default quote validity |
| UF-32 Expenses and accounts | The tax figures the accounts reports use |
