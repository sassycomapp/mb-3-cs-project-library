---
title: "incident-response-plan"
doc-id: "mb-aafcc135"
updated: "2026-11-11"
---
# Mybizz Consulting Services — Incident Response Plan

How a security incident is found, contained and resolved, and the notices the client's privacy law requires: Mybizz tells the client without undue delay, and the client notifies its supervisory authority within 72 hours and the people affected where the law requires it.

**Standard:** [[[default:specifications-global/spec-security-architecture]]](C:/dev/project-library-global/specifications-global/spec-security-architecture.md)

## Roles

- **Mybizz is the controller** of its own clients' details (the client registry and billing records), and **the processor** of the personal data each client holds in its client instance.
- **David** leads every incident. No outside supplier is engaged before the insurer has been called (Containment, step 3).

## Detection

An incident is any event that may expose, change or destroy data, or stop a client's service, through a security cause. It is detected by:

- an App 5 alert: a heartbeat lost, an error spike, an unexpected change in a client's container or database, a failed restore drill;
- a supplier's security notice (DigitalOcean, Anvil, AWS, Cloudflare, Paddle, GitHub, Groq);
- a report to `abuse@mybizz.live`, or from a client or a member of the public;
- a sign-in alert from a supplier account.

On detection, David opens an incident record in App 5. The record carries the timeline: detected, contained, insurer called, Information Commissioner's Office reported, clients notified, closed, each with its time.

## Containment

1. Stop the harm: suspend the affected client instance with App 5's **Suspend** action, revoke or rotate any exposed key or token, and remove any unknown access.
2. Keep the evidence: do not delete logs, containers or data that show what happened. Record in the incident record what was seen and what was done.
3. Call the insurer's 24-hour incident line, given in the business insurance policy, before engaging any outside supplier. The insurer appoints the incident response specialists the policy pays for.
4. Follow the insurer's incident response team's direction on further containment.

## Notification

| Who | When | By whom |
|---|---|---|
| The insurer | At once, before any outside supplier is engaged | David |
| Each affected client | Without undue delay, with what is known, the likely effects and the measures taken | David, from App 5 |
| The Information Commissioner's Office | Within 72 hours of becoming aware of a breach of personal data that Mybizz controls, unless the breach is unlikely to result in a risk to people | David |
| The client's supervisory authority and the people affected, for data in the client's instance | Within the time the client's own privacy law sets; in the UK, within 72 hours | The client, with the information Mybizz gives it |

Each notice is recorded in the incident record with its time.

## Recovery

- Restore affected data from DigitalOcean's backups or from the client's locked nightly copy in its own R2 bucket ([[[default:security-global/infrastructure/sec-backup-recovery-integrity|Backup and Recovery Integrity]]](C:/dev/project-library-global/security-global/infrastructure/sec-backup-recovery-integrity.md)).
- Where the whole platform is affected, run App 5's **Rebuild** action ([[[default:specifications-global/spec-deployment|Deployment Specification]]](C:/dev/project-library-global/specifications-global/spec-deployment.md) section 10).
- Release a fixed image through the normal staged release.
- Return each suspended client instance to service once it is clean, and tell the client.

## Review

Within 14 days of closing the incident, David records in the incident record what happened, why, what changed to stop it happening again, and whether any fixed security finding needs a regression test. The insurer receives the evidence its claim process asks for.

