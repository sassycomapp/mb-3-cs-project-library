---
title: "36 — F-21 Blog and content"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-339672ce"
updated: "2026-11-17"
---

## Description

**Feature:** F-21 Blog and content (UF-16, UF-42; `spec-sm-syndication.md` in full; SR-37, SR-37a; FR-21).
**Depends on:** 33 (edit mode and pages), 13 (SharePanelComponent), 14 (ReaderShareComponent), 09 (AI captions and the AI helper in the editor).

### Forms

- `blog.BlogListForm` (+ local `BlogListTiledTemplate`, `BlogListListTemplate`, loaded by `business_profile.blog_list_view`)
- `blog.BlogPostForm` (+ local `BlogPostClassicTemplate`, `BlogPostTwoColumnTemplate`, `BlogPostFullWidthTemplate`, loaded by `blog_posts.template`)
- `blog.BlogAdminListForm` — admin: Content & Website → Blog; Share on each published post
- `blog.BlogEditorForm` — rich text editor, layout, excerpt, category, tags, featured image, video, SEO settings, Publish Now / Save Draft / Schedule
- `blog.CategoryManagementForm`, `blog.CommentModerationForm`

### Server functions

- `server_blog/blog_service` (renamed from `server_blog/service.py` at this step, per the divergence inventory)
- `server_blog/comment_service` — approve, spam, delete; only approved is public; comments switched off in Settings
- `server_blog/share_service` — with ticket 13
- `server_blog/blog_image_endpoint` — `/_/api/blog-image/{slug}`: published-post featured image only, else 404
- `server_blog/blog_tasks` — scheduling

### Tables

- `blog_categories`, `blog_posts`, `blog_comments`, `files`

### Acceptance tests

- `SR-37`, `SR-37a`; UF-16, UF-42; spec-sm-syndication.md (the share panel, AI captions, previews, `?ref=` tracking); visits and new contacts per platform

### Security criteria

- B1 — admin forms Owner/Manager; edit mode audited (AC-S12)
- The blog-image endpoint serves published posts' featured images only (the access matrix's intentionally-public list)
- Comments: only approved shown; the moderation actions are Owner/Manager

## Comments
