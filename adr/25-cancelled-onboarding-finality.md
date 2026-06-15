# 25 — Onboarding Finality
[Cancelled]
Date: 2026-05-31
Source: Plan-tune cross-reference audit — formalising onboarding design decision

---

## Context

The Mybizz CS onboarding process was originally designed as a replayable flow with separate bespoke forms. The revised onboarding design (per the settings-based onboarding decision) establishes that onboarding is a one-time, non-replayable experience with no separate forms.

The question arose of what happens after onboarding is completed, and whether the Owner can re-enter or replay the onboarding process.

---

## Decision

**Onboarding is a one-time, non-replayable process.**

Once the Owner completes onboarding (defined as: the Legal Compliances switch is enabled and minimum required fields are filled), the onboarding state is locked. There is no in-app replay, no re-entry point, and no Settings → Onboarding re-entry flow.

### Architectural Rules

1. **Completion is final:**
   - Onboarding completes when the Legal Compliances switch is enabled and minimum required fields are filled
   - After completion, `business_profile.onboarding_status` is set to `"completed"` and `onboarding_completed` is set to `True`
   - These fields become read-only after completion

2. **No replay UI:**
   - There is no "Replay onboarding" button
   - There is no "Back to Onboarding" link in Settings
   - The Settings form is always accessible, but the onboarding guidance overlay does not reappear

3. **Post-completion changes:**
   - Operational settings (palette, services, display currency, website content) are changed via Settings at any time
   - System Currency cannot be changed after the first transaction (ADR-16)
   - Legal acknowledgements are immutable once accepted
   - Payment gateway can be changed under approved platform rules (ADR-24)
   - Any rare exception requiring onboarding data modification must be requested via Mybizz support

4. **Mybizz_management amendment log:**
   - Mybizz_management maintains an append-only amendment log of credential and configuration changes
   - It never overwrites — it always holds the latest values plus the full history
   - This is outside the client instance scope but is a system-level behaviour

---

## Consequences

### Positive
- Simplified onboarding flow with clear completion state
- No confusion about where to make post-onboarding changes
- Clean separation between onboarding (one-time) and Settings (ongoing)
- Legal and compliance posture is clear and auditable

### Negative
- Owner cannot replay onboarding to review or change initial choices
- Any error in initial setup (e.g., wrong business name) requires Mybizz support
- System Currency is permanently locked after first transaction

### Neutral
- Settings form remains the single interface for all configuration
- Onboarding is a guided first-run experience of Settings, not a separate system

---

## Related Documents

- `adr/17-onboarding-vs-settings-boundary.md` — Settings as the single configuration surface
- `adr/16-system-currency-selection-and-immutability.md` — Currency immutability
- `adr/24-payment-gateway-mutability.md` — Gateway change rules
- `adr/23-onboarding-resumability.md` — Progress preservation during onboarding
- `implementation/onboarding-implementation-plan.md` — Settings-based onboarding implementation

---

*End of ADR-025*
