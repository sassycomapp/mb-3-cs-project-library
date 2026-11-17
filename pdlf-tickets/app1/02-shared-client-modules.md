---
title: "02 — Shared client modules and the translation engine"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-508143a5"
updated: "2026-11-17"
---

## Description

**Feature:** The shared client code every form imports (scaffold-spec `shared/`; F-25 translation engine; F-31).
**Depends on:** None (can start immediately).

### Forms

- None. This ticket builds client Modules only (no forms).

### Server functions

- None.

### Client modules

- `shared/globals.py` — every shared state variable of `globals-contract.md` (`booking_flow_state`, `order_flow_state`, the current-record variables, `session_user` projection), read with `getattr`, cleared on completion
- `shared/navigation_helpers.py`
- `shared/validation_utils.py`
- `shared/formatting_utils.py`
- `shared/translations/__init__.py` — `t()`, `t_plural()`, `translate_form()`; `formats.py`; `glossary.py`; phrase files `en.py`, `fr.py`, `es.py`, `de.py`, `it.py` (spec-translation)

### Tables

- None (state is client-side and never trusted by the server).

### Acceptance tests

- `ST-14` — the translation engine translates every fixed text; language gating per F-25
- `ST-05`, `LG-06` — code rules; relative imports only

### Security criteria

- B1: `globals.py` state is never trusted by the server — the server re-reads every record and re-checks every rule (design doc §23.1)
- `session_user` carries only role, name, email, preferences and `is_demo_visitor` — never the restricted `users` columns

### Notes

- Relative imports only (`from ...shared import formatting_utils`); no import by the app's package name.

## Comments
