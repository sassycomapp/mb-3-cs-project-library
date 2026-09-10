---
document: Mybizz CS — Design Direction (Canonical)
doc-id: design-direction
state: Live
date-created: 2026-07-09T144326+0200
---

# Mybizz CS — Design Direction (Canonical)

**Status:** Final
**Re-issued:** 2026-06-28 — Aligned with `role-property-assignment-mechanism` ADR, `responsive-behaviour-mechanism` ADR, `htmltemplate-use` ADR
**Vertical:** Consulting & Services

**Note:** Generic M3 rules (typography scale, spacing scale, elevation, accessibility
requirements, the `wrap_on` responsive mechanism, state-specification patterns) live in
`specifications-global/spec-m3-design-standards.md` and are not repeated here. This document contains only what is
specific to this project's actual choices.

---

## Visual Direction

**"Clean Professional SaaS"** — a refined, business-focused M3 application for consulting and
services businesses (salons, spas, therapists, consultants, coaches).

**Rationale:** M3's clean surfaces and clear hierarchy suit business productivity; a professional
aesthetic builds trust with this specific user base; the existing `theme.css` already implements
most of this direction.

**Do emulate:** card-based layouts, left navigation rail (desktop) / modal overlay (mobile),
elevated cards for emphasis, filled buttons for primary actions.

**Do not emulate:** playful/childish design, dark mode as primary (light mode is default for this
app), decorative shadows/gradients/animations, complex data visualization in V1, glassmorphism or
neumorphism.

## Color — Rule

**All colors come from the Anvil M3 token/Color Scheme system. No hardcoded hex or RGB values,
anywhere, without exception.**

Semantic status roles (`status-success`, `status-pending`, `status-error`) are a fixed platform
standard — see `specifications-global/spec-m3-design-standards.md` Section 10. This project uses those roles as defined
there; nothing project-specific to state here.

## Anvil Role Map (this project's actual role names)

Reference: `ui-customization-approach` ADR (UI Customization Approach). The *pattern* of defining named roles per component
category is a general standard (see `specifications-global/spec-m3-design-standards.md`); the specific names below are this
project's chosen implementation of that pattern.

### Buttons
| Role Name | Appearance | Use Case |
|---|---|---|
| `primary-button` | filled | Primary actions (Save, Submit, Create) |
| `secondary-button` | outlined | Secondary actions (Cancel, Back, Delete) |
| `tertiary-button` | text | Tertiary actions (Learn More, Help) |
| `icon-button` | icon | Icon-only actions |
| `fab-button` | filled + circular | Floating Action Button |

### Cards
| Role Name | Card Type | Use Case |
|---|---|---|
| `surface-card` | outlined | Standard content container |
| `elevated-card` | elevated | Emphasized/featured content |
| `tonal-card` | filled | Background for content groups |
| `data-card` | outlined | Data grid rows, list items |
| `metric-card` | elevated | Dashboard metrics, KPIs |

### Containers, Headings, Navigation, Forms, Alerts
(Same pattern — see prior version of this document for the full name list if needed; omitted
here for brevity since the categories themselves are generic and only the presence of this
mapping is project-specific.)

## Project-Specific Value Deviations from M3 Defaults

Where this project's actual values differ from M3's own defaults (stated explicitly so the
deviation is visible, not silently inherited):

| Element | M3 Default | This Project's Value | Reason |
|---|---|---|---|
| Card radius | 12px | 8px | "MUI overlay" aesthetic choice |

---

*Design Direction (Canonical) v2.0 — split from the original design-direction.md. Generic content
moved to `specifications-global/spec-m3-design-standards.md`.*
