---
title: "BlogListForm-requirement"
status: draft
doc-id: "mb-ad38b21b"
updated: "2026-11-25"
---
# BlogListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogListForm` |
| Purpose | The public blog listing in the tiled grid or list-with-thumbnail view, loaded by `business_profile.blog_list_view` (ticket 36, F-21) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/blog` — Anvil Routing; `Route.meta` sets the share tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_blog_list`) | The page's one content column | — | mobile | — |
| Template host | Anvil `ColumnPanel` (`col_blog_template`) | The local template chosen by `business_profile.blog_list_view` | — | mobile | — |
| Category filter | `m3.DropdownMenu` (`dd_blog_category`, `allow_none=True`) | The posts' filing categories (F-21: categories) | `outlined-select` | — | `blog.filter.category` |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |
| Edit mode host | `components.EditModeComponent` | Placed once on the page (ticket 11) | — | — | `editmode.button` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Posts | `blog_posts` | published posts' public subset: `title`, `slug`, `excerpt`, `featured_image`, `published_at`, `category_id`, `view_count` |
| Categories | `blog_categories` | `name` |
| List view | `business_profile` | `blog_list_view` (`tiled` / `list`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` / `blog_service` public read | The published posts arrive; `Route.meta` sets the share tags |
| Open a post | the template's post card click | None | `blog.BlogPostForm` opens for the post |
| Filter by category | `dd_blog_category_change` | None | The template reloads with the category's posts |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Article icon (`mi:article`) |
| Heading | `blog.empty.title` |
| Body text | `blog.empty.body` |
| Primary action | None — posts arrive as the client publishes |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

## 7. Text

| Phrase key | English text |
|---|---|
| `blog.filter.category` | Category |
| `blog.empty.title` | No posts yet |
| `blog.empty.body` | Posts appear here as they are published. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The blog's public listing |
| UF-20 Lead capture | A post's call to action links to a landing page |
