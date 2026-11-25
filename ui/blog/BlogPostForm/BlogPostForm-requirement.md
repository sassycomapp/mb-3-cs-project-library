---
title: "BlogPostForm-requirement"
status: draft
doc-id: "mb-7b146f85"
updated: "2026-11-25"
---
# BlogPostForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogPostForm` |
| Purpose | The public blog post: the three post templates loaded by `blog_posts.template`, reader share buttons, comments and the pullquote (ticket 36, F-21) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/blog/{slug}` — Anvil Routing; `Route.meta` sets the share tags (spec-sm-syndication section 5) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_blog_post`) | The page's one content column | — | mobile | — |
| Template host | Anvil `ColumnPanel` (`col_post_template`) | The local template chosen by `blog_posts.template` | — | mobile | — |
| Title | `m3.Heading` (`hdg_post_title`) | The post's title | `headline-large` | — | — |
| Reader share | `components.ReaderShareComponent` | The reader share buttons below the post title and at the end of the post (ticket 14) | — | mobile | — |
| Pullquote | `m3.Heading` (`hdg_post_pullquote`) | The post's pullquote, styled by the type role (`blog_posts.pullquote`) | `headline-small` | mobile | — |
| Comments section | `m3.Card` (`card_comments`, outlined) | The approved comments; shown only where comments are switched on (F-21; `business_profile.enable_comments`) | `surface-card` | mobile | — |
| Comment row | `m3.ListItem` (`li_comment_*`) | Commenter name, date, comment text | — | — | — |
| Comment form fields | `m3.TextBox` (`txt_comment_name`), `m3.TextBox` (`txt_comment_email`, type `email`), `m3.TextArea` (`txt_comment_text`) | The visitor's comment | `outlined-input` | mobile | `blog.comment.name`, `blog.comment.email`, `blog.comment.text` |
| Comment submit button | `m3.Button` (`btn_submit_comment`, filled) | Submits the comment for approval | `primary-button` | — | `blog.comment.submit` |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |
| Edit mode host | `components.EditModeComponent` | Placed once on the published post (ticket 11) | — | — | `editmode.button` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The post | `blog_posts` | `title`, `content`, `pullquote`, `featured_image`, `video_url`, `video_type`, `template`, `published_at`, `view_count`, `author_id`, `category_id`, `tags` |
| Comments | `blog_comments` | `commenter_name`, `commenter_email`, `comment_text`, `status` (`approved`) |
| The view | `blog_posts` | `view_count` incremented |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `blog_service` public read (`server_blog/blog_service`) | The post arrives; `Route.meta` sets the share tags; the view is counted |
| Submit a comment | `btn_submit_comment_click` | `comment_service` submit (`server_blog/comment_service`) | The comment is stored as `pending`; only approved comments are public |
| Play the video | via VideoDisplayComponent | `record_consent` | The consent is recorded; the embed loads |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Article icon (`mi:article`) |
| Heading | `blog.post.unavailable.title` |
| Body text | `blog.post.unavailable.body` — the post is not published |
| Primary action | `public.nav.blog` |

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
| `blog.comment.name` | Name |
| `blog.comment.email` | Email |
| `blog.comment.text` | Your comment |
| `blog.comment.submit` | Post comment |
| `blog.comment.thanks` | Thank you! Your comment appears once it's approved. |
| `blog.comments.off` | Comments are off for this post. |
| `blog.post.unavailable.title` | This post isn't available |
| `blog.post.unavailable.body` | It may have been unpublished. See all our posts. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The published post's public view and edit mode |
| UF-42 Sharing a blog post | The reader share buttons |
| UF-20 Lead capture | The post's call to action links to a landing page |
