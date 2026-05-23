# Mybizz CS — Design Direction

**Status:** Final  
**Date:** 2026-05-10  
**Vertical:** Consulting & Services  
**Platform:** Anvil.works with Material Design 3 (M3)  
**Version:** 1.0 (Consolidated & Approved)

---

## Design Direction Summary

### Visual Direction: "Clean Professional SaaS"

A refined, business-focused Material 3 design emphasizing clarity, efficiency, and trustworthiness for consulting and services businesses (salons, spas, therapists, consultants, coaches).

**Rationale:**
- Material 3's clean surfaces, clear typography hierarchy, and intuitive navigation are ideal for business productivity
- Professional aesthetic builds trust with business users
- Existing theme.css already implements 80% of this direction — we refine and extend

### Do Emulate

- ✅ M3 color system with primary/secondary containers for hierarchy
- ✅ Card-based layouts for content organization
- ✅ 4px spacing rhythm (already in theme.css)
- ✅ Roboto typography with clear scale (display, headline, title, body)
- ✅ Filled buttons for primary actions, outlined for secondary
- ✅ Left navigation rail for desktop, modal overlay for mobile
- ✅ Elevated cards for emphasis, outlined for standard content
- ✅ Rounded corners (12px cards, 100px buttons)

### Do Not Emulate

- ❌ Playful/childish designs (no bright gradients, no overly rounded corners >24px)
- ❌ Dark mode as primary (stick to light mode for business clarity)
- ❌ Overly decorative elements (no unnecessary shadows, gradients, or animations)
- ❌ Complex data visualizations in V1 (keep charts simple)
- ❌ Glassmorphism or neumorphism trends (stick to solid surfaces)

---

## Design Tokens

### Color Palette (M3 System)

**Primary Palette:**
- Primary: `%color:Primary%`
- On Primary: `%color:On Primary%`
- Primary Container: `%color:Primary Container%`
- On Primary Container: `%color:On Primary Container%`

**Secondary Palette:**
- Secondary: `%color:Secondary%`
- On Secondary: `%color:On Secondary%`
- Secondary Container: `%color:Secondary Container%`
- On Secondary Container: `%color:On Secondary Container%`

**Neutral Palette:**
- Surface: `%color:Surface%`
- On Surface: `%color:On Surface%`
- Surface Variant: `%color:Surface Variant%`
- On Surface Variant: `%color:On Surface Variant%`
- Outline: `%color:Outline%`
- Background: `%color:Background%`
- On Background: `%color:On Background%`

**Status Colors:**
- Error: `%color:Error%`
- On Error: `%color:On Error%`
- Disabled Container: `%color:Disabled Container%`
- On Disabled: `%color:On Disabled%`
- Light Overlay 1%: `%color:Light Overlay 1%`
- Light Overlay 2%: `%color:Light Overlay 2%`
- Dark Overlay 1%: `%color:Dark Overlay 1%`
- Dark Overlay 2%: `%color:Dark Overlay 2%`

**Hardcoded Exception:**
- None identified at this time. All colors use Anvil tokens.

### Typography

**Font Family:** Roboto (Google Fonts) — fallback: Noto Sans, Arial, sans-serif

| Scale | Size | Line Height | Weight | Use Case |
|-------|------|-------------|--------|----------|
| Display | 57px | 64px | 400 | Hero headlines (rare) |
| Headline Large | 32px | 40px | 400 | Page titles |
| Headline Small | 22px | 28px | 500 | Section headers |
| Title | 18px | 24px | 500 | Card titles |
| Body Large | 14px | 20px | 400 | Body text |
| Body | 14px | 20px | 400 | Standard text |
| Button | 14px | 14px | 500 | Button labels |
| Label | 12px | 16px | 500 | Form labels |
| Caption | 12px | 16px | 400 | Helper text, captions |

### Spacing Scale (4px Base Unit)

| Token | Value | Use Case |
|-------|-------|----------|
| `--space-1` | 4px | Tiny spacing, tight gaps |
| `--space-2` | 8px | Small spacing, component padding |
| `--space-3` | 12px | Medium spacing, section gaps |
| `--space-4` | 16px | Large spacing, card padding |
| `--space-6` | 24px | Huge spacing, section margins |
| `--space-8` | 32px | Extra huge, hero spacing |

### Border Radius Rules

| Element | Radius | Notes |
|---------|--------|-------|
| Buttons | 100px | Pill shape |
| Cards | 12px | Standard |
| Dialogs | 28px | Modal dialogs |
| Inputs (Outlined) | 4px | Text boxes, dropdowns |
| Inputs (Filled) | 0px | Bottom border only |
| Chips/Badges | 8px | Tags, filters |
| Navigation Items | 100px | Pill shape |
| Data Grid | 24px | Header, container |

### Elevation Rules

| Element | Elevation | Notes |
|---------|-----------|-------|
| Surface | 0dp | Flat |
| Card (Elevated) | 2dp | Standard shadow |
| Card (Tonal) | 0dp | Background color only |
| Dialog | 24dp | Modal shadow |
| Navigation Rail | 0dp | Border only |
| App Bar | 0dp | Border only |

### Density Guidance

| Density | Row Height | Padding | Gap | Use Case |
|---------|------------|---------|-----|----------|
| Compact | 40px | 8px | 4-8px | Data-heavy screens, tables |
| Default | 48px | 12px | 8-16px | Standard forms, lists |
| Comfortable | 56px | 16px | 12-24px | Read-only views, forms |

### Input State Specifications

| State | Border Color | Background | Text Color | Notes |
|-------|--------------|------------|------------|-------|
| **Default** | Outline | Surface Variant | On Surface | Normal state |
| **Focused** | Primary | Surface Variant | On Surface | Active input, focus ring |
| **Error** | Error | Surface Variant | Error | Validation failed |
| **Disabled** | On Disabled | Dark Overlay 1% | On Disabled | 38% opacity |
| **Read-only** | Outline | Surface | On Surface Variant | Display only |

**Hardcoded Exception:**
- None identified. All states use Anvil tokens.

---

## Anvil Role Map

### Buttons

| Role Name | Appearance | Use Case | Min Height |
|-----------|------------|----------|------------|
| `primary-button` | filled | Primary actions (Save, Submit, Create) | 40px |
| `secondary-button` | outlined | Secondary actions (Cancel, Back, Delete) | 40px |
| `tertiary-button` | text | Tertiary actions (Learn More, Help) | 40px |
| `icon-button` | icon | Icon-only actions (Edit, Delete, More) | 36px |
| `fab-button` | filled + circular | Floating Action Button (Create New) | 56px |

### Cards

| Role Name | Card Type | Use Case |
|-----------|-----------|----------|
| `surface-card` | outlined | Standard content container |
| `elevated-card` | elevated | Emphasized content (featured items) |
| `tonal-card` | filled | Background for content groups |
| `data-card` | outlined | Data grid rows, list items |
| `metric-card` | elevated | Dashboard metrics, KPIs |

### Containers

| Role Name | Use Case |
|-----------|----------|
| `page-container` | Main content wrapper |
| `section-container` | Content sections within pages |
| `form-container` | Form grouping |
| `card-container` | Card-based layouts |
| `grid-container` | Responsive grids |

### Headings

| Role Name | Size | Use Case |
|-----------|------|----------|
| `page-title` | Headline Large | Page headings |
| `section-title` | Headline Small | Section headings |
| `card-title` | Title | Card headings |
| `label-title` | Body Large | Form labels |

### Navigation

| Role Name | Use Case |
|-----------|----------|
| `nav-item` | Navigation rail items |
| `nav-item-selected` | Active navigation item |
| `app-bar-link` | App bar navigation links |
| `breadcrumb-item` | Breadcrumb navigation |

### Forms

| Role Name | Input Type | Use Case |
|-----------|------------|----------|
| `outlined-input` | TextBox, TextArea | Standard text input |
| `outlined-select` | DropdownMenu | Select dropdown |
| `outlined-date` | DatePicker | Date picker |
| `input-error` | All inputs | Error state |
| `input-disabled` | All inputs | Disabled state |
| `input-readonly` | All inputs | Read-only state |

### Alerts & Feedback

| Role Name | Use Case |
|-----------|----------|
| `alert-success` | Success messages |
| `alert-error` | Error messages |
| `alert-warning` | Warning messages |
| `alert-info` | Informational messages |
| `notification` | Toast notifications |

### Data Display

| Role Name | Use Case |
|-----------|----------|
| `tonal-data-grid` | Data grids, tables |
| `metric-display` | Metric cards, KPIs |
| `chart-container` | Plot/Chart containers |
| `tag` | Tags, badges |
| `chip` | **Custom Component** — composed from `m3.Button` or `m3.Card` per ADR-014 |

---

## Component Implementation Table

### Buttons

| Component | Anvil Type | Role | Key Properties | Designer Settings | theme.css | Advanced Notes |
|-----------|------------|------|----------------|-------------------|-----------|----------------|
| Primary Button | Button | `primary-button` | appearance: filled, min-height: 40px | Set appearance to "filled" | `.anvil-role-filled-button` | Use `role` property for semantic styling |
| Secondary Button | Button | `secondary-button` | appearance: outlined, min-height: 40px | Set appearance to "outlined" | `.anvil-role-outlined-button` | Border color from `Outline` token |
| Tertiary Button | Button | `tertiary-button` | appearance: text, min-height: 40px | Set appearance to "text" | Custom CSS | Text button, no border |
| Icon Button | Button | `icon-button` | appearance: icon, icon-only | Add icon, set appearance to "icon" | Custom CSS | Circular icon buttons need custom CSS |
| FAB | Button | `fab-button` | appearance: filled, circular, 56px | Set appearance, enable circular in Designer | `.anvil-role-tonal-button` | Not native M3 - use tonal-button with 56px circular |

### Cards

| Component | Anvil Type | Role | Key Properties | Designer Settings | theme.css | Advanced Notes |
|-----------|------------|------|----------------|-------------------|-----------|----------------|
| Surface Card | ColumnPanel | `surface-card` | border: 1px solid, border-radius: 12px | Set border in Designer | `.anvil-role-outlined-card` | Use ColumnPanel for card content |
| Elevated Card | ColumnPanel | `elevated-card` | border-radius: 12px, shadow: 2dp | Set shadow in Designer | `.anvil-role-elevated-card` | Shadow uses M3 elevation tokens |
| Tonal Card | ColumnPanel | `tonal-card` | background: Surface Variant, border-radius: 12px | Set background color in Designer | `.anvil-role-tonal-card` | No border for tonal cards |
| Metric Card | ColumnPanel | `metric-card` | elevated + custom metric layout | Combine elevated-card + metric styling | Extend `elevated-card` | Add metric icon + value + label structure |

### Navigation

| Component | Anvil Type | Role | Key Properties | Designer Settings | theme.css | Advanced Notes |
|-----------|------------|------|----------------|-------------------|-----------|----------------|
| Nav Rail Item | Link/Button | `nav-item` | padding: 4px 16px, border-radius: 100px | Set padding, add icon | `.left-nav a` | Use Link components with icons |
| Selected Nav | Link/Button | `nav-item` | background: Secondary Container | Set role property | `.left-nav a.anvil-role-selected` | Visual only - logic in code |
| App Bar | Container | - | min-height: 56px, fixed position | Set in standard-page.html | `.app-bar` | Use provided theme template |
| Sidebar Toggle | Button | - | icon-only, 30px | Add icon button | `.sidebar-toggle` | Provided in theme |

### Forms

| Component | Anvil Type | Role | Key Properties | Designer Settings | theme.css | Advanced Notes |
|-----------|------------|------|----------------|-------------------|-----------|----------------|
| Text Input | TextBox | `outlined-input` | appearance: outlined, min-height: 48px | Set appearance to "outlined" | `.anvil-role-outlined` | Border from `Outline` token |
| Select Dropdown | DropdownMenu | `outlined-select` | appearance: outlined, height: 48px | Set appearance in Designer | `.anvil-role-outlined` | Custom arrow from theme |
| Date Picker | DatePicker | `outlined-date` | appearance: outlined, min-height: 48px | Set appearance in Designer | `.anvil-role-outlined` | Uses M3 date picker |
| Error State | TextBox | `input-error` | border-color: Error, error message | Set error property in code | `.anvil-role-input-error` | Error color from `Error` token |
| Disabled State | TextBox | `input-disabled` | background: Dark Overlay 1%, color: On Disabled | Set disabled property | `.anvil-role-input-disabled` | 38% opacity |

### Data Display

| Component | Anvil Type | Role | Key Properties | Designer Settings | theme.css | Advanced Notes |
|-----------|------------|------|----------------|-------------------|-----------|----------------|
| Data Grid | DataGrid | `tonal-data-grid` | border-radius: 24px, header background | Set in Designer | `.anvil-role-tonal-data-grid` | Header has rounded top corners |
| Metric Display | ColumnPanel | `metric-display` | elevated + custom layout | Combine elevated-card + metric styling | Extend `metric-card` | Icon + value + label structure |
| Chart | Plot | `chart-container` | border-radius: 12px, border: 1px | Set in Designer | `.plot-container.plotly` | Uses Plotly.js |
| Tag | Label | `tag` | background: Surface Variant, border-radius: 8px | Set background in Designer | Custom CSS | Use Label with custom role |
| Chip | **Custom Component** | `chip` | **Composed from `m3.Button` or `m3.Card`** | See ADR-014 | Custom CSS | Not a native M3 component |

---

## Screen System

### Homepage / Landing Page

**Layout:**
- Full-width hero section with primary CTA
- Feature cards in 3-column grid (desktop) / 1-column (mobile)
- Pricing section with 2-3 tier cards
- Footer with navigation links

**Responsive Behavior:**
- Desktop: Fixed left nav, 3-column grids
- Tablet: Fixed left nav, 2-column grids
- Mobile: Modal nav, 1-column grids

**Anvil Implementation:**
- Use `standard-page.html` template
- Hero: ColumnPanel with image + text + CTA buttons
- Features: FlowPanel with 3 ColumnPanels (cards)
- Pricing: FlowPanel with 3 Card containers
- Footer: ColumnPanel with 4-column grid

**Key Visual Details:**
- Hero: 64px padding top/bottom, 24px horizontal
- Cards: 16px padding, 12px border-radius
- Grid gaps: 16px between cards
- Buttons: 16px horizontal spacing

### Dashboard

**Layout:**
- Left navigation rail (collapsible on mobile)
- App bar with page title + user menu
- Metric cards row (4 metrics)
- Main content area with charts + data tables

**Responsive Behavior:**
- Desktop: 240px fixed nav, content fills remainder
- Tablet: 240px fixed nav, 2-row metric grid
- Mobile: Modal nav, 1-column metric grid

**Anvil Implementation:**
- Nav: ColumnPanel with nav items (Links)
- App bar: Provided by template
- Metrics: FlowPanel with 4 Card containers
- Charts: ColumnPanel with Plot components
- Tables: DataGrid with tonal styling

**Key Visual Details:**
- Metric cards: 16px padding, 12px border-radius
- Chart containers: 24px padding, 12px border-radius
- Table header: 48px row height, 12px padding
- Grid gaps: 16px between metrics

### Form Page

**Layout:**
- Left navigation rail
- App bar with page title + Save/Cancel buttons
- Form in single-column layout
- Form groups with clear labels

**Responsive Behavior:**
- Desktop: Two-column form layout for wide forms
- Tablet: Two-column form layout
- Mobile: Single-column form layout

**Anvil Implementation:**
- Form groups: ColumnPanel per section
- Inputs: TextBox, DropdownMenu, DatePicker with outlined role
- Labels: Label components with title role
- Actions: FlowPanel with primary/secondary buttons

**Key Visual Details:**
- Form group spacing: 24px between groups
- Input height: 48px min-height
- Label spacing: 8px above input
- Button spacing: 8px between buttons

### Detail Page

**Layout:**
- Left navigation rail
- App bar with page title + action buttons
- Header section with primary info
- Content sections in cards
- Related items list

**Responsive Behavior:**
- Desktop: Two-column layout (details + related)
- Tablet: Two-column layout
- Mobile: Single-column layout

**Anvil Implementation:**
- Header: Card with elevated role
- Sections: ColumnPanels with tonal cards
- Related items: DataGrid or list of cards
- Actions: FlowPanel with action buttons

**Key Visual Details:**
- Header card: 24px padding, 24px border-radius
- Section cards: 16px padding, 12px border-radius
- Related items: 8px row height, 12px padding
- Action buttons: 16px horizontal spacing

### List Page

**Layout:**
- Left navigation rail
- App bar with page title + filter/search
- List of items in data grid or card list
- Pagination controls

**Responsive Behavior:**
- Desktop: Data grid with 6+ columns
- Tablet: Data grid with 4 columns or card list
- Mobile: Card list with 1 item per row

**Anvil Implementation:**
- Search/Filter: FlowPanel with TextBox + DropdownMenu
- List: DataGrid with tonal styling OR FlowPanel with cards
- Pagination: FlowPanel with page numbers + next/prev
- Empty state: Label with helpful message

**Key Visual Details:**
- Data grid: 48px row height, 12px padding
- Card list: 16px padding, 12px border-radius
- Search box: 48px height, 16px padding
- Pagination: 40px height, 8px horizontal spacing

### Settings / Admin Page

**Layout:**
- Left navigation rail (nested)
- App bar with page title
- Settings sections in cards
- Toggle switches for boolean settings

**Responsive Behavior:**
- Desktop: Two-column layout for settings
- Tablet: Two-column layout
- Mobile: Single-column layout

**Anvil Implementation:**
- Sections: ColumnPanel with surface cards
- Settings: RowPanel with Label + toggle/dropdown
- Save area: FlowPanel with primary button
- Info boxes: Tonal cards with info icons

**Key Visual Details:**
- Section cards: 24px padding, 12px border-radius
- Settings rows: 48px height, 16px padding
- Toggle: Custom toggle or checkbox with styling
- Save button: 16px horizontal spacing from info

---

## Anvil Design Rules

### General Rules

1. **Always use M3 components** — Never use Anvil Extras components unless explicitly noted as advanced/optional
2. **Use roles for styling** — Never hardcode colors or styles in Designer — always use roles
3. **Follow 4px spacing** — All margins/padding must be multiples of 4px
4. **Use theme colors** — Never hardcode hex colors — always use `%color:Token%` syntax
5. **Test on mobile** — All designs must work on mobile (375px minimum)

### Designer Rules

1. **Set appearance properties** — Always set `appearance` property on buttons and inputs
2. **Use role property** — Always assign semantic roles to components
3. **Set min-height** — Buttons must have `min-height: 40px`
4. **Use ColumnPanels for cards** — Never use Container for cards — use ColumnPanel
5. **Enable Write Back** — Only enable Write Back for forms that need data binding
6. **Use full-width rows** — Use full-width rows for card headers/footers
7. **Set proper padding** — Card padding: 16px, Section padding: 24px

### Code Rules

1. **Assign roles in code** — Always assign roles programmatically in `__init__`
2. **Use theme tokens** — Never hardcode colors in code — use `%color:Token%`
3. **Validate inputs** — Always validate user input before server calls
4. **Handle errors** — Always show user-friendly error messages
5. **Use RBAC** — Always check user roles before showing sensitive data

### Responsive Rules

1. **Mobile-first** — Design for mobile first, then expand for desktop
2. **Breakpoint at 998px** — Mobile < 998px, Desktop >= 998px
3. **Nav behavior** — Fixed nav on desktop, modal overlay on mobile
4. **Grid columns** — 1 column mobile, 2 columns tablet, 3+ columns desktop
5. **Card density** — Compact density on mobile, default on desktop

### Accessibility Rules (WCAG 2.1 AA)

1. **Contrast** — Minimum 4.5:1 for normal text, 3:1 for large text (24px+)
2. **Focus states** — Visible focus indicators on all interactive elements
3. **Keyboard navigation** — All functions accessible via keyboard (Tab, Enter, Escape)
4. **Input states** — Clear visual distinction for default/focused/error/disabled/read-only
5. **Labels** — All form inputs have associated labels (visible or aria-label)
6. **Error messages** — Error messages are descriptive and linked to inputs

### Property-Setting Rule

**Every property that can be set programmatically must be handled programmatically.**

- **Designer:** Only set properties that cannot realistically or safely be set in code
- **Code:** Set all other properties programmatically in `__init__` or event handlers

**Examples:**
- ✅ **Designer:** Component text, appearance (filled/outlined), visibility
- ✅ **Code:** Roles, data bindings, dynamic text, validation, disabled state

---

## Implementation Priority

### Phase 1 (MVP)

**Goals:** Core scaffolding and foundational UI

**Tasks:**
1. ✅ Theme tokens (already in theme.css)
2. ✅ standard-page.html (already in place)
3. ✅ Basic roles (buttons, cards, inputs)
4. ✅ Dashboard layout
5. ✅ Form layouts
6. ✅ Navigation rail (desktop + mobile)
7. ✅ Page layouts (dashboard, form, list)

**Timeline:** ~1-2 weeks

### Phase 2 (Enhancement)

**Goals:** Complete feature set and polish

**Tasks:**
8. Data grid styling (tonal-data-grid)
9. Chart containers (plotly integration)
10. Alert/notification roles
11. Chip/tag roles
12. Modal/dialog styling
13. Empty state components
14. Loading states and spinners

**Timeline:** ~2-3 weeks

### Phase 3 (Optimization)

**Goals:** Advanced features and optimization

**Tasks:**
15. Dark mode support (deferred)
16. Advanced animations
17. Performance optimization
18. Accessibility enhancements
19. Custom components (Material Web if needed)
20. Design system documentation

**Timeline:** ~1-2 weeks

---

## Hardcoded Exceptions

At this time, **no hardcoded exceptions** are required. All colors, spacing, and styling can be handled via Anvil tokens and theme.css.

If hardcoded values become necessary in the future, they will be documented using the explicit format:

```
Hardcoded Exception:
- value: #xxxxxx
- reason: <why token syntax could not be used>
- scope: <exact component / case>
```

---

*End of file — design-direction.md (v1.0, Consolidated & Approved)*
