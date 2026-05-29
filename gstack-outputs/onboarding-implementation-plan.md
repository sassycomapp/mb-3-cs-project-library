# Mybizz CS Onboarding System - Engineering Plan

## Executive Summary

This plan defines an implementation-ready onboarding system for new client instances in Mybizz CS. The system distinguishes between **operator provisioning** (Mybizz staff setup) and **client business-owner onboarding** (in-app guided setup), with clear boundaries between each.

**Summary of the onboarding implementation plan:**

Three distinct flows defined:

Operator provisioning (Mybizz_management app)
Client business-owner onboarding (client instance)
User signup (separate, anytime after onboarding)
12 forms specified with exact inputs, outputs, validations, and navigation
7-step client onboarding sequence:
Welcome → Business Info → Palette → Currency Confirm → Gateway Confirm → First Service → Completion
Data state transitions mapped for each step
8 missing wireframes identified for creation
5 open architectural decisions with recommended defaults
Complete validation rules and failure mode handling
The plan is now ready for implementation planning and wireframe creation.

---

## 1. Onboarding Architecture

### 1.1 Three Distinct Flows

| Flow Type | Purpose | Location | Trigger | Responsible Party |
|-----------|---------|----------|---------|-------------------|
| **Operator Provisioning** | Create new client instance with initial configuration | `Mybizz_management` app | Sales contract signed | Mybizz operator |
| **Client Onboarding** | Business owner configures their instance after instance creation | Client instance (master_template) | Instance created & Owner account exists | Business owner |
| **User Signup** | Add additional users to existing instance | Client instance (master_template) | Owner invites staff/manager | Owner/Manager |

### 1.2 Key Architectural Principles

1. **Operator provisioning creates the instance shell** - no business data, just infrastructure
2. **Client onboarding transforms shell into operational business** - adds business profile, services, payment setup
3. **User signup is separate** - happens anytime after onboarding completion
4. **Currency is immutable after first transaction** - must be set during onboarding
5. **Payment gateway is set during onboarding** - only one gateway per instance (Stripe OR Paystack)

---

## 2. Operator Provisioning Flow (Mybizz_management)

### 2.1 Purpose
Create a new client instance shell with minimal configuration, then notify the business owner to complete onboarding.

### 2.2 Steps

| Step | Action | Data Created | Output |
|------|--------|--------------|--------|
| 1 | Create new Anvil instance (copy from master_template) | New app ID, new database | Instance ready |
| 2 | Create Owner user account | `users` row with Owner role | Owner credentials |
| 3 | Create empty `business_profile` row | `business_profile` row with `instance_id` reference | Profile exists |
| 4 | Set default currency (from contract) | `business_profile.currency` | Currency locked |
| 5 | Set default payment gateway (from contract) | `business_profile.payment_gateway` | Gateway configured |
| 6 | Generate onboarding token | `onboarding_tokens` table | Token sent to owner |
| 7 | Send welcome email with login credentials | Email via Brevo | Owner notified |

### 2.3 Forms Required (Mybizz_management)

1. **`InstanceProvisionForm`** - Operator enters client details
   - Inputs: Business name, owner email, currency, payment gateway, tier (Standard/Premium)
   - Output: Creates instance, owner account, business_profile
   - Next: `ProvisionConfirmationForm`

2. **`ProvisionConfirmationForm`** - Shows created instance details
   - Display: Instance ID, owner email, currency, gateway
   - Output: Send welcome email
   - Next: Return to operator dashboard

### 2.4 Data Created at Operator Provisioning

**Tables affected:**
- `users` - Owner account (Owner role)
- `business_profile` - Empty profile with currency and gateway set
- `onboarding_tokens` - Token for owner to access onboarding flow
- New Anvil instance (copy from master_template)

**Fields set:**
```
business_profile:
  - instance_id: (auto-generated)
  - currency: (from contract - immutable after first transaction)
  - payment_gateway: (Stripe OR Paystack - from contract)
  - onboarding_status: "in_progress"
  - onboarding_completed: False
```

---

## 3. Client Business-Owner Onboarding Flow (Client Instance)

### 3.1 Purpose
Guide business owner through configuring their instance to become fully operational.

### 3.2 Complete Onboarding Sequence

| Step | Form Name | Purpose | Mandatory Fields | Next Step |
|------|-----------|---------|------------------|-----------|
| 1 | `OnboardingWelcomeForm` | Welcome message, explain process | None | Step 2 |
| 2 | `OnboardingBusinessInfoForm` | Business name, address, contact details | business_name, email, phone, timezone | Step 3 |
| 3 | `OnboardingPaletteForm` | Select color palette | palette_id | Step 4 |
| 4 | `OnboardingCurrencyConfirmForm` | Confirm currency (already set by operator) | None (read-only) | Step 5 |
| 5 | `OnboardingGatewayConfirmForm` | Confirm payment gateway (already set by operator) | None (read-only) | Step 6 |
| 6 | `OnboardingFirstServiceForm` | Create first service | service_name, pricing_model, price/duration | Step 7 |
| 7 | `OnboardingCompletionForm` | Welcome to Mybizz, next steps | None | Dashboard |

### 3.3 Detailed Form Specifications

#### Step 1: OnboardingWelcomeForm

**Purpose:** Welcome message and orientation

**UI Components:**
- `Heading` (headline-large): "Welcome to Mybizz CS"
- `Text` (body-large): "Let's set up your business in 7 quick steps"
- `ColumnPanel` - Progress indicator (7 steps, step 1 active)
- `Button` (filled): "Get Started"

**Inputs:** None

**Outputs:**
- Sets `business_profile.onboarding_step = 1`
- Navigation: Opens `OnboardingBusinessInfoForm`

**Validations:** None

**Failure mode:** If owner exits, `onboarding_status` remains "in_progress"

---

#### Step 2: OnboardingBusinessInfoForm

**Purpose:** Collect core business information

**UI Components:**
- `Heading` (headline-small): "Business Information"
- `TextBox` - Business name (required)
- `TextBox` - Business email (required, must match owner email)
- `TextBox` - Business phone (required)
- `TextBox` - Business address (required)
- `DropdownMenu` - Timezone (required, IANA timezone list)
- `TextBox` - Website URL (optional)
- `TextBox` - Social media links (optional, one per line)
- `Button` (filled): "Continue"
- `Button` (outlined): "Skip for Now" (goes to step 3)

**Inputs:**
- `business_name` (required, 2-100 chars)
- `business_email` (required, valid email format)
- `business_phone` (required, E.164 format)
- `business_address` (required, street address)
- `timezone` (required, IANA timezone string)
- `website_url` (optional, URL format)
- `social_links` (optional, newline-separated URLs)

**Outputs:**
- Updates `business_profile` table:
  - `business_name`
  - `business_email`
  - `business_phone`
  - `business_address`
  - `timezone`
  - `website_url`
  - `social_links`
- Sets `business_profile.onboarding_step = 2`
- Navigation: Opens `OnboardingPaletteForm`

**Validations:**
- All required fields must be present
- Email must match owner account email
- Phone must be valid E.164 format
- Timezone must be valid IANA string

**Failure mode:**
- If validation fails, show inline errors
- If owner exits, data saved but `onboarding_status = "in_progress"`

---

#### Step 3: OnboardingPaletteForm

**Purpose:** Select brand color palette

**UI Components:**
- `Heading` (headline-large): "Choose Your Color Palette"
- `Text` (body-large): "Select a color scheme that fits your business"
- `FlowPanel` - 4 palette buttons (Professional Blue, Warm Coral, Calm Teal, Sophisticated Purple)
- `ColumnPanel` - Preview panel (hidden initially, shows selected palette preview)
- `Button` (filled): "Continue with This Palette"
- `Text` (caption): "You can change this later in Settings"

**Inputs:**
- `palette_id` (required, one of: `palette_professional`, `palette_warm`, `palette_calm`, `palette_sophisticated`)

**Outputs:**
- Updates `business_profile`:
  - `palette = palette_id`
  - `palette_updated_at = datetime.now()`
- Sets `business_profile.onboarding_step = 3`
- Navigation: Opens `OnboardingCurrencyConfirmForm`

**Validations:**
- Must select one palette

**Failure mode:**
- If owner exits without selecting, must select before continuing
- Data saved if owner exits after selection

---

#### Step 4: OnboardingCurrencyConfirmForm

**Purpose:** Confirm currency (already set by operator during provisioning)

**UI Components:**
- `Heading` (headline-small): "Confirm Your Currency"
- `Text` (body-medium): "Your system currency has been set during instance setup"
- `Text` (title-medium, bold): Currency symbol (e.g., "$ USD", "R ZAR", "£ GBP")
- `Text` (caption): "This cannot be changed after your first transaction"
- `Text` (caption): "You can add display currencies later for multi-currency support"
- `Button` (filled): "Confirm Currency"

**Inputs:** None (read-only confirmation)

**Outputs:**
- Sets `business_profile.onboarding_step = 4`
- Navigation: Opens `OnboardingGatewayConfirmForm`

**Validations:** None (informational only)

**Failure mode:**
- Owner can skip this step (button navigates to next step)
- No data changes

---

#### Step 5: OnboardingGatewayConfirmForm

**Purpose:** Confirm payment gateway (already set by operator during provisioning)

**UI Components:**
- `Heading` (headline-small): "Confirm Payment Gateway"
- `Text` (body-medium): "Your payment gateway has been configured during instance setup"
- `Text` (title-medium, bold): Gateway name (e.g., "Stripe" or "Paystack")
- `Text` (caption): "This cannot be changed after your first transaction"
- `Text` (caption): "You can add API keys in the Vault later"
- `Button` (filled): "Confirm Gateway"

**Inputs:** None (read-only confirmation)

**Outputs:**
- Sets `business_profile.onboarding_step = 5`
- Navigation: Opens `OnboardingFirstServiceForm`

**Validations:** None (informational only)

**Failure mode:**
- Owner can skip this step (button navigates to next step)
- No data changes

---

#### Step 6: OnboardingFirstServiceForm

**Purpose:** Create first service to enable bookings

**UI Components:**
- `Heading` (headline-small): "Create Your First Service"
- `Text` (body-medium): "Add your first service to start accepting bookings"
- `TextBox` - Service name (required)
- `DropdownMenu` - Pricing model (required): `duration` or `unit`
- `FlowPanel` - Conditional inputs based on pricing model:
  - If `duration`: `TextBox` - Duration (minutes, required), `TextBox` - Price (required)
  - If `unit`: `TextBox` - Price (required)
- `TextArea` - Service description (optional)
- `DropdownMenu` - Category (optional, from `blog_categories`)
- `FileLoader` - Service image (optional)
- `DropdownMenu` - Meeting type (required): `In-Person`, `Video Call`, `Phone Call`
- `Button` (filled): "Create Service & Continue"
- `Button` (outlined): "Skip for Now" (goes to completion)

**Inputs:**
- `service_name` (required, 2-100 chars)
- `pricing_model` (required): `duration` or `unit`
- `duration` (required if pricing_model=`duration`, integer, 5-480 minutes)
- `price` (required, decimal, > 0)
- `description` (optional, max 2000 chars)
- `category_id` (optional, link to `blog_categories`)
- `image` (optional, media)
- `meeting_type` (required): `In-Person`, `Video Call`, `Phone Call`
- `is_active` (optional, default True)

**Outputs:**
- Creates new row in `services` table:
  - `name`
  - `pricing_model`
  - `duration` (if applicable)
  - `price`
  - `description`
  - `category_id`
  - `image`
  - `meeting_type`
  - `is_active`
- Sets `business_profile.onboarding_step = 6`
- Navigation: Opens `OnboardingCompletionForm`

**Validations:**
- Service name required
- Pricing model required
- Duration required if pricing_model=`duration`
- Price required, must be > 0
- Meeting type required

**Failure mode:**
- If validation fails, show inline errors
- If owner skips, sets `services_count = 0` in business_profile

---

#### Step 7: OnboardingCompletionForm

**Purpose:** Welcome to Mybizz, show next steps

**UI Components:**
- `Heading` (headline-large): "You're All Set!"
- `Text` (body-large): "Welcome to Mybizz CS"
- `ColumnPanel` - Summary of what was configured:
  - Business name
  - Palette selected
  - Currency confirmed
  - Gateway confirmed
  - Services created (count)
- `ColumnPanel` - Next steps:
  - "Add more services"
  - "Set up your availability"
  - "Invite team members"
  - "Customize your website"
- `Button` (filled): "Go to Dashboard"
- `Button` (outlined): "Watch Tutorial"

**Inputs:** None

**Outputs:**
- Sets `business_profile.onboarding_status = "completed"`
- Sets `business_profile.onboarding_completed_at = datetime.now()`
- Navigation: Opens `DashboardForm`

**Validations:** None

**Failure mode:**
- If owner exits, `onboarding_status` remains "completed"
- Owner can re-enter onboarding via Settings → Onboarding

---

### 3.4 Data State Transitions

**Initial state (after operator provisioning):**
```
business_profile:
  - onboarding_status: "in_progress"
  - onboarding_step: 0
  - onboarding_completed: False
  - onboarding_completed_at: null
  - currency: (set by operator)
  - payment_gateway: (set by operator)
```

**After each step:**
- Step 2: `onboarding_step = 2`, business info saved
- Step 3: `onboarding_step = 3`, palette saved
- Step 4: `onboarding_step = 4`, currency confirmed
- Step 5: `onboarding_step = 5`, gateway confirmed
- Step 6: `onboarding_step = 6`, first service created
- Step 7: `onboarding_status = "completed"`, `onboarding_step = 7`, `onboarding_completed = True`

**Final state:**
```
business_profile:
  - onboarding_status: "completed"
  - onboarding_step: 7
  - onboarding_completed: True
  - onboarding_completed_at: 2026-XX-XX XX:XX:XX
  - business_name: "Business Name"
  - business_email: "owner@business.com"
  - business_phone: "+1234567890"
  - business_address: "123 Main St"
  - timezone: "America/New_York"
  - palette: "palette_professional"
  - currency: "USD"
  - payment_gateway: "Stripe"
  - services_count: 1
```

---

## 4. User Signup Flow (Separate from Onboarding)

### 4.1 Purpose
Add additional users to an already-onboarded instance

### 4.2 Forms

1. **`SignupForm`** - End-user registration
   - Inputs: Business email, password, confirm password
   - Output: Creates user in `users` table with Customer role
   - Navigation: `LoginForm`

2. **`InviteUserForm`** - Owner/Manager invites team member
   - Inputs: Invitee email, role (Manager/Admin/Staff)
   - Output: Creates user in `users` table, sends invite email
   - Navigation: `UserManagementForm`

3. **`LoginForm`** - User authentication
   - Inputs: Email, password
   - Output: Authenticates user, creates session
   - Navigation: `DashboardForm` (if Owner/Manager/Admin/Staff) or `CustomerPortalForm` (if Customer)

---

## 5. Failure Modes and Recovery

### 5.1 Partial Completion Handling

| Scenario | Behavior | Recovery |
|----------|----------|----------|
| Owner exits mid-onboarding | `onboarding_status = "in_progress"`, data saved | Owner can re-enter from Settings → Onboarding |
| Owner completes onboarding, then exits | `onboarding_status = "completed"` | Can re-enter via Settings → Onboarding (shows "Resume" or "Replay") |
| Validation fails at step N | Step N not incremented, errors shown | Fix errors, continue |
| Owner skips a step | Step completed with defaults, step incremented | Can edit later in Settings |

### 5.2 Re-entry Rules

- **Before completion:** Owner can re-enter at any time, resumes at last completed step
- **After completion:** Owner can re-enter to edit settings (shows "Replay" mode)
- **Currency/gateway:** Cannot be changed after onboarding (immutable after first transaction)
- **Palette:** Can be changed anytime via Settings

### 5.3 Resumability

- All form data is saved to `business_profile` table
- `onboarding_step` tracks current position
- `onboarding_status` tracks overall state
- Owner can resume from Settings → Onboarding → "Resume Onboarding"

---

## 6. Open Architectural Decisions

### 6.1 Decision: Currency Selection

**Current state:** Currency is set by operator during provisioning (immutable after first transaction)

**Open questions:**
1. Should operator set currency, or should owner select during onboarding?
2. Should currency be changeable after onboarding (with restrictions)?

**Recommended default:**
- Operator sets currency during provisioning (from sales contract)
- Owner confirms during onboarding (read-only)
- Currency immutable after first transaction (as per platform-overview.md)

**Rationale:**
- Aligns with sales contract
- Prevents currency confusion
- Matches existing documentation

---

### 6.2 Decision: Payment Gateway Selection

**Current state:** Gateway is set by operator during provisioning

**Open questions:**
1. Should operator set gateway, or should owner select during onboarding?
2. Should gateway be changeable after onboarding?

**Recommended default:**
- Operator sets gateway during provisioning (from sales contract)
- Owner confirms during onboarding (read-only)
- Gateway immutable after first transaction

**Rationale:**
- Aligns with sales contract
- Prevents payment processing issues
- Matches existing documentation

---

### 6.3 Decision: Onboarding vs Settings

**Current state:** Palette can be changed in both Onboarding and Settings

**Open questions:**
1. Should onboarding be strictly for initial setup only?
2. Should all onboarding steps be editable in Settings after completion?

**Recommended default:**
- Onboarding is for initial setup only
- All onboarding data is editable in Settings after completion
- Settings has dedicated sections: Business Info, Palette, Currency/Gateway (read-only), Services

**Rationale:**
- Clear separation of concerns
- Allows owner to reconfigure after onboarding
- Prevents onboarding from becoming a one-time-only process

---

### 6.4 Decision: Premium vs Standard Tier

**Current state:** Two onboarding tiers exist (Standard: guided call, Premium: Mybizz handles setup)

**Open questions:**
1. How does Premium tier onboarding differ from Standard?
2. Does Premium tier owner still complete onboarding, or does Mybizz do it for them?

**Recommended default:**
- **Standard tier:** Owner completes full onboarding flow
- **Premium tier:** Mybizz operator completes business info and service setup during provisioning, owner only does palette selection and confirmation steps

**Rationale:**
- Distinguishes the two tiers
- Premium tier saves owner time
- Still requires owner confirmation for currency/gateway

---

## 7. Canonical V1 Onboarding Flow

### 7.1 Summary

```
Operator Provisioning (Mybizz_management)
├── Create instance shell
├── Create Owner account
├── Set currency (from contract)
├── Set payment gateway (from contract)
└── Send welcome email with login credentials

Client Onboarding (Client instance)
├── Step 1: Welcome (OnboardingWelcomeForm)
├── Step 2: Business info (OnboardingBusinessInfoForm)
├── Step 3: Palette selection (OnboardingPaletteForm)
├── Step 4: Currency confirmation (OnboardingCurrencyConfirmForm)
├── Step 5: Gateway confirmation (OnboardingGatewayConfirmForm)
├── Step 6: First service (OnboardingFirstServiceForm)
└── Step 7: Completion (OnboardingCompletionForm) → Dashboard

User Signup (Separate, anytime after onboarding)
├── SignupForm (end-user registration)
├── InviteUserForm (owner/manager invites)
└── LoginForm (authentication)
```

### 7.2 Required Forms (Complete List)

**Mybizz_management (Operator Provisioning):**
1. `InstanceProvisionForm`
2. `ProvisionConfirmationForm`

**Client Instance (Business-Owner Onboarding):**
3. `OnboardingWelcomeForm`
4. `OnboardingBusinessInfoForm`
5. `OnboardingPaletteForm`
6. `OnboardingCurrencyConfirmForm`
7. `OnboardingGatewayConfirmForm`
8. `OnboardingFirstServiceForm`
9. `OnboardingCompletionForm`

**Client Instance (User Management):**
10. `SignupForm`
11. `InviteUserForm`
12. `LoginForm`

### 7.3 Wireframes Required

**Missing wireframes (must be created):**
1. `wireframe-onboarding-OnboardingWelcomeForm.html`
2. `wireframe-onboarding-OnboardingBusinessInfoForm.html`
3. `wireframe-onboarding-OnboardingCurrencyConfirmForm.html`
4. `wireframe-onboarding-OnboardingGatewayConfirmForm.html`
5. `wireframe-onboarding-OnboardingFirstServiceForm.html`
6. `wireframe-onboarding-OnboardingCompletionForm.html`
7. `wireframe-management-InstanceProvisionForm.html`
8. `wireframe-management-ProvisionConfirmationForm.html`

**Existing wireframes (already created):**
- `wireframe-auth-SignupForm.html`
- `wireframe-auth-LoginForm.html`
- `wireframe-custom-component-OnboardingPaletteForm.html`

---

## 8. Data Table Changes Required

### 8.1 business_profile Table Additions

| Column | Type | Required | Default | Notes |
|--------|------|----------|---------|-------|
| `onboarding_status` | Text | Yes | "in_progress" | "in_progress" or "completed" |
| `onboarding_step` | Integer | Yes | 0 | Current step (0-7) |
| `onboarding_completed` | True/False | Yes | False | Overall completion status |
| `onboarding_completed_at` | Date/Time | No | null | When onboarding completed |
| `business_name` | Text | No | null | From Step 2 |
| `business_email` | Text | No | null | From Step 2 |
| `business_phone` | Text | No | null | From Step 2 |
| `business_address` | Text | No | null | From Step 2 |
| `timezone` | Text | No | null | From Step 2 |
| `website_url` | Text | No | null | From Step 2 |
| `social_links` | Text | No | null | From Step 2 |
| `palette` | Text | No | null | From Step 3 |
| `palette_updated_at` | Date/Time | No | null | When palette last updated |
| `currency` | Text | No | null | From operator provisioning |
| `payment_gateway` | Text | No | null | From operator provisioning |
| `services_count` | Integer | No | 0 | From Step 6 |

### 8.2 New Tables Required

**onboarding_tokens** (for operator-to-owner communication)
| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `id` | Row ID | Auto | Primary key |
| `instance_id` | Link to users | Yes | Owner user |
| `token` | Text | Yes | Unique token |
| `created_at` | Date/Time | Auto | When created |
| `expires_at` | Date/Time | Yes | Token expiration |
| `used` | True/False | Yes | Whether token used |

---

## 9. Validation Rules Summary

### 9.1 Required Validations

| Form | Field | Validation |
|------|-------|------------|
| OnboardingBusinessInfoForm | business_name | Required, 2-100 chars |
| OnboardingBusinessInfoForm | business_email | Required, valid email, must match owner email |
| OnboardingBusinessInfoForm | business_phone | Required, E.164 format |
| OnboardingBusinessInfoForm | business_address | Required, non-empty |
| OnboardingBusinessInfoForm | timezone | Required, valid IANA timezone |
| OnboardingPaletteForm | palette_id | Required, one of 4 palettes |
| OnboardingFirstServiceForm | service_name | Required, 2-100 chars |
| OnboardingFirstServiceForm | pricing_model | Required, duration or unit |
| OnboardingFirstServiceForm | duration | Required if pricing_model=duration, 5-480 mins |
| OnboardingFirstServiceForm | price | Required, decimal > 0 |
| OnboardingFirstServiceForm | meeting_type | Required, In-Person/Video Call/Phone Call |

---

## 10. Next Steps

### 10.1 Immediate Actions Required

1. **Create missing wireframes** (8 forms)
2. **Add new columns to business_profile table** (17 columns)
3. **Create onboarding_tokens table**
4. **Implement server-side validation logic** for each form
5. **Create navigation logic** between onboarding steps
6. **Create Settings integration** for post-onboarding editing

### 10.2 Dependencies

- **Mybizz_management app** must be built before operator provisioning can be implemented
- **business_profile table schema** must be updated before onboarding can be implemented
- **Authentication system** must be in place before user signup can be implemented

### 10.3 Open Questions to Resolve

1. **Currency/gateway selection**: Should operator set or owner select? (Recommended: operator sets)
2. **Premium tier onboarding**: How does it differ from Standard? (Recommended: operator does more)
3. **Re-entry after completion**: Should owner be able to replay onboarding? (Recommended: yes, as "Replay" mode)
4. **Onboarding token expiration**: How long should tokens be valid? (Recommended: 7 days)
5. **Partial completion cleanup**: Should incomplete onboarding data be deleted after X days? (Recommended: no, keep for recovery)

---

## 11. Final Conclusion

This plan defines a complete, implementation-ready onboarding system for Mybizz CS that:

1. **Distinguishes three separate flows**: operator provisioning, client onboarding, user signup
2. **Defines all 12 required forms** with exact inputs, outputs, validations, and navigation
3. **Maps data/state transitions** for each onboarding step
4. **Handles failure modes** with resumability and recovery
5. **Identifies open decisions** with recommended defaults
6. **Aligns with existing architecture** (dependency-based master_template, separate client instances)

**The canonical V1 onboarding flow is:**
- Operator provisions instance shell (currency + gateway set from contract)
- Owner completes 7-step onboarding (business info, palette, confirm currency/gateway, create first service)
- Owner completes onboarding → Dashboard
- Owner can invite team members anytime
- Owner can edit onboarding data in Settings after completion

**This plan is ready for implementation pending resolution of 5 open architectural decisions.**
