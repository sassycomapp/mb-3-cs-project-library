---
title: "42 — F-25 Ease of use, in-app help and language"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-ecf1c03a"
updated: "2026-11-17"
---

## Description

**Feature:** F-25 Ease of use, in-app help and language (`spec-translation` in full; SR-41, SR-41a; FR-25; test ST-14).
**Depends on:** 10 (HelpTooltipComponent), 02 (the translation engine).

### Forms

- None new. This ticket completes the feature across the application: the tooltips switch (saved in `users.preferences.tooltips`, applied on every page via the layouts), the help entries per process with their videos in the app language, and the phrase-key completion.

### Server functions

- `update_preferences` (server_shared/notification_service — own rows only) carries the tooltips preference with the notification preferences

### Tables

- `users` (preferences)

### Acceptance tests

- `ST-14` — every fixed text translated; a language is offered only when every phrase key exists in it (English complete at launch; French, German, Spanish, Italian follow one at a time)
- `SR-41`, `SR-41a` — self-service help on every screen; the three always-available controls work from the layouts

### Security criteria

- B1 — preferences are the signed-in user's own rows only

## Comments
