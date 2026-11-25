---
title: "BlogAdminListForm-requirement"
status: draft
doc-id: "mb-d26027e4"
updated: "2026-11-25"
---
# BlogAdminListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogAdminListForm` |
| Purpose | The admin blog list: Content & Website → Blog, with Share on each published post (ticket 36, F-21) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_blog_admin`) | "Blog", `headline-large` | `page-title` | — | `blog.admin.title` |
| New Post button | `m3.Button` (`btn_new_post`, filled) | Opens BlogEditorForm in create mode (DESIGN.md empty states: "+ New Post") | `primary-button` | — | `blog.admin.new` |
| Status filter | `m3.DropdownMenu` (`dd_blog_status`) | Draft, Scheduled, Published, Archived (`blog_posts.status`) | `outlined-select` | — | `blog.admin.status` |
| Search field | `m3.SearchBar` (`srch_blog_admin`) | Search posts by title | — | — | `blog.admin.search` |
| Posts grid | Anvil `DataGrid` (`dg_blog_posts`) | Title, category, template, status, published, views | — | mobile | — |
| Post row | Anvil `DataRowPanel` (`drp_blog_post_*`) | One post per row | — | — | — |
| Status badge | `m3.Chip` (`chip_post_status_*`, type `input`, removable=False) | The post's status | — | — | — |
| Share button | `m3.IconButton` (`ibtn_share_post_*`, icon `mi:share`) | Share on each published post; opens `components.SharePanelComponent` | `icon-button` | — | `blog.admin.share` | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `blog.admin_list`) | The feature's tooltip and help video | — | — | `help.blog.admin_list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `blog_posts` | `title`, `category_id`, `template`, `status`, `published_at`, `view_count` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_posts` | `blog_service` list read (`server_blog/blog_service`) | The grid arrives |
| New post | `btn_new_post_click` | None | `blog.BlogEditorForm` opens in create mode |
| Open a post | the row click | None | `blog.BlogEditorForm` opens |
| Share a post | `ibtn_share_post_*_click` | `generate_share_captions` (`server_blog/share_service`) | The share panel opens with the post's captions (ticket 13) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Article icon (`mi:article`) |
| Heading | `blog.empty.title` |
| Body text | `blog.empty.body` |
| Primary action | `blog.admin.new` (+ New Post) |

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
| `blog.admin.title` | Blog |
| `blog.admin.new` | + New Post |
| `blog.admin.status` | Status |
| `blog.admin.search` | Search posts |
| `blog.admin.share` | Share |
| `blog.status.draft` | Draft |
| `blog.status.scheduled` | Scheduled |
| `blog.status.published` | Published |
| `blog.status.archived` | Archived |
| `blog.empty.title` | No blog posts yet |
| `blog.empty.body` | Blog posts help you share expertise and attract visitors to your site. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing your blog | `master_template` help entry `blog.admin_list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The blog's admin half |
| UF-42 Sharing a blog post | The Share button on each published post |
