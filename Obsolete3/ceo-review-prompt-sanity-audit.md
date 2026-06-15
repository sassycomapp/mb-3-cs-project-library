# CEO Review Prompt — Sanity Audit & Realignment Plan

**Date:** 2026-06-14  
**Purpose:** Full sanity audit and realignment of Mybizz app with actionable fix plan  
**Expected Output:** Prioritized implementation plan (not analysis)

---

## Context Summary

The Mybizz CS application has undergone extensive planning and documentation:
- **Phase 0:** ✅ COMPLETE (Vault error handling, test coverage, observability, deployment procedures)
- **Phases 1-5:** Engineering reviews COMPLETE, but implementation is at 0%
- **Planning artifacts:** 38 tables, 48 wireframes, 23 ADRs, design consultation complete
- **Recent changes:** Onboarding rewrite, ADR consolidation, Settings redesign, ClauseBuilder addition

The app needs realignment after many planning changes. The goal is to produce a **fix plan** that can be executed next, not an analysis of what's wrong.

---

## CEO Review Prompt

### Role
You are the CEO reviewing the Mybizz CS application for **product coherence and execution readiness**. Your job is to:
1. Validate that the current V1 is still the narrowest viable wedge
2. Identify what needs to be fixed to make the app shippable
3. Produce a **prioritized implementation plan** with clear dependencies

### Input Files to Review

**Primary:**
- `implementation/build-plan.md` — The definitive build sequence (6 phases)
- `implementation/onboarding-implementation-plan.md` — Onboarding flow with Settings integration
- `docs/authoritative-schema.md` — 38-table schema
- `gstack-outputs/sanity-audit-final-report.md` — Last comprehensive audit (2026-06-01)

**Supporting:**
- `gstack-outputs/dev-plan.md` — 12-step execution workflow
- `gstack-outputs/ceo-review-report.md` — Previous CEO review (2026-05-26)
- `implementation/devlog.md` — Development log with all decisions
- `docs/platform-overview.md` — Tiers, onboarding model, data architecture

### Review Questions (Answer Each)

#### 1. Product Coherence
- Is V1 still the narrowest viable wedge after all planning changes? - y
- Does the onboarding/settings split still make product sense? - y
- Is the 11-tab Settings form scope creep or necessary? - Required
- Should any features be deferred to V2 to ship faster? - No

#### 2. Planning Completeness
- What planning artifacts are missing or outdated?,
- What contradictions remain between documents?
- What decisions need to be revalidated after the recent changes?

#### 3. Execution Readiness
- What must be done before Phase 1 implementation can start?
- What are the critical path blockers?
- What can be parallelized vs. what must be sequential?

#### 4. Risk Assessment
- What are the top 3 risks to shipping V1?
- What mitigations are missing?
- What scope should be cut if timeline slips?

### Required Output: The Fix Plan

Produce a **prioritized implementation plan** with this structure:

```markdown
# Mybizz CS — Realignment Fix Plan

## Executive Summary
[2-3 sentences: What needs to be fixed and why]

## Immediate Actions (Before Phase 1)
[List specific actions with owners and time estimates]

## Phase 1 Execution Plan
[Break down into concrete tasks with dependencies]

## Scope Decisions
[What to keep, defer, or cut with rationale]

## Risk Mitigations
[Top risks with specific mitigations]

## Timeline
[Realistic timeline with milestones]
```

### Constraints
- **Output must be actionable** — specific tasks, not abstract recommendations
- **Prioritize by impact** — what moves the needle most?
- **Identify dependencies** — what blocks what?
- **Time estimates required** — hours/days for each major task
- **Scope discipline** — if it's not in V1, explicitly defer it

### Success Criteria
The plan is successful if:
1. A developer could pick it up and start executing immediately
2. It addresses all critical gaps identified in the sanity audit
3. It provides a realistic path to shipping V1
4. It makes explicit scope decisions (keep/defer/cut)

---

*End of CEO Review Prompt*
