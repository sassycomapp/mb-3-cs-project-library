# Invoices Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** invoices/ [Phase 2]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md, build-plan.md Stage 2.5

---

## 1. Wireframes Created

### 1.1 InvoiceEditorForm

- **File:** `wireframes/invoices/InvoiceEditorForm/wireframe-invoices-InvoiceEditorForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `InvoiceEditorForm` in `invoices/` package [Phase 2]
  - `Clearly defined:` DESIGN.md InvoiceEditorForm Scope: "Create or edit invoices, supports create and edit workflows"
  - `Clearly defined:` DESIGN.md Tax Handling: "15% SA VAT, display-time calculation"
- **Form type:** Editor form — create/edit invoices with customer selection, dates, currency, line items, notes
- **Components:** Page title, customer DropdownMenu (W), invoice date DatePicker (W), due date DatePicker (W), currency DropdownMenu (W), line items DataGrid, add line item Button, notes TextArea (W), action buttons (Cancel, Save Draft, Send Invoice)
- **Empty state:** Not applicable — editor form, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.2 InvoiceListForm

- **File:** `wireframes/invoices/InvoiceListForm/wireframe-invoices-InvoiceListForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `InvoiceListForm` in `invoices/` package [Phase 2]
  - `Clearly defined:` DESIGN.md InvoiceListForm Scope: "Search by invoice number or customer name, filter by status, row actions"
  - `Clearly defined:` DESIGN.md Empty States: "Invoice list — Receipt icon, No invoices yet"
- **Form type:** List form — DataGrid with search, status filter, row actions
- **Components:** Page title, search TextBox (W), status DropdownMenu (W), create invoice Button, DataGrid (7 columns), empty state (4 required components)
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "receipt"), `hdg_empty_state` (Heading "No invoices yet"), `txt_empty_state` (Text), `btn_empty_action` (Button "+ Create Invoice")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.3 InvoiceViewerForm

- **File:** `wireframes/invoices/InvoiceViewerForm/wireframe-invoices-InvoiceViewerForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `InvoiceViewerForm` in `invoices/` package [Phase 2]
  - `Clearly defined:` DESIGN.md InvoiceViewerForm Completeness: "Invoice number, status badge, Bill To, dates, line items, totals, notes, status-appropriate actions"
  - `Clearly defined:` DESIGN.md Status-based permitted actions table for InvoiceViewerForm
  - `Clearly defined:` globals-contract.md: `current_invoice` set by InvoiceListForm, read by InvoiceViewerForm
- **Form type:** Viewer — read-only detail with status-dependent action buttons
- **Components:** Invoice title + number + status badge, Bill To section (contact name, email, phone, address), dates section (invoice date, due date, currency), line items DataGrid, totals (subtotal, tax 15%, total), notes, action buttons (Cancel Invoice, Download PDF, Record Payment)
- **Empty state:** Not applicable — single-entity viewer. If `globals.current_invoice` is None, redirect to InvoiceListForm.
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** All 3 items in the task list received their own complete wireframe.

---

## 3. Specification Gaps

| Gap | Impact | Resolution |
|-----|--------|------------|
| InvoiceEditorForm line item DataGrid column configuration not in DESIGN.md | Minor — columns inferred from invoice_items schema | Columns: Description (TextBox W), Quantity (TextBox W), Unit Price (TextBox W), Amount (calculated) |

---

## 4. Summary

| Item | Status |
|------|--------|
| Wireframes created | 3 (InvoiceEditorForm, InvoiceListForm, InvoiceViewerForm) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 3 wireframes pass all 15 checklist items |
| Specification gaps | 1 (minor, resolved) |

---

*End of report — 2026-06-19*
