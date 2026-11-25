---
title: "ServiceCategoriesForm-requirement"
status: draft
doc-id: "mb-98dfefd2"
updated: "2026-11-25"
---
# ServiceCategoriesForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `services.ServiceCategoriesForm` |
| Purpose | Manage the service categories the catalogue and the public Services page group by (ticket 21, F-10) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `services.ServicesForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_categories`) | "Service categories", `headline-large` | `page-title` | — | `services.categories` |
| Categories list | Anvil `ColumnPanel` (`col_categories`) | The categories in order | — | mobile | — |
| Category row | `m3.ListItem` (`li_category_*`) | One category per row: name, service count | — | — | — |
| Category name field | `m3.TextBox` (`txt_category_name_*`, outlined) | The category's name, editable in place | `outlined-input` | — | `services.category` |
| Delete button | `m3.IconButton` (`ibtn_delete_category_*`, icon `mi:delete`) | Removes a category with no services | `icon-button` | — | `services.category.delete` |
| Add category field | `m3.TextBox` (`txt_new_category`, outlined) | The new category's name | `outlined-input` | — | `services.category.new` |
| Add button | `m3.Button` (`btn_add_category`, filled) | Adds the category | `primary-button` | — | `services.category.add` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms each change | — | — | `services.categories.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `services.categories`) | The feature's tooltip and help video | — | — | `help.services.categories.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Categories | `services` | `category` values (the category list is the distinct set the services carry; the schema holds no separate categories table) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the categories | `form_show` → `load_categories` | `catalogue_service` categories read (`server_services/catalogue_service`) | The categories and their counts arrive |
| Rename a category | `txt_category_name_*_change` + save | `catalogue_service` save | The category is renamed on its services |
| Add a category | `btn_add_category_click` | `catalogue_service` save | The new category is available to the services |
| Delete a category | `ibtn_delete_category_*_click` | `catalogue_service` save | A category with no services is removed |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Service icon (`mi:spa`) |
| Heading | `services.categories.empty.title` |
| Body text | `services.categories.empty.body` |
| Primary action | `services.category.add` |

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
| `services.categories` | Categories |
| `services.category.new` | New category name |
| `services.category.add` | Add category |
| `services.category.delete` | Delete category |
| `services.categories.saved` | Categories saved. |
| `services.categories.empty.title` | No categories yet |
| `services.categories.empty.body` | Categories group your services on your website and in your catalogue. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing service categories | `master_template` help entry `services.categories` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05 Services appointment booking | The public Services page groups by these categories |
| UF-16 Content management | The categories appear on the public website |
