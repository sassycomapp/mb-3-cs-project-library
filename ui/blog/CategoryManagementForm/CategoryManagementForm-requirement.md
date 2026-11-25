---
title: "CategoryManagementForm-requirement"
status: draft
doc-id: "mb-9a8f9044"
updated: "2026-11-25"
---
# CategoryManagementForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.CategoryManagementForm` |
| Purpose | The blog's filing categories (ticket 36, F-21) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_blog_categories`) | "Blog categories", `headline-large` | `page-title` | — | `blog.categories.title` |
| Categories list | Anvil `ColumnPanel` (`col_blog_categories`) | The categories in order | — | mobile | — |
| Category row | `m3.ListItem` (`li_blog_category_*`) | One category per row: name, post count | — | — | — |
| Category name field | `m3.TextBox` (`txt_blog_category_name_*`, outlined) | Editable in place | `outlined-input` | — | `blog.filter.category` |
| Delete button | `m3.IconButton` (`ibtn_delete_blog_category_*`, icon `mi:delete`) | Removes an unused category | `icon-button` | — | `services.category.delete` |
| Add field | `m3.TextBox` (`txt_new_blog_category`, outlined) | The new category's name | `outlined-input` | — | `services.category.new` |
| Add button | `m3.Button` (`btn_add_blog_category`, filled) | Adds the category | `primary-button` | — | `services.category.add` |
| Help icon | `components.HelpTooltipComponent` (help entry `blog.categories`) | The feature's tooltip and help video | — | — | `help.blog.categories.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Categories | `blog_categories` | `name`, `slug`, `description` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the categories | `form_show` → `load_categories` | `blog_service` categories read (`server_blog/blog_service`) | The categories arrive |
| Rename | `txt_blog_category_name_*_change` + save | `blog_service` save | The category is renamed on its posts |
| Add | `btn_add_blog_category_click` | `blog_service` save | The new category is available |
| Delete | `ibtn_delete_blog_category_*_click` | `blog_service` save | An unused category is removed |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Article icon (`mi:article`) |
| Heading | `blog.categories.empty.title` |
| Body text | `blog.categories.empty.body` |
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
| `blog.categories.title` | Blog categories |
| `blog.categories.empty.title` | No categories yet |
| `blog.categories.empty.body` | Categories file your posts so readers find them. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Filing your posts | `master_template` help entry `blog.categories` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The categories the blog lists by |
