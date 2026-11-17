---
title: "19 — F-31 Design system"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-076ba193"
updated: "2026-11-17"
---

## Description

**Feature:** F-31 Design system (DESIGN.md in full; `m3-custom-theme.md` §4 and the theme's usage guide; `anvil-ui-build-standard.md`; the design ADRs; FR-31; SR-50).
**Depends on:** 01 (layouts), 02 (shared client), 18 (the Brand Palette tab).

### Forms

- None new. `PaletteSettingsForm` is built at ticket 18. This ticket completes the design system's application: the startup theming calls on `auth.LoginForm` (`m3x_theming.apply_saved()`, `m3x_theming.set_palette(business_profile.palette)`, `m3x_theming.set_mode("system")`), dark mode from the palettes, and the M3 compliance rule every form carries.

### Server functions

- None new (the palette is read from `business_profile` by the startup code through the settings service).

### Tables

- `business_profile` (`palette`, `palette_updated_at`)

### Acceptance tests

- `ST-11`, `DC-12`, `DC-13` — the M3 and interface rules, applied to every form built by every ticket of this run
- The eight named palettes apply live through `m3x_theming.set_palette`; light and dark follow the device setting
- The three status colours are role-named only (`status-success`, `status-pending`, `status-error`)

### Security criteria

- Hard Rule 2 — no hardcoded colours anywhere; every colour resolves through a CSS custom property from the palette system
- ADRs honoured: `material-3-theme-component-scope`, `anvil-extras-exclusion`, `htmltemplate-use`, `dark-mode`, `design-rules`, `responsive-behaviour-mechanism`, `role-property-assignment-mechanism`, `ui-customization-approach`

### Notes

- The per-form M3 checks run at each feature's build step; this ticket closes the system-level proofs.

## Comments
