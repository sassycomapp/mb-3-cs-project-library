---
title: "mb-3-cs-c19-app1-ticket-list"
document: Step 19 ticket list — mb-3-cs c19 app1
state: The approved ticket breakdown, in build order, after David's three corrections
doc-id: "mb-dee4d4b1"
updated: "2026-11-17"
---

# Step 19 — Ticket List (run app1, route Application)

52 tickets: the four order-1 foundations, the 11 shared custom components (order 1, ahead of their users), F-01 as order 2, and every other feature F-02–F-37 in the order its dependencies allow. One ticket file per feature in `pdlf-tickets/app1/`, named `NN-<slug>.md`. Approved by David with three corrections (F-16 added as ticket 22; the dashboard moved after the bookings ticket with its blockers; F-03's blockers completed).

The order is the plan's sequence; a ticket starts when its blockers are complete (the frontier). Every ticket names its forms by module path, its server functions by name, its tables as the authoritative schema names them, its acceptance tests by ID from the test plan, and its security criteria from the threat model.

| # | Ticket | Feature | Forms | Blocked by |
|---|---|---|---|---|
| 1 | `01-layouts` | the shell (F-31, §9) | layouts.AdminLayout, CustomerLayout, PublicLayout, BlankLayout, ErrorLayout | — |
| 2 | `02-shared-client-modules` | F-25, F-31 | none (Modules) | — |
| 3 | `03-server-foundation` | F-01, F-02 seams | none | — |
| 4 | `04-integration-seams` | common aspects 6–8 | none | 3 |
| 5 | `05-component-footer` | F-06 | components.FooterComponent | 1, 2 |
| 6 | `06-component-video-display` | F-09, F-25 | components.VideoDisplayComponent | 2 |
| 7 | `07-component-parallax` | F-09 | components.ParallaxComponent | 2 |
| 8 | `08-component-star-rating` | F-23 | components.StarRatingComponent | 2 |
| 9 | `09-component-ai-helper` | F-26, F-27 | components.AIHelperComponent | 2, 4 |
| 10 | `10-component-help-tooltip` | F-25 | components.HelpTooltipComponent | 2, 6 |
| 11 | `11-component-edit-mode` | F-06, F-08, F-21 | components.EditModeComponent | 2, 9 |
| 12 | `12-component-clause-builder` | F-22 | components.ClauseBuilder | 2, 4 |
| 13 | `13-component-share-panel` | F-21 | components.SharePanelComponent | 2, 4 |
| 14 | `14-component-reader-share` | F-21 | components.ReaderShareComponent | 2 |
| 15 | `15-component-paypal-button` | F-15 | components.PayPalButtonComponent | 2, 4 |
| 16 | `16-f01-accounts-roles-access` | F-01 | auth.LoginForm, PasswordResetForm, TwoFactorForm, SetPasswordForm (SignupForm deleted, C2) | 1, 2, 3 |
| 17 | `17-f02-vault` | F-02 | settings.VaultForm | 3, 16 |
| 18 | `18-f05-settings-hub` | F-05 | settings.SettingsForm (+7 local tabs), settings.UserManagementForm | 16, 3 |
| 19 | `19-f31-design-system` | F-31 | none new (startup theming, palettes) | 1, 2, 18 |
| 20 | `20-f17-crm-contacts` | F-17 | crm.ContactListForm, ContactEditorForm, ContactViewerForm, ContactImportForm, CompanyListForm, CompanyViewerForm, SegmentManagerForm | 16, 3 |
| 21 | `21-f10-service-catalogue` | F-10 | services.ServicesForm, ServiceEditorForm, ServiceViewerForm, ServiceCategoriesForm, ResourceListForm, GroupSessionEditorForm | 16, 3 |
| 22 | `22-f16-dual-currency` | F-16 | none new (CurrencyTab completed) | 18, 21 |
| 23 | `23-f15-invoicing-payments` | F-15 | invoices.InvoiceListForm, InvoiceEditorForm, InvoiceViewerForm, PaymentListForm; settings.PaymentGatewayForm | 4, 16, 18, 20, 22 |
| 24 | `24-f11-bookings-scheduling` | F-11 | bookings.BookingCalendarForm (+views), BookingListForm, BookingFlowForm, BookingEditorForm, BookingViewerForm, AvailabilitySettingsForm | 9, 15, 20, 21, 23 |
| 25 | `25-f04-dashboard` | F-04 | dashboard.DashboardForm (+2 local), analytics.DashboardAnalyticsForm, MarketingDashboardForm, RevenueReportForm | 23, 24, 29 |
| 26 | `26-f12-orders` | F-12 | orders.OrderFlowForm, OrderListForm, OrderViewerForm | 15, 20, 21, 23 |
| 27 | `27-f13-session-packages` | F-13 | packages.PackageListForm, PackageEditorForm | 15, 20, 21, 23 |
| 28 | `28-f14-time-tracking` | F-14 | time_tracking.TimeEntryListForm, TimeEntryEditorForm | 16, 21, 23 |
| 29 | `29-f18-email-team-inbox` | F-18 | inbox.TeamInboxForm, ComposeForm; settings.EmailSettingsForm, DomainSettingsForm | 4, 16, 20 |
| 30 | `30-f08-landing-pages` | F-08 | landing_pages.LandingPageListForm, LandingPageEditorForm, LandingPage (+3 local templates) | 2, 11, 20 |
| 31 | `31-f19-lead-capture` | F-19 | crm.LeadCaptureForm | 20, 29, 30 |
| 32 | `32-f20-tasks-automation` | F-20 | crm.TaskListForm, AutomationListForm, AutomationEditorForm | 20, 23, 29 |
| 33 | `33-f06-public-website` | F-06 | public_pages.HomePage, AboutPage, ContactPage, ServicesPage, ServiceDetailPage, PrivacyPolicyPage, TermsConditionsPage, CookiePolicyPage, PageListForm, MediaLibraryForm | 1, 2, 5, 11, 20 |
| 34 | `34-f07-homepage-templates` | F-07 | public_pages.HomePageClassicTemplate, HomePageServicesTemplate, HomePageBookingTemplate, HomePageMinimalistTemplate, HomepageEditorForm | 6, 7, 8, 33 |
| 35 | `35-f09-visual-media` | F-09 | none new (carousel hero, media rules) | 6, 7 |
| 36 | `36-f21-blog-content` | F-21 | blog.BlogListForm (+2 local), BlogPostForm (+3 local), BlogAdminListForm, BlogEditorForm, CategoryManagementForm, CommentModerationForm | 9, 13, 14, 33 |
| 37 | `37-f23-reviews-testimonials` | F-23 | reviews.ReviewListForm, ReviewViewerForm, ReviewSubmitForm | 8, 20, 24 |
| 38 | `38-f35-sales-pipeline` | F-35 | sales.DealBoardForm, DealEditorForm, QuoteListForm, QuoteEditorForm, QuoteAcceptPage | 2, 20, 23 |
| 39 | `39-f24-customer-portal` | F-24 | portal.MyDashboardForm, MyBookingsForm, MyPackagesForm, MyQuotesForm, MyInvoicesForm, MyReviewsForm, AccountForm | 16, 23, 24, 27, 37, 38 |
| 40 | `40-f22-clause-builder` | F-22 | settings.PolicyEditorForm | 12, 18 |
| 41 | `41-f30-security-compliance` | F-30 | none new (the cookie banner on the public pages) | 3, 16, 33, 40 |
| 42 | `42-f25-help-language` | F-25 | none new (tooltips switch, help entries) | 2, 10 |
| 43 | `43-f26-ai-assistance` | F-26 | none new (ten assists; sample data and Clear) | 9, 25 |
| 44 | `44-f27-whatsup-voice` | F-27 | ai.WhatsUpForm | 2, 43 |
| 45 | `45-f28-accounts` | F-28 | accounts.ExpenseListForm, ExpenseEditorForm, AccountsForm | 3, 4, 23 |
| 46 | `46-f29-data-backups-history` | F-29 | none new (DataTab completed) | 3, 4, 17 |
| 47 | `47-f34-management-instance` | F-34 | none (the seven endpoints, heartbeat, usage) | 3, 4 |
| 48 | `48-f37-plan-billing` | F-37 | none new (PlanBillingTab completed) | 18, 47 |
| 49 | `49-f32-help-support` | F-32 | support.HelpForm | 4, 47 |
| 50 | `50-f36-demo-apps` | F-36 | none new (demo mode) | 3, 23, 29, 43 |
| 51 | `51-f03-onboarding` | F-03 | onboarding.OnboardingForm (+DomainStep, ImportStep) | 17, 18, 19, 20, 22, 23, 29, 33, 40, 42, 43 |
| 52 | `52-f33-hosting` | F-33 | none (plan-fit proofs) | 47 |

Divergence assignments carried by the tickets: C2 (SignupForm deleted) → 16; the six `service.py` renames → 16 (auth_service), 20's neighbours n/a, 21 (catalogue_service), 23/24 (booking_service), 25 (dashboard_service), 18 (settings_service); `EmailSetupForm` → `EmailSettingsForm` → 29; Brevo module deleted (C1) → 29; Vault duplicates consolidated (C3) → 3/17; `stripe_service` → `stripe_gateway` → 4/23; `task_service` → `server_automation/` → 32; `metadata_service` → `intake_service` → 24; `policy_service` to `server_settings/` → 12.

Existing forms are rebuilt as HTML templates at the same module path inside their feature's ticket (the 91 pre-PDLF YAML forms; ST-11).
