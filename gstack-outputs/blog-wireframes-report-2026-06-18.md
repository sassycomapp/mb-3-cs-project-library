# Blog Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-18
**Agent:** Build
**Authority:** ADR-027, scaffold-spec.md (v2), nomenclature.md, m3_component_mapping.md, DESIGN.md, authoritative-schema.md, build-plan.md (Phase 4)

---

## Wireframes Created

### 1. wireframe-blog-BlogEditorForm.html

- **Path:** `wireframes/blog/BlogEditorForm/wireframe-blog-BlogEditorForm.html`
- **Architectural backing:** scaffold-spec.md line 239–241 (`BlogEditorForm/` in `blog/` package, Phase 4). build-plan.md Stage 4.1: "Blog posts can be created with rich text editor", "SEO fields functional".
- **Form type:** Editor form — full-width, max-width 900px, two-column layout (main content + sidebar).
- **Components:** Page title heading, action bar (Save Draft / Publish / Cancel), success message (hidden), main content card (title, slug, excerpt, content, featured image), sidebar card (status, category, template dropdowns; SEO fields; pullquote; video URL).
- **Empty state:** Not applicable — editor forms do not display data-driven content.
- **ADR-27 Section 7 compliance:** ALL 15 items PASS.

### 2. wireframe-blog-BlogListForm.html

- **Path:** `wireframes/blog/BlogListForm/wireframe-blog-BlogListForm.html`
- **Architectural backing:** scaffold-spec.md line 233–235 (`BlogListForm/` in `blog/` package, Phase 4). build-plan.md Stage 4.1: "Public blog listing and post pages functional". Navigation: `nav_blog — BlogListForm (blog_enabled)`.
- **Form type:** List form — full-width, max-width 1100px.
- **Components:** Page title heading, filter bar (search TextBox, category DropdownMenu, tiled/list view toggle Buttons), tiled view panel (RepeatingPanel), list view panel (hidden, RepeatingPanel), pagination (Previous / page info / Next).
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "article"), `hdg_empty_state` (Heading "No blog posts yet"), `txt_empty_state` (Text), `btn_empty_action` (Button "Create Your First Post").
- **ADR-27 Section 7 compliance:** ALL 15 items PASS.

### 3. wireframe-blog-BlogListForm-RowTemplate.html

- **Path:** `wireframes/blog/BlogListForm/BlogListForm-RowTemplate/wireframe-blog-BlogListForm-RowTemplate.html`
- **Architectural backing:** The BlogListForm uses a DataGrid (`dg_` prefix per nomenclature.md). DataGrids require row templates for custom rendering. The scaffold-spec lists BlogListForm with tiled/list view panels, and the DataGrid is the admin-facing list view.
- **Form type:** Row template — embedded in BlogListForm DataGrid.
- **Components:** Featured image thumbnail, category badge (Text), post title (Link), excerpt (Text), meta row (author, date, view count), status badge (Text), edit IconButton.
- **Empty state:** Not applicable — row template, not a standalone form.
- **ADR-27 Section 7 compliance:** ALL 15 items PASS.

### 4. wireframe-blog-BlogPostForm.html

- **Path:** `wireframes/blog/BlogPostForm/wireframe-blog-BlogPostForm.html`
- **Architectural backing:** scaffold-spec.md line 236–238 (`BlogPostForm/` in `blog/` package, Phase 4). build-plan.md Stage 4.1: "Public blog listing and post pages functional". Routing: `/blog/:slug — Blog post`. Blog post has 3 layout variants (classic, two_column, full_width) built INTO the form per scaffold-spec v2.
- **Form type:** Public-facing read-only detail + comment form.
- **Components:** Post header card (category badge, title, meta), featured image, main content + sidebar row, pullquote (hidden, for full_width template), video embed (hidden, VideoDisplayComponent), social sharing buttons, comments section (comments list + add comment form), FooterComponent.
- **Empty state (comments):** 4 required components present: `ico_empty_comments` (Icon "chat_bubble_outline"), `hdg_empty_comments` (Heading "No comments yet"), `txt_empty_comments` (Text), `btn_empty_comment_action` (Button "Write a Comment").
- **ADR-27 Section 7 compliance:** ALL 15 items PASS.

### 5. wireframe-blog-CategoryManagementForm.html

- **Path:** `wireframes/blog/CategoryManagementForm/wireframe-blog-CategoryManagementForm.html`
- **Architectural backing:** scaffold-spec.md line 242–244 (`CategoryManagementForm/` in `blog/` package, Phase 4). build-plan.md Stage 4.1: "Blog categories functional (create, edit, reorder)".
- **Form type:** List/editor hybrid — full-width, max-width 900px.
- **Components:** Page title heading, action bar (Add Category button), category DataGrid, add/edit form card (hidden, with name/slug/description inputs + Save/Cancel buttons).
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "category"), `hdg_empty_state` (Heading "No categories yet"), `txt_empty_state` (Text), `btn_empty_action` (Button "Create First Category").
- **ADR-27 Section 7 compliance:** ALL 15 items PASS.

### 6. wireframe-blog-CommentModerationForm.html

- **Path:** `wireframes/blog/CommentModerationForm/wireframe-blog-CommentModerationForm.html`
- **Architectural backing:** scaffold-spec.md line 245–247 (`CommentModerationForm/` in `blog/` package, Phase 4, NEW). build-plan.md Stage 4.1: "Comments moderation controls". DESIGN.md Blog Admin Controls: "Comment moderation controls".
- **Form type:** List form — full-width, max-width 900px.
- **Components:** Page title heading, filter bar (status DropdownMenu, post DropdownMenu), comment DataGrid, pagination (Previous / page info / Next).
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "chat_bubble_outline"), `hdg_empty_state` (Heading "No comments to moderate"), `txt_empty_state` (Text), `btn_empty_action` (Button "View All Comments").
- **ADR-27 Section 7 compliance:** ALL 15 items PASS.

---

## Forms/Sub-Templates Skipped (No Architectural Backing)

### BlogListTiledTemplate

- **Path:** `wireframes/blog/BlogListForm/BlogListTiledTemplate/`
- **Reason:** scaffold-spec.md v2 explicitly states: "BlogListTiledTemplate → build into BlogListForm. Build as a content panel inside BlogListForm, toggled by view mode." This is a layout variant, not a standalone form. It does NOT qualify for its own wireframe. The tiled view is represented as `col_tiled_view` (ColumnPanel) inside the BlogListForm wireframe.

### BlogListListTemplate

- **Path:** `wireframes/blog/BlogListForm/BlogListListTemplate/`
- **Reason:** Same as BlogListTiledTemplate. scaffold-spec.md v2: "BlogListListTemplate → build into BlogListForm. Build as a content panel inside BlogListForm, toggled by view mode." The list view is represented as `col_list_view` (ColumnPanel, hidden by default) inside the BlogListForm wireframe.

### BlogPostClassicTemplate

- **Path:** `wireframes/blog/BlogPostForm/BlogPostClassicTemplate/`
- **Reason:** scaffold-spec.md v2: "BlogPostClassicTemplate → build into BlogPostForm. Build as a content panel inside BlogPostForm, selected by `post.template`." This is a layout variant, not a standalone form.

### BlogPostTwoColumnTemplate

- **Path:** `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/`
- **Reason:** scaffold-spec.md v2: "BlogPostTwoColumnTemplate → build into BlogPostForm. Build as a content panel inside BlogPostForm, selected by `post.template`." The sidebar (`card_sidebar`) is represented as a hidden ColumnPanel inside the BlogPostForm wireframe.

### BlogPostFullWidthTemplate

- **Path:** `wireframes/blog/BlogPostForm/BlogPostFullWidthTemplate/`
- **Reason:** scaffold-spec.md v2: "BlogPostFullWidthTemplate → build into BlogPostForm. Build as a content panel inside BlogPostForm, selected by `post.template`." The pullquote (`card_pullquote`) is represented as a hidden Card inside the BlogPostForm wireframe.

---

## Specification Gaps

No specification gaps found. All blog forms have clear architectural backing in scaffold-spec.md, build-plan.md (Phase 4 Stage 4.1), DESIGN.md (Blog Design System section), and authoritative-schema.md (blog_posts, blog_categories, blog_comments tables).

---

## Summary

| Item | Status |
|------|--------|
| Wireframes created | 6 (BlogEditorForm, BlogListForm, BlogListForm-RowTemplate, BlogPostForm, CategoryManagementForm, CommentModerationForm) |
| Layout variants skipped | 5 (BlogListTiledTemplate, BlogListListTemplate, BlogPostClassicTemplate, BlogPostTwoColumnTemplate, BlogPostFullWidthTemplate) |
| ADR-27 compliance | All 6 wireframes pass all 15 checklist items |
| Specification gaps | None |
