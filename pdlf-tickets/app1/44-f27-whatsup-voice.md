---
title: "44 — F-27 WhatsUp voice"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-4ee59047"
updated: "2026-11-17"
---

## Description

**Feature:** F-27 WhatsUp voice (UF-31; `spec-ai-implementation.md` — the voice module, tiers 1–5, proactive briefings, conversational follow-up, natural-sounding voice, browser compatibility; SR-45, SR-45a; FR-27).
**Depends on:** 43 (the AI layer's rules and assists), 02 (shared client).

### Forms

- `ai.WhatsUpForm` — the mobile voice view, opened from the home-screen shortcut (`/whatsup`, signed-in team members only); tap the microphone and speak; the transcription, the intent (assist, task, question, navigation, form filling, business question), the follow-up session

### Server functions

- `server_ai/voice_service` — speech-to-text and the voice intents through the request layer
- `server_ai/briefing_service` — the morning briefing: today's bookings, outstanding invoices, overdue tasks, new leads, flagged items; switched on or off per user

### Tables

- None — voice content is never stored or logged (pass-through only).

### Acceptance tests

- `SR-45`, `SR-45a`; tests 4.8–4.9; UF-31
- Tier 3 and tier 5 questions are read-only; tier 4 writes nothing before the user confirms the filled form; tier 5 sends figures and placeholder labels only, never a person's name

### Security criteria

- B5, AC-S13 — the voice channel is never a personal-data side channel; nothing saved before confirmation at tier 4

## Comments
