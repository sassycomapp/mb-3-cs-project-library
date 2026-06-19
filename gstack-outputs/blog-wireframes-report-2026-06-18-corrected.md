# Blog Wireframes Report — ADR-27 Compliance (Corrected Re-Run)

**Date:** 2026-06-18
**Agent:** Build
**Authority:** ADR-027, scaffold-spec.md (v2), nomenclature.md, m3_component_mapping.md, DESIGN.md, authoritative-schema.md, build-plan.md (Phase 4), design-consultation-plan-descriprive.md (blog template specifications)

---

## Scope

12 items total. 6 already complete (re-verified). 6 created in this pass. 0 skipped.

---

## Already Complete — Re-Verified Compliant

### 1. wireframe-blog-BlogEditorForm.html

- **Path:** `wireframes/blog/BlogEditorForm/wireframe-blog-BlogEditorForm.html`
- **Outcome:** Already complete — re-verified compliant.
- **ADR-27 Section 7:** All 15 items PASS.

### 2. wireframe-blog-BlogListForm.html

- **Path:** `wireframes/blog/BlogListForm/wireframe-blog-BlogListForm.html`
- **Outcome:** Already complete — re-verified compliant.
- **ADR-27 Section 7:** All 15 items PASS.

### 3. wireframe-blog-BlogListForm-RowTemplate.html

- **Path:** `wireframes/blog/BlogListForm/BlogListForm-RowTemplate/wireframe-blog-BlogListForm-RowTemplate.html`
- **Outcome:** Already complete — re-verified compliant.
- **ADR-27 Section 7:** All 15 items PASS.

### 4. wireframe-blog-BlogPostForm.html

- **Path:** `wireframes/blog/BlogPostForm/wireframe-blog-BlogPostForm.html`
- **Outcome:** Already complete — re-verified compliant.
- **ADR-27 Section 7:** All 15 items PASS.

### 5. wireframe-blog-CategoryManagementForm.html

- **Path:** `wireframes/blog/CategoryManagementForm/wireframe-blog-CategoryManagementForm.html`
- **Outcome:** Already complete — re-verified compliant.
- **ADR-27 Section 7:** All 15 items PASS.

### 6. wireframe-blog-CommentModerationForm.html

- **Path:** `wireframes/blog/CommentModerationForm/wireframe-blog-CommentModerationForm.html`
- **Outcome:** Already complete — re-verified compliant.
- **ADR-27 Section 7:** All 15 items PASS.

---

## Created in This Pass

### 7. wireframe-blog-BlogListListTemplate.html — CREATED

- **Path:** `wireframes/blog/BlogListForm/BlogListListTemplate/wireframe-blog-BlogListListTemplate.html`
- **Architectural backing:** scaffold-spec.md v2 lists `BlogListListTemplate` as a layout variant inside `BlogListForm`. DESIGN.md Blog Design System: "List with Thumbnail View — Row-based layout with image and content split. Better for denser reading and editorial scan patterns." design-consultation-plan-descriprive.md: "2-column layout (image left, content right). Each row shows: featured image (240px x 160px), category badge, post title (title-medium), excerpt (3-4 lines, body-medium), meta (author, date). Alternating row background."
- **Distinct visual structure:** Row-based list with thumbnail image left, content right. Different from tiled grid (cards) and from the parent form's overall structure. Each row: 240px image, category badge, title link, excerpt, meta row, status badge.
- **ADR-27 Section 7:** All 15 items PASS.

### 8. wireframe-blog-BlogListTiledTemplate.html — CREATED

- **Path:** `wireframes/blog/BlogListForm/BlogListTiledTemplate/wireframe-blog-BlogListTiledTemplate.html`
- **Architectural backing:** scaffold-spec.md v2 lists `BlogListTiledTemplate` as a layout variant inside `BlogListForm`. DESIGN.md Blog Design System: "Tiled Grid View — Multi-column grid on desktop. Card-based presentation. Featured image, category badge, title, excerpt, metadata, and read-more action."
- **Distinct visual structure:** 3-column card grid. Each card: featured image (16:9), category badge, title link, excerpt (2-3 lines), meta, "Read more" link. Fundamentally different from list-with-thumbnail layout.
- **ADR-27 Section 7:** All 15 items PASS.

### 9. wireframe-blog-BlogPostClassicTemplate.html — CREATED

- **Path:** `wireframes/blog/BlogPostForm/BlogPostClassicTemplate/wireframe-blog-BlogPostClassicTemplate.html`
- **Architectural backing:** scaffold-spec.md v2 lists `BlogPostClassicTemplate` as a layout variant inside `BlogPostForm`. DESIGN.md Blog Post Templates: "Classic Centered — Centered single-column article layout. Best for long-form editorial content."
- **Distinct visual structure:** Single column, max 720px, centred. Full-width featured image (800px max). No sidebar. No pullquote. Headline-large title. Different from two-column (has sidebar) and full-width (has pullquote, no width constraint).
- **ADR-27 Section 7:** All 15 items PASS.

### 10. wireframe-blog-BlogPostFullWidthTemplate.html — CREATED

- **Path:** `wireframes/blog/BlogPostForm/BlogPostFullWidthTemplate/wireframe-blog-BlogPostFullWidthTemplate.html`
- **Architectural backing:** scaffold-spec.md v2 lists `BlogPostFullWidthTemplate` as a layout variant inside `BlogPostForm`. DESIGN.md Blog Post Templates: "Full-Width Immersive — Full-width visual treatment. Best for image-rich or storytelling content. Pullquotes and generous line height may be used."
- **Distinct visual structure:** Full-width hero image, no max-width constraint on content, pullquote card visible (key differentiator from classic and two-column), no sidebar. Different emphasis on visual immersion.
- **ADR-27 Section 7:** All 15 items PASS.

### 11. wireframe-blog-BlogPostTwoColumnTemplate.html — CREATED

- **Path:** `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/wireframe-blog-BlogPostTwoColumnTemplate.html`
- **Architectural backing:** scaffold-spec.md v2 lists `BlogPostTwoColumnTemplate` as a layout variant inside `BlogPostForm`. DESIGN.md Blog Post Templates: "Two-Column with Sidebar — Main article plus sidebar. Sidebar may include author bio, related posts, categories, and newsletter content. Best for engagement, internal linking."
- **Distinct visual structure:** Two-column layout (60/40). Featured image left-aligned (300x200) with title/meta right. Sidebar visible (not dashed) containing author bio, related posts, categories, newsletter signup. Fundamentally different from classic (no sidebar) and full-width (pullquote instead of sidebar).
- **ADR-27 Section 7:** All 15 items PASS.

### 12. wireframe-blog-CategoryManagementForm-RowTemplate.html — CREATED

- **Path:** `wireframes/blog/CategoryManagementForm/CategoryManagementForm-RowTemplate/wireframe-blog-CategoryManagementForm-RowTemplate.html`
- **Architectural backing:** CategoryManagementForm uses a DataGrid (`dg_categories` in the existing wireframe). DataGrids require row templates for custom rendering. The wireframe shows columns: Name, Slug, Description, Post Count, Actions.
- **Distinct visual structure:** Row template showing category name, slug, description, post count, and edit/delete icon buttons in a horizontal layout.
- **ADR-27 Section 7:** All 15 items PASS.

---

## Skipped

**None.** All 12 items from the task list are accounted for above.

---

## Cross-Check Against Full Input List

| # | Item Path | Outcome |
|---|-----------|---------|
| 1 | `wireframes/blog/BlogEditorForm/wireframe-blog-BlogEditorForm.html` | Already complete — re-verified |
| 2 | `wireframes/blog/BlogListForm/wireframe-blog-BlogListForm.html` | Already complete — re-verified |
| 3 | `wireframes/blog/BlogListForm/BlogListForm-RowTemplate/wireframe-blog-BlogListForm-RowTemplate.html` | Already complete — re-verified |
| 4 | `wireframes/blog/BlogListForm/BlogListListTemplate/wireframe-blog-BlogListListTemplate.html` | CREATED |
| 5 | `wireframes/blog/BlogListForm/BlogListTiledTemplate/wireframe-blog-BlogListTiledTemplate.html` | CREATED |
| 6 | `wireframes/blog/BlogPostForm/wireframe-blog-BlogPostForm.html` | Already complete — re-verified |
| 7 | `wireframes/blog/BlogPostForm/BlogPostClassicTemplate/wireframe-blog-BlogPostClassicTemplate.html` | CREATED |
| 8 | `wireframes/blog/BlogPostForm/BlogPostFullWidthTemplate/wireframe-blog-BlogPostFullWidthTemplate.html` | CREATED |
| 9 | `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/wireframe-blog-BlogPostTwoColumnTemplate.html` | CREATED |
| 10 | `wireframes/blog/CategoryManagementForm/wireframe-blog-CategoryManagementForm.html` | Already complete — re-verified |
| 11 | `wireframes/blog/CategoryManagementForm/CategoryManagementForm-RowTemplate/wireframe-blog-CategoryManagementForm-RowTemplate.html` | CREATED |
| 12 | `wireframes/blog/CommentModerationForm/wireframe-blog-CommentModerationForm.html` | Already complete — re-verified |

**All 12 items accounted for. No omissions.**

---

## Specification Gaps

None found. All blog forms and layout variants have clear architectural backing in scaffold-spec.md, DESIGN.md, build-plan.md, and authoritative-schema.md.

---

## Summary

| Item | Status |
|------|--------|
| Already complete (re-verified) | 6 |
| Created in this pass | 6 |
| Skipped | 0 |
| ADR-27 compliance (all 12) | All 15 checklist items PASS |
| Specification gaps | None |
