---
title: "BlogPostFullWidthTemplate-requirement"
status: draft
doc-id: "mb-44f48ad5"
updated: "2026-11-25"
---
# BlogPostFullWidthTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogPostForm.BlogPostFullWidthTemplate` |
| Purpose | The full-width immersive blog post: full-width visual treatment, pullquotes and generous line height (ticket 36, F-21; DESIGN.md Blog Post Templates) |
| Screen type and width | Public page — 900 px (a template of the blog post) |
| Layout | Hosted by `blog.BlogPostForm` |
| Route | None — a local template |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Template container | Anvil `ColumnPanel` (`col_post_full_width`) | The immersive article | — | mobile | — |
| Featured image | Anvil `Image` (`img_post_featured_full`) | The full-width featured image | — | mobile | — |
| Pullquote | `m3.Heading` (`hdg_post_pullquote_full`) | The pullquote with generous line height | `headline-small` | mobile | — |
| Content | `m3.Text` (`text_post_content_full`) | The post's content | `body-large` | mobile | — |
| Reader share | `components.ReaderShareComponent` | Below the title and at the end (ticket 14) | — | mobile | — |

Best for image-rich or storytelling content (DESIGN.md).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Content | `blog_posts` | `featured_image`, `content`, `pullquote` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None of its own | — | — | The template renders |

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

*(No phrase keys of its own: the texts are the client's own content.)*


## 8. Help Videos

| Process | Help video |
|---|---|
| None | The blog post's video covers the templates |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The full-width immersive template |
