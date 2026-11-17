---
title: "01 — Layouts and the application shell"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-41450162"
updated: "2026-11-17"
---

## Description

**Feature:** The application shell every page sits on (scaffold-spec Layouts; architecture reference §9; F-31, F-25).
**Depends on:** None (can start immediately).

The five layout forms with the admin and customer menus, the three always-available controls, and the sample-data banner. This is the first ticket of the build order (mo-ui-production §3): every page form sits on a layout.

### Forms

- `layouts.AdminLayout` — NavigationRailLayout; admin menu per architecture reference §9 (menu items, order, role visibility); the sample-data banner
- `layouts.CustomerLayout` — NavigationDrawerLayout; the customer portal menu
- `layouts.PublicLayout` — NavigationDrawerLayout; public header and the FooterComponent slot
- `layouts.BlankLayout` — no header, no navigation: sign-in, landing pages, errors
- `layouts.ErrorLayout` — 404, 403, 500 on BlankLayout

The three always-available controls (tooltips on/off, help videos menu, dashboard) live in the layout forms (spec-ui-standards §12). Navigation uses the layout's own `NavigationLink` components with loop-capturing lambda handlers and `open_form()` (decision D5); `navigate_to` is never used.

### Server functions

- `get_layout_context` (server_settings/settings_service) — palette, feature switches (`marketing_enabled`, `blog_enabled`), onboarding status, sample-data-banner state; returns only what the calling role may see

### Tables

- `business_profile` (read), `config` (read), `users` (role, read)

### Acceptance tests

- `BR-03` — restricted links hidden, not disabled
- `IN-02` — `get_layout_context` as every role per the access matrix
- `ST-11` — forms are well-formed HTML templates; `DC-12`, `DC-13` — M3 and interface rules
- `ST-14` — every visible text is a phrase key

### Security criteria

- B1: the interface hides what a role cannot use and is never the enforcement (AC-S15; AC-S1 pattern)
- B7: sign-out in both menus; no role state decided client-side
- No colour values in any template (Hard Rule 2); roles only

### Notes

- Existing forms (if any at these paths) are rebuilt as HTML templates at the same module path; `form_template.yaml` forms are replaced by `form_template.html` + `__init__.py` (spec-ui-standards §4).
- Every existing package folder and its `__init__.py` are never removed.

## Comments
