---
title: "AIHelperComponent-requirement"
status: draft
doc-id: "mb-ba2646e2"
updated: "2026-11-25"
---
# AIHelperComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.AIHelperComponent` |
| Purpose | The one reusable AI helper: a trigger button on the host form, a floating window with the assist selector, the working area and the result area, result actions, voice input and spoken output (ticket 09, F-26, F-27) |
| Screen type and width | Custom component; the floating window opens over the current page without navigation, in the host's viewport |
| Layout | A component form built from theme components; every request goes through the shared AI request layer in `server_ai/`, routed through App 5 |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Trigger button | `m3.Button` (`btn_ai_trigger`, filled, icon `mi:auto_awesome`) | The theme's filled button is the one primary action on the host area where it is placed; the host places it beside the field it serves (spec-ai-implementation, Host Contexts) | `primary-button` | — | `ai.open` |
| Floating window container | `m3.Card` (`card_ai_window`, elevated) | The floating window over the current page; elevated for emphasis (DESIGN.md cards: elevated only where emphasis is needed) | `elevated-card` | mobile | — |
| Window title | `m3.Text` (`text_ai_title`) | The assist's name | `title-medium` | — | set per assist, e.g. `ai.assist.bio.title` |
| Assist selector | `m3.DropdownMenu` (`dd_assist`) | The theme's dropdown menu lists the ten assists; the host's context preselects the matching one, and the user can switch | `outlined-select` | — | `ai.assist.label` |
| Working area | Anvil `ColumnPanel` (`col_ai_inputs`) | The selected assist's input fields, loaded by the component (dynamic content region) | — | mobile | — |
| Voice input button | `m3.IconButton` (`ibtn_mic`, icon `mi:mic`) | The microphone starts speech-to-text through the server (Groq Whisper via App 5); the transcription fills the assist's fields as if typed (voice tier 1) | `icon-button` | — | `ai.speak` |
| Generate button | `m3.Button` (`btn_generate`, filled) | The request's one primary action | `primary-button` | — | `ai.generate` |
| Result area | Anvil `ColumnPanel` (`col_ai_result`) | The generated result, readable and editable before use | — | mobile | — |
| Result text | `m3.Text` (`text_ai_result`) | The generated text | `body-medium` | — | — |
| Spoken-output button | `m3.IconButton` (`ibtn_speak`, icon `mi:volume_up`) | Browser SpeechSynthesis by default; Groq TTS where the natural-sounding voice is on (F-27) | `icon-button` | — | `ai.speak_result` |
| Copy action | `m3.Button` (`btn_copy`, text) | Low-emphasis action (spec-ui-standards section 8.1) | `tertiary-button` | — | `ai.result.copy` |
| Insert action | `m3.Button` (`btn_insert`, outlined) | Places the result into the host field the component was opened from | `secondary-button` | — | `ai.result.insert` |
| Replace action | `m3.Button` (`btn_replace`, text) | Replaces the host field's content | `tertiary-button` | — | `ai.result.replace` |
| Append action | `m3.Button` (`btn_append`, text) | Appends to the host field's content | `tertiary-button` | — | `ai.result.append` |
| Cancel action | `m3.Button` (`btn_cancel`, text) | Closes the window; the host form keeps working | `tertiary-button` | — | `ai.result.cancel` |
| Loading indicator | `m3.LoadingIndicator` (`load_ai`, hidden, contained) | Shown while the request runs, over other content | — | — | — |
| Failure message | `m3.Text` (`text_ai_error`, hidden) | A clear message when the AI is unavailable; the host form keeps working (ticket 09, SR-43) | `body-small` | — | `ai.unavailable` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Assist list | — | The ten assists held in `server_ai/assists` (ticket 43) |
| Host context | — | The host passes its context; the component preselects the matching assist |
| Voice transcript | — | Speech-to-text through `server_ai/ai_request_layer`; never stored or logged anywhere (ticket 09) |
| Result | — | Held in the component state only; nothing of the AI channel is stored or logged in the instance or App 5 (AC-S13) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open the window | `btn_ai_trigger_click` | — | The floating window opens over the current page with the matching assist preselected |
| Switch assist | `dd_assist_change` | — | The working area loads the assist's fields |
| Generate | `btn_generate_click` | `request(assist, inputs, context)` (`server_ai/ai_request_layer`; routed through App 5) | The result appears in the result area; failures show `ai.unavailable` and the host form keeps working |
| Speak the input | `ibtn_mic_click` | `request` with the audio transcript (`server_ai/voice_service` path via App 5) | The transcription fills the assist's fields as if typed |
| Speak the result | `ibtn_speak_click` | None | The browser speaks the result (SpeechSynthesis or Groq TTS) |
| Copy | `btn_copy_click` | None | `navigator.clipboard.writeText` copies the result |
| Insert / Replace / Append | `btn_insert_click` / `btn_replace_click` / `btn_append_click` | None | The result goes into the host field; the window closes |
| Cancel | `btn_cancel_click` | None | The window closes; nothing is written |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the window opens with the assist's fields |
| Heading | The assist's title |
| Body text | `ai.assist.hint` states what the selected assist does and what to fill in |
| Primary action | Generate (`ai.generate`) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

The helper is placed on the host forms whose roles the access matrix allows; UF-30 lists Owner, Manager, Admin, Staff. Data-dependent assists 9 and 10 give results on the sample data from the first day and on real data once it is cleared; on thin real data they say what they need (UF-30).

## 7. Text

| Phrase key | English text |
|---|---|
| `ai.open` | Get AI help |
| `ai.assist.label` | Assist |
| `ai.generate` | Generate |
| `ai.speak` | Speak your request |
| `ai.speak_result` | Read the result aloud |
| `ai.result.copy` | Copy |
| `ai.result.insert` | Insert |
| `ai.result.replace` | Replace |
| `ai.result.append` | Append |
| `ai.result.cancel` | Cancel |
| `ai.unavailable` | The AI helper is unavailable right now. You can keep working — nothing was lost. |
| `ai.assist.hint` | Set per assist with the assist's own text |

## 8. Help Videos

| Process | Help video |
|---|---|
| Using the AI helper | `master_template` help entry `ai.helper` — played in the app language |
| Speaking instead of typing | `master_template` help entry `ai.voice` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-30 Using the AI helper | The whole flow: open, switch assist, fill, generate, insert |
| UF-31 WhatsUp voice | Voice input is tier 1 of the same component; the mobile view is `ai.WhatsUpForm` |
| UF-16 Content management and edit mode | The website-text function in edit mode uses this helper (F-06, F-08, F-21) |
| UF-42 Sharing a blog post | The social caption assist runs through this component's request layer |
| UF-43 Clearing the sample data | Assists 9 and 10 move from sample data to the client's own data when it is cleared |
