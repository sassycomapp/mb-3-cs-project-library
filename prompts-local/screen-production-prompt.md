---
title: "screen-production-prompt"
doc-id: "screen-production-prompt"
---
# Screen Production Prompt — /design-html

**Purpose:** Produce screen HTML/CSS mockups that are true representations of wireframes, with MUI aesthetic overlay applied, for conversion to PNG screenshots that Anvil AI will use as visual references when building the live UI.
**Skill:** `/design-html`
**Date:** 2026-06-28

---

## Prompt

```
/design-html

You are producing screen HTML/CSS mockups for the Mybizz CS application. Each screen is a faithful HTML/CSS rendering of its corresponding wireframe, with the MUI aesthetic overlay applied. These screens will be screenshotted to PNG for Anvil AI to use as visual references when building the live UI in Anvil.

---

## DOCUMENTATION TO STUDY — Read before producing any screen

Read every file below in full before producing your first screen. Do not proceed until all are read.

### Instructional documents (for Anvil AI — you must understand what the live builder will follow):
1. [[anvil-ui-build-standard-v1-2026-07-05|v1]]
2. [[anvil-ui-exception-review-procedure|Per-UI Exception Review Procedure]]

### Design and specification documents:
3. [[DESIGN|DESIGN]]
4. [[design-direction|Design Direction (Canonical)]]
5. [[spec-screen-production-standard|Screen Production Standard]]
6. [[spec-m3_component_mapping|m3_component_mapping.md]]
7. `/mnt/c/dev/project-library-global/specifications-global/spec-nomenclature.md`
8. [[spec-component-properties|Component Properties Specification]]

### Theme and style reference:
9. `~~/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/docs/theme_patched.css~~ — removed; no longer on disk anywhere (verified 2026-09-18). Use the M3 theme dependency + global design standards above for token values.`
10. `~~/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/docs/theme_mui_patch.css~~ — removed; no longer on disk anywhere (verified 2026-09-18).`
11. `/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/output-gstack/design-reference-screens-old.html` — the existing style reference

### ADRs:
12. [[adr-role-property-assignment-mechanism|Role Property Assignment Mechanism (Designer vs. Code)]] — role set in code
13. [[adr-responsive-behaviour-mechanism|Responsive Behaviour Mechanism]] — wrap_on, not breakpoints
14. [[adr-htmltemplate-use|HTMLTemplate Use]] — HTMLTemplate banned
15. [[adr-material-3-theme-component-scope|Material 3 Theme Component Scope]] — permitted M3 components
16. [[adr-design-rules|Wireframe Design Rules]] — wireframe design rules

---

## COMPLIANCE CHECKLIST — Apply to every screen

Before considering any screen complete, verify every item below. Save a completed copy of this checklist alongside each screen.

### A. Wireframe fidelity
- [ ] Every component in the wireframe's Section 2 appears in the screen
- [ ] No component appears in the screen that is not in the wireframe
- [ ] Component hierarchy matches wireframe (containers contain the correct children)
- [ ] Component types match wireframe (Button not TextBox, Card not ColumnPanel, etc.)

### B. Page chrome exclusion
- [ ] No left sidebar unless the wireframe includes it inside its outermost container
- [ ] No app bar unless the wireframe includes it inside its outermost container
- [ ] No footer unless the wireframe includes it inside its outermost container
- [ ] No navigation elements unless the wireframe includes them inside its outermost container
- [ ] The screen contains ONLY what the wireframe declares inside its outermost container boundary

### C. Repeating panels and row templates
- [ ] DataGrid row content is NOT rendered in the host screen
- [ ] RepeatingPanel row content is NOT rendered in the host screen
- [ ] Row template components appear only in their own row template screen, not in the parent form screen
- [ ] The host screen shows the DataGrid/RepeatingPanel container with a placeholder annotation

### D. Sample data (per Standard §8)
- [ ] All data fields show sample data, not empty
- [ ] Sample data is South African consulting/services vertical
- [ ] Currency is ZAR (R symbol)
- [ ] Names are plausible South African names
- [ ] Dates, phone numbers, addresses are realistic format
- [ ] No "Lorem ipsum" placeholder text
- [ ] No production data

### E. MUI aesthetic overlay (all 9 variables)
- [ ] 1. Surface distinction — body uses `var(--surface)`, panels/nav use `var(--surface-container-low)`
- [ ] 2. Card shadow — `hsla(220,30%,5%,0.07) 0 4px 16px 0, hsla(220,25%,10%,0.07) 0 8px 16px -5px`
- [ ] 3. Card radius — 8px
- [ ] 4. Typography heading weight — 600
- [ ] 5. Dividers — outline-variant at 40% opacity
- [ ] 6. State hover opacity — 0.08
- [ ] 7. State pressed/focused opacity — 0.12
- [ ] 8. App bar — `var(--surface)` background, 1px hairline bottom border
- [ ] 9. Left nav — `var(--surface-container-low)` background, 1px hairline right border

### F. Colour and token compliance
- [ ] No hardcoded hex values
- [ ] No hardcoded rgb()/rgba() (except MUI shadow hsla values)
- [ ] All colors resolve through CSS custom properties from Professional Blue palette
- [ ] Status colours (green/orange/red) use semantic exception only

### G. Component compliance
- [ ] No `Label` component (use `Text`)
- [ ] No `HTMLTemplate` component
- [ ] No `SidesheetContent` component
- [ ] No `HtmlPanel` component
- [ ] InteractiveCard uses only `elevated`, `filled`, or `outlined` (not `tonal`)
- [ ] All components are from the approved M3 list or approved custom component list

### H. Naming and structure
- [ ] Component names follow `nomenclature.md` prefixes
- [ ] Material Icons imported where icon components are used
- [ ] Icon annotations use `mi:` prefix
- [ ] `ico_` prefix for Icon (IconButton used decoratively)

### I. Output quality
- [ ] Screen renders correctly in a browser
- [ ] No broken layout or overflow issues
- [ ] Typography matches the project scale (Roboto, correct sizes/weights)
- [ ] Spacing follows the 4px grid

---

## PROCESS — One screen at a time

For each screen, follow this exact process:

### Step 1: Read the wireframe
Read the wireframe HTML file in full. Identify:
- The outermost container boundary (everything outside is page chrome, excluded)
- All components inside the boundary
- Any DataGrid or RepeatingPanel (row content goes in a separate screen)
- Any hidden panels (visible = False)
- Any conditional visibility

### Step 2: Read the corresponding checklist template
Use the compliance checklist above. You will fill it in for this specific screen.

### Step 3: Produce the screen HTML
Create the screen as a standalone HTML file that:
- Renders the wireframe's component structure faithfully
- Applies the MUI aesthetic overlay from `theme_mui_patch.css`
- Includes sample data per Standard §8 (South African, ZAR, plausible)
- Excludes all page chrome not in the wireframe
- Excludes DataGrid/RepeatingPanel row content
- Uses Professional Blue palette tokens throughout
- Imports Material Icons where needed

### Step 4: Verify against the checklist
Run through every item in the compliance checklist. Fix any failures before saving.

### Step 5: Handle discrepancies immediately
If you find a discrepancy between the wireframe and any other document (Standard, design-direction, ADRs, etc.), STOP and report it. Do not silently resolve it. Do not move on to the next screen. Report the discrepancy and wait for resolution.

### Step 6: Save the screen and checklist
Save the screen HTML to:
`/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/screens/{package}/{FormName}/screen-{package}-{FormName}.html`

Save the completed checklist to:
`/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/screens/{package}/{FormName}/checklist-screen-{package}-{FormName}.md`

### Step 7: Confirm completion
Report: screen saved, checklist saved, any discrepancies found (if none, say "no discrepancies").

### Step 8: Move to the next wireframe
After completing Step 7, immediately proceed to the next wireframe in the scaffold order and repeat Steps 1–7 for that wireframe. Do not wait for user confirmation to continue — the goal is a screen for every wireframe.

**Scaffold order:** Follow the package sequence defined in [[scaffold-spec|Scaffold Specification]]:
1. `auth/` — LoginForm, SignupForm, PasswordResetForm, PasswordResetCompleteForm
2. `onboarding/` — OnboardingForm
3. `dashboard/` — DashboardForm, MetricCard, ActivityFeed
4. `layouts/` — AdminLayout, CustomerLayout, BlankLayout, ErrorLayout
5. `crm/` — ContactListForm, ContactEditorForm, ContactViewerForm, EmailCampaignListForm, EmailCampaignEditorForm, EmailBroadcastForm, SegmentManagerForm, TaskListForm, LeadCaptureForm
6. `services/` — ServicesForm, ServiceEditorForm, ServiceViewerForm, ServiceCategoriesForm
7. `bookings/` — BookingListForm, BookingEditorForm, BookingViewerForm, BookingCalendarForm (+ row templates), AvailabilitySettingsForm (+ row template)
8. `invoices/` — InvoiceListForm, InvoiceEditorForm, InvoiceViewerForm (+ row template)
9. `analytics/` — DashboardAnalyticsForm, RevenueReportForm (+ row template)
10. `blog/` — BlogListForm (+ templates), BlogEditorForm, BlogPostForm (+ templates), CategoryManagementForm (+ template), CommentModerationForm (+ row template)
11. `settings/` — SettingsForm, EmailSetupForm, PaymentGatewayForm, VaultForm, UserManagementForm, PolicyEditorForm, ThemeConfigForm
12. `public_pages/` — HomePage (+ templates), AboutPage, ContactPage, PrivacyPolicyPage, TermsConditionsPage
13. `landing_pages/` — LandingPageListForm, LandingPageEditorForm, LandingPage
14. `components/` — ClauseBuilder, FooterComponent, VideoDisplayComponent, TimeLapseCarouselComponent, ParallaxComponent

**Within each package**, process forms in the order listed above. Row templates and sub-templates are processed immediately after their parent form.

**Wireframe location pattern:** `wireframes/{package}/{FormName}/wireframe-{package}-{FormName}.html`
**Screen save pattern:** `screens/{package}/{FormName}/screen-{package}-{FormName}.html`

**When to stop:**
- After every wireframe has a corresponding screen, report total screens produced.
- If a discrepancy is found and requires user resolution (Step 5), pause and wait. Resume from that wireframe once resolved.

---

## CRITICAL RULES

1. **No batching.** One screen per iteration. Complete it fully before starting the next.
2. **No templatizing.** Each screen is produced individually from its wireframe.
3. **Wireframe is the sole structural authority.** The screen must match the wireframe exactly. Nothing added, nothing omitted.
4. **Discrepancies are raised immediately.** Do not accumulate a list. Fix or report before moving on.
5. **Screens are HTML/CSS mockups.** They approximate the final Anvil UI using HTML/CSS. They are not Anvil Forms.
6. **Screens will be converted to PNG.** The PNG is what Anvil AI receives as a visual reference. Ensure the HTML renders cleanly for screenshot conversion.
7. **The Anvil build documents are for the Anvil AI agent, not for you.** You are the screen producer. The Standard, Prompt Template, and Exception Procedure are what Anvil AI will follow when building the live UI. Your screens must be faithful enough that Anvil AI can build correctly from them.

---

## START

Begin with the first wireframe in the scaffold order:
`/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/ui/analytics/DashboardAnalyticsForm/DashboardAnalyticsForm-wireframe.html`

After completing each screen, Step 8 instructs you to proceed to the next wireframe automatically. Continue until every wireframe has a corresponding screen.
```

---
Scaffold order:

blog				
	BlogEditorForm			
		blog-BlogEditorForm		
	BlogListForm			
		BlogListListTemplate		
			blog-BlogListListTemplate	
		BlogListTiledTemplate		
			blog-BlogListTiledTemplate	
		blog-BlogListForm		
	BlogPostForm			
		BlogPostClassicTemplate		
			blog-BlogPostClassicTemplate	
		BlogPostFullWidthTemplate		
			blog-BlogPostFullWidthTemplate	
		BlogPostTwoColumnTemplate		
			BlogPostCategoriesTemplate	
				blog-BlogPostCategoriesTemplate
			BlogPostRelatedPostsTemplate	
				blog-BlogPostRelatedPostsTemplate
			blog-BlogPostTwoColumnTemplate	
		blog-BlogPostForm		
	CategoryManagementForm			
		CategoryManagementForm-RowTemplate		
			blog-CategoryManagementForm-RowTemplate	
		blog-CategoryManagementForm		
	CommentModerationForm			
		CommentModerationForm-RowTemplate		
			blog-CommentModerationForm-RowTemplate	
		blog-CommentModerationForm		
bookings				
	AvailabilitySettingsForm			
		AvailabilitySettingsFormRowTemplate		
			bookings-AvailabilitySettingsFormRowTemplate	
		bookings-AvailabilitySettingsForm (1).png		
		bookings-AvailabilitySettingsForm		
		bookings-AvailabilitySettingsForm.png		
	BookingCalendarForm			
		tmpl_day_event		
			bookings-tmpl_day_event	
		tmpl_month_day_cell		
			bookings-tmpl_month_day_cell	
		tmpl_week_event		
			bookings-tmpl_week_event	
		tmpl_week_timeslot		
			bookings-tmpl_week_timeslot	
		bookings-BookingCalendarForm		
	BookingEditorForm			
		bookings-BookingEditorForm		
	BookingListForm			
		BookingListForm-RowTemplate		
			bookings-BookingListForm-RowTemplate	
		bookings-BookingListForm		
	BookingViewerForm			
		bookings-BookingViewerForm		
		bookings-BookingViewerForm.png		
components				
	ClauseBuilder			
		components-ClauseBuilder		
	FooterComponent			
		components-FooterComponent		
	ParallaxComponent			
		components-ParallaxComponent		
	TimeLapseCarouselComponent			
		components-TimeLapseCarouselComponent		
	VideoDisplayComponent			
		components-VideoDisplayComponent		
crm				
	ContactEditorForm			
		crm-ContactEditorForm		
	ContactListForm			
		crm-ContactListForm		
	ContactViewerForm			
		crm-ContactViewerForm		
	EmailBroadcastForm			
		crm-EmailBroadcastForm		
	EmailCampaignEditorForm			
		crm-EmailCampaignEditorForm		
	EmailCampaignListForm			
		crm-EmailCampaignListForm		
	LeadCaptureForm			
		crm-LeadCaptureForm		
	SegmentManagerForm			
		crm-SegmentManagerForm		
	TaskListForm			
		crm-TaskListForm		
dashboard				
	ActivityFeed			
		checklist-dashboard-ActivityFeed.md		
		dashboard-ActivityFeed		
	DashboardForm			
		dashboard-DashboardForm		
	MetricCard			
		checklist-dashboard-MetricCard.md		
		dashboard-MetricCard		
invoices				
	InvoiceEditorForm			
		invoices-InvoiceEditorForm		
	InvoiceListForm			
		invoices-InvoiceListForm		
	InvoiceViewerForm			
		invoices-InvoiceViewerForm		
landing_pages				
	LandingPage			
		EventRegistrationTemplate		
			landing_pages-EventRegistrationTemplate	
		LeadCaptureTemplate		
			landing_pages-LeadCaptureTemplate	
		VslTemplate		
			landing_pages-VslTemplate	
		landing_pages-LandingPage		
	LandingPageEditorForm			
		landing_pages-LandingPageEditorForm		
	LandingPageListForm			
		landing_pages-LandingPageListForm		
layouts				
	AdminLayout			
		layouts-AdminLayout		
	BlankLayout			
		layouts-BlankLayout		
	CustomerLayout			
		layouts-CustomerLayout		
	ErrorLayout			
		layouts-ErrorLayout		
onboarding				
	OnboardingForm			
		checklist-onboarding-OnboardingForm.md		
		onboarding-OnboardingForm		
public_pages				
	AboutPage			
		public_pages-AboutPage		
	ContactPage			
		public_pages-ContactPage		
	HomePage			
		HomePageBookingTemplate		
			public_pages-HomePageBookingTemplate	
		HomePageClassicTemplate		
			public_pages-HomePageClassicTemplate	
		HomePageMinimalistTemplate		
			public_pages-HomePageMinimalistTemplate	
		HomePageServicesTemplate		
			public_pages-HomePageServicesTemplate	
		public_pages-HomePage		
	PrivacyPolicyPage			
		public_pages-PrivacyPolicyPage		
	TermsConditionsPage			
		public_pages-TermsConditionsPage		
services				
	ServiceCategoriesForm			
		services-ServiceCategoriesForm		
	ServiceEditorForm			
		services-ServiceEditorForm		
	ServicesForm			
		services-ServicesForm		
	ServiceViewerForm			
		services-ServiceViewerForm		
settings				
	EmailSetupForm			
		settings-EmailSetupForm		
	PaymentGatewayForm			
		settings-PaymentGatewayForm		
	PolicyEditorForm			
		settings-PolicyEditorForm		
	SettingsForm			
		settings-SettingsForm		
	ThemeConfigForm			
		settings-ThemeConfigForm		
	UserManagementForm			
		settings-UserManagementForm		
	VaultForm			
		settings-VaultForm		
	

*End of Screen Production Prompt*
