# Investigation: BlogListForm RepeatingPanel Row Template Gap Analysis

**Date:** 2026-06-23
**Agent:** Build
**Type:** Read-only investigation — no file changes

---

## Executive Summary

**No gap exists.** Both row templates already exist and are correctly referenced. The prior report was wrong about them being "skipped." BlogListForm-RowTemplate is an orphan that should be retired.

---

## Evidence Gathered

### 1. BlogListForm wireframe (wireframe-blog-BlogListForm.html)

**Lines 68-70:**
```
rp_blog_tiled (RepeatingPanel)
[BlogListTiledTemplate — tiled card grid layout]
```

**Lines 77-79:**
```
rp_blog_list (RepeatingPanel)
[BlogListListTemplate — list with thumbnail layout]
```

The BlogListForm wireframe explicitly references TWO named templates: `BlogListTiledTemplate` for `rp_blog_tiled` and `BlogListListTemplate` for `rp_blog_list`. It does NOT reference `BlogListForm-RowTemplate` anywhere.

### 2. BlogListTiledTemplate wireframe (EXISTS)

**Path:** `wireframes/blog/BlogListForm/BlogListTiledTemplate/wireframe-blog-BlogListTiledTemplate.html`

**Structure (line 36):** `tmpl_tiled_card (RepeatingPanel row template)`

**Components per card:**
- `img_featured` (Image) — [featured image 16:9]
- `txt_category_badge` (Text) — [category name]
- `nav_post_title` (Link) — [post title]
- `txt_excerpt` (Text) — [excerpt — 2 to 3 lines]
- `txt_meta` (Text) — [author] | [date] | [view count]
- `nav_read_more` (Link) — "Read more"

**DESIGN.md match:** "Tiled Grid View — Multi-column grid on desktop. Card-based presentation. Featured image, category badge, title, excerpt, metadata, and read-more action." — **PERFECT MATCH**

### 3. BlogListListTemplate wireframe (EXISTS)

**Path:** `wireframes/blog/BlogListForm/BlogListListTemplate/wireframe-blog-BlogListListTemplate.html`

**Structure (line 37):** `tmpl_list_row (RepeatingPanel row template)`

**Components per row:**
- `img_featured` (Image) — [featured image 240x160]
- `txt_category_badge` (Text) — [category name]
- `nav_post_title` (Link) — [post title]
- `txt_excerpt` (Text) — [excerpt — 3 to 4 lines]
- `txt_author` (Text) — [author name]
- `txt_date` (Text) — [published date]
- `txt_view_count` (Text) — [view count]
- `txt_status_badge` (Text) — [status badge]

**DESIGN.md match:** "List with Thumbnail View — Row-based layout with image and content split. Better for denser reading and editorial scan patterns." — **PERFECT MATCH**

### 4. BlogListForm-RowTemplate wireframe (ORPHAN)

**Path:** `wireframes/blog/BlogListForm/BlogListForm-RowTemplate/wireframe-blog-BlogListForm-RowTemplate.html`

**Structure (line 36):** `rp_blog_row (RepeatingPanel row template)` — note: name does NOT match either `rp_blog_tiled` or `rp_blog_list`

**Components:**
- `img_featured` (Image) — [featured image thumbnail]
- `txt_category_badge` (Text)
- `nav_post_title` (Link)
- `txt_excerpt` (Text)
- `txt_author` (Text)
- `txt_date` (Text)
- `txt_view_count` (Text)
- `txt_status_badge` (Text)
- `btn_edit` (IconButton) mi:edit

**Comparison:** This is nearly identical to BlogListListTemplate, with one extra component (`btn_edit`). It is NOT referenced by the BlogListForm wireframe.

### 5. Prior report (blog-wireframes-report-2026-06-18.md)

**Lines 69-77 claimed:**
> BlogListTiledTemplate — SKIPPED: "scaffold-spec.md v2 explicitly states: 'BlogListTiledTemplate → build into BlogListForm.'"
> BlogListListTemplate — SKIPPED: Same reason.

**This is factually incorrect.** Both wireframes exist on disk at:
- `wireframes/blog/BlogListForm/BlogListTiledTemplate/wireframe-blog-BlogListTiledTemplate.html`
- `wireframes/blog/BlogListForm/BlogListListTemplate/wireframe-blog-BlogListListTemplate.html`

The prior report either did not check the filesystem or misinterpreted the scaffold-spec instruction. The scaffold-spec says to "build into BlogListForm" meaning the templates are used INSIDE BlogListForm (as RepeatingPanel row templates), not that they don't need wireframes.

---

## Answers to Investigation Questions

### Q1: Does wireframe-blog-BlogListForm.html explicitly associate BlogListForm-RowTemplate with EITHER rp_blog_tiled or rp_blog_list?

**NO.** The BlogListForm wireframe references `BlogListTiledTemplate` and `BlogListListTemplate` by name for the two RepeatingPanels. It does NOT reference `BlogListForm-RowTemplate` at all. BlogListForm-RowTemplate is an orphan — not linked to any component in the parent form.

### Q2: Is BlogListForm-RowTemplate's actual content/structure consistent with a DataGrid row template (admin-facing)?

**NO.** The BlogListForm wireframe contains NO DataGrid component — only two RepeatingPanels (`rp_blog_tiled` and `rp_blog_list`). The prior report's claim that "BlogListForm uses a DataGrid" is factually incorrect.

BlogListForm-RowTemplate's component composition (image + content split, category badge, title, excerpt, author, date, view count, status badge) is nearly identical to BlogListListTemplate. It matches the "List with Thumbnail View" description from DESIGN.md.

### Q3: TRUE current state

**a) Does a row template exist for rp_blog_tiled?**
YES — `wireframes/blog/BlogListForm/BlogListTiledTemplate/wireframe-blog-BlogListTiledTemplate.html` (container label: `tmpl_tiled_card`). Correctly referenced by BlogListForm line 69 as `[BlogListTiledTemplate — tiled card grid layout]`.

**b) Does a row template exist for rp_blog_list?**
YES — `wireframes/blog/BlogListForm/BlogListListTemplate/wireframe-blog-BlogListListTemplate.html` (container label: `tmpl_list_row`). Correctly referenced by BlogListForm line 78 as `[BlogListListTemplate — list with thumbnail layout]`.

**c) Does BlogListForm-RowTemplate belong to a third, separate component (e.g. an admin DataGrid)?**
NO. There is no DataGrid in BlogListForm. BlogListForm-RowTemplate is an orphan — a near-duplicate of BlogListListTemplate with one extra component (`btn_edit`). It is not referenced by any component in the parent form.

### Q4: Recommended corrective action

**BlogListForm-RowTemplate should be retired (deleted).** It is a redundant duplicate of BlogListListTemplate. The correct row templates already exist and are properly wired:

| RepeatingPanel | Template Wireframe | Status |
|---|---|---|
| `rp_blog_tiled` | BlogListTiledTemplate | EXISTS and correctly referenced |
| `rp_blog_list` | BlogListListTemplate | EXISTS and correctly referenced |
| `rp_blog_row` (orphan) | BlogListForm-RowTemplate | NOT referenced — retire |

The prior report's claim that these templates were "skipped" was incorrect. Both templates exist on disk and are properly integrated into the BlogListForm wireframe.

---

## Root Cause of Prior Report Error

The blog-wireframes-report-2026-06-18.md stated that BlogListTiledTemplate and BlogListListTemplate were "skipped" with the architectural reason: "scaffold-spec.md v2 explicitly states: 'BlogListTiledTemplate → build into BlogListForm.'"

This misinterpreted the scaffold-spec instruction. "Build into BlogListForm" means the templates are used as RepeatingPanel row templates INSIDE BlogListForm — not that they don't need their own wireframes. The prior report either did not check the filesystem for existing files or confused "build into" with "don't build separately."

Additionally, the prior report claimed BlogListForm uses a DataGrid (`dg_` prefix per nomenclature.md). The BlogListForm wireframe contains NO DataGrid — only two RepeatingPanels (`rp_blog_tiled` and `rp_blog_list`). This false premise led to the incorrect conclusion that BlogListForm-RowTemplate was for a DataGrid.

---

*End of Investigation — 2026-06-23*
