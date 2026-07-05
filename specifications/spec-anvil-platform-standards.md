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

## 10. Two-Level Secrets Model (Pattern)

- **Anvil Secrets** holds exactly one item: a master encryption key, set at provisioning, never
  changed, used exclusively inside a dedicated vault-service module to encrypt/decrypt
  application-level secrets. No other code calls `anvil.secrets.get_secret()` directly.
- **The Vault** (an application-level encrypted Data Table) holds every other credential — client
  API keys, integration tokens. Retrieved only via a `get_vault_secret(name)` function, never
  accessed directly by integration modules.
- **Access control:** restrict Vault access to the Owner role, require step-up authentication
  (e.g. TOTP) on every open with no grace period, and send a notification on every access.

## 11. Secret Enforcement and Masking Pattern

Enforce at the point of persistence, not just at the point of retrieval — use a hard-fail assert
so a secret can never be accidentally saved outside the Vault:

```python
_PAYMENT_SECRET_COLUMNS = frozenset({
    'stripe_secret_key', 'paystack_secret_key', 'paypal_secret'
})
assert not (_PAYMENT_SECRET_COLUMNS & set(data.keys())), (
    f"Secret key columns must not be saved here: "
    f"{_PAYMENT_SECRET_COLUMNS & set(data.keys())}"
)
```

`AssertionError` must always be re-raised before any broader exception handler — never swallowed.

**Masking:** any function returning configuration containing a secret field returns `'***'` for
that field, never the real value. Client-side save methods must never include a secret field in
an outbound payload, even a masked one — omit the field entirely rather than round-trip it.

## 12. RBAC and Data Access Pattern

- Every server function operating on application data carries an RBAC decorator
  (`@require_role` / `@require_permission` or equivalent).
- Role enforcement is always server-side. Client-side navigation visibility (hiding a nav link
  for a restricted role) is a UX convenience only — never the actual security boundary.
- All Data Tables are set to "No access" for client code, without exception. All data access
  goes through server functions, which enforce `instance_id` filtering, authentication, and input
  validation that direct client access would bypass.

## 13. Rate Limiting Pattern

Enforce rate limits via a Data Table, not an in-memory counter — a Data Table survives server
restarts and works correctly across Anvil's multi-server environment; an in-memory counter does
not. Clean up expired rate-limit records on a background task schedule rather than letting the
table grow unbounded.

## 14. Shared Numeric Security Standards

These values are fixed platform defaults — the same values referenced by both a project's
canonical security document and its development policy, so they exist in exactly one place:

| Standard | Value |
|---|---|
| Password minimum length | 8 characters |
| Password complexity | At least one uppercase, one lowercase, one number |
| Rate limit — unauthenticated | 10 requests/minute/IP |
| Rate limit — authenticated | 100 requests/minute/user |
| Session inactivity timeout | 30 minutes |
| Audit log retention | 2 years |
| Background-task threshold | Any operation expected to exceed ~22 seconds (75% of Anvil's 30-second server-call timeout) |

A project may override any of these explicitly in its own canonical document, stating the
override and the reason — never silently diverge.

---

*Anvil Platform Standards v1.1 — Sections 10–14 added, extracted from a project's
security-architecture and development-policy documents during a canonical/standards
reconciliation. Section 14 resolves a duplication previously present in both source documents.*

