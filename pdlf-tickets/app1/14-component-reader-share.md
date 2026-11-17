---
title: "14 — Shared component: ReaderShareComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-1523213e"
updated: "2026-11-17"
---

## Description

**Feature:** F-21; shared custom component 11 of 11.
**Depends on:** 02 (phrase keys).

### Forms

- `components.ReaderShareComponent` — icon buttons for Facebook, LinkedIn, WhatsApp and Copy link; each shared link carries `?ref=` (spec-sm-syndication.md)

### Server functions

- None (the share links are public addresses of the post).

### Tables

- None directly (visit tracking rides the post's counters and the `?ref=` source).

### Acceptance tests

- `ST-11`, `DC-13` — component standards; icon-only buttons carry names
- spec-sm-syndication.md — the reader share rules

### Security criteria

- Tokens only; no colour-only state

## Comments
