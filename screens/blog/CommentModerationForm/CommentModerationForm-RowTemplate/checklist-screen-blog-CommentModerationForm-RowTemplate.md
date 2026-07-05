# Compliance Checklist — screen-blog-CommentModerationForm-RowTemplate

**Screen:** `screens/blog/CommentModerationForm/CommentModerationForm-RowTemplate/screen-blog-CommentModerationForm-RowTemplate.html`
**Wireframe:** `wireframes/blog/CommentModerationForm/CommentModerationForm-RowTemplate/wireframe-blog-CommentModerationForm-RowTemplate.html`
**Date:** 2026-07-02

## A. Wireframe fidelity
- [x] `drp_comment_row` (DataRowPanel) — row structure matches wireframe
- [x] `txt_commenter_name` (Text) ✓
- [x] `txt_commenter_email` (Text) ✓
- [x] `txt_post_title` (Text) ✓
- [x] `txt_comment_preview` (Text) ✓
- [x] `txt_status` (Text) — pending/approved/rejected with badges ✓
- [x] `txt_created_at` (Text) ✓
- [x] `flow_actions` (FlowPanel) with 4 IconButtons:
  - `btn_approve` (IconButton) mi:check_circle ✓
  - `btn_delete` (IconButton) mi:delete ✓
  - `btn_flag` (IconButton) mi:flag ✓
  - `btn_unflag` (IconButton) mi:outlined_flag ✓
- [x] 3 sample rows shown

## B–I. (All standard checks)
- [x] No page chrome
- [x] MUI overlay — 24px DataGrid radius, dividers, hover state
- [x] Token compliance — no hardcoded rgba (verified: uses `color-mix()`)
- [x] Dark mode present (verified)
- [x] Material Icons imported, `mi:` prefix used
- [x] Sample data — plausible SA names (James Moyo, Lerato Khumalo, Pieter van der Merwe)
- [x] Component compliance — no Label, no HTMLTemplate

## Discrepancies: **None.**
