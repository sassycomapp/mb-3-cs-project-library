---
title: "anvil-ui-build-prompt-template"
doc-id: "mb-f6ba2b48"
updated: "2026-10-01"
---
# Anvil Build Prompt — Template

This is the reusable wrapper only. It contains no information about any specific UI —
the variable section below gets filled in fresh for each of the 70 builds, using the
exception notes produced by the Per-UI Exception Review Procedure (separate document).

---

## One-time bootstrap (do this once, before UI #1 — not part of the per-UI prompt, and not an agent-authoring task)

The MUI overlay CSS doesn't need to be authored by the agent from a text description —
a complete, correct, deployment-ready `theme_patched.css` already exists (including
reduced-motion support). The one-time bootstrap is simply:

1. Replace this app's `Assets/theme.css` with the provided `theme_patched.css` file
   directly.
2. Confirm in the Anvil Editor that the replacement took (check a known selector, e.g.
   `.anvil-role-elevated-card`, is present in the Assets view).

No prompt to Anvil's AI agent is needed for this step — it's a direct file replacement
you do yourself in the Assets panel.

Role assignment (the mechanism that makes this CSS actually apply to built components)
is decided and covered in the Standard §6.0 — set programmatically in code, per ADR-43
in `adr/43-role-property-assignment-mechanism.md`. No separate instruction is needed here; it's part of
the per-UI prompt's general "follow the Standard" instruction below.

---

## Per-UI prompt template

```
Read anvil-ui-build-standard.md in this app's Assets before starting, and follow it
exactly — naming, tokens, approved components, property fidelity (§6), form-boundary
rules (§9), and icon matching (§10) all apply.

Build the Form named {{FORM_NAME}} in the {{PACKAGE}} package from this wireframe:

{{WIREFRAME_HTML}}

{{EXCEPTION_NOTES}}

Before finishing, confirm against the Standard's Final QA Gate (§12), and list anywhere
the screenshot and wireframe disagreed and how you resolved it.
```

**Placeholders:**
- `{{FORM_NAME}}` — e.g. `AvailabilitySettingsForm`
- `{{PACKAGE}}` — e.g. `bookings`
- `{{WIREFRAME_HTML}}` — the wireframe file's content, pasted verbatim, unedited
- `{{EXCEPTION_NOTES}}` — output of the Exception Review Procedure for this specific UI; often empty, since the Standard now covers most recurring cases on its own

---

*If you rename the Standard file or keep a version suffix in its filename, update the
`Read ...` line above to match.*
