# Mybizz CS — Architecture

**Vertical:** Consulting & Services only
**Architecture:** Dependency-based (master_template → client instances)
**Schema:** 43 tables (see `authoritative-schema.md`)

---

## 1. The Five-App System

Mybizz runs on five applications within a single Anvil account (see ADR-42):

### mb-3-cs
The development workspace. All feature work happens here. Connected to GitHub as the shared
source of truth between Anvil.works and VS Code. Never a dependency for any client instance.

### master_template
The sole source of all server-side logic and UI forms for every client instance. Tracks the
`stable` GitHub branch. A frozen, versioned snapshot of mb-3-cs's proven work — never given to
clients directly. Client instances depend on it entirely; there is no copy to fall out of date
and no separate "deploy" step for a client instance — it simply reflects whatever
`master_template` currently contains.

### blank_client_template
The provisioning clone source. Contains the 43-table schema, one startup module, and
`master_template` as a dependency. **Zero server modules, zero forms.** Every new client instance
is cloned from this app, not built from scratch.

### `[client-name]-cs` (one per paying client)
Contains the schema, the startup module, and an active dependency on `master_template`. Nothing
else. **This emptiness is intentional, not incomplete** — see the warning below.

### Mybizz_management
The platform operator's internal tooling — client provisioning, health monitoring, billing
automation, update distribution. A separate Anvil application, never part of `master_template`.
Deferred to post-launch; built only after `master_template` reaches launch and the first client
has been manually onboarded.

**Note:** mb-2-cs was the original development application and has been deprecated. All
development has migrated to mb-3-cs.

**See:** `9-multi-vertical-to-single-vertical-conversion.md`, ADR-42 (Mandatory README in Every
Client Instance)

---

## ⚠️ Do Not Add Forms or Server Modules to a Client Instance

Because a client instance (App 4) inherits all logic and UI live from `master_template`, adding a
form or server module directly to a client instance does not extend that instance — it silently
and permanently overrides the dependency for that instance only, with no error or warning. That
instance stops receiving any future update to `master_template`. If a change is needed for a
specific client, it belongs in `mb-3-cs`, tested against a private client instance, then merged
`develop` → `stable` into `master_template` — after which every client instance, including the
one that prompted the change, receives it automatically. Per ADR-42, every client instance
carries a README stating this explicitly from the moment of provisioning; if you are working
directly in a client instance app and don't see that README, stop and locate it before proceeding.

---

## 2. Client Instances — Clone Lineage

Each subscriber's client instance is provisioned by cloning `blank_client_template` (App 3), not
by building a fresh app. The clone brings the schema, the startup module, and the
`master_template` dependency — nothing else.

- **Data isolation is structural.** Each client is a separate Anvil app with a separate
  database. `app_tables` always resolves to the current client's data. Cross-client access is
  architecturally impossible.
- **`master_template` is stateless.** It contains zero client data. All state lives in the
  client's own Data Tables.
- **Clients never see source code.** `master_template` is consumed as a compiled dependency.

---

## 3. Development Workflow

Changes made in VS Code are pushed to GitHub. Anvil syncs from GitHub. Changes made in the Anvil
Designer are pulled into VS Code before any code draft prompt is run. Designer work must always
precede code work for any given form. This workflow applies to `mb-3-cs` only — see the warning
above regarding client instances.

---

## 4. Package Structure

Feature-based organization. Packages are never renamed or merged during active development.

```
client_code/
  auth/          layouts/       dashboard/     settings/
  bookings/      services/      customers/     crm/
  blog/          public_pages/  shared/

server_code/
  server_auth/       server_settings/   server_dashboard/
  server_bookings/   server_services/   server_customers/
  server_marketing/  server_payments/   server_blog/
  server_shared/     server_analytics/  server_emails/
```

**Note:** The client-side CRM and marketing package is named `crm/` — not `marketing/`. The
server-side package remains `server_marketing/`.

**See:** `6-crm-package-replaces-marketing.md`

---

## 5. Module Architecture

See `anvil-platform-standards.md` Section 3 for the general Three-Tier Module Pattern (Forms /
Client Modules / Server Modules / Pure Logic Modules) — not repeated here. This project's specific
authority for the pattern: ADR-035 (Form Architecture and State Management).

---

## 6. Navigation Architecture

See `m3-design-standards.md` Section 7 for the general authenticated/public navigation patterns
(plain `Link` vs. Routing, lambda loop-capture) — not repeated here. This project's specific
choices and menu structures:

| Area | System |
|---|---|
| Admin & customer portal | Custom HtmlTemplate + plain Link components + lambda click handlers |
| Public website pages | Anvil Routing dependency (`@router.route`) |

**This project does not use native M3 navigation components** (`NavigationDrawerLayoutTemplate`,
`NavigationLink`, `NavigationRailLayout`) even though the M3 theme dependency fully supports them
— this is a usage decision based on the custom layout approach, not a platform limitation. If a
future architecture review determines native M3 navigation layouts are better suited, this may
change without requiring an ADR update.

**See:** `2-navigation-lambda-link-open-form.md`, ADR-037 (UI Customization Approach)

### AdminLayout Navigation Structure
```
Dashboard                          (always)
▼ Sales & Operations
  Bookings                         (always)
  Services                         (always)
▼ Customers & Marketing
  Contacts                         (always)
  Campaigns                        (marketing_enabled)
  Broadcasts                       (marketing_enabled)
  Segments                         (marketing_enabled)
  Tasks                            (marketing_enabled)
▼ Content & Website
  Blog                             (blog_enabled)
  Pages                            (always)
  Media                            (always)
▼ Finance & Reports
  Invoices                         (always)
  Payments                         (always)
  Reports                          (always)
  Analytics                        (always)
  Time Entries                     (always)
Settings                           (Owner / Manager only)
Vault                              (Owner only)
[Sign out — bottom of sidebar, all roles]
```

### CustomerLayout Navigation Structure
```
My Dashboard                       (always)
My Bookings                        (always)
My Invoices                        (always)
My Reviews                         (always)
Support Tickets                    (always)
Account                            (always)
[Sign out — bottom of sidebar]
```

**Note:** The CustomerLayout is deferred to V2. Customers book via the public site and the
business manages all bookings from the admin panel in V1.

---

## 7. Startup and Authentication Routing

The Startup Form is `auth.LoginForm` (defined in `anvil.yaml`). The `__init__` method checks
authentication and routes before `init_components()`:

1. If logged in as Owner/Manager/Admin/Staff → open `DashboardForm` with AdminLayout, return
2. If logged in as Customer → open customer portal with CustomerLayout, return
3. If not logged in → proceed with LoginForm display

Calling `open_form()` in `__init__` must always be followed by `return`.

**See:** ADR-034 (Anvil Platform Constraints — server call latency)

---

## 8. The Sidesheet

The M3 Sidesheet is available on `NavigationDrawerLayout` and `NavigationRailLayout`. It is
appropriate for secondary content or detail panels alongside main content without replacing it.
`NavigationRailLayout` with Sidesheet is well-suited to step-by-step guided workflows: pass
accumulated data via `self.item` between steps; submit to the server in a single call at the
final step.

---

*Architecture v2.0 — Section 1 corrected from three named apps to the actual five-app system
(ADR-42); Section 2 clarified to state the `blank_client_template` clone lineage explicitly; a
warning against modifying client instances added. Sections 5–6 trimmed of content now duplicated
in the Standards Library (`anvil-platform-standards.md`, `m3-design-standards.md`).*

**Outstanding, not fixed here:** ADR-42 states "the 36-table schema" for `blank_client_template`;
`authoritative-schema.md` confirms 43 tables. That correction belongs in ADR-42 itself.
