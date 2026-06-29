Design workflow (To be confirmed)
1. /plan-design-review (First)
•	Purpose: Interactive design plan review
•	What it does: Rates design dimensions 0-10, identifies gaps, proposes fixes to the design plan
•	When: Before any implementation or HTML generation
•	For Phase 1: Review auth screens, dashboard, settings, vault layouts
2. /plan-eng-review (Second)
•	Purpose: Engineering/architecture review
•	What it does: Locks in execution plan - architecture, data flow, diagrams, edge cases, test coverage
•	When: After design review, before implementation
•	For Phase 1: Review auth flow, RBAC, Vault encryption, navigation structure
3. /design-html (Third)
•	Purpose: Generate production HTML/CSS
•	What it does: Creates Pretext-native HTML/CSS from approved designs
•	When: After design and engineering plans are locked
•	Output: Production-ready UI components
4. /design-review (Fourth)
•	Purpose: Visual QA and fixes
•	What it does: Finds visual inconsistencies, spacing issues, hierarchy problems, and fixes them
•	When: After HTML generation to audit and polish the output


