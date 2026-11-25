---
title: "BlogPostTwoColumnTemplate-requirement"
status: draft
doc-id: "mb-d3151b33"
updated: "2026-11-25"
---
# BlogPostTwoColumnTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogPostForm.BlogPostTwoColumnTemplate` |
| Purpose | The two-column blog post with sidebar: main article plus sidebar (ticket 36, F-21; DESIGN.md Blog Post Templates) |
| Screen type and width | Public page — 900 px (a template of the blog post) |
| Layout | Hosted by `blog.BlogPostForm` |
| Route | None — a local template |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Two-column container | Anvil `FlowPanel` (`flow_post_two_column`) | The article and the sidebar in one row that stacks on a phone | — | mobile | — |
| Article column | Anvil `ColumnPanel` (`col_post_article`) | The main article | — | mobile | — |
| Sidebar column | Anvil `ColumnPanel` (`col_post_sidebar`) | The sidebar may include author bio, related posts, categories and newsletter content (DESIGN.md) | — | mobile | — |
| Author card | `m3.Card` (`card_post_author`, outlined) with `m3.Avatar` | The author's bio | `surface-card` | mobile | — |
| Related posts card | `m3.Card` (`card_related_posts`, outlined) | Related posts by tags (DESIGN.md Blog Admin Controls: related post strategy by tags) | `surface-card` | mobile | — |
| Categories card | `m3.Card` (`card_post_categories`, outlined) | The blog's categories | `surface-card` | mobile | — |
| Reader share | `components.ReaderShareComponent` | Below the title and at the end (ticket 14) | — | mobile | — |

Best for engagement and internal linking (DESIGN.md).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Article | `blog_posts` | `content`, `pullquote` |
| Author | `users` | `full_name`, `photo` (`blog_posts.author_id`) |
| Related posts | `blog_posts` | posts sharing the post's `tags` |
| Categories | `blog_categories` | `name` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open a related post | the related-post link | None | `blog.BlogPostForm` opens for it |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the template renders its content |
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
| `blog.related.title` | Related posts |
| `blog.categories.title` | Categories |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The blog post's video covers the templates |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The two-column template |
| UF-20 Lead capture | The sidebar's newsletter content links to a capture form |
