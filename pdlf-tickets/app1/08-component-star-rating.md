---
title: "08 — Shared component: StarRatingComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-e24b84ea"
updated: "2026-11-17"
---

## Description

**Feature:** F-23; shared custom component 5 of 11.
**Depends on:** 02 (phrase keys).

### Forms

- `components.StarRatingComponent` — input mode: one to five stars, selectable by pointer and keyboard, with an accessible label for each value; display mode: read-only stars with the rating also given as text ("4 out of 5"); filled stars Primary, empty stars Outline, tokens only

### Server functions

- None.

### Tables

- None (hosts write `reviews.rating`, `testimonials.rating`).

### Acceptance tests

- `ST-11`, `DC-13` — component standards; keyboard completion and visible focus; no state shown by colour alone

### Security criteria

- Tokens only; no colour-only state

## Comments
