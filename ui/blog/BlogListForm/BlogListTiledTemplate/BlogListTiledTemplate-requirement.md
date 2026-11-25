---
title: "BlogListTiledTemplate-requirement"
status: draft
doc-id: "mb-83f5d668"
updated: "2026-11-25"
---
# BlogListTiledTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogListForm.BlogListTiledTemplate` |
| Purpose | The tiled grid blog listing: multi-column grid on desktop, card-based (ticket 36, F-21; DESIGN.md Blog List Views) |
| Screen type and width | Public page — 900 px (a view of the blog listing) |
| Layout | Hosted by `blog.BlogListForm` |
| Route | None — a local template |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Grid | Anvil `GridPanel` (`grid_blog_tiled`) | Multi-column grid on desktop that stacks on a phone | — | mobile | — |
| Post card | `m3.InteractiveCard` (`icard_blog_tiled_*`, outlined) | Featured image, category badge, title, excerpt, metadata, read-more action (DESIGN.md Tiled Grid View) | `surface-card` | mobile (320 px card in a grid) | — |
| Category badge | `m3.Chip` (`chip_post_category_*`, type `input`, removable=False) | The post's category | — | — | — |
| Read-more link | `m3.Link` (`lnk_read_more_*`) | Opens the post | `tertiary-button` | — | `blog.read_more` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Cards | `blog_posts` | `featured_image`, `category_id`, `title`, `excerpt`, `published_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open a post | `icard_blog_tiled_*_click` | None | `blog.BlogPostForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the grid shows the cards |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

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
| `blog.read_more` | Read more |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The blog listing's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The tiled view |
