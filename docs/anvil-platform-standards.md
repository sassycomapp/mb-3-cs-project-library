# PDLF Standards Library — Anvil Platform Mechanics

**Location:** `C:\pdlf\standards-library\anvil-platform-standards.md`
**Scope:** Project-agnostic. Extracted patterns and rules — not any single project's specific
values, keys, or data.
**Source:** Extracted from `mb-3-cs` project documentation (anvil-spec-table.md, architecture.md,
scaffold-spec.md), generalized for reuse across any PDLF-run Anvil project.

---

## 1. Form File Structure

- Forms are folders: `FormName/`
- Code lives in `FormName/__init__.py`
- Designer config lives in `FormName/form_template.yaml`
- **Never modify `form_template.yaml` programmatically** — it's Designer-managed.

## 2. Server Function Pattern

Every server function should:
- Return a response envelope, not a bare value or exception:
  ```python
  {'success': True, 'data': result}
  {'success': False, 'error': msg}
  ```
- Have a docstring with Args, Returns, Raises.
- Have type hints on all public functions.
- Log via `logging.getLogger(__name__)` — never `print` in server code.
- Check authentication at entry: `user = anvil.users.get_user()`.
- Avoid bare `except:` and `except: pass` — preserve tracebacks with `raise` / `raise ... from e`.

## 3. Three-Tier Module Pattern (+ Pure Logic)

1. **Forms** — presentation only. Event handlers call methods; methods contain logic.
2. **Client Modules** — UI coordination, input validation, server-call orchestration.
3. **Server Modules** — business rules, persistence, security enforcement, external API calls.
4. **Pure Logic Modules** — zero Anvil imports, no side effects, deterministic. This is what
   makes Level 1 testing possible (see `testing-methodology-standards.md`) — not a style
   preference.

## 4. Data Binding Pattern

- Set `self.item` before calling `self.init_components()`.
- **Write Back toggle (W) must be enabled in the Designer's Data Bindings panel** — it cannot be
  set in code.
- Call `refresh_data_bindings()` after modifying `self.item` in place.
- Do not use data bindings on forms that manage their own server calls directly (e.g. settings
  forms) — the two patterns conflict.

## 5. Background Tasks

- Decorator: `@anvil.server.background_task`
- No timeout limit (unlike the standard server call's ~30-second limit).
- **Threshold rule:** any operation expected to exceed roughly 75% of the server-call timeout
  (i.e. ~22 seconds against a 30-second limit) must use a background task instead of a normal
  server call.

## 6. Uplink — Mechanics and Safety

Full testing usage: see `testing-methodology-standards.md`. Platform mechanics:

```python
import anvil.server, os
anvil.server.connect(os.environ['ANVIL_UPLINK_KEY'])
try:
    result = anvil.server.call('server_function_name')
finally:
    anvil.server.disconnect()
```

- Always use a **Server** Uplink key for server-side work (not Client).
- **Never hardcode Uplink keys** — read from environment variables.
- Always disconnect after the operation.
- Restart the Anvil server runtime after making changes in the Designer, before the next Uplink
  session — stale runtime state is a real, documented failure mode.
- Uplink-connected code can call an app's *own* already-deployed server-module functions directly
  (`anvil.server.call('function_name')`), not only functions defined in the Uplink script itself —
  this is what makes remote integration testing possible without a browser.

## 7. Deployment Environments

- Anvil deploys via named Deployment Environments, each with its own URL, code version
  (branch/commit), and database — not a generic CI/CD pipeline.
- Code can branch on environment to avoid per-deploy edits:
  ```python
  if anvil.app.environment.name == "Production":
      ...
  elif anvil.app.environment.name == "Staging":
      ...
  ```
- Rollback is re-pointing an environment to a different commit and republishing — not a code
  revert, which does not un-publish an app.

## 8. Debugging Tools (Interactive, Not Autonomous)

- **Interactive Debugger**: breakpoints, call-stack inspection, works client- and server-side.
  Breakpoints only fire in the Development Environment, never in published versions.
  Server-code breakpoints require the Business Plan or above.
- **Server Console / App Console**: Python REPLs for manually executing and inspecting code.
- These are tools for a human at the keyboard. There is no confirmed autonomous "runs tests and
  repairs code by itself" capability built into Anvil.works — don't assume one exists without
  direct confirmation.

## 9. Anvil AI — UI Construction Inputs

Anvil AI can build a competent Designer UI, but needs three specific inputs, not one:
1. Wireframe HTML
2. Screen HTML (see `screen-and-wireframe-production-standards.md`)
3. A screen PNG (rendered from the screen HTML via an external tool)

Given all three, it also wires components to their code-behind functions as part of the build.

## 10. Security Patterns

Moved to `spec-security.md` — the two-level secrets model, secret enforcement/masking, RBAC/data
access pattern, rate limiting, and shared numeric security standards now live there as their own
dedicated file, the same way testing and M3 design already have their own files.

## 15. Data Table Access Patterns

- **Row identification:** use Anvil's built-in Row IDs (`row.get_id()`). Do not create custom
  auto-increment columns.
- **Table linking:** store Row objects directly in link columns, not integer IDs:
  ```python
  contact = app_tables.contacts.add_row(instance_id=user, email='jane@example.com')
  booking = app_tables.bookings.add_row(instance_id=user, contact=contact)
  ```
- **Data isolation:** every query must filter by `instance_id=user`. Never trust a
  client-provided `instance_id`.
- **Mandatory columns:** every table has `instance_id` (link → users), `created_at` (datetime),
  and `updated_at` (datetime, if the row is mutable).
- **Query patterns:** `get()` for a single record (returns `None` if not found); `search()` for
  multiple records (returns a `SearchIterator`); paginate with slicing (`[:50]`); sort with
  `tables.order_by()`; avoid converting a `SearchIterator` to a list unless actually necessary.
- **Transactions:** use `@tables.in_transaction` for counter increments and any multi-step
  operation that requires atomicity.

## 16. Constants

No hardcoded URLs, API keys, or magic strings in code. Use a dedicated `constants.py` module.
Sensitive values go in Anvil Secrets or the Vault (Section 10), never in `constants.py` itself.

## 17. Global Client-Side Error Handler

Register one global handler rather than relying solely on per-call `try/except`:

```python
def global_error_handler(err):
    anvil.server.call('log_error_to_server', repr(err))
    alert("An unexpected error occurred. Please try again.", title="System Error")

anvil.set_default_error_handling(global_error_handler)
```

## 18. File and Module Naming Conventions

| Element | Convention | Example |
|---|---|---|
| Folders | PascalCase | `ServerAuth`, `ContactListForm` |
| Files | lowercase_with_underscores | `contact_service.py` |
| Server modules | `{purpose}_service.py` | `booking_service.py` |
| Integration modules | `{purpose}_integration.py` | `brevo_integration.py` |
| Client modules | `{purpose}_utils.py` | `validation_utils.py` |
| Database tables | lowercase_snake | `contacts`, `bookings` |
| Link columns | `{table}_id` or singular name | `contact_id`, `service_id` |

A project's own terminology mapping (e.g. what "Client" vs "Customer" means for that business)
belongs in that project's own nomenclature document, not here — this table is the reusable
file/module naming pattern only.

---

*Anvil Platform Standards v1.2 — Sections 15–18 added, consolidating spec-coding-standards.md's
unique content (Data Table patterns, Constants rule, global error handler, naming conventions)
into this single file. spec-coding-standards.md is now fully absorbed — everything else in it
(response envelope, pure logic modules, form file structure, docstrings/type hints, logging,
exception handling, authentication checks, client-side try/except wrapping, data binding) was
already present in Sections 1–3 above and has not been duplicated a second time.*


