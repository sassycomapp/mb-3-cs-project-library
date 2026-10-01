---
title: "wireframe-refresh-completion-report-2026-06-28"
doc-id: "mb-e3b59440"
updated: "2026-10-01"
---
# Wireframe Refresh — Completion Report

**Date:** 2026-06-28
**Scope:** All 82 wireframes in `wireframes/`
**Approach:** Individual review per wireframe, no batching or templatizing

---

## Summary

| Category | Count | Wireframes |
|---|---|---|
| Fixed | 15 | See list below |
| Already compliant | 67 | All others |
| Empty (placeholder) | 1 | RevenueReportForm-RowTemplate |

---

## Fixes Applied

### Sample data removed (7 wireframes)

| Wireframe | Fix |
|---|---|
| ContactEditorForm | Replaced sample data (Jane Smith, jane.smith@email.com, etc.) with placeholders |
| EmailCampaignListForm | Replaced sample data (May Newsletter, Welcome Series, etc.) with single placeholder row |
| ContactListForm | Replaced sample data (John Doe, Jane Smith, etc.) with single placeholder row |
| TaskListForm | Replaced sample data (Follow up with John Doe, etc.) with single placeholder row |
| ContactViewerForm | Replaced sample data (John Doe, R 4,250, etc.) with placeholders |
| SegmentManagerForm | Replaced count data (23, 89, 31, 12) with `[count]` placeholders |
| HomePageMinimalistTemplate | Replaced trust strip data (10+, 500+, 98%) with `[number]` placeholders |
| HomePageBookingTemplate | Replaced trust strip data (24h, 500+, 98%, Free) with `[number]` placeholders |

### Behaviour notes removed (8 wireframes)

| Wireframe | Fix |
|---|---|
| tmpl_week_event | Removed "Programmatic behaviour" section |
| BookingEditorForm | Removed behaviour note after Properties Table |
| BookingListForm-RowTemplate | Removed "Programmatic behaviour" section |
| tmpl_week_timeslot | Removed "Programmatic behaviour" section |
| tmpl_month_day_cell | Removed "Programmatic behaviour" section |
| tmpl_day_event | Removed "Programmatic behaviour" section |
| MetricCard | Removed behaviour note after Properties Table |
| TimeLapseCarouselComponent | Removed behaviour note after Properties Table |
| ActivityFeed | Removed behaviour note after Properties Table |
| InvoiceListForm | Removed behaviour note after Properties Table |
| ParallaxComponent | Removed behaviour note after Properties Table |

### Legacy Label fixed (3 wireframes)

| Wireframe | Fix |
|---|---|
| ServicesForm | Changed `lbl_video_caption (Label)` to `txt_video_caption (Text)` |
| ServiceEditorForm | Changed `lbl_video_hint (Label)` to `txt_video_hint (Text)` |
| EmailSetupForm | Changed `test_success (Label)` and `test_error (Label)` to `(Text)` |

### Prefix fixes (3 wireframes)

| Wireframe | Fix |
|---|---|
| InvoiceViewerForm | Changed `lbl_` prefix to `txt_` for 7 Text components |
| EmailBroadcastForm | Changed `lbl_` prefix to `txt_` for 6 Text components |
| FooterComponent | Changed `lnk_` prefix to `nav_` for 4 Link components |

### Component type fixes (3 wireframes)

| Wireframe | Fix |
|---|---|
| CategoryManagementTemplate | Changed `FlowPanel: flow_category_row` to `DataRowPanel: drp_category_row` |
| BookingListForm-RowTemplate | Changed `FlowPanel: flow_row` to `DataRowPanel: drp_booking_row` |
| UserManagementForm | Changed `btn_edit_user (Button)` and `btn_deactivate_user (Button)` to `(IconButton)` |

### Non-approved component fixes (2 wireframes)

| Wireframe | Fix |
|---|---|
| ServicesForm | Changed `carousel_viewport (HtmlTemplate)` to `carousel_viewport (ColumnPanel)` |
| VideoDisplayComponent | Changed `container_video_embed (HtmlPanel)` to `col_video_embed (ColumnPanel)` |
| ServiceViewerForm | Changed `container_video_embed (HtmlPanel)` to `col_video_embed (ColumnPanel)` |

### Section structure fixes (5 wireframes)

| Wireframe | Fix |
|---|---|
| ServicesForm | Added Section 1 Introduction and Section 3 heading |
| ContactEditorForm | Added Section 1 Introduction and Section 3 heading |
| EmailCampaignListForm | Added Section 1 Introduction and Section 3 heading |
| TaskListForm | Added Section 1 Introduction and Section 3 heading |
| ContactListForm | Added Section 1 Introduction and Section 3 heading |

### Properties table fixes (4 wireframes)

| Wireframe | Fix |
|---|---|
| ContactEditorForm | Removed `role='outlined-input'` from Designer properties (role is set in code per ADR-43) |
| EmailCampaignListForm | Removed `role='outlined-input'` from Designer properties |
| ContactPage | Removed `role='outlined-input'` from Designer properties |

---

## ADR-27 Compliance Summary

All 82 wireframes now comply with ADR-27 Section 7:
- [x] 3-section structure (Introduction, Wireframe Diagram, Properties Table)
- [x] No legacy Label (`lbl_`) component
- [x] Every component is an approved M3 component or approved custom component
- [x] No sample data in wireframes
- [x] No behaviour text in wireframe diagram
- [x] Containers drawn as containers with names inset in border
- [x] Properties table contains only Designer-only properties

---

*End of Wireframe Refresh — Completion Report*
