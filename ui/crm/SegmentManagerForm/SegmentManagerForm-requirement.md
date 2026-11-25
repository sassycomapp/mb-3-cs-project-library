---
title: "SegmentManagerForm-requirement"
status: draft
doc-id: "mb-ba73ec17"
updated: "2026-11-25"
---
# SegmentManagerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.SegmentManagerForm` |
| Purpose | Prebuilt and custom segments and the rule builder with "all of" / "any of", preview and count (ticket 20, F-17; UF-22) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_segments`) | "Segments", `headline-large` | `page-title` | — | `crm.segments.title` |
| Segments list | Anvil `ColumnPanel` (`col_segment_list`) | The saved segments, prebuilt and custom, with their counts | — | mobile | — |
| Segment row | `m3.ListItem` (`li_segment_*`, clickable) | One segment per row: name, type, count; the click loads its rules | — | — | — |
| New Segment button | `m3.Button` (`btn_new_segment`, filled) | Starts the builder (UF-22 step 1: New Segment) | `primary-button` | — | `crm.segments.new` |
| Rule builder panel | `m3.Card` (`card_rules`, outlined) | The segment's rules | `surface-card` | mobile | — |
| Rule row | Anvil `FlowPanel` (`flow_rule_*`) | One rule: field, operator, value | — | mobile | — |
| Rule field selector | `m3.DropdownMenu` (`dd_rule_field_*`) | Tags, status, total spend, number of bookings, last booking date, lead source (UF-22 step 1) | `outlined-select` | — | `crm.segments.rule.field` |
| Rule operator selector | `m3.DropdownMenu` (`dd_rule_operator_*`) | The field's operators (is, is not, more than, less than, in the last) | `outlined-select` | — | `crm.segments.rule.operator` |
| Rule value field | `m3.TextBox` (`txt_rule_value_*`, outlined) | The rule's value | `outlined-input` | — | `crm.segments.rule.value` |
| Remove rule button | `m3.IconButton` (`ibtn_remove_rule_*`, icon `mi:close`) | Removes a rule | `icon-button` | — | `crm.segments.rule.remove` |
| Match selector | `m3.ButtonGroup` (`bgrp_match`, connected, single) | "all of" / "any of" (UF-22 step 2) | — | — | `crm.segments.match.all`, `crm.segments.match.any` |
| Add rule button | `m3.Button` (`btn_add_rule`, outlined) | Adds a rule row | `secondary-button` | — | `crm.segments.rule.add` |
| Preview panel | `m3.Card` (`card_preview`, outlined) | The matching contacts and their count (UF-22 step 3) | `surface-card` | mobile | — |
| Preview count | `m3.Heading` (`hdg_preview_count`) | The count, recalculated by the preview | `headline-small` | — | — |
| Preview list | Anvil `ColumnPanel` (`col_preview_rows`) | The matching contacts | — | mobile | — |
| Name field | `m3.TextBox` (`txt_segment_name`, outlined) | The segment's name (UF-22 step 4) | `outlined-input` | — | `crm.segments.name` |
| Save button | `m3.Button` (`btn_save_segment`, filled) | Names and saves the segment | `primary-button` | — | `crm.segments.save` |
| Delete button (custom only) | `m3.Button` (`btn_delete_segment`, text) | Removes a custom segment | `tertiary-button` | — | `crm.segments.delete` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.segments`) | The feature's tooltip and help video | — | — | `help.crm.segments.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Segments | `segments` | `segment_name`, `segment_type` (`prebuilt` / `custom`), `filter_criteria`, `contact_count`, `is_active` |
| The count | `segments` | `contact_count` — recalculated nightly (ticket 20); the preview recalculates live |
| Matching contacts | `contacts` | the rule fields: `tags`, `status`, `total_spent`, `last_contact_date`, `source` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the segments | `form_show` → `load_segments` | `segment_service` list read (`server_customers/segment_service`) | The segments and their counts arrive |
| Open a segment | `li_segment_*_click` | `segment_service` read | The builder loads the segment's rules |
| Preview | the rule handlers | `segment_service` preview (the pure evaluation module beside it) | The matching contacts and their count appear |
| Save | `btn_save_segment_click` | `segment_service` save | The segment is saved; it updates automatically and can be used in campaigns and broadcasts (UF-22 step 4) |
| Delete | `btn_delete_segment_click` | `segment_service` delete | The custom segment is removed |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | People icon (`mi:group`) |
| Heading | `crm.segments.empty.title` (DESIGN.md: "No segments yet") |
| Body text | `crm.segments.empty.body` (DESIGN.md: "Segments group your contacts by behaviour or attributes. They are created automatically or manually." — no primary action, per DESIGN.md's segment empty state) |
| Primary action | None — the empty state explains that segments are auto-created (DESIGN.md: no action for the segment list) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix (`crm.SegmentManagerForm`).

## 7. Text

| Phrase key | English text |
|---|---|
| `crm.segments.title` | Segments |
| `crm.segments.new` | + New Segment |
| `crm.segments.name` | Segment name |
| `crm.segments.match.all` | All of |
| `crm.segments.match.any` | Any of |
| `crm.segments.rule.field` | Field |
| `crm.segments.rule.operator` | Matches |
| `crm.segments.rule.value` | Value |
| `crm.segments.rule.add` | + Add rule |
| `crm.segments.rule.remove` | Remove rule |
| `crm.segments.save` | Save segment |
| `crm.segments.delete` | Delete |
| `crm.segments.preview.title` | Matching contacts |
| `crm.segments.empty.title` | No segments yet |
| `crm.segments.empty.body` | Segments group your contacts by behaviour or attributes. They are created automatically or manually. |
| `crm.segments.type.prebuilt` | Ready-made |
| `crm.segments.type.custom` | Custom |

## 8. Help Videos

| Process | Help video |
|---|---|
| Building a segment | `master_template` help entry `crm.segments` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-22 Custom segment builder | The whole flow: rules, all/any, preview, name and save |
| UF-23 / UF-24 Campaigns | The saved segments the campaigns send to |
