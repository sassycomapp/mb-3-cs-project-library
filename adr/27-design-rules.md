# ADR 027: Design Rules

- Status: Accepted
- Date: 2026-06-10

## Context

The project requires a single authoritative design-rules document to govern how design artifacts are created and reviewed.

## Decision

The design rules are defined as follows:

1. Do not use the legacy Label component in Material 3 wireframes; use only approved Material 3 text components.
2. Architecture determines wireframes, and wireframes determine screens. Screens do not determine wireframes.
3. FlowPanels are for arranging components in horizontal lines with wrapping. ColumnPanels are for vertically stacked components.
4. Every user-visible text element shown or implied in a wireframe must be represented by its own explicit component in the wireframe. No visible heading, label, caption, helper text, or section title may exist only as implied text.
5. The wireframe properties table must list only properties that can only be set in the visual designer. Any property that can be set or overridden programmatically is not allowed in the properties table.
6. All behaviour, data, layout logic, and explanatory or descriptive text belong in the specification documents and must not appear anywhere in the wireframe, including notes, captions, or annotations.
7. Writeback (W) must be set in Designer for DatePicker and any other components requiring text input.
8. No sample data may be included in wireframes.
9. All components used must be approved Anvil Material 3 Theme components.

## Consequences

- Wireframes remain strictly limited to UI construction information.
- Specifications remain the authoritative location for behaviour, data rules, and explanatory detail.
- Reviews can reject non-compliant wireframes against a single accepted project decision.
- Future changes to these rules should be made by updating or superseding this ADR.
