---
title: "PageListForm-requirement"
status: draft
doc-id: "mb-82a50abb"
updated: "2026-11-25"
---
# PageListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.PageListForm` |
| Purpose | The admin pages list: name, SEO title, address and published state (ticket 33, F-06; UF-16) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Content & Website → Pages) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_page_list`) | "Pages", `headline-large` | `page-title` | — | `pages.title` |
| Pages grid | Anvil `DataGrid` (`dg_pages`) | Name, public name, SEO title, address, published | — | mobile | — |
| Page row | Anvil `DataRowPanel` (`drp_page_*`) | One page per row | — | — | — |
| Published switch | `m3.Switch` (`sw_page_published_*`) | The page's published state (`pages.is_published`) | — | — | `pages.published` | — | — |
| Homepage link | `m3.Button` (`btn_open_homepage_editor`, filled) | Opens `public_pages.HomepageEditorForm` (the homepage's own editor) | `primary-button` | — | `homepage.title` |
| Media button | `m3.Button` (`btn_open_media`, outlined) | Opens `public_pages.MediaLibraryForm` | `secondary-button` | — | `media.title` |
| Help icon | `components.HelpTooltipComponent` (help entry `public_pages.page_list`) | The feature's tooltip and help video | — | — | `help.public_pages.page_list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `pages` | `name`, `page_name`, `page_title`, `slug`, `is_published`, `updated_by`, `updated_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_pages` | `page_content_service` read (`server_settings/page_content_service`) | The grid arrives |
| Edit a page's meta | the row's fields | `page_content_service` save | The name, SEO title, address and published state are saved (UF-16: page name, SEO title, address and published state) |
| Publish / unpublish | `sw_page_published_*_change` | `page_content_service` save | The page goes live or comes down; every save is audited (AC-S12) |
| Open the homepage editor | `btn_open_homepage_editor_click` | None | `public_pages.HomepageEditorForm` opens |
| Open the media library | `btn_open_media_click` | None | `public_pages.MediaLibraryForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Page icon (`mi:web`) |
| Heading | `pages.empty.title` |
| Body text | `pages.empty.body` |
| Primary action | `homepage.title` |

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
| `pages.title` | Pages |
| `pages.published` | Published |
| `pages.seo_title` | SEO title |
| `pages.address` | Address |
| `pages.empty.title` | No pages yet |
| `pages.empty.body` | Your website's pages appear here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing your website's pages | `master_template` help entry `public_pages.page_list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | Managing pages (UF-16) |
