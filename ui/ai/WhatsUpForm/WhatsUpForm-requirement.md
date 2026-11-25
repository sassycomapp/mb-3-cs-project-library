---
title: "WhatsUpForm-requirement"
status: draft
doc-id: "mb-0b80ad1a"
updated: "2026-11-25"
---
# WhatsUpForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `ai.WhatsUpForm` |
| Purpose | The mobile voice view, opened from the home-screen shortcut (`/whatsup`, signed-in team members only): tap the microphone and speak; the transcription, the intent and the follow-up session (ticket 44, F-27; UF-31) |
| Screen type and width | Editor, viewer, form — compact mobile view (the voice view's own compact pattern; the flow's answers use body-large for speech-following readability) |
| Layout | `layouts.BlankLayout` (the voice view is chrome-free) |
| Route | `/whatsup` — Anvil Routing; signed-in team members only (ticket 44) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_whatsup`) | The voice view's one column | — | mobile | — |
| Greeting | `m3.Text` (`text_whatsup_greeting`) | The morning briefing plays here when it is switched on (UF-31 morning briefing) | `body-large` | — | `whatsup.greeting` |
| Microphone button | `m3.FloatingActionButton` (`fab_mic`, icon `mi:mic`, large, floating) | The voice view's one primary control: tap and speak | `fab-button` | — | `whatsup.tap_to_speak` |
| Listening indicator | `m3.LoadingIndicator` (`load_listening`, hidden, contained) | Shown while the audio is captured and transcribed | — | — | — |
| Transcript display | `m3.Text` (`text_whatsup_transcript`) | "The speech is transcribed and understood" (UF-31 step 2) | `body-medium` | mobile | — |
| Answer display | `m3.Card` (`card_whatsup_answer`, elevated) | The answer shown (and spoken via browser SpeechSynthesis or Groq TTS) | `elevated-card` | mobile | — |
| Answer text | `m3.Text` (`text_whatsup_answer`) | The answer's text | `body-large` | mobile | — |
| Spoken-output button | `m3.IconButton` (`ibtn_speak_answer`, icon `mi:volume_up`) | Plays the answer aloud | `icon-button` | — | `ai.speak_result` |
| Confirm bar (tier 4) | Anvil `FlowPanel` (`flow_whatsup_confirm`, hidden) with `m3.Button` (`btn_confirm_voice_form`, filled) and `m3.Button` (`btn_discard_voice_form`, text) | Voice form filling: the form opens filled in, with any missing field highlighted; nothing is saved until the user confirms (UF-31 step 3; tier 4) | — | mobile | `whatsup.confirm`, `editmode.bar.cancel` |
| Follow-up hint | `m3.Text` (`text_whatsup_followup`) | The session's follow-up hint (UF-31 step 4) | `body-small` | — | `whatsup.followup` |
| Unavailable message | `m3.Text` (`text_whatsup_unavailable`, hidden) | "If the data cannot be retrieved, Mybizz CS says so clearly" (UF-31 step 5); the voice channel fails gracefully with a spoken message | `body-medium` | — | `whatsup.unavailable` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Voice content | — | Nothing stored or logged anywhere (pass-through only; ticket 44) |
| The briefing | `bookings`, `invoice`, `tasks`, `leads` | the briefing function's reads |
| The session context | component state | the short context window held in the component state (F-27) |
| Tier 5 figures | the summary tables | the analysis functions' figures; every person's name replaced by a placeholder label before the figures go to the AI layer (F-27) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Speak | `fab_mic_click` | the voice path of `server_ai/ai_request_layer` (`server_ai/voice_service`) | Groq Whisper transcribes server-side; the AI layer interprets the intent; Mybizz CS acts (UF-31 step 3): assist, task, question, navigation, form filling, business question |
| Confirm a filled form | `btn_confirm_voice_form_click` | the host form's save | The record is saved only on confirmation (tier 4 never saves without confirmation) |
| Discard a filled form | `btn_discard_voice_form_click` | None | Nothing is saved |
| Play the briefing | on open (when switched on) | `briefing_service` (`server_ai/briefing_service`) | The 20–30 second spoken summary: today's bookings, outstanding invoices, overdue tasks, new leads, flagged items |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the view opens with its microphone |
| Heading | None — not applicable |
| Body text | `whatsup.greeting` — the greeting and the briefing |
| Primary action | `whatsup.tap_to_speak` (the microphone) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Signed-in team members only (ticket 44).

## 7. Text

| Phrase key | English text |
|---|---|
| `whatsup.greeting` | Ask me anything about your business. |
| `whatsup.tap_to_speak` | Tap and speak |
| `whatsup.confirm` | Confirm |
| `whatsup.followup` | Ask a follow-up — I remember what we just talked about. |
| `whatsup.unavailable` | I couldn't get that just now. Try again in a moment. |
| `whatsup.listening` | Listening… |

## 8. Help Videos

| Process | Help video |
|---|---|
| Using WhatsUp by voice | `master_template` help entry `ai.voice` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-31 WhatsUp voice | The whole flow: speak, transcribe, act, follow-up, briefing |
| UF-30 Using the AI helper | Voice input to every assist (tier 1) |
| UF-26 Tasks and automation | Voice-created tasks (tier 2) |
