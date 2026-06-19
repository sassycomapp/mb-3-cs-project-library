# Components Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** components/ [Phase 4]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md

---

## 1. Wireframes Created

### 1.1 ClauseBuilder

- **File:** `wireframes/components/ClauseBuilder/wireframe-components-ClauseBuilder.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md ClauseBuilder section: "Shared custom component used by Privacy Policy and Terms & Conditions editing flows"
  - `Clearly defined:` scaffold-spec.md lists `ClauseBuilder/` in `components/` package [Phase 4]
  - `Clearly defined:` ADR-018 specifies clause builder architecture
- **Form type:** Shared custom component — clause editor parameterised by document_type
- **Components:** Category dropdown, add custom clause button, seeded clauses RepeatingPanel, custom clauses RepeatingPanel, add clause form (hidden), edit clause form (hidden)
- **Empty state:** Not applicable — component displays data from host form, not standalone data-driven content
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.2 FooterComponent

- **File:** `wireframes/components/FooterComponent/wireframe-components-FooterComponent.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Custom Components Inventory: "All public pages (6+)"
  - `Clearly defined:` scaffold-spec.md lists `FooterComponent/` in `components/` package [Phase 4]
- **Form type:** Shared custom component — bottom page banner
- **Components:** Logo image, business name, accreditations, social icon buttons (4), legal links (3), contact info (email, phone, map link, contact page link), copyright text, back to top link
- **Empty state:** Not applicable — always displays content from business_profile
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.3 ParallaxComponent

- **File:** `wireframes/components/ParallaxComponent/wireframe-components-ParallaxComponent.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Custom Components Inventory: "Multiple public pages"
  - `Clearly defined:` scaffold-spec.md lists `ParallaxComponent/` in `components/` package [Phase 4]
- **Form type:** Shared custom component — parallax scroll effect
- **Components:** Background image, foreground ColumnPanel with heading, body text, CTA button
- **Empty state:** Not applicable — display-only component
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.4 TimeLapseCarouselComponent

- **File:** `wireframes/components/TimeLapseCarouselComponent/wireframe-components-TimeLapseCarouselComponent.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Custom Components Inventory: "HomePage, ServicesForm"
  - `Clearly defined:` scaffold-spec.md lists `TimeLapseCarouselComponent/` in `components/` package [Phase 4]
- **Form type:** Shared custom component — auto-rotating image carousel
- **Components:** Image track (FlowPanel with 3 images), navigation buttons (prev/next), dot indicators (3 buttons)
- **Empty state:** Not applicable — display-only component
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.5 VideoDisplayComponent

- **File:** `wireframes/components/VideoDisplayComponent/wireframe-components-VideoDisplayComponent.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md VideoDisplayComponent section: "Display-only video embed component"
  - `Clearly defined:` scaffold-spec.md lists `VideoDisplayComponent/` in `components/` package [Phase 4]
- **Form type:** Shared custom component — display-only video embed
- **Components:** ColumnPanel (visible when video_url valid), HtmlPanel for YouTube iframe, optional caption text. Hidden state when no URL.
- **Empty state:** Not applicable — programmatically hidden when no video URL
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** All 5 items in the task list received their own complete wireframe.

---

## 3. ADR-27 Section 7 Compliance Checklist (per wireframe)

All 5 wireframes pass every item:

| # | Checklist Item | ClauseBuilder | FooterComponent | ParallaxComponent | TimeLapseCarousel | VideoDisplayComponent |
|---|---|---|---|---|---|---|
| 1 | Section 0 reading completed | PASS | PASS | PASS | PASS | PASS |
| 2 | Exactly three sections, correct order | PASS | PASS | PASS | PASS | PASS |
| 3 | NOT formatted as "Screen" type | PASS | PASS | PASS | PASS | PASS |
| 4 | No legacy Label (lbl_) | PASS | PASS | PASS | PASS | PASS |
| 5 | All components approved M3 | PASS | PASS | PASS | PASS | PASS |
| 6 | Every visible text has explicit component | PASS | PASS | PASS | PASS | PASS |
| 7 | No sample data | PASS | PASS | PASS | PASS | PASS |
| 8 | No behaviour/explanatory text in diagram | PASS | PASS | PASS | PASS | PASS |
| 9 | Containers drawn as containers with names inset | PASS | PASS | PASS | PASS | PASS |
| 10 | No border on non-bordered components | PASS | PASS | PASS | PASS | PASS |
| 11 | Properties table: Designer-only only | PASS | PASS | PASS | PASS | PASS |
| 12 | writeback (W) specified where needed | PASS | PASS | N/A | N/A | N/A |
| 13 | All names follow nomenclature.md | PASS | PASS | PASS | PASS | PASS |
| 14 | Containers used for intended purpose | PASS | PASS | PASS | PASS | PASS |
| 15 | Architecturally matches upstream | PASS | PASS | PASS | PASS | PASS |

---

## 4. Specification Gaps

No specification gaps found. All 5 components have clear architectural backing in DESIGN.md, scaffold-spec.md, and ADR-018.

---

## 5. Summary

| Item | Status |
|------|--------|
| Wireframes created | 5 (ClauseBuilder, FooterComponent, ParallaxComponent, TimeLapseCarouselComponent, VideoDisplayComponent) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 5 wireframes pass all 15 checklist items |
| Specification gaps | None |

---

*End of report — 2026-06-19*
