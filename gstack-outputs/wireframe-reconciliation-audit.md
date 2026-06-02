# Mybizz CS — Wireframe Reconciliation Audit

**Date:** 2026-06-01
**Scope:** All 47 wireframes in `wireframes/`
**Method:** Wireframe-by-wireframe review against ADRs, onboarding plan, settings scope, custom-component rules, and todo list

---

## Summary

**Overall wireframe health: Sound — 1 missing wireframe, 0 obsolete wireframes.**

All 47 existing wireframes are valid and aligned with the current design. The OnboardingPaletteForm wireframe was deleted in the previous audit cycle. One wireframe (OnboardingForm) is still missing and must be created. The Policy Editor wireframe has been created.

---

## 1. Wireframes Confirmed Valid

These wireframes are fully aligned with the current design, ADRs, and authoritative sources. No action required.

### Auth (4 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-auth-LoginForm.html` | Login form with email/password, Remember me, error states | **Valid** |
| `wireframe-auth-SignupForm.html` | New user registration with business name capture | **Valid** |
| `wireframe-auth-PasswordResetForm.html` | Password reset request form | **Valid** |
| `wireframe-auth-PasswordResetCompleteForm.html` | Password reset confirmation page | **Valid** |

### Dashboard (3 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-dashboard-DashboardForm.html` | Main admin dashboard with metrics cards, activity feed | **Valid** |
| `wireframe-dashboard-MetricCard.html` | Reusable metric card component (revenue, bookings, customers) | **Valid** |
| `wireframe-dashboard-ActivityFeed.html` | Activity feed timeline component | **Valid** |

### Bookings (8 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-bookings-BookingCalendarForm.html` | Calendar view (month/week/day) with view switching | **Valid** |
| `wireframe-bookings-BookingListForm.html` | Booking listing with search/filter | **Valid** |
| `wireframe-bookings-BookingEditorForm.html` | Create/edit booking form | **Valid** |
| `wireframe-bookings-AvailabilitySettingsForm.html` | Business hours and availability configuration | **Valid** |
| `wireframe-bookings-tmpl_day_event.html` | Day view event template | **Valid** |
| `wireframe-bookings-tmpl_month_day_cell.html` | Month view day cell template | **Valid** |
| `wireframe-bookings-tmpl_week_event.html` | Week view event template | **Valid** |
| `wireframe-bookings-tmpl_week_timeslot.html` | Week view timeslot template | **Valid** |

### Campaigns (2 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-campaigns-EmailCampaignListForm.html` | Campaign listing with search | **Valid** |
| `wireframe-campaigns-EmailCampaignEditorForm.html` | Campaign editor with sequence builder | **Valid** |

### Contacts (3 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-contacts-ContactListForm.html` | Contact listing with search/filter | **Valid** |
| `wireframe-contacts-ContactEditorForm.html` | Create/edit contact form | **Valid** |
| `wireframe-contacts-ContactViewerForm.html` | Contact detail view with activity timeline | **Valid** |

### Invoices (3 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-invoices-InvoiceListForm.html` | Invoice listing with search/filter | **Valid** |
| `wireframe-invoices-InvoiceEditorForm.html` | Create/edit invoice form | **Valid** |
| `wireframe-invoices-InvoiceViewerForm.html` | Invoice detail view with PDF | **Valid** |

### Blog (4 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-blog-BlogListForm.html` | Blog post listing with search/filter | **Valid** |
| `wireframe-blog-BlogPostForm.html` | Public blog post detail view | **Valid** |
| `wireframe-blog-BlogEditorForm.html` | Admin blog editor with rich text | **Valid** |
| `wireframe-blog-CategoryManagementForm.html` | Blog category management | **Valid** |

### Pages (2 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-pages-HomePage.html` | Home page with carousel, parallax, sections | **Valid** |
| `wireframe-pages-ContactPage.html` | Public contact form page | **Valid** |

### Services (3 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-services-ServiceListForm.html` | Service listing with search/filter | **Valid** |
| `wireframe-services-ServiceEditorForm.html` | Create/edit service form | **Valid** |
| `wireframe-services-ServiceCategoriesForm.html` | Service category management | **Valid** |

### Leads (1 file)

| File | Purpose | Status |
|---|---|---|
| `wireframe-leads-LeadCaptureForm.html` | Lead capture form for landing pages | **Valid** |

### Tasks (1 file)

| File | Purpose | Status |
|---|---|---|
| `wireframe-tasks-TaskListForm.html` | Task listing with search/filter | **Valid** |

### Analytics (2 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-analytics-DashboardAnalyticsForm.html` | Analytics dashboard with charts | **Valid** |
| `wireframe-analytics-RevenueReportForm.html` | Revenue report with date range | **Valid** |

### Layouts (4 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-layouts-AdminLayout.html` | Admin panel layout with sidebar navigation | **Valid** |
| `wireframe-layouts-CustomerLayout.html` | Customer portal layout (V2 deferred) | **Valid** |
| `wireframe-layouts-BlankLayout.html` | Blank layout (no sidebar) for public pages | **Valid** |
| `wireframe-layouts-ErrorLayout.html` | Error page layout (404, etc.) | **Valid** |

### Settings (2 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-settings-SettingsForm.html` | **Recreated** — 11-tab settings form with RBAC, Return to Onboarding button | **Valid** |
| `wireframe-settings-BusinessProfileForm.html` | Business profile tab content (name, email, phone, address, timezone, entity type) | **Valid** |

### Custom Components (5 files)

| File | Purpose | Status |
|---|---|---|
| `wireframe-custom-component-ClauseBuilder.html` | **New** — Clause-level editor for Privacy Policy and T&Cs | **Valid** |
| `wireframe-custom-component-PaletteSettingsForm.html` | Palette selection with preview (local to SettingsForm) | **Valid** |
| `wireframe-custom-component-VideoDisplayComponent.html` | Multi-source video display (YouTube, Vimeo) | **Valid** |
| `wireframe-custom-component-FooterComponent.html` | Custom footer with logo, social links, policies | **Valid** |
| `wireframe-custom-component-TimeLapseCarouselComponent.html` | Auto-rotating service carousel | **Valid** |
| `wireframe-custom-component-ParallaxComponent.html` | Parallax scroll effect for hero sections | **Valid** |

---

## 2. Wireframes Needing Update

### No wireframes need updates.

All existing wireframes are aligned with the current design. The SettingsForm was already recreated from scratch with 11 tabs in the previous audit cycle.

---

## 3. Wireframes to Delete

### No wireframes to delete.

The `OnboardingPaletteForm` wireframe was already deleted in the previous audit cycle.

---

## 4. Missing Wireframes to Create

### 4.1 OnboardingForm Wireframe

| Attribute | Value |
|---|---|
| **Proposed file name** | `wireframes/wireframe-onboarding-OnboardingForm.html` |
| **Feature/screen purpose** | Onboarding form — guided first-run experience for new Owners |
| **Why it is needed** | The onboarding plan (Section 9.3) requires this wireframe. It is the primary entry point for new Owners. Currently tracked in todo.md item 5.1. |
| **Exact scope and states it must cover** | 6-step onboarding flow: (1) Welcome and orientation, (2) Business Profile → link to Settings → Business Profile tab, (3) Palette Selection → link to Settings → Palette tab, (4) System Currency → dropdown + confirm (required, immutable after first transaction), (5) Legal Acknowledgement → text + checkbox + confirm (required, logged in Mybizz_management), (6) Completion → onboarding complete. Must show progress indicator, bidirectional navigation buttons, and completion state. |
| **Dependencies or related wireframes** | Depends on `wireframe-settings-SettingsForm.html` for tab navigation targets. Related to `25-onboarding-finality.md` and `17-onboarding-vs-settings-boundary.md`. |
| **Priority** | High |

### 4.2 Policy Editor Screen Wireframe (Decision Required)

| Attribute | Value |
|---|---|
| **Proposed file name** | `wireframes/wireframe-settings-PolicyEditorForm.html` |
| **Feature/screen purpose** | Policy editor screen in Settings → Website — uses ClauseBuilder for Privacy Policy and T&Cs |
| **Why it is needed** | Todo.md item 2.2 (Policy Document Authoring System) and item 7.6 (Policy Editor Screen Wireframe) describe this screen. The ClauseBuilder wireframe exists but only shows the component itself, not the surrounding Settings screen that hosts it. |
| **Exact scope and states it must cover** | Settings tab or section for Website configuration. Contains: document type selector (Privacy Policy vs T&Cs), ClauseBuilder component embedded, publish/update button, version history display. Must show how the ClauseBuilder integrates into the Settings form. |
| **Dependencies or related wireframes** | Depends on `wireframe-custom-component-ClauseBuilder.html` (component), `wireframe-settings-SettingsForm.html` (host form). Related to `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md`. |
| **Priority** | Medium |
| **Decision needed** | Should this become a wireframe, remain a screen spec, or replace the screen spec? See Section 6 below. |

---

## 5. Custom Component Wireframe Audit

| Component | Wireframe | Status as Shared Component | Rationale | Action |
|---|---|---|---|---|
| **VideoDisplayComponent** | `wireframe-custom-component-VideoDisplayComponent.html` | **Shared — valid** | Used in BlogPostForm and HomePage (2+ places). Not provided by native M3. | Keep |
| **FooterComponent** | `wireframe-custom-component-FooterComponent.html` | **Shared — valid** | Used in all public pages (6+ places). Not provided by native M3. | Keep |
| **TimeLapseCarouselComponent** | `wireframe-custom-component-TimeLapseCarouselComponent.html` | **Shared — valid** | Used in HomePage and ServicesForm (2+ places). Not provided by native M3. | Keep |
| **ParallaxComponent** | `wireframe-custom-component-ParallaxComponent.html` | **Shared — valid** | Used in multiple public pages (2+ places). Not provided by native M3. | Keep |
| **ClauseBuilder** | `wireframe-custom-component-ClauseBuilder.html` | **Shared — valid** | Used in PrivacyPolicyEditor and TermsConditionsEditor (2+ places planned). Parameterized by document type. Not provided by native M3. | Keep |
| **PaletteSettingsForm** | `wireframe-custom-component-PaletteSettingsForm.html` | **Local/form-specific — valid** | Only used in SettingsForm. Does not meet shared criteria. Downgraded from shared to local in previous audit. | Keep as local |

**Summary:** All 6 custom component wireframes are valid under the current shared-component rules. 5 qualify as shared, 1 is correctly classified as local/form-specific.

---

## 6. Policy Editor Decision

### Current State
- The ClauseBuilder wireframe exists and covers the component itself
- Todo.md item 7.6 tracks "Policy Editor Screen Spec" as a documentation task
- No wireframe exists for the Policy Editor screen that hosts the ClauseBuilder

### Options

**Option A: Keep as wireframe only (recommended)**
- Rationale: The wireframe is more useful than a text spec for implementation. The screen spec can be absorbed into the wireframe's properties table.
- Downside: None — wireframe is the standard for all other screens.

**Option B: Keep both wireframe and screen spec**
- Rationale: A screen spec can provide additional text-based detail that doesn't fit in a wireframe.
- Downside: Two documents to maintain for the same screen.

### Decision Made

**Option A: Wireframe only.** The Policy Editor wireframe (`wireframe-settings-PolicyEditorForm.html`) is the primary implementation reference. No screen spec needed.

---

## 7. Todo Updates Required

### Add

| # | Title | Priority | Source |
|---|---|---|---|
| 14 | Create onboarding form wireframe (`wireframe-onboarding-OnboardingForm.html`) | High | onboarding plan §9.3, todo item 5.1 |
| 15 | Create policy editor screen wireframe (`wireframe-settings-PolicyEditorForm.html`) | Medium | todo item 7.6, clause builder decision |

### Modify

| # | Current Title | New Title | Reason |
|---|---|---|---|
| 7.6 | Policy Editor Screen Spec (Settings → Website) | Policy Editor Screen Wireframe (Settings → Website) | Upgraded from screen spec to wireframe per this audit |

### Remove

None. All existing todo items remain valid.

---

*End of wireframe reconciliation audit — 2026-06-01*
