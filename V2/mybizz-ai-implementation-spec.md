---
document: "Mybizz AI Implementation Specification"
doc-id: mybizz-ai-implementation-spec
state: Live
date-created: 2026-06-15
---

# Mybizz AI Implementation Specification

## Purpose

This document defines a practical architecture and rollout plan for adding a reusable AI assistance layer to the Mybizz application. The goal is to introduce AI in a way that increases user value, minimizes duplicate build effort, and preserves the existing workflow structure of the app.[cite:244][cite:247]

The preferred strategy is to build one reusable AI helper component that can serve multiple AI features through a shared interaction model, rather than building separate standalone AI tools inside each page.[cite:244] The AI layer is intended as a post-core enhancement, meaning it should be implemented after the core application has been coded and tested.[cite:247]

## Objectives

The AI feature set should achieve five outcomes:

- Deliver high perceived value to end users early, especially during onboarding and setup.[cite:245]
- Reuse one architectural pattern across all AI assists so that the first implementation does the heavy lifting for the rest.[cite:244][cite:249]
- Keep users in context by offering AI assistance in a floating window instead of forcing separate navigation flows.[cite:244]
- Make future AI expansion simple by allowing additional assists to be added through the same shared framework.[cite:244]
- Reserve data-dependent AI features for later phases, once sufficient user activity exists to support them meaningfully.[cite:245]

## Architectural model

### Core concept

The recommended architecture is a single reusable AI helper custom component used across the app.[cite:244] Users interact with one trigger button placed in relevant forms, which opens a floating AI window containing a menu of available assists, a dynamic working area, and an output area for generated results.[cite:244]

This architecture is preferable to embedding separate AI implementations into individual pages because it centralizes the integration point, reduces maintenance effort, and allows the same component to serve all ten features.[cite:244] It also keeps the user on the current page, which preserves workflow continuity and reduces interface disruption.[cite:244]

### Component responsibilities

The AI helper component should contain the following responsibilities:

- A reusable trigger button that can be dropped into any relevant interface.[cite:244]
- A floating modal or panel that opens without forcing page navigation.[cite:244]
- A menu or selector listing the available AI assists.[cite:244]
- A dynamic content region that loads the selected assist’s fields, instructions, and result view.[cite:244]
- A shared API request function used by every assist.[cite:244]
- A copy-to-clipboard action on every result so users can immediately reuse generated output.[cite:244]

### Context awareness

A key refinement is contextual awareness: the helper should detect the page or workflow where it was opened and preselect the most relevant AI assist automatically.[cite:244] For example, the invoicing area should open with the invoice message generator preselected, while a profile or onboarding context should default to the bio/profile writer.[cite:244]

This does not mean the user is locked into one assist. The menu should still allow switching, but the initial default should feel intelligent and reduce friction.[cite:244]

## Feature inventory

The ten proposed AI services and their intended implementation order are listed below.

| Priority | Service | Rationale |
|---|---|---|
| 1 | Bio/profile writer | Highest perceived value, easiest to build, and strong onboarding impact.[cite:245] |
| 2 | Service description generator | Useful during setup, saves client time, and requires a straightforward prompt pattern.[cite:245] |
| 3 | FAQ generator | Naturally derived from service descriptions with little extra client effort.[cite:245] |
| 4 | Proposal generator | Strong direct revenue value for clients and an obvious premium-feeling feature.[cite:245] |
| 5 | Blog post drafts | Strong fit with content publishing and social/blog workflows already present in the app.[cite:245] |
| 6 | Email response suggestions | Everyday utility feature that keeps users inside the application.[cite:245] |
| 7 | Invoice message generator | Small but polished feature that improves professionalism.[cite:245] |
| 8 | Terms and conditions plain language explainer | Good strategic fit with clause-builder-related functionality.[cite:245] |
| 9 | Client sentiment summary | Better deferred until customers have enough stored communication or client data.[cite:245] |
| 10 | Booking and availability insights | Best implemented last because it depends on booking history and usage patterns.[cite:245] |

## Rollout phases

### Phase 1: Foundation

Phase 1 should focus on the reusable framework rather than breadth of feature count. The main deliverable is the AI helper component itself, including trigger button, floating window, assist selector, shared request pipeline, result handling, and clipboard support.[cite:244]

This phase should also define the canonical data flow for every assist: capture inputs, compose prompt, call the chosen model, return structured text, show result, allow copy, and allow user editing/saving into the relevant workflow.[cite:249] Once this loop works well, the rest of the rollout becomes mostly repeatable prompt and field design work.[cite:249]

### Phase 2: First-value assists

The first live assist should be the bio/profile writer because it combines high user appeal with low implementation complexity.[cite:245][cite:249] After that, service description generation and FAQ generation should follow quickly because they fit onboarding and setup workflows and can reuse very similar prompt handling.[cite:245]

The goal of this phase is to validate the interaction model, prompt quality, and save-back workflow while delivering clear end-user value early.[cite:245][cite:249]

### Phase 3: Revenue and workflow assists

Once the first-value assists are stable, the next wave should cover proposal generation, blog post drafts, email response suggestions, invoice message generation, and the plain-language terms explainer.[cite:245] These features extend AI from onboarding into recurring daily workflow usage.[cite:245]

This phase is important because it turns AI from a novelty into an ongoing retention feature inside the product.[cite:245][cite:244]

### Phase 4: Data-dependent intelligence

Client sentiment summaries and booking/availability insights should be intentionally deferred until enough user data exists to make the outputs credible and useful.[cite:245] Implementing them too early would produce weaker results and increase build complexity before the app has enough real-world usage to justify them.[cite:245]

These features should therefore be treated as later-stage intelligence features rather than part of the first production AI wave.[cite:245]

## Data flow and integration pattern

Each AI assist should follow the same underlying execution pattern:

1. Collect user input from either the current form context or the floating helper input fields.[cite:249]
2. Compose a prompt using page context plus user-supplied data.[cite:249][cite:244]
3. Send the request through one shared AI request function.[cite:244]
4. Display the generated response inside the floating helper result area.[cite:249]
5. Allow the user to copy, edit, and save the result into the appropriate application field or workflow.[cite:244][cite:249]

The build advantage of this pattern is that the code structure repeats while prompts and field mappings change by assist type.[cite:249] That is why the first assist is the hardest and the rest become progressively easier to add.[cite:249]

## Screen and UX considerations

The AI helper should feel like a contextual assistant rather than a separate subsystem. It should be reachable anywhere it is relevant, but it should not dominate core workflows or interrupt form completion unnecessarily.[cite:244]

The floating-window approach is central to that experience because it keeps the current page visible and allows the user to remain grounded in the task they were already performing.[cite:244] The component should support quick open, quick close, assist switching, result copying, and clean handoff back into the host form.[cite:244]

The result interaction should always include:

- Readable generated output.
- One-click copy to clipboard.[cite:244]
- The ability to edit before saving or using the result.[cite:249]
- Clear action choices such as replace, insert, append, or cancel where relevant.

## Suggested host contexts

The likely host areas for the first rollout are the areas already associated with the proposed service list. These include onboarding/profile setup, services configuration, FAQs, proposals, blog/social publishing, email-related workflows, invoicing, and clause-related legal explanation areas.[cite:245][cite:248]

Each host form should use the same helper button and the same floating component, with contextual defaults varying by form.[cite:244] This supports the architectural principle of one component, one integration point, many assist types.[cite:244]

## Technical approach

### Shared service layer

The implementation should expose one shared AI request layer used by all assists.[cite:244] That layer should handle prompt submission, model invocation, result return, and error handling consistently across the app.

This architecture keeps model choice and request logic centralized, making it easier to improve prompts, swap providers, or add common safeguards later without touching every host form.[cite:244]

### Prompt modularity

Each assist should be defined primarily by:

- its assist identity,
- its required input fields,
- its context mapping,
- its prompt template,
- and its result-handling behaviour.

This modularity allows new assists to be added by defining a new menu item and associated prompt/configuration rather than rebuilding the component shell every time.[cite:244]

### Save-back logic

Generated content should not remain isolated in the helper. It should be easy to transfer into the relevant workflow field, either through direct insertion or user-controlled copy/edit/save actions.[cite:249]

The result should be that AI output feels embedded in normal business workflows rather than like a detached experiment.[cite:244][cite:249]

## Delivery estimate

A practical estimate discussed for the reusable helper architecture plus all ten assists was roughly three to four days, including prompt tuning, because the architecture avoids rebuilding the same pattern repeatedly.[cite:244] The credibility of that estimate depends on keeping the design centralized and resisting the temptation to build bespoke page-specific AI tools.[cite:244]

The first assist will consume the most effort because it establishes the API connection, result-handling logic, and UX conventions for the rest.[cite:249] After that, expansion should be much more mechanical.[cite:249]

## Risks and design guardrails

Several guardrails should shape later implementation:

- AI should remain an enhancement layer, not a dependency for core app operation.[cite:247]
- Early rollout should prioritize useful text generation tasks over analytics-heavy features that require mature data.[cite:245]
- The UI should preserve user context rather than redirecting people into separate AI pages.[cite:244]
- The architecture should stay centralized so future additions do not multiply maintenance overhead.[cite:244]
- Prompt quality and output usability matter more than feature count in the first release.[cite:244][cite:249]

## Recommended next development sequence

1. Reserve clean architectural space in relevant forms for future AI helper usage if not already present.[cite:247]
2. Build the reusable AI helper component shell and shared request pipeline.[cite:244]
3. Implement the bio/profile writer as the first production assist.[cite:245][cite:249]
4. Add service description and FAQ generation as the next setup-oriented assists.[cite:245]
5. Expand into proposals, blog drafts, email suggestions, invoice messaging, and terms explanation.[cite:245]
6. Add sentiment summary and booking insights only after sufficient production data exists.[cite:245]

## Outcome target

If implemented this way, Mybizz gains a coherent AI layer that feels integrated, premium, and expandable rather than bolted on.[cite:244] The product value comes not only from the ten individual assists, but from the fact that they are delivered through one elegant reusable assistant model that is easy to place throughout the app.[cite:244]
