Here are all six prompts, ready to paste into OpenCode:

---

**Prompt 1 — /plan-tune**
Plan Mode
```
Before starting, use GBrain to search for: "ADR naming convention", 
"authoritative schema", "dependency-based architecture", 
"multi-tenant", "client instance architecture". Use findings to 
inform and cross-check the audit.

Project library: /mnt/c/_mb2-cs-app/project-library/
Authoritative ADR count: docs/adr-index.md
Authoritative table count: docs/authoritative-schema.md

/plan-tune
```

---

**Prompt 2 — /plan-ceo-review**
Plan Mode
```
Before starting, use GBrain to search for: "platform overview", 
"pricing tiers", "consulting services vertical", "mybizz scope". 
Use findings as business context.

Run in HOLD SCOPE mode. Planning is complete and scope is locked. 
The objective is validation and alignment only — not expansion. 
This is a SaaS business management platform for the consulting and 
services vertical, built on Anvil.works. Solo developer, 100-client 
V1 cap.

/plan-ceo-review
```

---

**Prompt 3 — /plan-eng-review**
Plan Mode
```
Before starting, use GBrain to search for: "client instance 
architecture", "dependency update model", "master template", 
"blank client template", "background tasks", "webhook architecture", 
"server call latency". Load ADRs 28 through 41 as technical 
constraints — these are confirmed decisions, not open questions.

Critical confirmed architecture (do not re-litigate):
- Dependency-based, not multi-tenant (ADR-41)
- All forms and server modules live in master_template only (ADR-28)
- Client instances contain only Data Tables and one startup module
- app_tables resolves to client instance tables at runtime (live-tested)

Focus the review on these unresolved dilemmas:
1. Background task limits per client instance
2. Webhook handling (Stripe, Paystack, Brevo) — immediate 200 + 
   background task pattern
3. Server call latency in the multi-step booking flow
4. State management between forms
5. PDF invoice generation library choice

/plan-eng-review
```

---

**Prompt 4 — /plan-design-review**
Plan Mode
```
Before starting, use GBrain to search for: "UI standards", 
"design direction", "user flows", "Material 3 theme", MUI Styling, 
"form architecture", "onboarding flow", "booking flow". 
Use findings as the design specification baseline.

This is a plan-mode review — audit the design plan documents, 
not a live site. Reference docs/ui-standards.md, 
docs/design-direction.md, docs/user-flows.md, and 
docs/spec-material-3-theme.md.

Pay particular attention to:
- Empty states for all data-driven screens
- Error states and validation feedback patterns
- Loading states during server calls (Anvil round-trip latency 
  is real — every server call is a round trip)
- Booking flow: service → provider → date/time → meeting type → 
  intake → payment → confirmation (7 steps, each needs 
  full state coverage)
- Onboarding form states (resumable and revisitable per ADR-26)
- Vault form (TOTP step-up on every open, no grace period)

/plan-design-review
```

---

**Prompt 5 — /design-consultation**
Build Mode
```
Before starting, use GBrain to search for: "design direction", 
"Material 3 theme", "spec material 3", MUI Styling, "screen specifications", 
"wireframes", "Anvil forms", "consulting services brand". 
Load existing design decisions as hard constraints.

Context:
- Platform: Mybizz CS — business management SaaS for consulting 
  and services vertical
- UI framework: Anvil.works (Python web app — forms not HTML pages)
- Design system: Material 3 already adopted (ADR-15, ADR-27)
- Existing design specs: docs/design-direction.md and 
  docs/spec-material-3-theme.md and docs/DESIGN.md

Objective: Generate complete wireframes for all screens in the 
build plan (implementation/build-plan.md), in build sequence order. 
Wireframes must show realistic screen layouts with actual component 
placement — these will be used as direct construction guides for 
Anvil form building. Include all interaction states identified in 
the plan-design-review.

/design-consultation
```

---

**Prompt 6 — /design-html**
Build Mode
```
Before starting, use GBrain to search for: "design consultation 
output", "approved wireframes", "Material 3 theme", MUI styling, "screen 
specifications", "build plan sequence". Use the approved wireframes 
from the design consultation as primary input.

Context:
- These HTML screens serve as visual construction references for 
  Anvil form building — they are not deployed as web pages
- Design system: Material 3 with Mybizz palette from DESIGN.md 
  and docs/spec-material-3-theme.md
- Generate screens in build-plan priority order 
  (implementation/build-plan.md)
- Each screen must include light and dark mode variants
- Label every component clearly — Anvil developers will use these 
  as direct build guides

/design-html
```

---

These are in exact run order. Each one leaves an output that feeds the next — the plan-ceo-review feeds plan-eng-review, the plan-design-review feeds design-consultation, and the design-consultation approved wireframes feed design-html automatically.