---
title: "BlogListListTemplate-requirement"
status: draft
doc-id: "mb-d124a790"
updated: "2026-11-25"
---
# BlogListListTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogListForm.BlogListListTemplate` |
| Purpose | The list-with-thumbnail blog listing: row-based layout with image and content split (ticket 36, F-21; DESIGN.md Blog List Views) |
| Screen type and width | Public page — 900 px (a view of the blog listing) |
| Layout | Hosted by `blog.BlogListForm` |
| Route | None — a local template |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Rows | Anvil `ColumnPanel` (`col_blog_list_rows`) | The posts in rows | — | mobile | — |
| Post row | `m3.ListItem` (`li_blog_post_*`, clickable, `three_line=True`) | Row-based layout with image and content split (DESIGN.md List with Thumbnail View); alternating row backgrounds where appropriate | — | — | — |
| Thumbnail | `m3.ListItem`'s `leading_image` | The post's featured image, 56 px square in the item's leading slot | — | — | — |
| Metadata | the item's `trailing_text` | The post's date | — | — | — |

Better for denser reading and editorial scan patterns (DESIGN.md).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Rows | `blog_posts` | `featured_image`, `title`, `excerpt`, `published_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open a post | `li_blog_post_*_click` | None | `blog.BlogPostForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the list shows the rows |
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

*(No phrase keys of its own: the texts are the client's own content.)*


## 8. Help Videos

| Process | Help video |
|---|---|
| None | The blog listing's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The list view |
