---
title: "deployment-procedures"
doc-id: "mb-f46b55a9"
updated: "2026-11-12"
---
# Mybizz CS — Deployment Procedures

How a release moves from App 1 through App 2 and the release image to the client instances, and how it is rolled back.

**Standard:** [[[default:specifications-global/spec-deployment]]](C:/dev/project-library-global/specifications-global/spec-deployment.md)

**Authority:** [[[default:specifications-global/spec-client-instance-hosting|Client Instance Hosting]]](C:/dev/project-library-global/specifications-global/spec-client-instance-hosting.md), [[[default:adr-global/adr-dependency-update-model|Dependency Update Model]]](C:/dev/project-library-global/adr-global/adr-dependency-update-model.md) ADR. Where a technical fact here differs from `mb-3-cs-architecture-reference.md`, the architecture reference is correct.

---

## Deployment Model

| App | Role in deployment | Hosted on |
|---|---|---|
| App 1 — `mb-3-cs-app1` | Development and testing; each release is tagged here (`vMAJOR.MINOR.PATCH`) | Mybizz's Anvil Business account |
| App 2 — `mb-3-cs-app2`, `master_template` | The release: holds exactly App 1's latest released tag | Mybizz's Anvil Business account |
| App 3 — `mb-3-cs-app3`, `blank_client_template` | The client app: the released table definitions, the startup module and the scheduled tasks | Mybizz's Anvil Business account |
| Release image — `mb-3-cs-app4` | App 3, App 2 and `m3` on the pinned Anvil App Server, built once per release, stored under the release tag | DigitalOcean Container Registry |
| App 4 — `[client-name]-cs` | Each client instance: one container running the release image, with its own database | DigitalOcean App Platform and Managed PostgreSQL |
| Test client instance | A client instance of its own, holding test data only | DigitalOcean App Platform and Managed PostgreSQL |
| Demo apps — `demo.mybizz.live`, `try.mybizz.live` | Two instances of the release for prospects (F-36), reset nightly | Mybizz's Anvil Business account |
| App 5 — `mb-3-cs-app5` | Receives heartbeats and usage reports; runs the New client, Release and Remove actions; sends release notes | Mybizz's Anvil Business account |

### The update flow

```
App 1 (tagged release, e.g. v1.4.0)
    ↓ the developer pushes the tag into App 2's repository (Step 42)
App 2 — master_template + App 3 — blank_client_template
    ↓ built into the release image
Test client instance
    ↓ App 5's Release action
Every client instance
```

- Server logic, forms, UI and new tables and columns reach every client instance through the release image; there is no per-client update action.
- No form or server module is ever added to App 3; one would override `master_template` for every client (reference section 2). The image build stops on it.
- Testing before release happens on the local Anvil App Server, on the release image and on the test client instance. There is no staging environment.

### Release steps

1. **Feature complete** in App 1 and signed off (`build-verified`).
2. **Release ready:** every test passes and no High or Critical finding is open (Step 41).
3. **Tag** the release in App 1.
4. **Back up** every client instance to its own R2 bucket.
5. **Push** the tag into App 2's repository, and the release's table changes into App 3's table definitions (Step 42). Only the developer performs this.
6. **Build** the release image; the table-change check and the App 3 check pass; the whole test suite passes on the image.
7. **Release to the test client instance** and pass its smoke test (Step 43).
8. **Release to every client instance** with App 5's Release action, after the developer approves (Step 44).
9. **Verify** (Post-Deploy Verification).
10. **Notify:** App 5 records the release and sends each client the release note by email. The note is informational: the update is already live.

### Staged rollout (every release)

1. App 5's Release action verifies the release image's signature, then moves the test client instance to the image, by its digest.
2. Then 10% of client instances.
3. Then every client instance.

Each stage passes its health check before the next starts: every container in the stage runs the new image by its digest, sends a heartbeat within 30 minutes and shows no rise in errors. A failed health check rolls that stage back automatically, stops the release and messages Mybizz ([[[default:adr-global/adr-dependency-update-model|Dependency Update Model]]](C:/dev/project-library-global/adr-global/adr-dependency-update-model.md)).

---

## Schema Changes

Each client's App Server builds and extends the client's tables from App 3's table definitions.

### Rules

1. The first release sets the full schema. Every later release only adds tables and nullable columns; it never removes, renames or retypes a table or column.
2. Every schema change is made first in App 1 (Step 17), recorded in `schema-migration-log.md`, and written into App 3's table definitions at Step 42.
3. The table-change check in `mb-3-cs-app4` stops any release that breaks rule 1.
4. Code that reads a new column handles rows where it is empty.
5. A column that is no longer used stays in place, unused.

### Checklist

- [ ] Change recorded in `schema-migration-log.md`
- [ ] Release note describes it
- [ ] App 3's table definitions updated
- [ ] Code handles rows where the new column is empty
- [ ] Table-change check passed
- [ ] Tested on the test client instance before any client instance
- [ ] The heartbeat's schema fingerprint confirms the change on every instance

---

## Post-Deploy Verification

After every release:

1. The test client instance shows the new release and passes its smoke test: sign-in, a booking with payment in test mode, an invoice, a transactional email through SES, an email received in the Team Inbox. The demo apps show the new release.
2. Every instance's next heartbeat reports the new release version and the expected schema fingerprint (`observability-requirements.md`).
3. `error_log` across instances shows no new error type in the first hour.
4. Background task status in the heartbeats shows every task running.
5. App 5's dashboard shows no instance missing a heartbeat.

---

## Rollback

Rollback moves client instances back to the previous release image, not a code revert.

1. Identify the last good release tag.
2. App 5 moves the affected client instances to that release image, with the App Server's `auto-migrate` off and `ignore-invalid-schema` on.
3. Verify on the test client instance and in the heartbeats.
4. App 5 sends each client a short note.
5. The next release switches `auto-migrate` back on.

A release only adds tables and nullable columns, so the previous release runs on the newer schema and no data is lost.

---

## Rollback Triggers

**Immediate rollback** (do not wait for investigation):
- Error rate above 10% across instances
- Critical functionality broken: sign-in, booking, payment, invoicing
- Data corruption detected
- A security vulnerability introduced

**Considered rollback** (investigate, then decide):
- Error rate above 5% across instances
- Response times more than 50% slower
- A rising number of support tickets about the release

---

## Deployment Checklist

### Before the release
- [ ] Every feature of the release has passed `build-verified`
- [ ] The whole test suite passes on the release image ([[[default:security-global/requirements/sec-requirements-pre-merge-master-template|Pre-Merge Requirements for master_template]]](C:/dev/project-library-global/security-global/requirements/sec-requirements-pre-merge-master-template.md))
- [ ] No High or Critical finding open (Step 41)
- [ ] No High or Critical image finding older than 14 days, and no end-of-life software in the image
- [ ] The release image is signed
- [ ] Release note written
- [ ] Schema change recorded and the table-change check passed, if the schema changes
- [ ] Vault and integration changes documented, if any
- [ ] Every client instance backed up to its R2 bucket

### The release
1. [ ] Tag pushed into App 2, and table changes into App 3 (Step 42)
2. [ ] Release image built, checked and tested
3. [ ] Test client instance on the release, smoke test passed (Step 43)
4. [ ] App 5's Release action run (Step 44)
5. [ ] Post-deploy verification passed
6. [ ] Error rates watched for 1 hour
7. [ ] Release note sent from App 5

### After the release
- [ ] Background task health checked in App 5
- [ ] Support tickets about the release reviewed after 24 hours
- [ ] Problems found recorded against the release in App 5

---

## Account-Level Failures

The structure has two levels of risk:

- **Mybizz's Anvil account** holds App 1, App 2, App 3, App 5 and the demo apps. Client instances keep running without it; releases and provisioning wait until it is back.
- **Mybizz CS's DigitalOcean project** holds the cluster, the test client instance and every client instance. A DigitalOcean failure in `LON1`, or a suspension of Mybizz's DigitalOcean account, stops every client.

| Scenario | Detection | Recovery | Prevention |
|---|---|---|---|
| Anvil account suspended | Email from Anvil; App 1, App 2 and App 5 unavailable | Client instances keep running; contact Anvil support at once; releases and provisioning wait | Billing kept current with a backup payment method; Anvil terms followed |
| One client container fails | Heartbeats from its instance stop; App 5 raises a critical alert | App Platform restarts it; if it does not recover, App 5 recreates the container from the current release image | Container memory watched in App 5 (F-33) |
| Cluster main node fails | Database errors in heartbeats | The standby node takes over automatically; clients are back within minutes | Standby node from the first client |
| DigitalOcean region outage or account suspension | Every client's heartbeats stop; DigitalOcean Uptime reports App 5's status endpoint fault | App 5's **Rebuild** action: a new Advanced cluster and new containers in another region, each client restored from DigitalOcean's backups or, when those are unreachable, from its locked nightly copy ([[[default:specifications-global/spec-deployment\|Deployment Specification]]](C:/dev/project-library-global/specifications-global/spec-deployment.md) section 10) | Nightly R2 copies locked for 30 days; the rebuild tested before the first paying client; a second payment card and billing alerts on the account |
| Mybizz's AWS account or SES sending suspended | SES errors on every send; App 5 raises a critical alert | Contact AWS support; outgoing email is queued in each instance's retry queue and sent when sending returns; senders' mail servers retry the delivery of received email for several days | Each client's SES tenant reputation policy pauses that client alone; monthly email caps per client; two CloudWatch alarms below Amazon's review levels; a hardware security key on the AWS account |
| Any account compromised | Unexpected commits in GitHub, unknown collaborators, unknown sessions | Follow `incident-response-plan.md`: contain, call the insurer's incident line, rotate every Bearer token, container key and Vault credential that may be exposed, audit Git history and revert unauthorised commits, and tell affected clients without undue delay | A hardware security key on every supplier account; two scoped DigitalOcean keys; the cluster's trusted sources; locked nightly copies; no collaborators beyond Mybizz |

Recovery expectations are stated in the client terms.

---

*End of file — Deployment Procedures*
