---
title: "07 — Shared component: ParallaxComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-6b8907f1"
updated: "2026-11-17"
---

## Description

**Feature:** F-09; shared custom component 4 of 11.
**Depends on:** 02 (phrase keys).

### Forms

- `components.ParallaxComponent` — background image moving at a different speed from the foreground on scroll; CSS and minimal `anvil.js` (never `eval`); disabled when the device requests reduced motion; text over the image keeps WCAG AA contrast through a token-derived overlay

### Server functions

- None.

### Tables

- None.

### Acceptance tests

- `ST-11`, `DC-13` — component standards; WCAG 2.2 AA contrast; reduced motion honoured (spec-ui-standards §13.9)

### Security criteria

- Tokens only — no hardcoded colours (Hard Rule 2)

## Comments
