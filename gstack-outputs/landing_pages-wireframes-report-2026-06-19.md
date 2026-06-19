# Landing Pages Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** landing_pages/ [Phase 3]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md, build-plan.md Stage 3.5

---

## 1. Wireframes Created

### 1.1 LandingPageListForm

- **File:** `wireframes/landing_pages/LandingPageListForm/wireframe-landing_pages-LandingPageListForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `LandingPageListForm` in `landing_pages/` package [Phase 3]
  - `Clearly defined:` build-plan.md Stage 3.5: "Landing Pages" with list management
  - `Clearly defined:` DESIGN.md Empty States: "Landing page list — Page icon, No landing pages yet"
- **Form type:** List form — page cards with status, analytics, and actions
- **Components:** Page title, create button, landing page cards (title, slug, status badge, views, conversions, rate, edit/view/delete buttons), empty state (4 required components)
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "description"), `hdg_empty_state` (Heading "No landing pages yet"), `txt_empty_state` (Text), `btn_empty_action` (Button "+ Create Landing Page")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.2 LandingPageEditorForm

- **File:** `wireframes/landing_pages/LandingPageEditorForm/wireframe-landing_pages-LandingPageEditorForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `LandingPageEditorForm` in `landing_pages/` package [Phase 3]
  - `Clearly defined:` build-plan.md Stage 3.5: "Three landing page templates working"
  - `Clearly defined:` DESIGN.md Custom Components: LeadCaptureTemplate, EventRegistrationTemplate, VslTemplate
- **Form type:** Editor form — create/edit landing pages with template selection
- **Components:** Page title, preview/publish buttons, settings card (title, slug, status), template selection cards (Lead Capture, Event Registration, VSL), content card (headline, subheadline, CTA, image URL), action buttons (Publish, Save Draft, Cancel)
- **Empty state:** N/A — editor form, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.3 LandingPage

- **File:** `wireframes/landing_pages/LandingPage/wireframe-landing_pages-LandingPage.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `LandingPage` in `landing_pages/` package [Phase 3]
  - `Clearly defined:` DESIGN.md: "LandingPage contains lead capture, event registration, and VSL layout panels"
  - `Clearly defined:` scaffold-spec.md v2: "LeadCaptureTemplate, EventRegistrationTemplate, VslTemplate → build into LandingPage"
- **Form type:** Public-facing landing page with 3 template variants
- **Components:** Hero section (ParallaxComponent), template-specific content panels (toggled by template_type), trust strip, footer (FooterComponent)
- **Empty state:** N/A — public-facing page, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.4 LeadCaptureTemplate

- **File:** `wireframes/landing_pages/LandingPage/LeadCaptureTemplate/LeadCaptureTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md: "LeadCaptureTemplate → build into LandingPage, selected by template_type"
  - `Clearly defined:` DESIGN.md: "Lead Capture: Name, email, phone, message form. Best for consultation offers."
- **Form type:** Template variant — lead capture form
- **Components:** Card (outlined), form heading, name TextBox (W), email TextBox (W), phone TextBox (W), message TextArea (W), submit Button, success message (hidden)
- **Empty state:** N/A — input form, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.5 EventRegistrationTemplate

- **File:** `wireframes/landing_pages/LandingPage/EventRegistrationTemplate/EventRegistrationTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md: "EventRegistrationTemplate → build into LandingPage, selected by template_type"
  - `Clearly defined:` DESIGN.md: "Event Registration: Name, email, event selection. Best for workshops and webinars."
- **Form type:** Template variant — event registration form
- **Components:** Card (outlined), form heading, name TextBox (W), email TextBox (W), event DropdownMenu (W), register Button, success message (hidden)
- **Empty state:** N/A — input form, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.6 VslTemplate

- **File:** `wireframes/landing_pages/LandingPage/VslTemplate/VslTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md: "VslTemplate → build into LandingPage, selected by template_type"
  - `Clearly defined:` DESIGN.md: "VSL (Video Sales Letter): Video embed + CTA button. Best for high-ticket offers."
- **Form type:** Template variant — video sales letter
- **Components:** Video card (VideoDisplayComponent), CTA card (heading, body text, submit button), success message (hidden)
- **Empty state:** N/A — input form, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** All 6 items in the task list received their own complete wireframe.

---

## 3. Specification Gaps

No specification gaps found. All 6 forms have clear architectural backing in scaffold-spec.md, DESIGN.md, and build-plan.md.

---

## 4. Summary

| Item | Status |
|------|--------|
| Wireframes created | 6 (LandingPageListForm, LandingPageEditorForm, LandingPage, LeadCaptureTemplate, EventRegistrationTemplate, VslTemplate) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 6 wireframes pass all 15 checklist items |
| Specification gaps | None |

---

*End of report — 2026-06-19*
