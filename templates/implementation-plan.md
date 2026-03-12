---
title: "{Product} Implementation Plan"
domain: "{domain}"
category: implementation-plan
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: []
tags: [planning, execution]
---

# {Product} Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development
> (if subagents available) or superpowers:executing-plans to implement this plan.
> Steps use checkbox syntax for tracking.

**Goal:** {One sentence describing what this builds}

**Architecture:** {2-3 sentences about approach}

**Tech Stack:** {Key technologies/libraries}

---

## Module Dependency Graph

```
{Layer 0 components}
  → {Layer 1 components}
    → {Layer 2 components}
      → ...
```

---

## Phase 1: {Name}

### Task 1-1: {Component}

**Files:**
- Create: `{path}`
- Modify: `{path}`
- Test: `{path}`

**Ref:** {Design-Doc §N.N}

- [ ] **Step 1: Write the failing test**
- [ ] **Step 2: Run test to verify it fails**
- [ ] **Step 3: Write minimal implementation**
- [ ] **Step 4: Run test to verify it passes**
- [ ] **Step 5: Commit**

<!-- Repeat tasks for each component in this phase -->

---

## Estimation

### Code Volume
| Component | LOC Range |
|-----------|-----------|

### Effort
| Team Size | Duration | Cost Range |
|-----------|----------|------------|

---

<!-- PLAN CONVENTIONS:
- Every task references a design document section (Ref: field)
- Every step has exact file paths and expected output
- TDD by default: test → fail → implement → pass → commit
- Each task is one commit
-->
