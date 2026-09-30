---
title: "mybizz-ai-implementation-spec-v2"
doc-id: "mb-7fc7c0e2"
---
# Mybizz AI Implementation Specification

**Version:** 2.0
**Status:** V2 Confirmed — Voice Module Added
**Previous version:** 1.0 (AI assists only, no voice layer)
**Change summary:** Added complete Voice Module specification (WhatsUp, TTS, Tier 1–3 voice capabilities). WhatsApp task creation removed and replaced by WhatsUp. Tiers 4 and 5 earmarked for V3.

---

## Purpose

This document defines a practical architecture and rollout plan for adding a reusable AI assistance layer to the Mybizz application, including a fully integrated voice module. The goal is to introduce AI and voice in a way that increases user value, minimises duplicate build effort, and preserves the existing workflow structure of the app.

The preferred strategy is to build one reusable AI helper component that can serve multiple AI features through a shared interaction model, rather than building separate standalone AI tools inside each page. The AI and voice layers are intended as post-core enhancements, meaning they should be implemented after the core application has been coded and tested.

---

## Objectives

The AI and voice feature set should achieve six outcomes:

- Deliver high perceived value to end users early, especially during onboarding and setup.
- Reuse one architectural pattern across all AI assists so that the first implementation does the heavy lifting for the rest.
- Keep users in context by offering AI assistance in a floating window instead of forcing separate navigation flows.
- Make future AI expansion simple by allowing additional assists to be added through the same shared framework.
- Reserve data-dependent AI features for later phases, once sufficient user activity exists to support them meaningfully.
- Provide a complete voice interaction loop (speak → understand → respond) that works on any browser, any device, with no additional cost to the client.

---

## Architectural Model

### Core concept

The recommended architecture is a single reusable AI helper custom component used across the app. Users interact with one trigger button placed in relevant forms, which opens a floating AI window containing a menu of available assists, a dynamic working area, and an output area for generated results.

Voice input is an additional input modality wired into the same component — the user speaks instead of typing, but the text flows through the same shared AI request pipeline. Voice does not require a separate component or separate integration point.

### Component responsibilities

The AI helper component contains the following responsibilities:

- A reusable trigger button that can be dropped into any relevant interface.
- A floating modal or panel that opens without forcing page navigation.
- A menu or selector listing the available AI assists.
- A dynamic content region that loads the selected assist's fields, instructions, and result view.
- A shared API request function used by every assist.
- A copy-to-clipboard action on every result so users can immediately reuse generated output.
- A microphone button wired to the Groq Whisper API for voice input.
- A TTS output function using browser SpeechSynthesis to speak responses back to the user.

### Context awareness

The helper detects the page or workflow where it was opened and preselects the most relevant AI assist automatically. For example, the invoicing area opens with the invoice message generator preselected, while a profile or onboarding context defaults to the bio/profile writer.

The menu still allows switching, but the initial default should feel intelligent and reduce friction.

---

## Voice Module — WhatsUp

### What it is

WhatsUp is the Mybizz mobile voice interaction feature. It replaces the previously proposed WhatsApp integration entirely. The business owner opens Mybizz on their phone browser, taps a microphone button, speaks a command or query, and Mybizz responds — in text and optionally spoken aloud.

No WhatsApp. No Meta. No additional integration. No cost to the client.

### Why WhatsUp not WhatsApp

WhatsApp integration requires a Meta Business account, a verified phone number registered with Meta, a paid API tier, and ongoing compliance with Meta's policies — a real integration, real ongoing cost, and dependency on a third party the platform does not control. WhatsUp delivers the same underlying capability (quick voice capture away from the desk) through the browser, within the existing architecture, at zero additional cost.

WhatsUp can be bookmarked as a home screen shortcut on the user's phone for one-tap access — effectively an app-like experience without a separate app.

### Technical stack

| Component | Technology | Cost |
|---|---|---|
| Speech to text | Groq Whisper API | Mybizz absorbs — negligible at scale (~$0.02/month per client at typical usage) |
| Text to speech | Browser SpeechSynthesis API | Zero — browser-native |
| Intent processing | Existing V2 shared AI request pipeline | No additional cost |
| Data retrieval | Existing Anvil server functions | No additional cost |

### The complete voice loop

```
User speaks →
Groq Whisper transcribes (server-side, private) →
AI layer interprets intent →
Mybizz retrieves data / creates task / navigates →
Response text generated →
Browser TTS speaks response back to user
```

### Browser compatibility

Groq Whisper handles transcription server-side — no browser dependency on the STT side. Browser SpeechSynthesis for TTS is supported across Chrome, Edge, Safari, and Firefox on mobile. The voice module works on any modern browser without requiring Chrome specifically.

---

## Voice Tiers

Voice capability is structured in three tiers for V2. Tiers 4 and 5 are earmarked for V3.

### Tier 1 — Voice input to existing AI assists

The simplest and most immediate value. Instead of typing into the AI helper fields, the user speaks. The spoken text is transcribed by Groq Whisper and passed into the existing AI helper as if typed. Zero additional AI logic required beyond wiring the mic.

**Examples:**
- "Write a bio for my yoga studio in Cape Town, focused on mindfulness and corporate wellness"
- "Generate a service description for a 60-minute deep tissue massage, R450, includes aromatherapy"
- "Draft a blog post about the top five benefits of regular reflexology"
- "Write an invoice message for Naledi's outstanding balance — professional but warm"
- "Create an FAQ for my legal consulting service"

All ten existing AI assists become voice-addressable.

### Tier 2 — Voice task creation (WhatsUp core)

The user speaks a task from anywhere — at their desk or on their phone. Mybizz creates the task directly. This is the WhatsUp core use case.

**Examples:**
- "Create a task — call Priya to confirm her appointment on Thursday"
- "Remind me to follow up with David about his proposal"
- "Add a task to update the services page before end of week"
- "Task — send Naledi her invoice before Friday"
- "Follow up with the new lead from the website"

### Tier 3 — Voice queries with spoken response

The user asks Mybizz a question and receives a spoken answer. These are read-only, single-data-point queries against existing V1 data. The response is spoken back via browser TTS.

**Schedule and bookings:**
- "What is my schedule for tomorrow?"
- "What bookings do I have this week?"
- "What time is my next appointment?"
- "Who is my first client today?"
- "Do I have any bookings on Friday?"

**Payments and revenue:**
- "What payments came in today?"
- "What is my revenue for this week?"
- "Which invoices are outstanding?"
- "Has Naledi paid her invoice?"

**Tasks:**
- "What tasks do I have due today?"
- "What is on my to-do list?"
- "Do I have any overdue tasks?"

**Clients:**
- "When did Sarah last book?"
- "How many clients do I have?"
- "What is Thabo's contact number?"

**Navigation shortcuts:**
- "Open my bookings"
- "Show me unpaid invoices"
- "Take me to my email campaigns"
- "Open the blog editor"

---

## AI Feature Inventory

The ten proposed AI services and their intended implementation order:

| Priority | Service | Rationale |
|---|---|---|
| 1 | Bio/profile writer | Highest perceived value, easiest to build, strong onboarding impact |
| 2 | Service description generator | Useful during setup, saves client time, straightforward prompt pattern |
| 3 | FAQ generator | Naturally derived from service descriptions with little extra client effort |
| 4 | Proposal generator | Strong direct revenue value, obvious premium-feeling feature |
| 5 | Blog post drafts | Strong fit with content publishing and social/blog workflows |
| 6 | Email response suggestions | Everyday utility feature that keeps users inside the application |
| 7 | Invoice message generator | Small but polished feature that improves professionalism |
| 8 | Terms and conditions plain language explainer | Good strategic fit with clause-builder functionality |
| 9 | Client sentiment summary | Deferred until customers have enough stored communication data |
| 10 | Booking and availability insights | Best implemented last — depends on booking history and usage patterns |

All ten assists are voice-addressable via Tier 1 from the moment voice is implemented.

---

## Rollout Phases

### Phase 1 — Foundation

Main deliverable: the AI helper component shell, including trigger button, floating window, assist selector, shared request pipeline, result handling, clipboard support, and Groq Whisper microphone input wired to the text fields.

This phase establishes the canonical data flow for every assist: capture inputs (typed or spoken), compose prompt, call the chosen model, return structured text, show result, allow copy, allow user editing and saving into the relevant workflow.

Voice Tier 1 is deliverable in this phase since it requires only wiring the mic to the existing input fields.

### Phase 2 — First-value assists and WhatsUp task creation

First live assist: bio/profile writer. Followed by service description generation and FAQ generation.

Voice Tier 2 (task creation) is delivered in this phase alongside the task-related AI assists. The mobile-optimised WhatsUp shortcut is published as a bookmarkable browser link.

### Phase 3 — Revenue and workflow assists plus voice queries

Proposal generation, blog post drafts, email response suggestions, invoice message generation, and the plain-language terms explainer.

Voice Tier 3 (spoken queries with TTS responses) is delivered in this phase, adding the data retrieval and TTS response layer on top of the established voice foundation.

### Phase 4 — Data-dependent intelligence

Client sentiment summaries and booking/availability insights. Intentionally deferred until enough user data exists to make the outputs credible and useful.

---

## Data Flow and Integration Pattern

Each AI assist follows the same underlying execution pattern:

1. Collect user input from either the current form context or the floating helper input fields (typed or spoken via Groq Whisper).
2. Compose a prompt using page context plus user-supplied data.
3. Send the request through one shared AI request function.
4. Display the generated response inside the floating helper result area.
5. Speak the response via browser SpeechSynthesis where appropriate (Tier 3 queries).
6. Allow the user to copy, edit, and save the result into the appropriate application field or workflow.

---

## Screen and UX Considerations

The AI helper should feel like a contextual assistant rather than a separate subsystem. The floating-window approach keeps the current page visible and allows the user to remain grounded in the task they were already performing.

The component supports quick open, quick close, assist switching, result copying, voice input, and spoken output, and clean handoff back into the host form.

The result interaction always includes:
- Readable generated output.
- One-click copy to clipboard.
- The ability to edit before saving or using the result.
- Clear action choices such as replace, insert, append, or cancel where relevant.
- Optional TTS playback button for spoken output.

### WhatsUp mobile UX

On mobile, the voice interaction is the primary mode. The user opens Mybizz via a home screen bookmark, taps the microphone, speaks, and hears the response. The floating AI helper adapts to mobile viewport. No app install required.

---

## Technical Approach

### Shared service layer

One shared AI request layer handles prompt submission, model invocation, result return, and error handling consistently across the app. The Groq Whisper transcription call is handled server-side within this same layer — audio bytes are sent to the Anvil server module, transcribed via Groq, and the resulting text is returned to the client before being passed into the AI helper prompt pipeline.

### Prompt modularity

Each assist is defined by its assist identity, required input fields, context mapping, prompt template, and result-handling behaviour. New assists are added by defining a new menu item and associated prompt and configuration — not by rebuilding the component shell.

### Cost model

| Service | Provider | Paid by | Estimated cost at 100 clients |
|---|---|---|---|
| AI text generation | LLM provider (configurable) | Mybizz | Depends on usage |
| Speech to text | Groq Whisper API | Mybizz | ~$2/month total |
| Text to speech | Browser SpeechSynthesis | Zero | Zero |

No additional cost passes to the client for the voice module.

---

## Delivery Estimate

The reusable helper architecture plus all ten assists was estimated at roughly three to four days including prompt tuning. Adding the voice module (Groq Whisper wiring + TTS + Tier 1–3 capability) adds approximately one additional day given the shared pipeline architecture.

---

## Risks and Design Guardrails

- AI and voice should remain enhancement layers, not dependencies for core app operation.
- Early rollout prioritises useful text generation tasks over analytics-heavy features.
- The UI preserves user context rather than redirecting into separate AI pages.
- The architecture stays centralized so future additions do not multiply maintenance overhead.
- Prompt quality and output usability matter more than feature count in the first release.
- Voice queries (Tier 3) must fail gracefully — if the data cannot be retrieved, Mybizz responds with a clear spoken message rather than silence or an error code.

---

## Recommended Development Sequence

1. Reserve clean architectural space in relevant forms for AI helper usage.
2. Build the reusable AI helper component shell and shared request pipeline, including Groq Whisper mic input.
3. Implement the bio/profile writer as the first production assist (voice-addressable immediately).
4. Add service description and FAQ generation.
5. Deliver WhatsUp task creation (Tier 2) and mobile shortcut.
6. Expand into proposals, blog drafts, email suggestions, invoice messaging, and terms explanation.
7. Add Tier 3 voice queries with TTS response layer.
8. Add sentiment summary and booking insights only after sufficient production data exists.

---

## Out of Scope for V2 — Earmarked for V3

The following voice capabilities are confirmed future work but explicitly excluded from V2:

**Tier 4 — Voice-driven form pre-fill**
The user speaks a complete record ("New booking — Sarah Johnson, Swedish massage, Friday 2pm") and Mybizz parses the spoken input into structured data, pre-filling the relevant form for confirmation. Requires per-form entity extraction prompt design.

**Tier 5 — Data queries requiring aggregation and intelligence**
Complex cross-referencing, trend analysis, predictive insights, and multi-step reasoning across data. Deferred until the AI layer is mature and meaningful data volume exists. Full specification in mybizz-ai-implementation-spec-v3.md.
