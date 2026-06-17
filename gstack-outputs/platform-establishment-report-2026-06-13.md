# Mybizz CS — Platform Establishment Report
## Anvil Account Structure, App Architecture, Provisioning, and Update Model

**Date:** 2026-06-13  
**Scope:** All decisions, findings, investigations, rationale, and unresolved items relating to the Anvil platform setup, app structure, client instance provisioning, and update model.  
**Status:** Working document — some items confirmed, some pending test, some unresolved.

---

## Table of Contents

1. Background and Starting Point
2. Anvil Account Structure — The Decision
3. Anvil Plan Analysis and Billing Model
4. The Five-App Structure
5. The Dependency Model — How It Actually Works
6. The Critical Architectural Test — app_tables Resolution
7. Client Instance Architecture — What a Client Instance Must Contain
8. The Code Precedence Problem — Why Client Instances Must Be Code-Free
9. The Update Model — How Changes Flow from Development to Clients
10. blank_client_template — Purpose, Contents, and Responsibilities
11. Client Instance Provisioning — The Full Activation Process
12. The Free Trial — Decision and Replacement
13. Anvil's Capabilities Relevant to This Architecture
14. Anvil's Shortcomings and How They Are Overcome
15. Items Confirmed by Live Test
16. Items Requiring Further Testing
17. Unresolved Items
18. Document Registry

---

## 1. Background and Starting Point

### What Was Known at the Start of This Session

The platform was described as follows at the outset of this planning session:

- A SaaS business management web application for the consulting and services vertical
- Built as a template and hosted on the mybizz Anvil account
- Clients receive a "cloned version as a dependency" — each client's instance hosted on a separate Anvil account
- Code lives on the central template; all data is in the client instance
- Updates are made to the template; notifications are sent to clients who pull updates as required
- A separate mybizz management app manages clients
- Integrations: Stripe, Paystack, Brevo
- Solo developer

### What Was Wrong or Unclear

Several elements of the above description were either incorrect, imprecise, or not fully thought through:

- The term "cloned version as a dependency" conflated two separate Anvil mechanisms (clone **** sex you know  dependency) that behave very differently
- "Each client's instance hosted on a separate Anvil account" was an assumption that had significant operational consequences not yet evaluated
- Whether `app_tables` in a dependency's server module resolves to the client instance's tables or the dependency's own tables had never been tested — it was a foundational assumption the entire architecture rested on
- The update pull model ("clients pull updates when required") had no defined mechanism — how a client would actually trigger an update had never been resolved
- The 30-day free trial was a notion, not a designed feature
- The nomenclature used in conversation (host app, dependency, template, clone) was inconsistent, causing significant confusion

This report documents how each of these was investigated, corrected, and resolved.

---

## 2. Anvil Account Structure — The Decision

### Original Assumption

The original model assumed each client would have their own separate Anvil account, owned and managed by mybizz. When a new client signed up, mybizz would purchase a new annual Anvil subscription for that client. The client would be unaware of Anvil entirely.

At 100 clients this means:
- 100 separate Anvil accounts
- 100 separate billing relationships
- 100 separate IDEs to log into for any management action
- Pushing a single update requires logging into 100 accounts and manually changing the dependency version in each

### Why This Was Rejected

The operational consequence of 100 separate accounts was examined in the context of updates. Anvil has no provisioning API — there is no way to change an app's dependency version programmatically. It must be done manually in the Anvil IDE, in each app's settings, one by one. At 100 clients, one update release means 100 manual logins and 100 manual actions.

Additionally, the management application (Mybizz_management) needs to read data from and send instructions to client instances. Cross-account communication in Anvil has no native mechanism — it would require HTTP endpoint calls between accounts, with the management application needing to maintain credentials for 100 separate accounts.

### The Decision — One Account, Multiple Apps

All client instances, all platform apps, and the management app are hosted on a **single mybizz Anvil account** as separate applications.

The client experience is identical either way — they receive a URL, they log in, they see their application. They are never aware of Anvil and never have IDE access.

### Consequences of This Decision

- One Anvil account to manage, one IDE, one billing relationship
- All apps visible from one place — provisioning, monitoring, and management are all in the same IDE
- Dependency version changes for any client app are done from the same account without logging out
- Mybizz_management can communicate with client instances via HTTP endpoints — all within the same account
- Resource limits (Data Table rows, storage, compute) apply at the account level and are shared across all apps — this has implications discussed in Section 3
- Anvil's per-app monitoring tools (logs, CPU, memory) are accessible for any client app without switching accounts

---

## 3. Anvil Plan Analysis and Billing Model

### The Original Cost Estimate

The platform-overview.md contained a direct cost figure of $18,000/year for Anvil hosting at 100 clients. This figure was based on the original per-account model — approximately 100 accounts at roughly $180/client/year.

### Plan Analysis

The following Anvil plans were evaluated against platform requirements:

**Hobby Plan (~$12/month annual):**
- One custom domain — insufficient (100 clients each need their own domain)
- No persistent server — insufficient (degrades performance; server restarts between requests)
- No private GitHub repository support — insufficient (development workflow requires private repos)
- Scheduled tasks: available
- Background task timeout: unlimited
- Verdict: Does not meet minimum requirements

**Business Plan (~$89/month annual, per seat):**
- Unlimited apps — confirmed ✅
- Multiple custom domains — confirmed ✅
- Persistent server — confirmed ✅
- Private GitHub repositories — confirmed ✅
- Scheduled tasks — confirmed ✅
- Background task timeout: unlimited ✅
- Multiple developer seats supported
- 1,000,000 Data Table rows
- 100 GB Data Table storage
- 100,000 outgoing emails per month
- Verdict: Meets all identified requirements. **This is the required plan.**

**Enterprise Plan:**
- On-site or private cloud deployment
- Enterprise SSO
- Tailored solution — contact for quote
- Verdict: Not required for this scale or use case

### The Billing Reality

One mybizz Anvil Business plan account at ~$89/month covers the entire platform — mb-3-cs, master_template, blank_client_template, Mybizz_management, and all 100 client instances. This is a significant reduction from the original $18,000/year estimate.

The original $18,000/year figure in platform-overview.md must be revised once the row/storage limit question below is answered.

### The Unresolved Row and Storage Limit Question

The Business plan states 1,000,000 Data Table rows and 100 GB storage. The pricing page does not explicitly state whether these limits apply per account (shared across all apps) or per app (each app gets the full allocation).

This distinction matters significantly:

- **If per app:** Each of 100 client instances gets 1,000,000 rows and 100 GB — no issue at any foreseeable scale
- **If per account:** 1,000,000 rows shared across all apps. At 100 clients: 10,000 rows per client instance across 36 tables. For an active consulting business managing bookings, contacts, invoices, emails, audit logs, and campaign data over multiple years, 10,000 rows total is potentially limiting.

**Action required:** Email Anvil sales with the specific question: *"On the Business plan with multiple apps on one account — do the Data Table row and storage limits apply per account (shared across all apps) or per app?"*

### Mitigation if Limits Are Per Account

Anvil offers an Extra Server Resources add-on at approximately $599/month (annual). This includes additional compute, additional storage, and an isolated database. Even at $89 + $599 = $688/month total, this is well within viable margins against platform revenue at capacity.

The Extra Server Resources add-on is the contingency if per-account limits prove restrictive at scale. It does not need to be activated at launch — only if/when limits are approached.

---

## 4. The Five-App Structure

### How It Evolved

The architecture began as a perceived two-app structure (template + client instance). Through the course of this session it became clear that the correct structure requires five distinct apps, each with a specific and non-overlapping role.

### The Five Apps

**App 1: mb-3-cs — Development Workspace**

All feature development happens here. Connected to GitHub as the shared source of truth between Anvil and OpenCode. Contains all 36 Data Tables (for development and testing purposes), all UI forms, all server modules, and all application code. mb-3-cs is never a dependency for any client instance. It is the authoring environment, not the publishing environment. Connected to GitHub `develop` branch.

**App 2: master_template — The Published Dependency**

The sole source of all server-side logic AND all UI forms for every client instance. Contains all server modules and all forms. Connected to the same GitHub repository as mb-3-cs but tracking the `stable` branch — they are siblings on the same repo, not a clone relationship. When client instances call `anvil.server.call('function_name')`, the function executes from master_template's server modules. When client instances display a form, that form is served from master_template via the dependency. master_template is updated by merging `develop` → `stable` in GitHub — the app tracks the branch and reflects changes automatically. master_template is never given to clients directly; they depend on it.

**App 3: blank_client_template — Provisioning Clone Source**

A purpose-built, maintained app whose sole function is to serve as the source from which new client instances are cloned. Contains all 36 Data Tables with the correct schema and no data. Contains one startup module — the only client-side code it holds. Has master_template set as a dependency (stable branch) so that the dependency is inherited automatically by every clone. Contains no server modules and no forms — both live in master_template. Is never modified except when the Data Table schema changes, at which point it must be updated so that future client instances get the correct structure at provisioning.

**App 4: [client-name]-cs — Client Instances**

One per client. Cloned from blank_client_template at provisioning. Inherits from the clone: all 36 Data Tables (the client's actual business data) and one startup module. Has master_template set as dependency — active. Contains no server modules and no forms of its own. All UI, all form code, and all server logic come from master_template via the dependency and propagate automatically when master_template is updated. Each client instance is completely isolated from every other — the data in one instance is architecturally inaccessible from another. Client instances are named using a consistent format (e.g. `acmeconsulting-cs`).

**App 5: Mybizz_management — Platform Operator Tooling**

The internal management application. Handles client provisioning automation, health monitoring, billing status, update distribution, and operator controls. This app is deferred — it will be built only after master_template reaches launch readiness and the first client has been manually onboarded. Until it exists, provisioning is manual and monitoring uses Anvil's per-app IDE tools. Mybizz_management communicates with client instances via bidirectional HTTP endpoints (see Section 13 and ADR-033).

### Role Boundaries — Non-Negotiable

| Action | Permitted in |
|---|---|
| Write server module code | mb-3-cs only |
| Write form code and UI | mb-3-cs only |
| Define/modify Data Table schema | mb-3-cs + blank_client_template + all existing client instances (simultaneously) |
| Store client business data | Client instances only |
| Execute server-side logic at runtime | master_template (via dependency) |
| Serve UI forms at runtime | master_template (via dependency) |
| Provision new client instances | Mybizz_management (future) / manual runbook (now) |
| Manage platform operations | Mybizz_management |

---

## 5. The Dependency Model — How It Actually Works

### The Terminology Problem

A significant portion of this session was spent resolving confusion caused by inconsistent use of the following terms: host app, dependency, template, clone, master template. These terms were being used interchangeably in ways that obscured what was actually happening technically.

**Terminology fixed for this project:**

- **mb-3-cs** — the development workspace. Always called this.
- **master_template** — the published dependency. Always called this.
- **client instance** — each client's app. Always called this.
- **blank_client_template** — the provisioning clone source. Always called this.
- The term "host app" is not used — it was causing confusion. In standard Anvil documentation, "host app" refers to the app that uses a dependency, which in our context is the client instance. This was being confused with master_template. The term is retired.

### What a Dependency Actually Is in Anvil

When App B depends on App A in Anvil:
App = master-template
App B = Client Instance

Server modules from App A are available to App B and can be invoked using anvil.server.call().
Forms from App A can be opened and used within App B.
Code from App A executes within App B’s runtime context. This means references such as app_tables inside App A’s code point to App B’s Data Tables, not App A’s.
App B overrides App A where names overlap — if both apps define a form, function, or other component with the same identifier, App B’s version is used.
Data Tables remain isolated per app — adding App A as a dependency does not expose App A’s Data Tables to App B, and App B’s tables are not automatically available to App A.

### How Client Instances Depend on master_template

Each client instance has master_template set as its dependency. When a user interacts with the client instance's UI, events call `anvil.server.call()`. Because the client instance has no server modules of its own, Anvil finds the function in master_template (the dependency) and executes it there. During that execution, `app_tables.invoice` refers to the client instance's `invoice` table — not master_template's tables.

### Branch Tracking vs Version Pinning

Anvil's dependency system supports two modes of tracking a dependency:

**Branch tracking:** The client instance always runs whatever code is currently on a named branch (e.g. `stable`) of the dependency app. When master_template's `stable` branch is updated (via a GitHub merge), all client instances tracking that branch immediately run the new code.

**Version pinning:** The client instance is pinned to a specific tagged version (e.g. `v1.1.0`) of the dependency. It does not update when the branch changes. The operator must manually change the pinned version to adopt a new release.

The default for this platform is branch tracking on `stable`. Version pinning is available as a staged rollout mechanism for high-risk releases.

### The Update Mechanism Misconception

The correct mental model: when master_template's `stable` branch changes, every client instance tracking that branch already runs the new code the next time any server function is called. There is no update action. The notification email that says "update available" is informational — by the time the client reads it, the update has already happened. The email describes what changed. No client action is required or possible.


---

## 6. The Critical Architectural Test — app_tables Resolution

### The Untested Assumption

The entire architecture rested on the assumption that `app_tables` called from within a master_template server module resolves to the calling client instance's Data Tables. If this assumption was false — if it resolved to master_template's own tables — then the architecture was broken at its foundation. Every feature, every table, every piece of client data storage depended on this being true.

This had never been tested. It was identified as the most important architectural question to verify before any further development.

### The Test — Conducted 2026-06-13

**Setup:**
- `datatable-test-host-app`: A new Anvil app acting as the dependency (equivalent to master_template). Contains a server module with a callable function that calls `app_tables.table_1.add_row(name=name, some_number=number)`. Contains its own `table_1` Data Table.
- `datatable-test-client-app`: A new Anvil app acting as the client instance. Depends on `datatable-test-host-app`. Contains its own `table_1` Data Table with identical schema. Contains a form with a button that calls `anvil.server.call('write_to_table', name, number)`.

**Server module code (in datatable-test-host-app):**
```python
import anvil.server
import anvil.tables as tables
from anvil.tables import app_tables

@anvil.server.callable
def write_to_table(name, number):
    app_tables.table_1.add_row(
        name=name,
        some_number=int(number)
    )
    return "Row written by dependency server module"
```

**Form code (in datatable-test-client-app):**
```python
@handle("btn_write", "click")
def btn_write_click(self, **event_args):
    result = anvil.server.call(
        'write_to_table',
        self.txt_name.text,
        self.txt_number.text
    )
    alert(result)
```

**Result:**
- Row appeared in `datatable-test-client-app`'s `table_1` ✅
- Row did NOT appear in `datatable-test-host-app`'s `table_1` ✅

### What This Confirms

`app_tables` in a dependency's server module resolves to the calling client instance's Data Tables at runtime. This is structural Anvil behaviour — not a configuration choice, not something that can accidentally revert.

**Implications:**
- The architecture is sound at its foundation
- Data isolation between client instances is structural and automatic
- One client's server calls cannot access another client's tables — ever
- No application-level data isolation enforcement is required
- All 36 tables, all server modules, all integrations operate against the correct client's data automatically

This finding has been documented in platform-overview.md Section 7.4 and forms the verified basis of ADR-028.

---

## 7. Client Instance Architecture — What a Client Instance Must Contain

### The Contents of a Client Instance

A correctly structured client instance contains exactly the following:

**One Startup Module**
The only client-side code a client instance holds. Imports and opens the first form from master_template. All subsequent navigation, UI rendering, form event handling, and server calls are served from master_template via the dependency.

```python
from master_template.MainForm import MainForm
from anvil import open_form
open_form(MainForm())
```

**All 36 Data Tables**
The client's complete business data. Bookings, contacts, invoices, services, team members, audit logs, vault secrets, payment configuration — everything. This data never leaves the client instance. It is never accessible from another client instance. The 36 tables come from `blank_client_template` at provisioning (correct schema, no data).

**master_template as Dependency**
Set to the `stable` branch of master_template's GitHub repository at provisioning. This wires the client instance to receive all server-side logic and all UI forms from master_template. The dependency is set once at provisioning and does not change unless a staged rollout requires version pinning.

**Anvil Secrets (one item only)**
The `encryption_key` — a Fernet symmetric key set by mybizz at provisioning. This is the only item ever placed in Anvil Secrets for any client instance. It is used exclusively by `vault_service.py` to encrypt and decrypt secrets stored in the Vault Data Table. It never changes after provisioning.

### What a Client Instance Must Not Contain

**Server modules.** If a client instance contains a server module with a function named `get_invoices`, and master_template also defines `get_invoices`, Anvil runs the client instance's version — not the dependency's. Updates to master_template have no effect on that client. This must never occur.

**Forms.** If a client instance contains its own forms, those forms take precedence over the dependency's forms with the same name. UI updates to master_template would not reach that client. This must never occur.

The only permitted exception to either rule is if a future requirement genuinely needs client-specific logic that cannot be handled through Data Table configuration. This would require a new ADR.

---

## 8. The Code Precedence Problem — Why Client Instances Must Be Code-Free

### The Initial Confusion

The provisioning model originally described was: "I clone master_template and create a client instance." This was presented as the complete provisioning step. The question was raised: does it matter that the client instance has all the code from master_template?

This question is central to whether the entire update model works, and the answer is yes — it matters enormously.

### How Anvil Code Precedence Works

When both a client instance and its dependency (master_template) define a server function with the same name:

1. A call to `anvil.server.call('my_function')` from the client instance's form
2. Anvil first looks for `my_function` in the client instance's own server modules
3. If found there, Anvil runs the client instance's version — the dependency is not consulted
4. If NOT found there, Anvil looks in the dependency (master_template) and runs it there

This means: a client instance cloned from master_template has all of master_template's server functions copied into its own server modules. When any of those functions are called, the client instance runs its own copy. The dependency is invisible. Every update pushed to master_template has zero effect on that client instance.

### Why Cloning master_template Directly Is Wrong

Cloning master_template produces an app with:
- All of master_template's server modules (copied in)
- All of master_template's UI forms (copied in)
- All of master_template's Data Tables (copied in, empty)
- master_template set as a dependency (if added separately)

This clone is now self-contained. It runs its own code. Adding master_template as a dependency on top of this clone changes nothing — the clone's own code always wins. The dependency is decorative.

### Why blank_client_template Solves This

blank_client_template contains no server modules and no forms. When it is cloned:
- The clone has no server modules — Anvil has nothing to find locally
- Every `anvil.server.call()` goes to the dependency (master_template) — the correct behaviour
- Every update to master_template immediately reaches this client instance
- Data Tables are present (copied from blank_client_template's correct schema)
- The clone is immediately ready for the activation process

### Test B — Forms from Dependency (2026-06-13) ✅

A further test established that forms do NOT need to live in the client instance at all. A bare app with only a startup module and no forms of its own successfully loaded and displayed a form defined in its dependency. This was tested using `test-dependency-forms` (containing MainForm) and `test-client-no-forms` (containing only a startup module).

This finding changes the architecture significantly: forms live in master_template, not in client instances. The startup module in each client instance imports and opens the first form from master_template. All UI and form code updates now propagate automatically — identical to server module propagation.

The statement "forms in the client instance are acceptable" from earlier in this session is superseded by Test B. Forms must not be in client instances for the same reason server modules must not be — they would take precedence over the dependency and block UI updates from propagating.

---

## 9. The Update Model — How Changes Flow from Development to Clients

### The Development to Release Pipeline

```
mb-3-cs  ←→  GitHub (develop branch)
                    ↓  merge when feature is tested and ready
master_template  ←→  GitHub (stable branch)
                    ↓  tracked automatically
All client instances depending on master_template stable branch
```

**Step 1 — Development**
All feature work and bug fixes are written in mb-3-cs on the `develop` GitHub branch. This is the authoring environment. mb-3-cs is connected to both Anvil and OpenCode via GitHub as the source of truth.

**Step 2 — Testing**
Features are tested in mb-3-cs before release. mb-3-cs has its own Data Tables for development and testing. Server modules are tested here. This is the only quality gate — once code merges to `stable`, it is live for all clients.

**Step 3 — Release**
When a feature or fix is confirmed ready, `develop` is merged into `stable` in the GitHub repository. master_template, which tracks the `stable` branch, reflects the update immediately and automatically. No action is required in the Anvil IDE.

**Step 4 — Propagation**
All client instances that have their dependency set to master_template `stable` branch immediately run the new server code on the next server call. There is no delay, no cache to invalidate, no per-client action. The propagation is automatic and simultaneous.

**Step 5 — Client Notification**
A release notification email is sent to all clients via Anvil built-in email (system email tier — not the client's Brevo account). The email contains release notes describing what changed and noting any relevant actions. By the time the client reads the email, the update is already live in their instance. The email is purely informational.

An in-app notification system (notification badge, what's new panel, dismiss on read) is planned as a feature within Mybizz_management. Until Mybizz_management is built, email is the sole notification channel.

### Staged Rollout

For high-risk releases, Anvil's version tag system enables staged rollout:

1. Tag the release commit in master_template's GitHub repository using the format `vMAJOR.MINOR.PATCH` (e.g. `v1.1.0`)
2. Select a subset of client instances (e.g. 5 clients) and change their dependency from `stable` branch to the `v1.1.0` tag
3. Monitor those clients for a defined period (e.g. 48 hours)
4. If no issues emerge, change the remaining client instances' dependencies to `stable` (or the tag)
5. All of the above is performed from the single mybizz Anvil account IDE

Staged rollout is optional. The default for all clients is `stable` branch tracking with simultaneous propagation.

### Release Discipline

Because merging `develop` → `stable` is a production action affecting all clients simultaneously, the discipline around this merge is the platform's quality gate. The following must be confirmed before any merge to `stable`:

- The feature or fix is fully tested in mb-3-cs
- No breaking changes to existing Data Table schemas without a migration plan
- The migration plan has been executed against `blank_client_template` and all existing client instances
- Release notes are written and ready to send
- Any changes to the Vault-stored secret structure, HTTP endpoint signatures, or background task schedules are documented

### What Updates Do Not Cover

The update model covers server-side logic, forms, and UI automatically. There is one exception:

- **Data Table schema** — tables in existing client instances do not change when master_template changes. Schema changes (new tables, new columns, modified columns) must be applied manually per existing client instance in the Anvil IDE, and also applied to blank_client_template for future clients. See Section 10 and ADR-030.

---

## 10. blank_client_template — Purpose, Contents, and Responsibilities

### Why It Exists

Three options were evaluated for how a new client instance acquires its 36 Data Tables at provisioning:

**Option 1: Clone master_template**
Produces a client instance with all of master_template's server modules copied in. As established in Section 8, this breaks the update model. Rejected.

**Option 2: Manual table creation per client**
Creating 36 tables by hand in the Anvil IDE for every new client. Error-prone, time-consuming, and not workable at 100 clients. Rejected.

**Option 3: A dedicated provisioning clone source**
A maintained app containing only the 36 tables (correct schema, no data) and master_template as dependency, with no code. Cloning this produces a correctly structured client instance immediately. Adopted.

### Contents of blank_client_template

| Component | Present | Details |
|---|---|---|
| All 36 Data Tables | ✅ | Correct schema; absolutely no data |
| Startup module | ✅ | One minimal module; opens first form from master_template |
| master_template as dependency | ✅ | Set to stable branch; inherited by every clone |
| health_log table | ✅ | Required by ADR-032 — must be added |
| error_log table | ✅ | Required by ADR-032 — must be added |
| Server modules | ❌ | Must never be added |
| UI forms | ❌ | Forms live in master_template; must not be here |
| Anvil Secrets | ❌ | Set individually per client at provisioning |
| Custom domain | ❌ | Configured per client at provisioning |
| Any data | ❌ | Must remain empty at all times |

### blank_client_template as a Maintained Asset

blank_client_template is not a set-and-forget artifact. It is a living asset that must be kept current with the platform's Data Table schema.

**When a schema change occurs (new table, new column, column modification, column deletion):**

The change must be applied in three places:
1. mb-3-cs — where development happens
2. blank_client_template — so all future client instances get the correct schema
3. Every existing client instance — manually, one by one, in the Anvil IDE

There is no programmatic table creation in Anvil. Schema migrations to existing client instances are manual IDE operations. This is a real operational cost that must be factored into decisions about schema changes after the first client is live.

**The rule:** A schema-affecting change must never be merged to `stable` (master_template) without a documented migration plan that has already been applied to all existing client instances and to blank_client_template.

### The User's Initial Resistance

When blank_client_template was first proposed, the user pushed back, arguing that the correct provisioning source was master_template. The objection was valid in the sense that an extra app adds complexity. However, the code precedence problem (Section 8) establishes that cloning master_template is architecturally wrong. blank_client_template is the minimum viable alternative — it is essentially "master_template without the code."

The user subsequently agreed to this approach after the code precedence problem was explained clearly.

---

## 11. Client Instance Provisioning — The Full Activation Process

### Overview

Every new client requires a defined activation process. Anvil has no provisioning API — there is no way to create apps, set dependencies, or configure settings programmatically. All provisioning is performed manually in the Anvil IDE and through the application's own UI.

The full process is documented in `implementation/client-activation-runbook.md`. What follows is the rationale and detail behind each phase.

### Phase 1 — Provision the Anvil Instance

**Clone blank_client_template**
The clone source is always blank_client_template — never master_template, never an existing client instance. The clone is renamed to the client identifier format `[client-name]-cs`.

After cloning, the following must be verified before proceeding:
- All 36 Data Tables are present (empty)
- master_template is set as dependency (stable branch)
- No server modules exist in the clone
- No Anvil Secrets are set yet

**Set encryption_key in Anvil Secrets**
A new Fernet symmetric key is generated and placed in Anvil Secrets as `encryption_key`. This is the only Anvil Secrets entry for any client instance. It must be unique per client — never reused from another instance. It does not change after provisioning. It is the root key for the Vault system (ADR-003).

**Configure domain**
The client receives either:
- A custom domain (requires DNS configuration, Anvil domain settings, SSL verification)
- An Anvil subdomain (e.g. `clientname.anvil.app` — configured in Anvil app settings)

The final URL must be recorded — it is required for management token setup and for client registry registration.

### Phase 2 — Management Integration

**Generate management Bearer token**
A unique cryptographically secure token is generated for this client instance. This token is:
- Stored encrypted in Mybizz_management's client registry
- Stored in the client instance's management configuration table

This token authenticates all HTTP requests from Mybizz_management to the client instance's management endpoints. It is unique per client — compromise of one token does not affect other clients.

**Register in client registry**
A new entry is created in Mybizz_management's client registry table. Until Mybizz_management is built, this is maintained as a manual register in a secure document. Fields include: client_id, app_name, base_url, management_token, subscription_status, plan, onboarded_date.

**Verify management endpoints**
A GET request to the client instance's `/management/health` endpoint (with Bearer token) should return HTTP 200 with a valid health payload. This confirms the management endpoint module is functional and the token is correctly configured.

### Phase 3 — Create Owner Account

One Owner user account is created in the client instance using the client's confirmed email address. The Owner role is the only role with access to the Vault and all Settings. There is exactly one Owner per instance. This is enforced in the application's role model.

### Phase 4 — Guided Setup Session

A 30-minute onboarding call is scheduled with the client. During this session, the client performs the following within their instance, guided by mybizz. The client enters all sensitive values — mybizz does not handle API keys or credentials.

**Vault Configuration**
The client opens VaultForm (requiring TOTP step-up authentication — every time, no grace period). They enter:
- Payment gateway secret key(s): `stripe_secret_key` and/or `paystack_secret_key`
- Email credential: `smtp_password` (from their Brevo SMTP account)
- Any other integration credentials applicable

After saving each value, it is encrypted immediately and never displayed again. Vault access is logged to the audit log and an email notification is sent to the Owner.

**Payment Gateway Webhook Registration**
For each active payment gateway, the client registers the webhook URL in their gateway dashboard (Stripe or Paystack). The URLs are specific to the client instance:
- Stripe: `[base_url]/api/webhooks/stripe`
- Paystack: `[base_url]/api/webhooks/paystack`

The webhook handlers in master_template are already implemented. The client's action is registering the correct URL in the gateway's developer settings.

**Brevo Configuration**
SMTP host, port, username are entered in the application's email configuration settings. The SMTP password is already in the Vault from the previous step — it is retrieved from there at send time, never re-entered or displayed.

A test email is sent and confirmed received before proceeding.

**Business Profile**
Business name, address, contact details, logo, brand colours, time zone (IANA format), currency, and locale settings are configured.

**Initial Data**
At minimum: one service category, one service. Additional team members are invited if applicable.

### Phase 5 — Completion

A welcome email is sent via Anvil built-in email containing the platform URL, login credentials (if not already provided), and a link to the getting started guide.

Subscription billing is confirmed active in Stripe or Paystack. The activation checklist is filed.

### The Provisioning Gap — Pre-Mybizz_management

Until Mybizz_management is built, the client registry is maintained manually. Management endpoint verification is performed manually. There is no automated health check, no automated billing integration, and no consolidated client view. This is accepted as the V1 state — the manual process is documented in the runbook and is feasible at the initial client volume.

Mybizz_management is built after the first client is manually onboarded and master_template is confirmed launch-ready.

---

## 12. The Free Trial — Decision and Replacement

### The Original Idea

A 30-day free trial was included in the original platform design, based on Anvil's free tier. The intent was analogous to other SaaS products — let prospects try before buying.

### Why It Was Abandoned

**The product requires full configuration before it is useful.** The activation process (Section 11) includes Vault setup, payment gateway credentials, Brevo SMTP, business profile, and initial service configuration. None of this is optional — the platform cannot do anything meaningful without it. A prospect on a free trial who has not completed this setup sees a shell, not a product. This creates a misleading and poor first impression.

**Anvil's free tier makes core features non-functional.** The free tier:
- Restricts background task timeout to 30 seconds (useless for meaningful work)
- Makes scheduled tasks unavailable (appointment reminders and campaign enrollment do not function)
- Has no persistent server (cold start latency on every session)
- Has severely limited Data Table row capacity

The features that demonstrate the platform's value — automated appointment reminders, campaign enrollment, the full booking flow — do not work on the free tier. A free trial on the free tier would demonstrate an inferior, broken version of the platform.

**The "look around" intent is better served by a live demo.** A guided demo on mybizz's own fully configured instance shows the real product working correctly. The prospect sees what they are actually buying.

### The Replacement

| Original | Replacement |
|---|---|
| 30-day free trial on Anvil free tier | Live guided demo on mybizz's fully configured instance |
| Self-serve trial access | 30-minute guided onboarding session post-signup |
| "Look around" limited functionality | 30-day money-back guarantee on first paid subscription |

All paying clients are provisioned on the Anvil Business plan from day one. No client instance ever runs on the free tier.

The money-back guarantee terms must be defined in the subscription agreement before the first client is onboarded.

---

## 13. Anvil's Capabilities Relevant to This Architecture

The following Anvil features are confirmed relevant to the platform and relied upon in the architecture:

**Dependency system with branch and version tracking**
Enables the entire update model. Client instances track master_template's `stable` branch. Updates propagate automatically on merge. Version tags enable staged rollout.

**`@anvil.server.callable` and `anvil.server.call()`**
The mechanism by which client instance forms call server logic in master_template. The foundation of the client-server architecture.

**`@anvil.server.http_endpoint`**
HTTP endpoints secured with tokens. Used for Stripe, Paystack, and Brevo webhooks, and for the management endpoint communication between Mybizz_management and client instances.

**Anvil Secrets**
Per-app encrypted secrets store. Used exclusively for the `encryption_key` in each client instance. No other secrets ever go here — the application-level Vault handles everything else.

**Data Tables and `app_tables`**
The primary data store. `app_tables` resolves to the calling client instance's tables when called from a dependency server module — confirmed by live test (Section 6).

**Scheduled background tasks**
Required for: hourly campaign enrollment processing, 24-hour appointment reminders, 1-hour appointment reminders, health heartbeat monitoring. Available from Hobby plan upward.

**Background tasks (launched via `anvil.server.launch_background_task()`)**
For longer-running asynchronous work that must not block the user's session.

**Anvil built-in email**
Used exclusively for system-level communication from mybizz to clients: release notifications, platform alerts, welcome emails. Not the client's Brevo email.

**App cloning within account**
The mechanism for provisioning new client instances. blank_client_template is cloned to create each new client instance. Confirmed working within the same Anvil account.

**Anvil monitoring tools (2026)**
- App Logs: per-app session logs, background task logs, print() output, searchable with regex
- CPU Usage and Memory Usage tabs (added February 2026): per-app compute and memory over time
- Profiling and Tracing tools (added January 2026): per-app performance bottleneck analysis

These are used for per-app diagnostic work. They are not used as the primary monitoring solution for 100 client instances — that role belongs to Mybizz_management.

---

## 14. Anvil's Shortcomings and How They Are Overcome

**No provisioning API**
There is no programmatic way to create apps, set dependencies, or configure Anvil settings. Every provisioning action must be performed manually in the IDE.
*Overcome by:* The documented manual activation runbook (`implementation/client-activation-runbook.md`). Mybizz_management will automate parts of this in a future phase, but will ultimately still be constrained by what Anvil exposes — which is not everything.

**No native cross-app data access**
Apps on the same account cannot directly read each other's Data Tables. There is no shared data store or message bus native to Anvil.
*Overcome by:* Bidirectional HTTP endpoints between Mybizz_management and client instances. Secured with per-client Bearer tokens. Defined in ADR-033.

**No native routing**
Anvil does not include URL routing out of the box. Navigation between forms uses `open_form()` or content panel patterns.
*Overcome by:* The community `routing` library (Harry Twose's library) — referenced in existing documentation. This is a known solved problem in the Anvil ecosystem.

**No native WebSocket push from server to client**
Anvil cannot natively push data from server to client in real time. The client must poll or the server must respond to a client-initiated call.
*Overcome by:* Client-side timer polling where real-time updates are needed. Background tasks for asynchronous processing. Streaming responses (newer Anvil feature) for applicable use cases. This has not been fully designed for this platform — it is flagged as a consideration for features requiring live data.

**Skulpt (client-side Python) limitations**
Anvil's client-side runtime uses Skulpt — a Python-to-JavaScript compiler. Not all Python 3 standard library modules work. `datetime` has quirks. `re` has historically had gaps. `pandas` is not available client-side.
*Overcome by:* Strict policy — anything touching money, dates, or complex logic lives in server modules. Client-side code handles UI events and display only. This applies to: date arithmetic for booking availability, currency formatting, number formatting, financial calculations.

**No programmatic table creation**
Data Tables cannot be created via code. They must be defined in the Anvil IDE.
*Overcome by:* blank_client_template containing the pre-defined schema. Schema migrations to existing instances are manual IDE operations.

**Per-app monitoring — not consolidated**
Anvil's monitoring tools are per-app and IDE-bound. There is no native consolidated view across multiple apps.
*Overcome by:* Application-level metrics written to designated tables in each client instance. Mybizz_management queries these via HTTP endpoints and presents a consolidated monitoring dashboard. Defined in ADR-032.

**No native PDF generation**
Anvil has no built-in PDF library. PDF generation must be server-side.
*Overcome by:* A server-side Python PDF library to be selected. This is an unresolved item — library choice not yet made. Must be resolved before the invoicing feature is built.

**Free tier limitations**
Anvil's free tier disables or severely restricts scheduled tasks, background tasks, persistent server, and Data Table capacity.
*Overcome by:* Decision to use the Anvil Business plan for all client instances from day one. Free trial abandoned (ADR-031). The free tier is not used for any production client instance.

---

## 15. Items Confirmed by Live Test

The following have been verified by actual test execution, not assumption:

| Finding | Test conducted | Date | Result |
|---|---|---|---|
| `app_tables` in dependency server module resolves to client instance's tables | Created two test apps; dependency server module wrote to `app_tables.table_1`; verified which app's table received the row | 2026-06-13 | ✅ Row appeared in client instance only |
| App cloning within same Anvil account works | Routine operational knowledge confirmed by user | Pre-session | ✅ Confirmed |
| Client instance with no forms of its own can load and display forms from a dependency | Created `test-dependency-forms` (with MainForm) and `test-client-no-forms` (startup module only); ran the client app | 2026-06-13 | ✅ MainForm loaded and displayed correctly from dependency |

---

## 16. Items Requiring Further Testing

The following must be tested before the first client is provisioned.

### Test A — Branch-Based Update Propagation

**What it tests:** Whether merging a change to master_template's `stable` branch automatically propagates to a client instance tracking that branch, without any action on the client app.

**Why it matters:** This is the mechanical basis of the entire update model (ADR-029). If branch tracking does not work as described in Anvil's documentation, the update model must be redesigned before the first client is onboarded.

**Test procedure:**
1. Create `test-dependency-app` in Anvil, connect to a GitHub repository on a `stable` branch
2. Create `test-client-app`, add `test-dependency-app` stable branch as dependency, no code of its own
3. Confirm `test-client-app` can call functions from `test-dependency-app`
4. Modify a server function in `test-dependency-app`; merge the change to the `stable` branch in GitHub
5. From `test-client-app`, call the modified function
6. Verify the new behaviour without making any change to `test-client-app`

**Pass criteria:** `test-client-app` exhibits the new behaviour immediately after the GitHub merge, with no manual action on `test-client-app`.

**Fail action:** If the test fails, investigate whether Anvil requires a manual resync or cache clear. If automatic propagation is not reliable, the update model requires redesign. A new ADR must be written.

### Test B — Startup Form from Dependency — ✅ COMPLETED 2026-06-13

**Result:** Passed. A bare Anvil app (`test-client-no-forms`) with only a startup module successfully loaded and displayed a form (`MainForm`) defined in its dependency (`test-dependency-forms`). No forms were required in the client app.

**Architectural consequence:** Forms live in master_template. Client instances require only a startup module and Data Tables. UI and form updates propagate automatically via the dependency — identical to server module propagation. ADR-028 and ADR-030 updated accordingly.

---

## 17. Unresolved Items

The following items were identified during this session and remain unresolved. They must be addressed before the relevant features are built.

### Item 1 — Anvil Row and Storage Limits: Per Account or Per App?

**What is unknown:** Whether the Business plan's 1,000,000 row and 100 GB storage limits apply to the entire account (shared across all apps) or to each app individually.

**Why it matters:** If per account, at 100 clients the effective limit is 10,000 rows per client — potentially tight for active businesses over time.

**Action required:** Email Anvil sales with the question before finalising the billing model. The $18,000/year figure in platform-overview.md must be revised once the answer is known.

**Resolution path:** If per account and limits are likely to be hit, activate the Extra Server Resources add-on (~$599/month, includes isolated database).

### Item 2 — PDF Invoice Generation Library

**What is unknown:** Which Python library will be used for server-side PDF generation.

**Why it matters:** Invoice and quote PDF generation is a confirmed V1 feature. The library choice affects server module dependencies and must be verified to work in Anvil's server environment.

**Action required:** Evaluate candidates (reportlab, WeasyPrint, fpdf2) for compatibility with Anvil's server runtime. Select and document before the invoicing feature is built.

### Item 4 — Dilemmas 4–9 (from session agenda — not yet addressed)

The following dilemmas were identified at the start of the session but time did not permit their resolution. They must be resolved before building any feature that touches the relevant area.

| Dilemma | Area affected | Cannot build without resolving |
|---|---|---|
| 4. Background Task Limits per client instance | Campaign enrollment, appointment reminders, health heartbeat | Any background task feature |
| 5. Webhook Architecture per client | Stripe, Paystack, Brevo webhook handlers | Payment and email integrations |
| 6. State Management Between Forms | Every form transition in the application | UI build start |
| 7. PDF Invoice Generation | Invoicing feature | Invoice and quote generation |
| 8. Skulpt Client-Side Limitations | Any client-side date, number, or currency work | Booking UI, financial displays |
| 9. Server Call Latency in Booking Flow | Booking UX | Booking feature build |

### Item 5 — Anvil Uplink Applicability

Anvil Uplink was mentioned during the session as potentially relevant. Uplink connects external Python processes to an Anvil app via a persistent connection. Its applicability to this architecture was flagged but not investigated.

**What needs investigation:** Whether Uplink has any role in the client instance → Mybizz_management communication pattern, or in any other part of the platform. Current assessment is that HTTP endpoints are sufficient and Uplink adds unnecessary infrastructure, but this should be confirmed.

---

## 18. Document Registry

### Documents Created This Session

| Document | Path | Type | Status |
|---|---|---|---|
| Client Instance Architecture | `adr/028-client-instance-architecture.md` | ADR | Confirmed |
| Dependency Update Model | `adr/029-dependency-update-model.md` | ADR | Confirmed — Test A pending |
| blank_client_template | `adr/030-blank-client-template.md` | ADR | Confirmed |
| Free Trial Abandoned | `adr/031-free-trial-abandoned.md` | ADR | Confirmed |
| Observability Architecture | `adr/032-observability-architecture.md` | ADR | Confirmed |
| Mybizz_management Visibility | `adr/033-mybizz-management-visibility.md` | ADR | Confirmed |
| Client Activation Runbook | `implementation/client-activation-runbook.md` | Runbook | Confirmed — subject to Test B result |
| Platform Overview Addendum | `docs/platform-overview-addendum.md` | Doc update | Pending append to platform-overview.md |
| OpenCode Technical Handover | `HANDOVER-opencode-2026-06-13.md` | Handover | Complete |

### Pre-Existing Documents Referenced and Unchanged

| Document | Path | Notes |
|---|---|---|
| Platform Overview | `docs/platform-overview.md` | Section 7.4 updated during session; addendum to be appended |
| Vault System | `docs/vault-system.md` | Confirmed unchanged; audit log pattern extended by ADR-032 |
| Payment Security Boundary | `adr/3-payment-security-boundary-vault.md` | Confirmed unchanged |
| Build Plan | `implementation/build-plan.md` | Not modified — requires update to reflect new tasks from this session |

### Required Updates to Pre-Existing Documents

**platform-overview.md:** Append all sections from `docs/platform-overview-addendum.md` at the positions indicated in that file.

**implementation/build-plan.md:** The following must be added as pre-launch tasks:
- Creation of `blank_client_template` app with 36 tables (including health_log and error_log)
- Creation of `server_modules/management_endpoints.py` in mb-3-cs as a Stage 1 deliverable
- Addition of `health_log` and `error_log` tables to the 36-table schema
- Execution of Test A (branch propagation) and Test B (startup form from dependency)
- Anvil billing question (email to Anvil sales)

---

*End of Platform Establishment Report — 2026-06-13*
