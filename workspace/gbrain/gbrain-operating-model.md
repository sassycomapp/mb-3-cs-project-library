# GBrain Operating Model

## Purpose
This document defines the operating role of GBrain in the Mybizz CS workspace.

GBrain is the advisory memory and recall layer for OpenCode and GStack workflows.
It is not the authoritative source of truth for code, documents, schema, architecture, or canonical shared tracking.

## Core principle
GBrain remembers approved and provenance-linked information so that future sessions can recover context quickly.

GBrain must not silently replace direct file reading when authoritative sources are available.

## Authority model
The authority order for this workspace is:

1. User decision in the active session.
2. Authoritative repo files and canonical tracking files.
3. Project and global OpenCode rules.
4. Validated current-session outputs.
5. GBrain memory.

If GBrain recall conflicts with current authoritative files, the files win.

## What GBrain is for
GBrain is intended to support:
- cross-session recall,
- recovery of prior validated decisions,
- retrieval of previous handoffs,
- memory of recurring workflow conventions,
- faster resumption after interrupted work.

## What GBrain is not for
GBrain must not:
- redefine project truth,
- override current repo content,
- bypass OpenCode permissions,
- auto-apply changes,
- silently blend stale and current information.

## Required memory behavior
Every retrieved memory item must expose:
- source path,
- source tier,
- authority label,
- ingested timestamp,
- freshness status,
- conflict warning if applicable.

## Ingestion rule
Only approved sources may be ingested.
Unapproved notes, speculative summaries, and unvalidated outputs must not be promoted into trusted memory.

## Freshness rule
GBrain memory must be revalidated or invalidated when its source changes or when its freshness policy expires.

## Safe use rule
GBrain may propose relevant prior context.
OpenCode agents must still read authoritative source files when making current decisions.

## Success criteria
GBrain is operational when:
- it can recall prior validated work with provenance,
- it can distinguish authoritative vs advisory memory,
- stale memory can be identified,
- current source files remain authoritative.