---
title: "backup-and-recovery"
doc-id: "mb-24b2e108"
updated: "2026-11-11"
---
# Mybizz Consulting Services — Backup and Recovery

What is backed up, how often, to which of the client's buckets, and how a client instance is restored.

**Standard:** [[[default:adr-global/adr-client-r2-storage-and-exit-handover]]](C:/dev/project-library-global/adr-global/adr-client-r2-storage-and-exit-handover.md)

## What Is Backed Up

| Copy | Holds | Where | Kept |
|---|---|---|---|
| DigitalOcean cluster backup | Every client database in the cluster | Inside DigitalOcean | 7 days, with point-in-time recovery |
| Daily copy | The client's full data | The client's own R2 bucket, under `backups/daily/`, in the client's own Cloudflare account | The last 7, each locked for 7 days |
| Weekly copy | The client's full data | The same bucket, under `backups/weekly/` | The last 4, each locked for 28 days |
| Pre-release copy | The client's full data | The same bucket, under `backups/daily/` | As a daily copy |

While a copy is locked, no key, Mybizz's included, can delete or overwrite it.

The client's files, archived rows and email bodies already live in its R2 bucket and are not copied again.

## Schedule

- DigitalOcean backs up the cluster daily.
- Each client instance copies its data to its R2 bucket every night, and again before each release.
- App 5 checks every day that each client's nightly copy arrived, with the right date and a plausible size, and messages David when one is missing.

## Restoring

- **One client, recent loss:** restore the client's database from DigitalOcean's point-in-time recovery to the moment before the loss.
- **One client, older loss or a damaged DigitalOcean backup:** restore the client's database from its latest good nightly copy.
- **The whole platform:** App 5's **Rebuild** action restores every client into a new cluster and new containers in another region, from DigitalOcean's backups or each client's nightly copy ([[[default:specifications-global/spec-deployment|Deployment Specification]]](C:/dev/project-library-global/specifications-global/spec-deployment.md) section 10). At most 24 hours of data is lost when the nightly copy is used.

## Testing the Restore

- **Monthly restore drill:** the test client instance restores one client's nightly copy into a scratch database in the cluster, compares its row counts with the source, deletes the scratch database and reports the result to App 5.
- **Lock test:** an attempt to delete a locked copy under `backups/` must fail (test RL-18).
- **Rebuild test:** before the first paying client, the rebuild restores the test client instance in another region.

