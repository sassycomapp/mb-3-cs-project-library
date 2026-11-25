---
title: "CommentModerationForm-requirement"
status: draft
doc-id: "mb-bf245afb"
updated: "2026-11-25"
---
# CommentModerationForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.CommentModerationForm` |
| Purpose | Blog → Comments: approve, mark as spam or delete (ticket 36, F-21; UF-16) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_comments`) | "Comments", `headline-large` | `page-title` | — | `comments.title` |
| Status filter | `m3.ButtonGroup` (`bgrp_comment_status`, connected, single: Pending/All) | The moderation queue first | — | mobile | `comments.filter.pending`, `comments.filter.all` |
| Comments list | Anvil `ColumnPanel` (`col_comment_rows`) | The comments in date order | — | mobile | — |
| Comment row | `m3.ListItem` (`li_comment_mod_*`) | Commenter, post, comment text, date | — | — | — |
| Approve button | `m3.Button` (`btn_approve_comment_*`, filled) | Approves; only approved is public (schema) | `primary-button` | — | `comments.approve` |
| Spam button | `m3.Button` (`btn_spam_comment_*`, outlined) | Marks as spam | `secondary-button` | — | `comments.spam` |
| Delete button | `m3.IconButton` (`ibtn_delete_comment_*`, icon `mi:delete`) | Deletes the comment | `icon-button` | — | `inbox.delete` |
| Help icon | `components.HelpTooltipComponent` (help entry `blog.comments`) | The feature's tooltip and help video | — | — | `help.blog.comments.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Comments | `blog_comments` | `post_id`, `commenter_name`, `commenter_email`, `comment_text`, `status`, `created_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the comments | `form_show` → `load_comments` | `comment_service` list read (`server_blog/comment_service`) | The queue arrives |
| Approve | `btn_approve_comment_*_click` | `comment_service` approve | The comment becomes public |
| Mark spam | `btn_spam_comment_*_click` | `comment_service` spam | The comment is marked `spam` |
| Delete | `ibtn_delete_comment_*_click` | `comment_service` delete | The comment is removed |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Comment icon (`mi:forum`) |
| Heading | `comments.empty.title` |
| Body text | `comments.empty.body` |
| Primary action | None — comments arrive as visitors leave them |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; the moderation actions are Owner/Manager (ticket 36).

## 7. Text

| Phrase key | English text |
|---|---|
| `comments.title` | Comments |
| `comments.filter.pending` | Awaiting approval |
| `comments.filter.all` | All |
| `comments.approve` | Approve |
| `comments.spam` | Spam |
| `comments.empty.title` | No comments waiting |
| `comments.empty.body` | Visitor comments arrive here for your approval. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Moderating comments | `master_template` help entry `blog.comments` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | Comments: approve, mark as spam or delete |
