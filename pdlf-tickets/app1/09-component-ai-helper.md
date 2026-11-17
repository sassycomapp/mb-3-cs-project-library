---
title: "09 — Shared component: AIHelperComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-e00bab3a"
updated: "2026-11-17"
---

## Description

**Feature:** F-26, F-27; shared custom component 6 of 11.
**Depends on:** 02 (phrase keys), 04 (the `ai_request_layer` seam).

### Forms

- `components.AIHelperComponent` — trigger button placed on the host form; floating window over the current page with the assist selector, working area and result area; the host passes its context and the component preselects the matching assist; result actions copy, insert, replace, append, cancel; microphone input (speech-to-text through the server) and spoken output; on failure a clear message and the host form keeps working

### Server functions

- `server_ai/ai_request_layer.request(assist, inputs, context)` — already created at ticket 04; this ticket completes the request/response contract the component needs (no assist logic — the ten assists are ticket 43)

### Tables

- None — AI and speech content is never stored or logged anywhere (design doc §23.9; AC-S13).

### Acceptance tests

- `SR-43` — every AI failure returns a clear message and leaves the host form working
- `SR-53a`, `SR-45a` — payload rules: no personal data beyond placeholder labels; nothing stored or logged
- Tests 4.8–4.9 (spec-test-specification) — AI channel isolation

### Security criteria

- B5 — every request carries the instance's Bearer token to App 5, which alone holds Mybizz's Groq keys; Groq Zero Data Retention on
- AC-S13 — a customer's name is never sent to the AI layer; nothing of the channel is stored or logged in the instance or App 5

## Comments
