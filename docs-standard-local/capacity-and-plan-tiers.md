---
title: "capacity-and-plan-tiers"
doc-id: "mb-e61ea2cc"
updated: "2026-11-17"
---
# Mybizz Consulting Services — Capacity

A typical client's use against the limits of its container and its share of Mybizz's database service, what shows that a client, a server or the database service needs more, and how App 5 flags it.

**Standard:** [[[default:specifications-global/spec-client-instance-hosting]]](C:/dev/project-library-global/specifications-global/spec-client-instance-hosting.md)

## Client Instance Limits

| Resource | Limit |
|---|---|
| Container | 1 GiB memory, raised to 2 GiB by App 5 above 80% use |
| Database connections | 3 per client |
| Database query time | 60 seconds per query |
| Idle open transaction | 120 seconds |
| Database size | Warning at 1 GiB; limit at 2 GiB |
| Application server | 12 places; a 1 GiB container takes one place and a 2 GiB container two |
| Database service | Ubicloud, with a standby throughout: `standard-2` to client 59, `standard-4` from 60, `standard-8` from 130, counting every Mybizz business |

The other per-client limits (email, AI and voice, data transfer) are in [[[default:adr-global/adr-fair-use-limits]]](C:/dev/project-library-global/adr-global/adr-fair-use-limits.md).

## Typical Client

## Signs More Is Needed

- A container above 80% of its memory.
- A client's database above 1 GiB.
- The business's last application server down to 2 free places.
- The active client count of every Mybizz business together reaching 60 or 130.
- Requests waiting for a database connection, seen as slow responses in a client's heartbeat.

## Raising a Container, a Server or the Database Service

App 5 makes every raise itself and messages David ([[[default:adr-global/adr-automatic-resizing]]](C:/dev/project-library-global/adr-global/adr-automatic-resizing.md)):

- **Container:** above 80% memory use, App 5 raises the client's container from 1 GiB to 2 GiB. A 2 GiB container above 80% is not raised further; David decides.
- **Server:** when the last application server has 2 free places, App 5's **Add server** action adds a server.
- **Database service:** App 5 raises the service to `standard-4` at 60 active clients and to `standard-8` at 130. Each stage runs in the next Sunday 06:00 UK maintenance window, with no client moved.
- App 5 never reduces a size.

## Planned High-Traffic Events

