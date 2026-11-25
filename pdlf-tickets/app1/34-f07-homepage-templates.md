---
title: "34 — F-07 Homepage templates"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-08fa1142"
updated: "2026-11-25"
---

## Description

**Feature:** F-07 Homepage templates (UF-17; DESIGN.md Homepage Template Variants — shared section inventory, differentiation matrix, video visibility rule; SR-13; FR-07).
**Depends on:** 33 (the public pages), 06 (VideoDisplayComponent), 07 (ParallaxComponent), 08 (StarRatingComponent).

### Forms

- `public_pages.HomePage.HomePageClassicTemplate`, `public_pages.HomePage.HomePageServicesTemplate`, `public_pages.HomePage.HomePageBookingTemplate`, `public_pages.HomePage.HomePageMinimalistTemplate` — local templates loaded by `business_profile.homepage_template` (module paths restated under the host form at Step 22, per David's sub-form path rule: every local template nests under its host form's module path)
- `public_pages.HomepageEditorForm` — layout selection, hero, intro, services showcase, video section, testimonials, trust strip, final call to action (DESIGN.md's shared section inventory; no Features element); auto-save every 30 seconds

### Server functions

- `settings_service` home-config functions — the `home_config` key of `config`, read by the templates and written by the editor

### Tables

- `config` (`home_config`), `business_profile` (`homepage_template`), `reviews`, `testimonials` (approved, shown on the homepage)

### Acceptance tests

- `SR-13`; UF-17; DESIGN.md's shared section inventory and video visibility rule (the section hides when the address is empty)
- At most 10 reviews and testimonials together have `show_on_homepage` set

### Security criteria

- B1 — the editor is Owner-only (access matrix `public_pages.HomepageEditorForm`)
- The configuration is data (`home_config`), never code (common aspect 3)

## Comments
