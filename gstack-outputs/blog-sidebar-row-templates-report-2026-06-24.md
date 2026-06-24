# BlogPostTwoColumnTemplate Row Templates — Production Report

**Date:** 2026-06-24
**Agent:** Build
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md

---

## 1. What Was Created

### 1.1 BlogPostRelatedPostsTemplate

**File:** `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/BlogPostRelatedPostsTemplate/wireframe-blog-BlogPostRelatedPostsTemplate.html`

**Purpose:** Row template for `rp_related_posts` RepeatingPanel inside BlogPostTwoColumnTemplate sidebar.

**Architectural backing:**
- scaffold-spec.md line 426: `BlogPostTwoColumnTemplate` → build into `BlogPostForm`
- DESIGN.md line 840–844: Two-Column with Sidebar — "Sidebar may include author bio, related posts, categories, and newsletter content."
- Screen reference: `screen-blog-BlogPostTwoColumnTemplate.html` lines 162–173 — each related post shows title (link) and date · view count (meta text)

**Component composition:**
| Component | Type | Purpose |
|---|---|---|
| `col_related_post` | ColumnPanel | Container for single related post item |
| `nav_post_title` | Link | Post title — navigates to post slug |
| `txt_meta` | Text | Date and view count (e.g. "28 May 2026 · 156 views") |

**Design rationale:** Compact sidebar widget. No excerpt (too verbose for sidebar). No category badge (redundant when reading a post in that category). No featured image (screen does not show thumbnails in related posts — title + meta only). Matches screen implementation exactly.

**Empty state:** Not included. Row templates only render when items exist. Empty state for "no related posts" belongs in the parent `BlogPostTwoColumnTemplate` (hide the "Related Posts" section title and `rp_related_posts` when list is empty).

### 1.2 BlogPostCategoriesTemplate

**File:** `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/BlogPostCategoriesTemplate/wireframe-blog-BlogPostCategoriesTemplate.html`

**Purpose:** Row template for `rp_categories` RepeatingPanel inside BlogPostTwoColumnTemplate sidebar.

**Architectural backing:**
- Same as above (DESIGN.md, scaffold-spec.md, screen reference)
- Screen reference: `screen-blog-BlogPostTwoColumnTemplate.html` lines 179–184 — each category shows "Category Name (count)"

**Component composition:**
| Component | Type | Purpose |
|---|---|---|
| `flow_category_item` | FlowPanel | Container — horizontal layout (name + count on one line) |
| `nav_category_name` | Link | Category name — navigates to filtered blog list |
| `txt_post_count` | Text | Post count in parentheses (e.g. "(12)") |

**Design rationale:** Minimal list item. Category name as link enables navigation to filtered blog list. Post count provides scannability. Matches screen structure (name + count on one line).

**Empty state:** Not included. Same reasoning as above.

---

## 2. Naming Decision

**Convention applied:** Distinct names per RepeatingPanel, following the `BlogListTiledTemplate` / `BlogListListTemplate` precedent. NOT a shared generic `-RowTemplate` suffix, because there are two RepeatingPanels in the same parent and a generic suffix cannot disambiguate which one it belongs to.

| RepeatingPanel | Template Name | Folder Path |
|---|---|---|
| `rp_related_posts` | `BlogPostRelatedPostsTemplate` | `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/BlogPostRelatedPostsTemplate/` |
| `rp_categories` | `BlogPostCategoriesTemplate` | `wireframes/blog/BlogPostForm/BlogPostTwoColumnTemplate/BlogPostCategoriesTemplate/` |

**Rationale:** These are sub-templates of a layout variant (`BlogPostTwoColumnTemplate`), which is itself a sub-form of `BlogPostForm`. The nesting mirrors the parent hierarchy.

---

## 3. ADR-27 Section 7 Compliance Checklist

### 3.1 BlogPostRelatedPostsTemplate

| # | Checklist Item | Result |
|---|---|---|
| 1 | Section 0 reading completed | PASS |
| 2 | Document contains exactly three sections, in correct order | PASS — Introduction, Wireframe Diagram, Properties Table |
| 3 | Document is NOT formatted as a "Screen" type document | PASS |
| 4 | No legacy `Label` (`lbl_`) component used anywhere | PASS |
| 5 | Every component is an approved M3 component | PASS — ColumnPanel, Link, Text (all fully supported) |
| 6 | Every visible text element has its own explicit component | PASS — `nav_post_title`, `txt_meta` |
| 7 | No sample data appears anywhere | PASS — placeholders used ([post title], [date]) |
| 8 | No behaviour, data, layout logic, or explanatory text in wireframe diagram | PASS |
| 9 | Containers drawn as containers with names inset in border | PASS — `col_related_post` label inset |
| 10 | No border drawn around non-bordered component types | PASS |
| 11 | Properties table contains ONLY Designer-only properties | PASS — table states "No Designer-only properties" |
| 12 | writeback (W) specified for text-input components | N/A — no text input components |
| 13 | All component names follow nomenclature.md exactly | PASS — `nav_`, `txt_`, `col_` prefixes |
| 14 | FlowPanel/ColumnPanel used only for intended structural purpose | PASS — ColumnPanel for vertical stack |
| 15 | Wireframe architecturally matches upstream definition | PASS — matches screen structure |

**Result: ALL 15 items PASS**

### 3.2 BlogPostCategoriesTemplate

| # | Checklist Item | Result |
|---|---|---|
| 1 | Section 0 reading completed | PASS |
| 2 | Document contains exactly three sections, in correct order | PASS |
| 3 | Document is NOT formatted as a "Screen" type document | PASS |
| 4 | No legacy `Label` (`lbl_`) component used anywhere | PASS |
| 5 | Every component is an approved M3 component | PASS — FlowPanel, Link, Text (all fully supported) |
| 6 | Every visible text element has its own explicit component | PASS — `nav_category_name`, `txt_post_count` |
| 7 | No sample data appears anywhere | PASS |
| 8 | No behaviour, data, layout logic, or explanatory text in wireframe diagram | PASS |
| 9 | Containers drawn as containers with names inset in border | PASS — `flow_category_item` label inset |
| 10 | No border drawn around non-bordered component types | PASS |
| 11 | Properties table contains ONLY Designer-only properties | PASS |
| 12 | writeback (W) specified for text-input components | N/A |
| 13 | All component names follow nomenclature.md exactly | PASS — `nav_`, `txt_`, `flow_` prefixes |
| 14 | FlowPanel/ColumnPanel used only for intended structural purpose | PASS — FlowPanel for horizontal layout |
| 15 | Wireframe architecturally matches upstream definition | PASS |

**Result: ALL 15 items PASS**

---

## 4. Content Gaps Flagged

| Gap | Impact | Mitigation |
|---|---|---|
| DESIGN.md does not specify sidebar widget composition for related posts or categories | Low — screen provides clear reference | Used screen as direct reference for component structure |
| `blog_categories` table has no `post_count` field | None — computed programmatically | `txt_post_count` populated at runtime via query |
| "Related post strategy by tags" mentioned in DESIGN.md Blog Admin Controls but not elaborated | Low — does not affect wireframe structure | Wireframe shows minimum structure; strategy logic is server-side |

---

## 5. Summary

| Item | Status |
|---|---|
| Wireframes created | 2 (BlogPostRelatedPostsTemplate, BlogPostCategoriesTemplate) |
| ADR-27 compliance | Both pass all 15 checklist items |
| Empty state in row templates | None (architecturally correct — belongs in parent) |
| Content gaps | 3 flagged, all low impact |
| Naming convention | Distinct names per RepeatingPanel (not generic `-RowTemplate`) |

---

*End of report*
