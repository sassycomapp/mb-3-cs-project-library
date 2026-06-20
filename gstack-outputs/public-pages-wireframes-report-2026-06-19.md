# public_pages Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** public_pages/ [Phase 4]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md

---

## 1. Wireframes Created

### 1.1 AboutPage

- **File:** `wireframes/public_pages/AboutPage/wireframe-public_pages-AboutPage.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 253–255 (`AboutPage/` in `public_pages/` package, Phase 4)
  - `Clearly defined:` DESIGN.md Content Panel Pattern: AboutPageTemplate
  - `Clearly defined:` scaffold-spec.md: FooterComponent shared across all public pages
- **Form type:** Public-facing read-only page — business information, values, accreditations
- **Components:** Hero section (Heading, Text, 2 Buttons), about text (Text), values section (3x Heading + 3x Text), accreditations (Heading, Text), FooterComponent
- **Empty state:** N/A — public-facing read-only page, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.2 ContactPage

- **File:** `wireframes/public_pages/ContactPage/wireframe-public_pages-ContactPage.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 256–258 (`ContactPage/` in `public_pages/` package, Phase 4)
  - `Clearly defined:` DESIGN.md Content Panel Pattern: ContactPageTemplate
  - `Clearly defined:` scaffold-spec.md routing: `/contact` — ContactPage
  - `Clearly defined:` scaffold-spec.md schema: `contact_submissions` table
- **Form type:** Public-facing contact form — submits to contact_submissions table
- **Components:** Page title (Heading), form card with 4 inputs (TextBox x3, TextArea x1), submit Button, FooterComponent
- **Empty state:** N/A — contact form, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.3 HomePage

- **File:** `wireframes/public_pages/HomePage/wireframe-public_pages-HomePage.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 250–252 (`HomePage/` in `public_pages/` package, Phase 4)
  - `Clearly defined:` DESIGN.md Homepage Template Variants section
  - `Clearly defined:` DESIGN.md Shared Section Inventory (8 sections)
  - `Clearly defined:` scaffold-spec.md: HomePage contains 4 layout variants
- **Form type:** Public-facing homepage — one form with four template variants
- **Components:** Hero (ParallaxComponent, Heading, Text, Button), Intro (Heading, Text), Services (TimeLapseCarouselComponent, 3x Card), Video (VideoDisplayComponent, Heading), Testimonials (Heading, 3x Card), Trust strip (ColumnPanel with 4 data points), CTA band (Heading, Text, Button), FooterComponent
- **Empty state:** N/A — public-facing page, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.4 HomePageBookingTemplate

- **File:** `wireframes/public_pages/HomePage/HomePageBookingTemplate/wireframe-public_pages-HomePage-BookingTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 430: HomePageBookingTemplate → build into HomePage
  - `Clearly defined:` DESIGN.md Template Differentiation Matrix: Booking variant
  - `Clearly defined:` DESIGN.md: Section order Hero → CTA band → Trust → Services → Testimonials → Video → Intro → Footer
- **Form type:** Conversion-focused homepage template variant
- **Components:** Hero (ParallaxComponent, Heading, Text, Button), CTA band (Heading, Text, 2 Buttons), Trust strip (4 data points), Services (TimeLapseCarouselComponent, 3x Card), Testimonials (Heading, 2x Card), Video (VideoDisplayComponent, Heading), Intro (Heading, Text), FooterComponent
- **Empty state:** N/A — template variant, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.5 HomePageClassicTemplate

- **File:** `wireframes/public_pages/HomePage/HomePageClassicTemplate/wireframe-public_pages-HomePage-ClassicTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 428: HomePageClassicTemplate → build into HomePage
  - `Clearly defined:` DESIGN.md Template Differentiation Matrix: Classic variant (default, balanced)
  - `Clearly defined:` DESIGN.md: Section order Hero → Intro → Services → Video → Testimonials → Trust strip → CTA band → Footer
- **Form type:** Classic homepage template variant — balanced, general-purpose
- **Components:** Same as HomePage (Classic is the default variant shown in the parent wireframe)
- **Empty state:** N/A — template variant, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.6 HomePageMinimalistTemplate

- **File:** `wireframes/public_pages/HomePage/HomePageMinimalistTemplate/wireframe-public_pages-HomePage-MinimalistTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 431: HomePageMinimalistTemplate → build into HomePage
  - `Clearly defined:` DESIGN.md Template Differentiation Matrix: Minimalist variant
  - `Clearly defined:` DESIGN.md: Section order Hero → Intro → Trust strip → CTA band → Footer. Hides carousel, full services, full testimonials, video.
- **Form type:** Minimalist homepage template variant — clean, minimal, generous whitespace
- **Components:** Hero (ParallaxComponent, Heading, Text, Button), Intro (Heading, Text), Trust strip (3 data points — compact), CTA band (Heading, Text, Button), hidden sections marker, FooterComponent
- **Empty state:** N/A — template variant, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.7 HomePageServicesTemplate

- **File:** `wireframes/public_pages/HomePage/HomePageServicesTemplate/wireframe-public_pages-HomePage-ServicesTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 429: HomePageServicesTemplate → build into HomePage
  - `Clearly defined:` DESIGN.md Template Differentiation Matrix: Services variant
  - `Clearly defined:` DESIGN.md: Section order Hero → Services → Trust strip → Testimonials → Intro → CTA band → Video → Footer
- **Form type:** Services-first homepage template variant
- **Components:** Hero (ParallaxComponent, Heading, Text, 2 Buttons), Services (TimeLapseCarouselComponent, 3x Card), Trust strip (4 data points), Testimonials (Heading, 2x Card), Intro (Heading, Text), CTA band (Heading, Text, Button), Video (VideoDisplayComponent, Heading), FooterComponent
- **Empty state:** N/A — template variant, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.8 PrivacyPolicyPage

- **File:** `wireframes/public_pages/PrivacyPolicyPage/wireframe-public_pages-PrivacyPolicyPage.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 259–261 (`PrivacyPolicyPage/` in `public_pages/` package, Phase 4)
  - `Clearly defined:` DESIGN.md PrivacyPolicyPage section
  - `Clearly defined:` scaffold-spec.md routing: `/privacy` — PrivacyPolicyPage
  - `Clearly defined:` scaffold-spec.md schema: `privacy_policy` table
  - `Clearly defined:` ADR-018: Legal Policy Responsibility Acknowledgement
- **Form type:** Public-facing read-only page — renders privacy policy clauses
- **Components:** Page title (Heading), last updated (Text), clauses ColumnPanel with 3 category groups (3x Heading + 3x Text), FooterComponent
- **Empty state:** N/A — public-facing read-only page, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.9 TermsConditionsPage

- **File:** `wireframes/public_pages/TermsConditionsPage/wireframe-public_pages-TermsConditionsPage.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 262–264 (`TermsConditionsPage/` in `public_pages/` package, Phase 4)
  - `Clearly defined:` DESIGN.md TermsConditionsPage section
  - `Clearly defined:` scaffold-spec.md routing: `/terms` — TermsConditionsPage
  - `Clearly defined:` scaffold-spec.md schema: `t_and_c` table
  - `Clearly defined:` ADR-018: Legal Policy Responsibility Acknowledgement
- **Form type:** Public-facing read-only page — renders terms and conditions clauses
- **Components:** Page title (Heading), last updated (Text), clauses ColumnPanel with 3 category groups (3x Heading + 3x Text), FooterComponent
- **Empty state:** N/A — public-facing read-only page, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** All 9 items in the task list received their own complete wireframe.

---

## 3. Prior Existing Wireframe Diagnostic

All 9 existing wireframes were non-compliant with ADR-27:

### Common violations across all wireframes:
1. **Extra "Architecture" section** — All wireframes had a 4th section with explanatory text about data sources, routing, ADRs, and implementation details. ADR-27 Section 2 requires exactly three sections.
2. **Explanatory text in wireframe diagram** — Multiple violations of ADR-27 Section 4.3: "Accreditation details rendered from business_profile.accreditations", "Google Maps Embed", "[ Parallax background — gradient layer with overlay ]", etc.
3. **Sample data in footer** — All wireframes showed "email@example.com" and "+1 234 567 890" in the FooterComponent rendering. ADR-27 Section 4.2 prohibits sample data.

### Specific issues:
- **HomePageClassicTemplate** — File contained Booking template content (mislabeled/corrupted). Filename had a trailing space before `.html`.
- **All HomePage templates** — Had "Architecture" sections with template differentiation notes, shared component lists, and routing information.

---

## 4. Specification Gaps

No specification gaps found. All 9 forms have clear architectural backing from scaffold-spec.md, DESIGN.md, and ADR-018.

---

## 5. Summary

| Item | Status |
|------|--------|
| Wireframes created | 9 (AboutPage, ContactPage, HomePage, HomePageBookingTemplate, HomePageClassicTemplate, HomePageMinimalistTemplate, HomePageServicesTemplate, PrivacyPolicyPage, TermsConditionsPage) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 9 wireframes pass all 15 checklist items |
| Specification gaps | None |
| Prior wireframes replaced | 9 (all non-compliant) |

---

*End of report — 2026-06-19*
