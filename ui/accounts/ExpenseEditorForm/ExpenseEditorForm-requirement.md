---
title: "ExpenseEditorForm-requirement"
status: draft
doc-id: "mb-cfdcaff8"
updated: "2026-11-25"
---
# ExpenseEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `accounts.ExpenseEditorForm` |
| Purpose | Record an expense: date, supplier, category, amount, tax included, payment method, notes, and the receipt area with Take Photo (on a phone) and Upload (ticket 45, F-28; UF-32; DESIGN.md Accounts Design) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `accounts.ExpenseListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_expense_editor`) | "New expense" / the expense's supplier, `headline-large` | `page-title` | — | `expenses.new` |
| Date field | `m3.DatePicker` (`dp_expense_date`, outlined) | The expense's date | `outlined-date` | — | `expenses.date` |
| Supplier field | `m3.TextBox` (`txt_expense_supplier`, outlined) | The supplier | `outlined-input` | — | `expenses.supplier` |
| Category selector | `m3.DropdownMenu` (`dd_expense_cat`) | The category (`expense_categories`, seeded with common categories and editable) | `outlined-select` | — | `expenses.filter.category` |
| Amount field | `m3.TextBox` (`txt_expense_amount`, type `number`, outlined) | Total paid, tax included | `outlined-input` | — | `expenses.amount` |
| Tax included field | `m3.TextBox` (`txt_expense_tax`, type `number`, outlined) | The tax included in the amount; 0 when none | `outlined-input` | — | `invoicing.tax_line` |
| Payment method selector | `m3.DropdownMenu` (`dd_expense_method`) | Card, bank transfer, cash, other | `outlined-select` | — | `payments.method.other` |
| Notes field | `m3.TextArea` (`txt_expense_notes`, outlined) | The notes | `outlined-input` | mobile | `invoices.notes` |
| Receipt area | `m3.Card` (`card_receipt`, outlined) | The receipt shows as a thumbnail (DESIGN.md Accounts Design) | `surface-card` | mobile | — |
| Take Photo button | `m3.Button` (`btn_take_photo`, outlined, icon `mi:photo_camera`) | On a phone, captures the receipt | `secondary-button` | — | `expenses.take_photo` |
| Upload field | `m3.FileLoader` (`fu_receipt`, `file_types="image/*"`) | Uploads the receipt image | — | — | `expenses.upload_receipt` |
| Receipt thumbnail | Anvil `Image` (`img_receipt`) | The receipt image (`expenses.receipt_r2_key`) | — | — | — |
| Save button | `m3.Button` (`btn_save_expense`, filled) | Saves the expense | `primary-button` | — | `services.session.save` |
| Void button | `m3.Button` (`btn_void_expense`, outlined) | Marks the expense void; never deleted (UF-32 step 3) | `secondary-button` | — | `expenses.void` |
| Cancel button | `m3.Button` (`btn_cancel_expense`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `accounts.expense_editor`) | The feature's tooltip and help video | — | — | `help.accounts.expense_editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Expense | `expenses` | `expense_id`, `expense_date`, `supplier`, `category_id`, `description`, `amount`, `tax_amount`, `currency`, `payment_method`, `status`, `recorded_by`, `notes` |
| Receipt | `expenses` | `receipt_r2_key` — the image in R2 at `expenses/receipts/` (ticket 45) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the expense | `form_show` → `load_expense` | `expense_service` read (`server_accounts/expense_service`) | The fields fill |
| Save | `btn_save_expense_click` | `record_expense` (`server_accounts/expense_service`; Owner/Manager) | The expense is recorded; the receipt image goes to the client's bucket at `expenses/receipts/` with the key in `receipt_r2_key` |
| Take a photo | `btn_take_photo_click` | None | The device camera feeds the receipt upload |
| Void | `btn_void_expense_click` | `expense_service` void | The expense is voided; it is left out of every report and export |

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
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `expenses.date` | Date |
| `expenses.supplier` | Supplier |
| `expenses.amount` | Amount (tax included) |
| `expenses.take_photo` | Take photo |
| `expenses.upload_receipt` | Upload receipt |
| `expenses.void` | Void this expense |

## 8. Help Videos

| Process | Help video |
|---|---|
| Recording an expense with a receipt | `master_template` help entry `accounts.expense_editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-32 Expenses, accounts reports and export | Recording an expense |
