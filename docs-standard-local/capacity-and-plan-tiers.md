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
| Cluster | Standard 2 GiB: 47 connections, 15 clients; Standard 4 GiB: 97 connections, 32 clients; Advanced 8 GiB with a standby: 197 connections, 65 clients; Advanced 16 GiB: 397 connections, 132 clients |

The other per-client limits (email, AI and voice, data transfer) are in [[[default:adr-global/adr-fair-use-limits]]](C:/dev/project-library-global/adr-global/adr-fair-use-limits.md).

## Typical Client

## Signs More Is Needed

- A container above 80% of its memory.
- A client's database above 1 GiB.
- The business's active client count reaching 12, 25 or 60.
- Requests waiting for a database connection, seen as slow responses in a client's heartbeat.

## Raising a Container or the Cluster

App 5 makes both raises itself and messages David ([[[default:adr-global/adr-automatic-resizing]]](C:/dev/project-library-global/adr-global/adr-automatic-resizing.md)):

- **Container:** above 80% memory use, App 5 raises the client's container from 1 GiB to 2 GiB. A 2 GiB container above 80% is not raised further; David decides.
- **Cluster:** App 5 raises the Standard cluster to 4 GiB at 12 active clients; at 25, once David approves, moves every client into an Advanced cluster with a standby, 8 GiB; and at 60 raises it to 16 GiB. Each stage runs in the next Sunday 06:00 UK maintenance window.
- App 5 never reduces a size.

## Planned High-Traffic Events

