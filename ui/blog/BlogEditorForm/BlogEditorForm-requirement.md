---
title: "BlogEditorForm-requirement"
status: draft
doc-id: "mb-29173a9b"
updated: "2026-11-25"
---
# BlogEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `blog.BlogEditorForm` |
| Purpose | Write and publish a post: the rich text editor, layout, excerpt, category, tags, featured image, video, SEO settings, Publish Now / Save Draft / Schedule (ticket 36, F-21; UF-16) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `blog.BlogAdminListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_blog_editor`) | "New post" / the post's title, `headline-large` | `page-title` | — | `blog.admin.new` |
| Title field | `m3.TextBox` (`txt_post_title`, outlined) | The post's title | `outlined-input` | — | `blog.title` |
| Slug field | `m3.TextBox` (`txt_post_slug`, outlined) | Unique; the public address `/blog/{slug}` | `outlined-input` | — | `services.slug` |
| Body editor | `m3.TextArea` (`txt_post_content`) | The rich text editor (user-formatted text; RichText is for such text, spec-ui-standards section 3.9) | `outlined-input` | mobile | `blog.editor.body` |
| Layout selector | `m3.DropdownMenu` (`dd_post_template`) | Classic centred, two-column with sidebar, full-width immersive (`blog_posts.template`) | `outlined-select` | — | `blog.editor.template` |
| Excerpt field | `m3.TextArea` (`txt_post_excerpt`, outlined) | The excerpt | `outlined-input` | — | `blog.editor.excerpt` |
| Category selector | `m3.DropdownMenu` (`dd_post_category`, `allow_none=True`) | The post's category | `outlined-select` | — | `blog.filter.category` |
| Tags field | `m3.Chip` set (`chip_post_tag_*`, type `input`, removable=True) in an Anvil `FlowPanel` | The post's tags | — | mobile | `crm.contact.tags` |
| Featured image | `m3.FileLoader` (`fu_post_image`) | The featured image (`blog_posts.featured_image`) | — | — | `services.images` |
| Video address field | `m3.TextBox` (`txt_post_video_url`) | The video (`blog_posts.video_url`); empty hides it | `outlined-input` | — | `services.video` |
| Author display | `m3.Avatar` (`av_post_author`) | The author (`blog_posts.author_id`) | — | — | — |
| SEO panel | `m3.Card` (`card_seo`, outlined) | Title, description, keywords (UF-16) | `surface-card` | mobile | — |
| SEO title field | `m3.TextBox` (`txt_meta_title`, outlined, `character_limit=60`) | The SEO title (`blog_posts.meta_title`) | `outlined-input` | — | `pages.seo_title` |
| SEO description field | `m3.TextArea` (`txt_meta_description`, outlined, `character_limit=160`) | The SEO description (`blog_posts.meta_description`) | `outlined-input` | — | `blog.editor.seo_description` |
| Keywords field | `m3.TextBox` (`txt_keywords`, outlined) | The keywords (`blog_posts.keywords`) | `outlined-input` | — | `blog.editor.keywords` |
| Publish Now button | `m3.Button` (`btn_publish_now`, filled) | Publishes; the social captions are written and the Share button appears (UF-16) | `primary-button` | — | `blog.editor.publish_now` |
| Save Draft button | `m3.Button` (`btn_save_draft_post`, outlined) | Saves as `draft` | `secondary-button` | — | `invoices.save_draft` |
| Schedule action | `m3.DatePicker` (`dp_schedule_at`) + `m3.TimePicker` (`tp_schedule_time`) + `m3.Button` (`btn_schedule_post`, outlined) | Schedule (date and time); the post publishes at its time by background task | `outlined-date` | mobile | `blog.editor.schedule` |
| Share trigger | the host's Share button after publishing | Opens `components.SharePanelComponent` (ticket 13) | — | — | `blog.admin.share` |
| AI helper trigger | `components.AIHelperComponent` (trigger) | The blog post drafts assist drafts the post (F-26) | — | — | `ai.open` |
| Help icon | `components.HelpTooltipComponent` (help entry `blog.editor`) | The feature's tooltip and help video | — | — | `help.blog.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Post | `blog_posts` | `title`, `slug`, `content`, `excerpt`, `featured_image`, `author_id`, `category_id`, `tags`, `template`, `pullquote`, `video_url`, `video_type`, `meta_title`, `meta_description`, `keywords`, `status`, `scheduled_at`, `published_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the post | `form_show` → `load_post` | `blog_service` read (`server_blog/blog_service`) | The fields fill |
| Publish Now | `btn_publish_now_click` | `blog_service` publish | The post is published; `published_at` set; the captions are generated through `share_service`; the Share button appears |
| Save Draft | `btn_save_draft_post_click` | `blog_service` save | Status `draft` |
| Schedule | `btn_schedule_post_click` | `blog_service` schedule | Status `scheduled` with `scheduled_at`; publishes by background task |
| AI draft | the AI trigger | `request(assist="blog_draft", ...)` (`server_ai/ai_request_layer`) | The draft appears for acceptance |
| Video type detection | the save path | `blog_service` save | `video_type` is detected from the address on save |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the editor always shows its fields |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

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
| `blog.title` | Title |
| `blog.editor.body` | Write your post |
| `blog.editor.template` | Layout |
| `blog.editor.excerpt` | Excerpt |
| `blog.editor.seo_description` | SEO description |
| `blog.editor.keywords` | Keywords |
| `blog.editor.publish_now` | Publish Now |
| `blog.editor.schedule` | Schedule |
| `blog.editor.scheduled` | Your post will publish at the time you chose. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Writing a blog post | `master_template` help entry `blog.editor` — played in the app language |
| Scheduling a post | `master_template` help entry `blog.schedule` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | Publishing a blog post (UF-16) |
| UF-30 Using the AI helper | The blog drafts assist |
| UF-42 Sharing a blog post | The Share button appears on publishing |
