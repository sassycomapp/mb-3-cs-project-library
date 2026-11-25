---
title: "ExpenseListForm-requirement"
status: draft
doc-id: "mb-c4f2b05a"
updated: "2026-11-25"
---
# ExpenseListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `accounts.ExpenseListForm` |
| Purpose | The expenses list with their statuses (ticket 45, F-28; UF-32) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Expenses) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_expenses`) | "Expenses", `headline-large` | `page-title` | — | `expenses.title` |
| New Expense button | `m3.Button` (`btn_new_expense`, filled) | Opens ExpenseEditorForm in create mode (DESIGN.md empty states: "+ New Expense") | `primary-button` | — | `expenses.new` |
| Category filter | `m3.DropdownMenu` (`dd_expense_category`, `allow_none=True`) | Filter by category | `outlined-select` | — | `expenses.filter.category` |
| Search field | `m3.SearchBar` (`srch_expenses`) | Search by supplier or description | — | — | `expenses.search` |
| Expenses grid | Anvil `DataGrid` (`dg_expenses`) | Date, supplier, category, amount, tax, payment method, status | — | mobile | — |
| Expense row | Anvil `DataRowPanel` (`drp_expense_*`) | One expense per row | — | — | — |
| Status badge | `m3.Chip` (`chip_expense_status_*`, type `input`, removable=False) | Recorded or Void | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `accounts.expenses`) | The feature's tooltip and help video | — | — | `help.accounts.expenses.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `expenses` | `expense_date`, `supplier`, `category_id`, `amount`, `tax_amount`, `currency`, `payment_method`, `status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_expenses` | `expense_service` list read (`server_accounts/expense_service`) | The grid arrives (void expenses are left out of every report and export, but they are shown here with their badge) |
| Filter / search | `dd_expense_category_change`, `srch_expenses_search` | the same read | The grid reloads |
| New expense | `btn_new_expense_click` | None | `accounts.ExpenseEditorForm` opens |
| Open an expense | the row click | None | `accounts.ExpenseEditorForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Receipt icon (`mi:receipt_long`) |
| Heading | `expenses.empty.title` (DESIGN.md: "No expenses yet") |
| Body text | `expenses.empty.body` (DESIGN.md: "Record what the business spends, with a photo of each receipt.") |
| Primary action | `expenses.new` (+ New Expense) |

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
| `expenses.title` | Expenses |
| `expenses.new` | + New Expense |
| `expenses.filter.category` | Category |
| `expenses.search` | Search expenses |
| `expenses.status.recorded` | Recorded |
| `expenses.status.void` | Void |
| `expenses.empty.title` | No expenses yet |
| `expenses.empty.body` | Record what the business spends, with a photo of each receipt. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Recording expenses | `master_template` help entry `accounts.expenses` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-32 Expenses, accounts reports and export | The expenses list |
