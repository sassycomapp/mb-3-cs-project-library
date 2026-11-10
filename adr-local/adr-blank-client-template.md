---
title: "adr-blank-client-template"
doc-id: "mb-8829bc8a"
updated: "2026-11-10"
---
# `adr-blank-client-template` ADR — App 3 as the Client App

**Status:** Accepted
**Date:** 2026-10-31
**Authority:** [[[default:adr-global/adr-client-instance-architecture|Client Instance Architecture]]](C:/dev/project-library-global/adr-global/adr-client-instance-architecture.md) ADR, [[[default:adr-global/adr-dependency-update-model|Dependency Update Model]]](C:/dev/project-library-global/adr-global/adr-dependency-update-model.md) ADR, [[[default:specifications-global/spec-client-instance-hosting|Client Instance Hosting]]](C:/dev/project-library-global/specifications-global/spec-client-instance-hosting.md), [[[default:specifications-global/spec-five-app-architecture-model|Five-App Architecture Model (Specification)]]](C:/dev/project-library-global/specifications-global/spec-five-app-architecture-model.md). Where a technical fact here differs from `mb-3-cs-architecture-reference.md`, the architecture reference is correct.

---

## Context

Every client instance runs on the Anvil App Server, as one container with its own database ([[[default:specifications-global/spec-client-instance-hosting|Client Instance Hosting]]](C:/dev/project-library-global/specifications-global/spec-client-instance-hosting.md)). The container needs an app to run: one that opens `master_template`, carries the schedule, and defines the full schema of `authoritative-schema.md`, from which the App Server builds the client's tables. That app must never hold its own forms or server modules: one would silently override `master_template` for every client (reference section 2).

---

## Decision

**App 3, `mb-3-cs-app3` (`blank_client_template`), is the client app every client instance runs.** It is held on Mybizz's Anvil Business account and built, with `master_template` and `m3`, into the release image defined in `mb-3-cs-app4`.

### Contents of App 3

| Component | Present | Notes |
|---|---|---|
| The table definitions of `authoritative-schema.md` | Yes | The released schema; the App Server builds and extends each client's tables from them |
| One startup module | Yes | Opens `master_template`'s startup form |
| `master_template` (App 2) as a dependency | Yes | Built into the release image |
| Scheduled tasks | Yes | The schedule of `integration-architecture.md` (Background Tasks), calling `master_template`'s background tasks |
| README stating the five-app rule | Yes | [[[default:adr-global/adr-client-instance-readme-five-app-system\|Mandatory README in Every Client Instance Documenting the Five-App System]]](C:/dev/project-library-global/adr-global/adr-client-instance-readme-five-app-system.md) ADR |
| Server modules | No | Never added; the image build stops on one |
| Forms | No | Forms live in `master_template`; the image build stops on one |
| Anvil Secrets | No | `encryption_key` is set per instance at provisioning, as the container's encrypted setting |
| Custom domain | No | The client's own domain is added to the client's container at onboarding |
| Email service setting | No | Each container's SMTP settings hold the SES SMTP endpoint and the client's credentials |
| Data | No | The sample data for the client's trade is loaded per instance at provisioning, and recorded in `sample_rows` |

### The startup module

The startup module opens `master_template`'s startup form, `auth.LoginForm`, which routes by role (reference section 10):

```python
from anvil import open_form
from {app2_package}.auth.LoginForm import LoginForm

open_form(LoginForm())
```

`{app2_package}` is the `package_name` in App 2's `anvil.yaml`. It is the only place where code imports by package name; every module inside `master_template` uses relative imports (`scaffold-spec.md`).

### Provisioning

1. App 5's New client action creates the client's database and starts the client's container `[client-name]-cs` from the current release image.
2. The App Server builds every table of App 3's table definitions in the client's empty database.
3. The remaining steps follow `mb-3-cs-architecture-reference.md` section 5 and the [[[default:specifications-global/spec-client-activation-runbook|Client Instance Activation Runbook]]](C:/dev/project-library-global/specifications-global/spec-client-activation-runbook.md).

### Schema responsibility

Every schema change is made in App 1, recorded in `schema-migration-log.md`, and written into App 3's table definitions at Step 42. After the first release, a change only adds tables and nullable columns; the table-change check in `mb-3-cs-app4` stops any other. Each client's App Server adds the change when its container starts on the release (`deployment-procedures.md`).

---

## Consequences

- A new client instance is correctly structured in minutes.
- Server logic, forms, UI and new tables and columns reach every instance through the release image.
- Every instance runs the same schema and the same schedule.
- After the first release, a table or column is never removed, renamed or retyped.

---

*End of `adr-blank-client-template` ADR*
