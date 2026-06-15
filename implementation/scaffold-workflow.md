# Mybizz CS — Anvil Scaffolding Workflow

**Purpose:** Step-by-step instructions for creating the `mb-3-cs` app in Anvil.works

**Prerequisites:**
- Anvil.works account
- GitHub account
- Access to `C:\_mb2-cs-app\` directory
- `scaffold-spec.md` — Reference for exact specifications

---

## Step 1: Create New Anvil App

1. **Log in to Anvil.works**
   - Go to https://anvil.works
   - Log in with your Anvil account

2. **Create New App**
   - Click "Create New App"
   - Name: `mb-3-cs`
   - Description: "Mybizz Consulting & Services Platform"
   - Click "Create"

3. **Set App Package Name**
   - Go to Settings → App Settings
   - Package name: `mb_3_cs`
   - Click "Save"

---

## Step 2: Install Dependencies

1. **Go to Settings → Dependencies**
2. **Install M3 Theme**
   - Click "Install Dependency"
   - Enter: `4UK6WHQ6UX7AKELK`
   - Name: `Material 3 Theme`
   - Click "Install"
3. **Install Routing**
   - Click "Install Dependency"
   - Enter: `3PIDO5P3H4VPEMPL`
   - Name: `Routing`
   - Click "Install"
4. **Set M3 as Default Theme**
   - Go to Settings → App Settings
   - Theme: `Material 3 Theme`
   - Click "Save"

---

## Step 3: Create Database Tables

Follow the exact specifications in `scaffold-spec.md` to create all 43 tables.

### Table Creation Order

**Create tables in this order** to respect foreign key dependencies:

1. **Core Tables** (create first, no dependencies):
   - `users` (extend Anvil Users)
   - `activity_log`
   - `business_profile`
   - `config`
   - `email_config`
   - `files`
   - `payment_config`
   - `rate_limits`
   - `theme_config`
   - `vault`

2. **Booking Tables**:
   - `availability_rules`
   - `availability_exceptions`
   - `booking_metadata_schemas`
   - `services`
   - `bookings`
   - `contact_submissions`

3. **CRM Tables**:
   - `contacts`
   - `contact_counter`
   - `contact_events`
   - `contact_campaigns`
   - `email_campaigns`
   - `segments`
   - `tasks`
   - `leads`
   - `lead_captures`

4. **Email Tables**:
   - `email_templates`
   - `email_log`
   - `webhook_log`

5. **Content Tables**:
   - `blog_categories`
   - `blog_posts`
   - `landing_pages`
   - `pages`

6. **Finance Tables**:
   - `invoice`
   - `invoice_items`
   - `time_entries`

**Reference:** `scaffold-spec.md` for exact field specifications.

---

## Step 4: Create Client Code Packages

Create the following folder structure in `client_code/`:

```
client_code/
├── auth/
│   ├── LoginForm/
│   ├── SignupForm/
│   └── PasswordResetForm/
├── dashboard/
│   ├── DashboardForm/
│   └── components/
│       ├── MetricCard/
│       └── ActivityFeed/
├── settings/
│   ├── SettingsForm/
│   ├── VaultForm/
│   ├── EmailSetupForm/
│   ├── PaymentGatewayForm/
│   ├── ThemeConfigForm/
│   └── UserManagementForm/
├── layouts/
│   ├── AdminLayout/
│   ├── CustomerLayout/
│   ├── BlankLayout/
│   └── ErrorLayout/
├── services/
│   ├── ServiceListForm/
│   ├── ServiceEditorForm/
│   ├── ServiceViewerForm/
│   └── ServiceCategoriesForm/
├── bookings/
│   ├── BookingCalendarForm/
│   ├── BookingListForm/
│   ├── BookingEditorForm/
│   ├── BookingViewerForm/
│   ├── AvailabilitySettingsForm/
│   └── TimeEntriesForm/
├── crm/
│   ├── ContactListForm/
│   ├── ContactEditorForm/
│   ├── ContactViewerForm/
│   ├── SegmentManagerForm/
│   ├── TaskListForm/
│   ├── EmailCampaignListForm/
│   ├── EmailCampaignEditorForm/
│   ├── EmailBroadcastForm/
│   └── LeadCaptureForm/
├── invoices/
│   ├── InvoiceListForm/
│   ├── InvoiceEditorForm/
│   └── InvoiceViewerForm/
├── blog/
│   ├── BlogListForm/
│   ├── BlogPostForm/
│   ├── BlogEditorForm/
│   └── CategoryManagementForm/
├── public_pages/
│   ├── HomePage/
│   ├── AboutPage/
│   ├── ContactPage/
│   ├── PrivacyPolicyPage/
│   ├── TermsConditionsPage/
│   └── LandingPage/
├── analytics/
│   ├── DashboardAnalyticsForm/
│   └── RevenueReportForm/
└── shared/
    ├── navigation_helpers/
    ├── validation_utils/
    └── formatting_utils/
```

**Reference:** `scaffold-spec.md` for form specifications.

---

## Step 5: Create Server Code Packages

Create the following folder structure in `server_code/`:

```
server_code/
├── server_auth/
│   ├── service
│   └── rbac
├── server_dashboard/
│   └── service
├── server_settings/
│   ├── service
│   ├── encryption_service
│   ├── vault_service
│   └── vault_totp_service
├── server_services/
│   └── service
├── server_bookings/
│   ├── service
│   ├── availability_service
│   ├── time_service
│   └── metadata_service
├── server_customers/
│   ├── contact_service
│   └── segment_service
├── server_marketing/
│   ├── campaign_service
│   ├── broadcast_service
│   ├── task_service
│   ├── lead_capture_service
│   └── brevo_campaigns_integration
├── server_payments/
│   ├── stripe_service
│   ├── paystack_service
│   ├── invoice_service
│   └── webhook_handlers
├── server_blog/
│   └── service
├── server_analytics/
│   └── reporting_service
└── server_shared/
    ├── utilities
    ├── validators
    ├── encryption_service
    ├── config
    └── constants
```

**Note:** Anvil automatically appends `.py` to server modules. Do NOT include `.py` in folder names when creating in Anvil Designer.

**Reference:** `scaffold-spec.md` for module specifications.

---

## Step 6: Configure Startup Form

1. **Go to Settings → App Settings**
2. **Startup Form:** `auth.LoginForm`
3. **Click "Save"**

---

## Step 7: Link to GitHub

1. **Go to Settings → GitHub**
2. **Connect GitHub account**
3. **Create new repository:** `mb-3-cs`
4. **Link repository to app**

---

## Step 8: Verify Scaffolding

**Checklist:**
- [ ] All 43 tables created
- [ ] All 36 forms created
- [ ] All 13 server packages created
- [ ] M3 theme installed and active
- [ ] Routing dependency installed
- [ ] Startup form configured
- [ ] GitHub linked

**Reference:** `scaffold-spec.md` for complete specifications.

---

## Next Steps

After scaffolding is complete:
1. Review `authoritative-schema.md` for complete table specifications
2. Review `phase-0-implementation.md` for Phase 0 requirements
3. Implement TODO 1-4 (Phase 0 tasks)
4. Begin Phase 1 (Foundation)

---

*End of file — scaffold-workflow.md*
