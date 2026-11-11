---
title: "capacity-and-plan-tiers"
doc-id: "mb-e61ea2cc"
updated: "2026-11-11"
---
# Mybizz Consulting Services — Capacity

A typical client's use against the limits of its container and its share of the business's database cluster, what shows that a client or the cluster needs more, and how App 5 flags it.

**Standard:** [[[default:specifications-global/spec-client-instance-hosting]]](C:/dev/project-library-global/specifications-global/spec-client-instance-hosting.md)

## Client Instance Limits

| Resource | Limit |
|---|---|
| Container | 1 GiB memory, raised to 2 GiB by App 5 above 80% use |
| Database connections | 3 per client |
| Database query time | 60 seconds per query |
| Idle open transaction | 120 seconds |
| Database size | Warning at 1 GiB; limit at 2 GiB |
| Cluster | 8 GiB: 197 connections, 65 clients; 16 GiB: 397 connections, 132 clients |

The other per-client limits (email, AI and voice, data transfer) are in [[[default:adr-global/adr-fair-use-limits]]](C:/dev/project-library-global/adr-global/adr-fair-use-limits.md).

## Typical Client

## Signs More Is Needed

- A container above 80% of its memory.
- A client's database above 1 GiB.
- The business's active client count reaching 60.
- Requests waiting for a database connection, seen as slow responses in a client's heartbeat.

## Raising a Container or the Cluster

App 5 makes both raises itself and messages David ([[[default:adr-global/adr-automatic-resizing]]](C:/dev/project-library-global/adr-global/adr-automatic-resizing.md)):

- **Container:** above 80% memory use, App 5 raises the client's container from 1 GiB to 2 GiB. A 2 GiB container above 80% is not raised further; David decides.
- **Cluster:** at 60 active clients, App 5 raises the cluster and its standby from 8 GiB to 16 GiB in the next Sunday 06:00 UK maintenance window.
- App 5 never reduces a size.

## Planned High-Traffic Events

