# Questions to Raise with Anvil
## Mybizz CS — Platform Architecture Clarifications

**Prepared by:** Mybizz CS development session  
**Date:** 2026-06-13  
**Contact:** support@anvil.works / Anvil sales team  
**Context:** We are building a multi-tenant SaaS platform on Anvil. All client instances are hosted as separate apps on a single Anvil Business plan account. We have up to 100 client apps on one account. Several architectural decisions depend on the answers below.

---

## 🔴 Critical — Architecture Depends on Answers

### 1. Data Table Row and Storage Limits: Per Account or Per App?

The Business plan states 1,000,000 rows and 100 GB storage. Do these limits apply to the entire account shared across all apps, or does each app on the account get its own full allocation?

**Why it matters:** We will have up to 100 client apps on one account. If limits are per account, the effective allocation per client is 10,000 rows and 1 GB — which may be insufficient for active businesses over time. If per app, there is no issue.

**Specific question:**
> *"On the Business plan with multiple apps on one account — do the Data Table row and storage limits apply per account (shared across all apps) or per app (each app gets the full 1,000,000 rows and 100 GB)?"*

"When vault_service.py in master_template calls anvil.secrets.get_secret('encryption_key'), does it resolve to the calling client instance's Anvil Secrets — or to master_template's own Anvil Secrets?"

---

### 2. Two Separate Apps on the Same GitHub Repository, Different Branches

Our update model requires two separate Anvil apps to connect to the same GitHub repository while tracking different branches:
- `mb-3-cs` (development workspace) → connects to `develop` branch
- `master_template` (published dependency) → connects to `stable` branch

**Specific question:**
> *"Can two separate Anvil apps both connect to the same GitHub repository while each tracks a different branch? For example, App A tracking `develop` and App B tracking `stable` on the same repo?"*

**Why it matters:** If this is not supported, our entire update pipeline must be redesigned. This is the foundational mechanism by which code changes flow from development to all client instances.

---

### 3. Background Task Limits on the Business Plan

Our platform requires multiple scheduled and background tasks per client app:
- Appointment reminders (24-hour and 1-hour before)
- Campaign enrollment processing (hourly scheduled task)
- Health heartbeat monitoring (scheduled task)
- Webhook event processing (background tasks triggered by HTTP endpoints)

With up to 100 client apps on one account, background task capacity is a significant concern.

**Specific questions:**
> 1. *"How many scheduled tasks can be registered per app on the Business plan?"*
> 2. *"How many concurrent background tasks can run per app?"*
> 3. *"Do background task limits apply per app or per account across all apps?"*
> 4. *"Is there a monthly or daily execution cap on background tasks, or is the only constraint concurrency?"*

---

### 4. Persistent Server: Per App or Per Account?

The Business plan includes a persistent server. We need to understand whether this means each app on the account maintains its own persistent server instance, or whether a single persistent server is shared across all apps on the account.

**Specific question:**
> *"On the Business plan with multiple apps on one account — does each app get its own persistent server instance, or is the persistent server a shared resource across all apps on the account?"*

**Why it matters:** With 100+ client apps, a shared persistent server would create performance and isolation concerns. Each client instance must be independently responsive.

---

## 🟠 Architectural — Affects Implementation Decisions

### 5. Custom Domain Limit on the Business Plan

Each of up to 100 client instances requires its own custom domain or subdomain.

**Specific question:**
> *"Does the Business plan support an unlimited number of custom domains across all apps on one account, or is there a cap on the number of custom domains?"*

---

### 6. Anvil Secrets Isolation Across Apps on the Same Account

Our security architecture (Vault system) requires that each client app's `encryption_key` stored in Anvil Secrets is completely inaccessible from any other app on the same account — including from our own platform management app.

**Specific question:**
> *"When multiple apps share one Anvil account, is each app's Anvil Secrets store completely isolated from all other apps on the account? Can one app ever access another app's secrets, even if both apps are owned by the same account?"*

**Why it matters:** If secrets are not fully isolated per app, our Vault security architecture is compromised.

---

### 7. Native App-to-App Communication Within the Same Account

Our management application (Mybizz_management) needs to read from and write to client instance apps on the same account. Our current solution is bidirectional HTTP endpoints with Bearer token authentication.

**Specific question:**
> *"Are there any native Anvil mechanisms for server-to-server communication between apps on the same account — beyond HTTP endpoint calls? For example, a direct server call mechanism, shared messaging queue, or inter-app event system?"*

**Why it matters:** If a more efficient or lower-latency mechanism exists natively in Anvil, we would use it instead of HTTP endpoints.

---

### 8. Anvil Uplink — Applicability to a Multi-App Same-Account Architecture

We are aware of Anvil Uplink as a mechanism for connecting external Python processes to an Anvil app. Our platform has all apps on the same Anvil account.

**Specific question:**
> *"Does Anvil Uplink have any applicable use case in an architecture where all apps are on the same Anvil account and communicate via HTTP endpoints? Is there a scenario where Uplink would be preferable to HTTP endpoints for app-to-app communication within the same account?"*

---

## 🟡 Practical — Needed Before First Build

### 9. Dependency Package Name in Client Apps

When `master_template` is added as a dependency to a client instance app, we need to know the exact package name used in import statements within the client app's startup module. For example:

```python
from PACKAGE_NAME.MainForm import MainForm
from anvil import open_form
open_form(MainForm())
```

**Specific question:**
> *"When App A is set as a dependency of App B, what determines the package name used to import from App A within App B's code? Is it the package name defined in App A's own settings, the name given when adding the dependency in App B's dependency settings, or something else?"*

**Why it matters:** This determines exactly how the startup module must be written in every client instance. Getting this wrong means client instances cannot start.

---

### 10. Branch Tracking — Propagation Timing and Mechanism

Our update model relies on client instances automatically receiving updates to `master_template` when the `stable` GitHub branch is updated via a merge.

**Specific questions:**
> 1. *"When a client app depends on another app's named branch (e.g. `stable`), and that branch is updated via a GitHub merge, does the client app automatically use the new code immediately — or is there a delay, cache, or manual resync step required?"*
> 2. *"If a client app user is in an active session when a dependency update is merged, do they see the update immediately, on next page navigation, or only on full session reload?"*
> 3. *"Is there any mechanism to be notified when a dependency update has successfully propagated to dependent apps?"*

**Why it matters:** The entire update model depends on propagation being automatic and reliable. If any manual step is required, the model must be adjusted before first client onboarding.

---

## Summary Table

| # | Question | Priority | Blocks |
|---|---|---|---|
| 1 | Row/storage limits per account vs per app | 🔴 Critical | Billing model, Extra Resources decision |
| 2 | Two apps on same GitHub repo, different branches | 🔴 Critical | Entire update pipeline |
| 3 | Background task limits on Business plan | 🔴 Critical | All background task features |
| 4 | Persistent server per app vs per account | 🔴 Critical | Performance architecture |
| 5 | Custom domain limit on Business plan | 🟠 Important | Client provisioning at scale |
| 6 | Anvil Secrets isolation across apps | 🟠 Important | Vault security model |
| 7 | Native app-to-app communication | 🟠 Important | Mybizz_management architecture |
| 8 | Uplink applicability | 🟠 Important | Management communication model |
| 9 | Dependency package name | 🟡 Practical | Startup module code |
| 10 | Branch tracking propagation timing | 🟡 Practical | Update model reliability |

---

*End of questions-to-raise-with-anvil.md*
