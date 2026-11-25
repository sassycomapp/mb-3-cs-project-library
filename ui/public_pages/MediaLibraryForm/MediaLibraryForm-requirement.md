---
title: "MediaLibraryForm-requirement"
status: draft
doc-id: "mb-3624c91f"
updated: "2026-11-25"
---
# MediaLibraryForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.MediaLibraryForm` |
| Purpose | Content & Website → Media: the media library the pages, services and blog draw from (ticket 33, F-06) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Content & Website → Media) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_media`) | "Media", `headline-large` | `page-title` | — | `media.title` |
| Upload field | `m3.FileLoader` (`fu_media_upload`, `multiple=True`, `file_types="image/*,video/*"`) | Uploads media into `files` | — | — | `media.upload` |
| Search field | `m3.SearchBar` (`srch_media`) | Search media by name | — | — | `media.search` |
| Media grid | Anvil `DataGrid` (`dg_media`) | Thumbnail, name, path, uploaded | — | mobile | — |
| Media row | Anvil `DataRowPanel` (`drp_media_*`) | One file per row | — | — | — |
| Row actions | `m3.IconButton` (`ibtn_copy_media_*`, `ibtn_delete_media_*`) | Copy the file's address; delete an unused file | `icon-button` | — | `media.copy`, `media.delete` |
| Help icon | `components.HelpTooltipComponent` (help entry `public_pages.media`) | The feature's tooltip and help video | — | — | `help.public_pages.media.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Files | `files` | `path`, `file`, `file_version`, `created_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the library | `form_show` → `load_media` | `page_content_service` media read (`server_settings/page_content_service`) | The grid arrives |
| Upload | `fu_media_upload_change` | `page_content_service` media save | The files are stored in `files` (F-06: images are uploaded to `files`) |
| Delete | `ibtn_delete_media_*_click` | `page_content_service` media delete | An unused file is removed; one in use is refused with a message |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Image icon (`mi:image`) |
| Heading | `media.empty.title` |
| Body text | `media.empty.body` |
| Primary action | The file loader's own upload action |

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
| `media.title` | Media |
| `media.upload` | Upload media |
| `media.search` | Search media |
| `media.copy` | Copy address |
| `media.delete` | Delete |
| `media.empty.title` | No media yet |
| `media.empty.body` | The images and videos your website uses live here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Using the media library | `master_template` help entry `public_pages.media` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management and edit mode | The uploads edit mode makes |
| UF-25 Homepage customisation | The hero image and testimonial photos come from here |
