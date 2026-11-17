---
title: "06 — Shared component: VideoDisplayComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-1fe69d1a"
updated: "2026-11-17"
---

## Description

**Feature:** F-09, F-25; shared custom component 3 of 11.
**Depends on:** 02 (phrase keys).

### Forms

- `components.VideoDisplayComponent` — display only; properties `video_url` and `video_type` set in code by the host form; sources `youtube`, `vimeo`, `dailymotion`, `direct`; an empty or invalid address hides the component

### Server functions

- None (host forms supply the address; address detection happens where the address is saved).

### Tables

- None directly (hosts hold the addresses: `business_profile.home_video_url`, `business_profile.services_video_url`, `blog_posts.video_url`, `services.video_url`).

### Acceptance tests

- `ST-11` — component standards
- Consent: a YouTube, Vimeo or Dailymotion video loads only after the visitor accepts cookies; before that a placeholder names the service with a Load button; a direct video address loads at once (pol-legal-documents-and-consent; decision D6 — Mybizz's own help videos at `media.mybizz.live` need no consent)

### Security criteria

- Consent gating before any embedded-content cookie; the component never sets a cookie itself

## Comments
