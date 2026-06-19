# Dashboard Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** dashboard/ [Phase 1]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md, build-plan.md Stage 1.3

---

## 1. Wireframes Created

### 1.1 ActivityFeed

- **File:** `wireframes/dashboard/ActivityFeed/wireframe-dashboard-ActivityFeed.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `ActivityFeed` in `dashboard/` package [Phase 1]
  - `Clearly defined:` build-plan.md Stage 1.3: "Dashboard displays metrics (revenue, bookings, customers)"
  - `Clearly defined:` DESIGN.md Empty States section: "Activity feed (no activity) — Activity icon, No recent activity"
- **Form type:** Data-driven component — displays recent business activity
- **Components:** Heading (title), RepeatingPanel (activity items), empty state (4 required components)
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "notifications_none"), `hdg_empty_state` (Heading "No recent activity"), `txt_empty_state` (Text), `btn_empty_action` (Button "Go to Dashboard")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.2 DashboardForm

- **File:** `wireframes/dashboard/DashboardForm/wireframe-dashboard-DashboardForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `DashboardForm` in `dashboard/` package [Phase 1]
  - `Clearly defined:` build-plan.md Stage 1.3: "Dashboard displays metrics (revenue, bookings, customers — initially static)"
  - `Clearly defined:` DESIGN.md Component and Role Map: "metric-card role for KPI and dashboard metrics"
- **Form type:** Dashboard form — metric cards, recent bookings, activity feed
- **Components:** Heading (title), Text (subtitle), FlowPanel (metrics row with 4 MetricCards), FlowPanel (content row with recent bookings DataGrid and ActivityFeed), empty state
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "dashboard"), `hdg_empty_state` (Heading "Welcome to your dashboard"), `txt_empty_state` (Text), `btn_empty_action` (Button "Complete Onboarding")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.3 MetricCard

- **File:** `wireframes/dashboard/MetricCard/wireframe-dashboard-MetricCard.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `MetricCard` in `dashboard/` package [Phase 1]
  - `Clearly defined:` DESIGN.md Component and Role Map: "metric-card role — Elevated card for KPI and dashboard metrics"
- **Form type:** Reusable component — KPI card for dashboard metrics
- **Components:** Card (elevated, metric-card role), Text (label), Text (value), Text (trend)
- **Empty state:** Not applicable — display-only component, always populated
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** All 3 items in the task list received their own complete wireframe.

---

## 3. Specification Gaps

No specification gaps found. All 3 dashboard forms have clear architectural backing in scaffold-spec.md, build-plan.md Stage 1.3, and DESIGN.md.

---

## 4. Summary

| Item | Status |
|------|--------|
| Wireframes created | 3 (ActivityFeed, DashboardForm, MetricCard) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 3 wireframes pass all 15 checklist items |
| Specification gaps | None |

---

*End of report — 2026-06-19*
