---
title: "README"
doc-id: "mb-73c33e9a"
updated: "2026-11-06"
---
# docs-standard-local

The standard documents every Mybizz project keeps, canonical to this project. Each is modelled on its standard in `project-library-global`, named on the document. A document here states this project's facts only; the rules stay in the global standard.

| Document | What it states |
|---|---|
| `business-requirements-document.md` | Why the business needs the application |
| `product-requirements-document.md` | What the product does, for whom |
| `software-requirements-specification.md` | The testable requirements |
| `globals-contract.md` | The client-side state in `globals.py` |
| `authoritative-schema.md` | Every data table and column |
| `custom-component-requirements-matrix.md` | Every custom component and why it is needed |
| `scaffold-spec.md` | The packages, forms, modules, navigation and startup |
| `api-specification.md` | Every HTTP endpoint: webhooks and management endpoints |
| `integration-architecture.md` | Every outside service and how it is connected |
| `third-party-services-register.md` | Every outside service, the data it receives and its agreement |
| `client-storage.md` | The client's own R2 buckets: what they hold and how it is keyed |
| `vault-implementation.md` | Where every secret is kept and who reads it |
| `notification-system.md` | Every email and alert the application sends |
| `observability-requirements.md` | Logging, metrics, alerts and the heartbeat to App 5 |
| `capacity-and-plan-tiers.md` | A client's use against its Anvil plan tier |
| `data-retention-schedule.md` | How long each kind of data is kept |
| `backup-and-recovery.md` | Backups to the client's buckets, and restoring them |
| `incident-response-plan.md` | Handling a security incident, and the POPIA notices it requires |
| `onboarding-implementation-plan.md` | Setting up a new client |
| `client-exit-handover.md` | What a leaving client receives |
| `deployment-procedures.md` | Releasing from App 1 through App 2, and rolling back |
