# Design Reference Screens Update Report — 2026-06-18

**File:** `gstack-outputs/design-reference-screens.html`
**Task:** Update design reference screens to reflect current design system with MUI aesthetic overlay.

---

## What Changed from Prior Version

The prior version (dated 2026-05-10) was created before the MUI aesthetic overlay was adopted, before AdminLayout and ErrorLayout were specified, and before empty state patterns and design remediation additions were applied.

### Major Changes

| Area | Old Version | New Version |
|------|-------------|-------------|
| Card border radius | 12px (M3 default) | 8px (MUI overlay) |
| Card shadow | M3 2dp elevation | MUI dual-layer `hsla(220,30%,5%,0.07)` |
| Heading weight | 400 (M3 default) | 600 (MUI override on Headline roles) |
| Dividers | Solid `#E7E0EC` | outline-variant at 40% opacity |
| Left nav background | Primary overlay gradient | `surface-container-low` (flat) |
| Left nav border | `#E7E0EC` | outline-variant at 40% opacity |
| App bar background | Surface with gradient on scroll | Surface token, hairline bottom border |
| Nav structure | Flat list (Dashboard, Bookings, Contacts, Finance, Campaigns, Blog, Settings) | Grouped per AdminLayout spec (Dashboard, Sales & Operations, Customers & Marketing, Content & Website, Finance & Reports, Settings, Vault, Sign Out) |
| Typography section | None | Full type scale (Display through Caption) with MUI weight annotations |
| Palette section | None | 4 palette swatches with target business types |
| Empty states | None | 2 examples (Invoice list, Vault) per DESIGN.md spec |
| MUI comparison | None | Side-by-side M3 default vs MUI applied |
| ErrorLayout | None | 404 example per design-remediation-report |
| Component labels | Present but inconsistent | Systematic `.component-label` annotations |
| Color tokens | Hardcoded hex throughout | CSS custom properties from Professional Blue palette |

### Sections in New Version

1. **Typography Scale** — Full type scale with MUI heading weight annotation
2. **AdminLayout** — Sidebar nav with correct groups, conditional visibility, MUI surface-container-low
3. **Buttons** — Filled, outlined, text, tonal, icon, disabled states
4. **Cards** — Outlined, elevated (MUI shadow), tonal variants
5. **Inputs** — Default, focused, error, disabled, dropdown, date picker
6. **Badges, Chips, Data Grid** — Status badges, filter chips, data grid with 24px radius
7. **Dashboard** — Metric cards, chart placeholders, recent bookings table
8. **Empty States** — Invoice list and Vault empty state patterns
9. **MUI Overlay Comparison** — Side-by-side M3 default vs MUI applied
10. **Palette System** — 4 palette swatches (Professional Blue default)
11. **ErrorLayout** — 404 page example

---

## MUI Variables Demonstrated

All 9 MUI aesthetic variables from DESIGN.md are visibly demonstrated:

| # | Variable | Where Demonstrated |
|---|----------|-------------------|
| 1 | Surface distinction | Body (surface) vs nav/panels (surface-container-low) — Section 2 |
| 2 | Card shadow (dual-layer) | Elevated cards in Sections 4, 7 — hover increases shadow |
| 3 | Card border radius (8px) | All card variants in Section 4 — annotated vs M3 default |
| 4 | Typography heading weight (600) | Headline Large and Small in Section 1, Section 9 comparison |
| 5 | Dividers (40% opacity) | Nav dividers, section breaks, Section 9 comparison |
| 6 | State hover opacity (0.08) | Nav items and buttons in Sections 2, 3 |
| 7 | State pressed/focused opacity (0.12) | Input focus in Section 5 |
| 8 | App bar (surface, hairline) | App bar in Sections 2, 7 |
| 9 | Left nav (surface-container-low, hairline) | Sidebar in Sections 2, 7 |

---

## Design Spec Gaps Found

| Gap | Status | Impact |
|-----|--------|--------|
| Booking status badge colors | Defined in design-remediation-report | Used Confirmed/Pending/Cancelled badges in data grid |
| AdminLayout nav item icons | Not specified (no icon names in DESIGN.md) | Used generic Unicode symbols as placeholders |
| Metric card icon specification | Not specified | Used generic Unicode symbols as placeholders |
| Empty state icon specification | Not specified beyond "icon" | Used Unicode symbols matching screen context |

---

## Source Documents Used

| Document | Purpose |
|----------|---------|
| `docs/DESIGN.md` | Canonical design system — tokens, typography, spacing, roles, empty states, MUI section |
| `docs/screen-production-standard.md` | Screen production rules — M3 token block, layout patterns, form components |
| `docs/DESIGN_palette_section.md` | Professional Blue palette tokens (32 tokens) |
| `docs/theme_patched.css` | Production theme.css with MUI patch — CSS custom properties used in output |
| `docs/theme_mui_patch.css` | MUI patch in isolation — 7 sections, 9 variables |
| `docs/m3_component_mapping.md` | Approved M3 components and their Anvil classes |
| `docs/ui-standards.md` | M3 mandate, component naming, Designer-only properties |
| `docs/spec-material-3-theme.md` | Theme architecture, dependency details, customization hierarchy |
| `adr/37-ui-customization-approach.md` | MUI is aesthetic-only CSS overlay, not component library |
| `adr/15-material-3-theme-component-scope.md` | Permitted M3 components, custom composition rules |
| `gstack-outputs/design-remediation-report-2026-06-16.md` | AdminLayout, ErrorLayout, empty states, booking design, Vault spec |

---

*End of Update Report — 2026-06-18*
