---
name: paf-plan
description: >
  PAF Phase 5: Implementation Planning. Use after paf-qa completes.
  Extracts buildable components from the design corpus, maps dependencies,
  groups into sprints, decomposes into bite-sized TDD tasks, produces
  effort estimation. Hands off to paf-execute.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, planning, tasks, estimation, sprints, design"
---

# PAF Implementation Planning (P5)

**Methodology reference:** Read `docs/paf/methodology/PAF-Phase-Guide.md` §P5 before proceeding.

## Prerequisites

- P4 exit gate passed (design corpus is implementation-ready)
- All design documents at `review` or `approved` status
- Know the assigned tier

## Procedure

### 1. Component Extraction

Walk every design document and extract every buildable component:

| Source Document | What to Extract |
|----------------|----------------|
| API Specification | Each endpoint group → task(s) |
| UI Design | Each screen → task(s) |
| Backend Architecture | Each database table, package, service boundary → task(s) |
| Functional Processes | Each process flow, state machine → task(s) |
| Security Architecture | Each auth mechanism, security control → task(s) |
| Frontend Architecture | Design system setup, component library, API client → task(s) |
| Operational | CI/CD pipeline, Docker images, monitoring → task(s) |

**T1 note:** At T1, most of these source documents won't exist. Extract tasks from whatever documents are available — typically Root Architecture (service boundaries, deployment, tech stack setup) and Decision Log (each decision with implementation implications). If Backend Architecture or API Specification were optionally included, extract from those too. The dependency mapping (§2) still applies; you'll just have fewer entry points.

### 2. Dependency Mapping

Order components by what must exist before other things can be built:

```
Foundation (project scaffold, config, database schema, design tokens)
  → Core domain logic (processes, state machines, business rules)
    → API layer (endpoints, middleware, auth, validation)
      → Frontend (design system, components, pages, API client)
        → Integration testing (cross-component E2E scenarios)
          → Operational (CI/CD, observability, release pipeline)
```

### 3. Sprint Grouping

Group components into phases. Each phase = 1–3 sprints (2-week cycles).

Expected scale by tier:
| Tier | Phases | Tasks | Sprints |
|------|--------|-------|---------|
| T1 | 2–3 | 15–30 | 3–5 |
| T2 | 5–8 | 40–80 | 6–12 |
| T3 | 10–20 | 150–300+ | 15–40 |

### 4. Task Decomposition

For each component, create a task following this structure:

```markdown
### Task {Phase}-{Number}: {Component Name}

**Files:**
- Create: `exact/path/to/file`
- Modify: `exact/path/to/existing`
- Test: `tests/path/to/test`

**Ref:** {Design-Document §Section}

- [ ] **Step 1: Write the failing test**
- [ ] **Step 2: Run test to verify it fails**
- [ ] **Step 3: Write minimal implementation**
- [ ] **Step 4: Run test to verify it passes**
- [ ] **Step 5: Commit**
```

**Task rules:**
- Every task references its design document section(s) via the **Ref** field
- Every step includes exact file paths
- TDD by default (test → fail → implement → pass → commit)
- Each task = one commit
- Steps are 2–5 minutes each

### 5. Cross-Reference Verification

After building the task list, verify bidirectional coverage:
- Every design document section is referenced by ≥1 task
- Every task references ≥1 design document section

If a design section has no corresponding task, either add a task or justify why it's not buildable (e.g., it's a design rationale section, not an implementation section).

### 6. Tooling Section

Read `docs/paf/methodology/PAF-Tooling-Guide.md`. Include a tooling recommendations section in the plan based on the product's tech stack and tier. Note synergies.

If a domain pack has `tooling_recommendations`, include those too.

### 7. Effort Estimation

Produce an estimation appendix:
- LOC estimates by component (use design doc section size and endpoint count as signals)
- Person-month ranges (pure human vs. AI-assisted, use 2–2.5x multiplier for AI)
- Timeline by team size (1, 2, 3-4, 5-6 developers)
- Cost ranges (use PAF-Tooling-Guide.md rate references or market rates)

Reference benchmarks: the ITSM proof case (T3) produced ~312 tasks across 37 sprints for 155 endpoints.

### 8. Save the Plan

Save to `docs/design/implementation-plan.md` using the template at `docs/paf/templates/implementation-plan.md`.

Use the `superpowers:writing-plans` skill format for task structure — that skill is the execution standard.

### 9. Exit Gate Checklist

- [ ] Every design section → ≥1 task
- [ ] Every task → ≥1 design section
- [ ] Dependency graph has no cycles
- [ ] Sprint grouping reviewed by user
- [ ] Estimation reviewed (sanity check)
- [ ] Plan saved
- [ ] **Declaration: "Plan is execution-ready"**

### 10. Handoff

Announce: "P5 complete. Plan saved to `docs/design/implementation-plan.md`." Present a summary of task count, sprint count, and effort estimate.
Ask: "Ready to proceed to P6 (Execution)? If you need time to review, you can resume later with `/paf-execute`."
Invoke `paf-execute` only after confirmation.

## When Things Go Wrong

- **Design section has no corresponding task:** Either add a task or justify why it's not buildable (e.g., it's rationale, not implementation). Document the justification.
- **Dependency cycle detected:** Present the cycle to the user. One dependency must be broken — identify which component can use a stub/interface initially.
- **Estimation seems unrealistic:** Cross-check against the ITSM proof case benchmarks (T3: ~312 tasks, ~37 sprints, ~155 endpoints). Adjust if the product's complexity differs.
- **Task decomposition produces too many tasks:** Group related micro-tasks into composite tasks. Each composite should still be ≤1 day of work.

## Related Skills

Works well with: `paf-qa`, `paf-execute`, `superpowers:writing-plans`
