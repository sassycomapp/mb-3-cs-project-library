---
title: "10 — Shared component: HelpTooltipComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-12558d1a"
updated: "2026-11-17"
---

## Description

**Feature:** F-25 (in-app help); shared custom component 7 of 11.
**Depends on:** 02 (phrase keys), 06 (the video dialog plays through `VideoDisplayComponent`).

### Forms

- `components.HelpTooltipComponent` — a small help `m3.IconButton` beside a feature's heading or field; accessible label is the feature's plain-language tooltip text; pressing opens a persistent `m3.RichTooltip` with the tooltip text and "Watch the video"; the action opens an `m3` Dialog with the feature's help video in the app language; closing returns focus to the icon; property `help_key` set in code; hidden when the user has switched tooltips off (`users.preferences.tooltips`)

### Server functions

- None — help entries (text and video address per language) are held in `master_template`, so every client receives new and corrected videos with each release.

### Tables

- `users` (preferences read for the tooltips switch — via the session user, never a client-supplied id)

### Acceptance tests

- `ST-11`, `ST-14` — component standards; every text a phrase key, translated
- `SR-41` — self-service help on every screen

### Security criteria

- B1 — the tooltips preference is read from the signed-in user's own record only

## Comments
