---
title: "11 — Shared component: EditModeComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-b96eec93"
updated: "2026-11-17"
---

## Description

**Feature:** F-06, F-08, F-21 (edit mode); shared custom component 8 of 11.
**Depends on:** 02 (phrase keys), 09 (the AI helper offered for text).

### Forms

- `components.EditModeComponent` — placed once on each public page form, landing page template and blog post template; visible only to a signed-in Owner or Manager; shows the Edit button; in edit mode marks every element whose name is registered as editable, edits it in place (text, image upload, video address), offers the AI helper for text, and saves every change in one server call

### Server functions

- `save_page_content` (server_settings/page_content_service) — the one save call; the host form registers its editable elements and the content record they belong to (`pages.components`, `config.home_config`, `landing_pages.config`, `blog_posts.content`)

### Tables

- `pages`, `config`, `landing_pages`, `blog_posts` (the content records); `activity_log` (every save recorded)

### Acceptance tests

- `SR-12`, `SR-12a` — edit mode publishes at once; Cancel discards
- `SC-12`, `IN-15` — every edit-mode save writes an audit row with who, what, when, before and after
- `ST-11` — component standards

### Security criteria

- B1 — visible only to Owner and Manager, and the save function carries exactly those roles (AC-S1); a refused call writes nothing
- AC-S12 — the audit trail records every edit

## Comments
