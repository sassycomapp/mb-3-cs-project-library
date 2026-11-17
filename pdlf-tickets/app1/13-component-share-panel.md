---
title: "13 — Shared component: SharePanelComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-19fcb544"
updated: "2026-11-17"
---

## Description

**Feature:** F-21 (social syndication); shared custom component 10 of 11.
**Depends on:** 02 (phrase keys), 04 (the AI captions are written server-side through the `ai_request_layer`).

### Forms

- `components.SharePanelComponent` — property: the blog post row; shows the post's preview and one AI-written caption per platform, editable and regenerable; copies with `navigator.clipboard.writeText` and opens platform share links or `navigator.share` through `anvil.js` (spec-sm-syndication.md; DESIGN.md Share Panel)

### Server functions

- `generate_share_captions` (server_blog/share_service) — one caption per platform through the `ai_request_layer`; reads and saves `blog_posts.share_captions`

### Tables

- `blog_posts` (read, `share_captions` write)

### Acceptance tests

- `SR-37a` — the share panel's behaviour; UF-42
- spec-sm-syndication.md — the share rules and `?ref=` tracking
- `ST-11` — component standards

### Security criteria

- B1 — caption generation is Owner/Manager only
- B5 — caption requests carry no personal data and are never stored or logged (AC-S13)

## Comments
