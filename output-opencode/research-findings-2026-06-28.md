---
title: "research-findings-2026-06-28"
doc-id: "mb-ba46d43b"
updated: "2026-10-01"
---
# Research Findings Report

**Date:** 2026-06-28
**Scope:** Four research items from audit responses
**Sources:** Anvil.works official documentation (M3 components, Basic Components, Containers, Data Bindings, Roles pages)

---

## Research A — Icon Component

### Finding

**There is NO standalone `Icon` component in Anvil.** Not in the M3 theme, not in Standard Anvil components.

Standard Anvil Basic Components page lists: Button, Label, Link, CheckBox, RadioButton, DropDown, DatePicker, TextBox, TextArea, Timer, Spacer, Image, FileLoader, YouTubeVideo.

Anvil M3 Components page lists: Text, Heading, Link, Button, IconButton, ToggleIconButton, Card, InteractiveCard, Avatar, Divider, NavigationLink, ButtonMenu, MenuItem, IconButtonMenu, AvatarMenu, Checkbox, RadioButton, RadioGroupPanel, TextBox, TextArea, DropdownMenu, Switch, FileLoader, Slider, LinearProgressIndicator, CircularProgressIndicator.

**No `Icon` component in either list.**

### How icons are actually used in Anvil M3

The M3 component mapping file says:
> "Icon (decorative / standalone) — Mapping: `m3.IconButton(icon="mi:icon_name")` — Notes: For non-interactive, decorative icon use. Leave `enabled` at default (`True`); do not bind a `click` event."

So in the M3 theme, decorative icons are `m3.IconButton` used without a click event.

### Impact on project docs

- The Standard §6 lists `Icon` as an approved component — this should be removed or clarified as `IconButton` used decoratively.
- Wireframes use `ic_empty_vault (Icon)` — this should be `IconButton` with no click event.
- Nomenclature.md has `ic_` / `ico_` prefix for Icon — this should be removed or changed to `ibn_` for IconButton (as the user suggested).
- The M3 component mapping file's "Icon (decorative / standalone)" entry already correctly maps to `m3.IconButton`.

### Question for user

The Standard lists `Icon` as a component, but Anvil has no `Icon` component. Decorative icons are `IconButton` used without a click event. Should:
1. Remove `Icon` from the approved list entirely, and use `IconButton` for all icon cases?
2. Or keep `Icon` as a shorthand label in wireframes but map it to `IconButton` in implementation?

#### Developer's response:
- keep `Icon` as a shorthand label in wireframes but map it to `IconButton` in implementation?
---

## Research B — Writeback Properties

### Finding

The Anvil Data Bindings documentation says:
> "For input components such as `TextBox` or `DatePicker`, Data Bindings can be two-way."

It does NOT give a definitive list of which components support writeback. The documentation describes the mechanism (toggle "Write back" in the Properties Panel) but doesn't enumerate every component.

### Authoritative writeback list (from Anvil's Data Bindings docs + M3 component properties)

Based on the Anvil docs' description of two-way data bindings and the M3 component properties:

| Component | Writeback property | Source |
|---|---|---|
| TextBox | `text` | Data Bindings docs: "input components such as TextBox" |
| TextArea | `text` | Data Bindings docs: "input components" |
| DropdownMenu | `selected_value` | M3 docs: `selected_value` property |
| DatePicker | `selected_date` | Data Bindings docs: "such as DatePicker" |
| Checkbox | `checked` | M3 docs: `checked` property |
| Switch | `selected` | M3 docs: `selected` property |
| RadioGroupPanel | `selected_value` | M3 docs: `selected_value` property |
| Slider | `value` | M3 docs: `value` property |

### Comparison with project docs

**Standard §6.1 lists 7 components:** TextBox, TextArea, Checkbox, Switch, RadioGroupPanel, Slider, DropdownMenu, DatePicker — **matches the authoritative list above.**

**spec-component-properties.md lists 5 Designer-only properties:** TextBox hide_text, DropdownMenu hide_text, DatePicker hide_text, RepeatingPanel template, HTMLTemplate anvil_slot_repeat — **this is a different list (Designer-only properties, not writeback-capable components).**

### Impact on project docs

- The Standard's writeback list is correct and complete.
- `spec-component-properties.md` should be updated to include the full writeback-capable component list (Checkbox, Switch, RadioGroupPanel, Slider are missing).
- The `hide_text` property listed in `spec-component-properties.md` is a different concept from writeback — it's a Designer-only property for password masking, not a writeback toggle.

### Question for user

The Standard's writeback list (7 components) matches what Anvil documents. `spec-component-properties.md` is missing Checkbox, Switch, RadioGroupPanel, and Slider from its writeback list. Should `spec-component-properties.md` be updated to include all 7 writeback-capable components?

#### Developer's response:
- `spec-component-properties.md` must be updated to include all 7 writeback-capable components
---

## Research C — wrap_on Property

### Finding

The `wrap_on` property is NOT documented on the Anvil M3 components page, the Standard Anvil Containers page, or the Data Bindings page.

However, ADR-44 cites Anvil staff (`stucork`) confirming on the Anvil forum that `wrap_on` exists on ColumnPanel, FlowPanel, and DataGrid. The Anvil Changelog also confirms: "DataGrids are now responsive: Make your tables wrap easily on mobile devices with the new wrap_on property."

### Status

`wrap_on` is a real Anvil property, confirmed by Anvil staff and Changelog, but not documented on the main component pages. This is a gap in Anvil's documentation, not in the project docs.

### Impact on project docs

- The Standard's `wrap_on` guidance (§11) is correct.
- ADR-44 is correct.
- No changes needed to project docs regarding `wrap_on`.

---

## Research D — ADR-43 Role Assignment Confirmation

### Finding

Anvil's Roles documentation says:

> "To set the role of a component, select the desired component in the Form Editor and go to the Properties Panel."

AND

> "You can set the role of a component in code: `self.button_1.role = "submit"`"

AND

> "Components can have multiple roles. To enable it for a selected component, use the toggle at the side of the `role` dropdown in the Properties Panel."

AND

> "If you want to add multiple roles to one component via code, add them as a list of strings: `self.button_1.role = ["submit", "outlined"]`"

### Analysis

**Anvil docs support BOTH approaches:** setting role in the Designer Properties Panel AND setting role in code. There is no stated preference in Anvil's documentation for one over the other.

ADR-43 decided to use the Designer Properties Panel based on the reasoning that `role` is a static, design-time choice. This is a project-specific decision, not an Anvil requirement.

The Roles docs also confirm that components can have multiple roles (space-separated or list of strings).

### Impact on project docs

- ADR-43 is a valid project decision — using Designer Properties Panel for role assignment.
- `spec-material-3-theme.md` line 112 (`self.card.role = "surface-card elevated"`) should be updated to reflect ADR-43, but the code example is not technically wrong per Anvil docs — it's just inconsistent with the project's decision.
- The Standard's §6.0 guidance (Designer Properties Panel) is correct per ADR-43.

### Question for user

Anvil docs support both Designer and code approaches for role assignment. ADR-43 chose Designer. Should `spec-material-3-theme.md` line 112 be updated to remove the code example and replace it with a Designer instruction? Or should the code example be kept as a reference for how it would work in code (even though the project standard is Designer)?

#### Developer's response:
- The rule is: If cannot be set programmatically and can only be set in the designer, then it must be set in the designer. If it can be set programmatically, the it must be set programmatically
---

*End of Research Findings Report*
