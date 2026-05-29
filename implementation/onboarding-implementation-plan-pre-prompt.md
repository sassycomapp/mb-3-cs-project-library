You’re right to push for “work once and work right” here. Below is a clean list of ADRs plus a concrete change-log of what will happen to the onboarding implementation plan, based on everything so far. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

## ADRs arising so far

These are decisions that either change current documented architecture or introduce new system behaviour, so they need ADRs.

1. **ADR: Payment gateway mutability**

- Current architecture: Each client instance uses one gateway (Stripe or Paystack), configured at onboarding; this is documented in the platform overview. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Proposed change: Allow the Owner to change the configured payment gateway later (subject to RBAC and any financial constraints), not only at onboarding. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Why ADR: This alters a documented invariant (“one gateway per instance configured at onboarding”) into a lifecycle rule (“one active gateway at a time, but can be switched under rules”). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)

2. **ADR: Onboarding finality (no replay, no in-app edits)**

- Current onboarding plan: Allows re-entering onboarding after completion and suggests a “Replay” mode with Settings → Onboarding. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Proposed decision: Onboarding is a **one-time** process. Once onboarding is completed, onboarding-specific data is locked and cannot be changed or replayed through the onboarding flow. Any rare changes must go through Mybizz (e.g., via support email) under defined policy, not via a replay UI. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Why ADR: This directly overturns the current “replay/edit via Settings” behaviour described in the onboarding plan and has implications for UX, data model, and support processes. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

3. **ADR: System vs Display Currency behaviour (clarified onboarding responsibility)**

- Current docs: System Currency is set at onboarding and immutable after the first transaction; Display Currency is optional and changeable. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- Clarified decision:  
  - System Currency is explicitly selected by the Owner during onboarding (not by Mybizz staff), with clear wording that it is permanent after first transaction. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
  - Display Currency configuration is explicitly a Settings/Dashboard responsibility, not part of onboarding. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Why ADR: The platform overview is consistent on immutability but not explicit about **who** sets System Currency; the onboarding plan currently assumes operator-set; this ADR locks the Owner-set behaviour and separates responsibilities. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)

4. **ADR: Role vocabulary and management-side responsibilities**

- Current state: Nomenclature file defines Client, Owner, Manager, Admin, Staff, Customer as mandatory terminology. The onboarding plan uses “operator provisioning” and “Mybizz operator”, terms not defined in nomenclature. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- Decision:  
  - Onboarding and instance creation documentation must use only roles defined in the nomenclature file, plus the application name `Mybizz_management` when referring to the internal tool. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
  - Any role-like concept used in onboarding (e.g., who performs instance setup) must be mapped to an existing role (most likely a Mybizz internal operator role defined via RBAC in the Mybizz_management app), or a new internal role must first be added to the nomenclature authority via a separate change. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- Why ADR: This is a governance decision: nomenclature is authoritative; the onboarding plan is being brought under that authority, which prevents future drift.

5. **ADR: Onboarding vs Settings boundary**

- Current onboarding plan: Suggests onboarding data is editable later in Settings and that onboarding can be “replayed”. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Proposed decision:  
  - Onboarding collects only data that can legitimately remain static for the lifetime of the client instance (system currency, legal KYC basics, etc.). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
  - Ongoing operational settings (palette, services, display currency, website content, etc.) are explicitly handled in Settings or other admin forms, not via onboarding. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Why ADR: This is a sharpened architectural boundary compared to the current plan and affects how many fields are stored as “onboarding data” vs regular settings.

6. **ADR: Onboarding acknowledgement of legal responsibility for policy pages**

- Current docs: Platform overview states that public pages include Privacy Policy and Terms & Conditions; it does not define how those are authored or who is responsible. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Decision:  
  - The Owner is explicitly responsible for their own Privacy Policy and Terms & Conditions content.  
  - The onboarding implementation **does not** build the clause-builder system; instead, onboarding includes an explicit acknowledgement checkbox that the Owner understands they must go to Settings → Privacy Policy and Settings → Terms & Conditions to configure their own documents. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
  - Mybizz records this acknowledgement as part of onboarding state (for audit/compliance) but does not enforce completion of the content. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Why ADR: This sets a clear responsibility boundary (Mybizz vs client) and introduces the legal-compliance acknowledgement as a platform behaviour. The clause-builder feature itself is a separate system design and would likely have its own ADR later.

7. **ADR: Gateway configuration responsibility (Mybizz vs Owner)**

- Current onboarding plan: Gateway is set by “operator” during provisioning and Owner only confirms read-only. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Your proposal: Gateway selection and registration is part of the Owner’s onboarding / ongoing management (via a “Payment Gateway” form/button), not a Mybizz provisioning task. Mybizz may pre-select a default in the contract, but actual operational configuration (API keys in The Vault, choice of gateway where both are possible) is the Owner’s responsibility. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Why ADR: This shifts payment responsibility and configuration steps between Mybizz_management and the client instance and interacts with ADR-003 (Payment Security Boundary). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)

8. **ADR: Onboarding data schema and `business_profile` / `businessprofile`**

- Current onboarding plan: Proposes a `business_profile` table with specific onboarding-related columns, including `instance_id` and various onboarding state fields. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Your decision:  
  - Table name and field names must match `authoritative-schema.md` (e.g., `businessprofile` rather than `business_profile`), and `instance_id` should not appear on the client-side profile solely as a cross-app reference. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
  - Only the minimal onboarding state columns required for resumability and completion tracking should be added; anything else should be consistent with the authoritative schema design decisions, not redefined ad hoc in the onboarding plan. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Why ADR: Changes to the schema, especially to core tables like `businessprofile`, are architectural decisions and must be captured, not drift in via one plan.

9. **ADR: Removal of “User Signup Flow” from onboarding spec**

- Current onboarding plan: Embeds `SignupForm`, `InviteUserForm`, and `LoginForm` as part of “Onboarding System – Engineering Plan”. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Decision:  
  - Onboarding implementation plan is scoped strictly to client instance creation and Owner onboarding.  
  - End-customer signup and general authentication (`SignupForm`, `LoginForm`, etc.) are governed by the Authentication & Administration design, not by onboarding. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Why ADR: This is a scope and document-ownership change: it keeps onboarding specialised and avoids the plan becoming a dumping-ground for unrelated flows.

If you prefer, some of these can be combined into a smaller number of ADRs (e.g., one ADR for “Onboarding Responsibilities & Boundaries” covering points 2, 3, 5, 6, 9), but logically these are the distinct decisions in play.

## Edits to the onboarding implementation plan

Here is what will actually change in `onboarding-implementation-plan.md` when we realign it.

### 1. Nomenclature and role language

- Replace generic “user” in onboarding context with the correct term: **Owner** (the client instance owner), and use **Client** when referring to the business entity subscribing to Mybizz. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- Remove the term “operator” entirely. Where the plan describes tasks in `Mybizz_management`, refer to the **Owner** in the context of signing up as a Client, and to internal Mybizz roles only if those roles have been added to the nomenclature file; otherwise the plan will describe actions generically as “Mybizz performs X in Mybizz_management” without inventing a role name. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- Ensure all user-role references match the RBAC roles in platform-overview: Owner, Manager, Admin, Staff, Customer. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)

### 2. Three flows – architecture but with corrected scope

The plan will still document three conceptual flows, but:

- **Client onboarding (Owner)**: Remains the central subject. This is the guided process inside the client instance that turns a blank instance into a live business setup. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- **Mybizz_management provisioning**: Will be documented only at a high level as a precondition (“Mybizz creates a client instance and Owner account in Mybizz_management”), not in deep detail; detailed provisioning forms may move to a separate Mybizz_management plan. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- **User signup (Customers, Staff, etc.)**: Removed from the onboarding plan and referenced only as “handled by the Authentication & Administration system”. No detailed forms here for Signup, Login, InviteUser. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

### 3. Onboarding steps – simplified and aligned with your intent

The 7-step sequence will be adjusted:

- **Step 1: Welcome** – Keep, but terminology updated (Owner, Client). No change in essence. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- **Step 2: Business Info** – Extended to include:
  - Client’s business tax number / relevant registration IDs.  
  - A selector for “signing up as Individual / Business” plus the understanding that Mybizz is providing a platform only; KYC fields are captured for Mybizz’s own client verification. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- **Palette selection** – Removed as a dedicated onboarding form. Instead:
  - Onboarding uses the **PaletteSettingsForm** custom component inside the main onboarding form or defers this entirely to Settings/Dashboard. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/99663a55-cf2a-412e-a6a4-53020b314264/wireframe-custom-component-PaletteSettingsForm-2.html)
- **Currency confirmation** – `OnboardingCurrencyConfirmForm` removed as a separate form:
  - System Currency will be selected (or confirmed if pre-filled) directly in the main onboarding process with clear language about immutability after first transaction. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- **Gateway confirmation** – `OnboardingGatewayConfirmForm` removed as a separate form:
  - Gateway configuration becomes part of a Payment Setup flow (either during onboarding or as an early Settings task), with the Owner responsible for choosing and configuring the gateway, consistent with Payment Security and The Vault usage. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- **First service** – This step may remain as an optional “create first service” within onboarding or be downgraded to a recommended next step; blog-related fields and `blog_categories` are removed from this form definition. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- **Completion** – Still includes a summary, but:
  - Removes the “Replay onboarding” language.  
  - Adds explicit acknowledgement items (see below) and makes onboarding state final. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

The plan will also remove or rephrase “skip for now” options where they conflict with the idea that onboarding is short, non-replayable, and for key static data.

### 4. Onboarding state model

- Keep a minimal set of onboarding state fields on `businessprofile` (or whatever `authoritative-schema.md` says) such as:
  - `onboarding_status` (in_progress / completed)  
  - `onboarding_completed_at`  
  - Possibly a simple `onboarding_version` or similar. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- Remove `instance_id` from the client-side profile table, since each instance is already a separate app and doesn’t need that field just to be “linked” back to Mybizz_management. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Remove fine-grained `onboarding_step` if you decide the flow does not need resumability at sub-step level; or keep it only if resumability is still required. Either way, this will be explicitly explained and aligned with the decision about replay vs non-replay. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

### 5. Currency and gateway responsibility

- System Currency:
  - Documented as being selected by the Owner during onboarding (with possibly a default pre-filled based on contract), immutable after first transaction. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
  - Display Currency: specifically excluded from onboarding; handled in Settings / Dashboard. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- Payment Gateway:
  - The old text “set by operator during provisioning” is removed.  
  - The plan notes that the Owner configures the gateway (Stripe or Paystack) via a Payment Gateway setup flow and stores API keys in The Vault; Anvil Secrets contains only `encryption_key` controlled by Mybizz, never by the Client. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)

### 6. Privacy Policy / Terms acknowledgement in onboarding

- Onboarding will not include the clause-builder UI and will not require that these pages be fully completed. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- Instead, the onboarding completion step will include:
  - A **checkbox**: “I understand that I must configure my own Privacy Policy and Terms & Conditions in Settings; Mybizz does not provide legal advice and I am responsible for the content of those pages.”  
  - Possibly two information lines referencing where to find these settings (e.g., Settings → Website → Privacy Policy / Terms & Conditions). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
- The fact that the Owner ticked this is stored (in `businessprofile` or a dedicated audit table) as an acknowledgement record. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

### 7. Removal of User Signup section

- Section “4. User Signup Flow (Separate from Onboarding)” will be removed from the onboarding plan and either:
  - replaced with a short note “Authentication & user signup flows are defined in the Authentication & Administration design”, or  
  - moved to a separate auth-focused document. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)

### 8. Tier and onboarding support model

- The confusing “Standard vs Premium” onboarding tiers described in the plan will be replaced with the business-model reality:
  - Tiers: Trial, Founder, Growth, Business, etc., differing by price and grandfathering; onboarding itself is the **same** for all tiers. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abad8111-d4de-45c1-8734-acc5f871cbd8/platform-overview.md)
  - Separate from tiers, there is a **service** dimension: self setup (with included 30-minute interactive session) vs $100 DFY setup. This is not a different software onboarding flow; it is an operational support offering. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)
- The plan will note this and avoid blending tier pricing with onboarding flow differences.

### 9. Tagging each item by status

Throughout the rewritten plan, each substantial element (form, field set, state rule) will be annotated as:

- **Confirmed** – from `platform-overview.md`, `nomenclature-2.md`, and authoritative schema. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/aebc24b8-0002-44f2-852c-25925b702eef/nomenclature-2.md)
- **Inferred** – logically derived but not explicitly in docs (e.g., some validation rules).  
- **Proposed** – changes or additions requiring ADRs (gateway mutability, onboarding finality, etc.). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9a9d2162-75da-43c6-856a-bbb1a112aa33/onboarding-implementation-plan.md)

This is crucial to keep the onboarding plan from silently modifying architecture.

***

If this matches your understanding, the next step is to turn this into a `/document-release` prompt that:

- instructs the agent to refactor `onboarding-implementation-plan.md` using the above edits,  
- lists the ADRs it must call out, and  
- enforces strict use of nomenclature and source tagging.

Before that, is there **any** part of the above that you would tighten further, especially around (a) including or excluding the “first service” step in onboarding, and (b) whether resumability (`onboarding_step`) is still required or you’re comfortable with “all or nothing” onboarding completion?