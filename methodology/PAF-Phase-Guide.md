# PAF Phase Guide

This document contains the complete procedure for each of PAF's 6 phases. Follow them in order. Each phase has entry criteria, activities, artifacts, and an exit gate.

For document type details, see [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md).
For quality gate details, see [PAF-Quality-Gates.md](PAF-Quality-Gates.md).
For tooling recommendations, see [PAF-Tooling-Guide.md](PAF-Tooling-Guide.md).

---

## P1: Intake & Scoping

**Purpose:** Capture enough about a product idea to assess its complexity and determine how much process to apply.

**Entry criteria:** A product idea exists (can be a sentence, a paragraph, or a full brief).

### Activities

#### 1.1 Intake Questionnaire

Ask these questions in conversational order, one at a time. Adapt follow-up questions based on answers.

**Core Identity (always asked):**
1. What does this product do? (one paragraph)
2. Who is it for? (target users and buyers)
3. What problem does it solve that isn't solved today?
4. How will it be deployed? (SaaS / on-prem / embedded / CLI / library)
5. What's the commercial model? (open source / freemium / licensed / internal)

**Complexity Signals (asked to determine tier):**
6. How many distinct user roles interact with the system?
7. Does it have a UI? How many primary screens?
8. Does it need persistent state? What kind? (relational / document / file / in-memory)
9. How many external systems does it integrate with?
10. Does it have multi-tenancy, billing, or marketplace concerns?
11. Are there security/compliance requirements? (auth, encryption, audit, regulatory)
12. Does it have multiple deployment components? (separate binaries, services, planes)

**Domain Probing (if a domain pack matches):**
13–N. Questions injected from the matching domain pack. See [PAF-Domain-Pack-Specification.md](PAF-Domain-Pack-Specification.md).

#### 1.2 Complexity Assessment

Score each signal against the tier thresholds (see [PAF-Overview.md](PAF-Overview.md) §Complexity Tiers). The highest tier triggered by any signal becomes the assigned tier.

**Override rule:** The stakeholder can override the tier upward (never downward). This is appropriate when the product is starting small but will grow — designing for T2 from the start avoids rework when complexity arrives.

#### 1.3 Scope Boundaries

Document what is in scope for v1 and what is explicitly deferred. Use version gates:
- **v1.0** — must be in the first release
- **Future** — acknowledged but deferred (with rationale)
- **Out of scope** — will not be built (with rationale)

#### 1.4 Decomposition Check (T3 only)

If the product scores T3, assess whether it is actually multiple independent products. Signals:
- Can two components be deployed and used independently?
- Do components have different user bases?
- Could components be on different release schedules?
- Do components share runtime state?

If yes to the first three and no to the last, decompose into sub-products. Each gets its own P1–P6 cycle. Define the sequencing (which sub-product is designed first).

### Artifacts

- **Intake Brief** — answers to the questionnaire, stored as a Markdown document
- **Complexity Score** — tier assignment with signal-by-signal justification
- **Scope Boundaries** — v1 vs. future vs. out-of-scope

### Exit Gate

- [ ] Intake brief complete (all core identity + complexity signal questions answered)
- [ ] Complexity tier assigned and confirmed by stakeholder
- [ ] Scope boundaries documented
- [ ] If T3: decomposition check passed (either single product confirmed or sub-products defined)

---

## P2: Requirements & Decisions

**Purpose:** Systematically explore what the product must do, make architectural decisions, and capture domain knowledge for reuse.

**Entry criteria:** P1 exit gate passed.

### Activities

#### 2.1 Requirements Exploration

Work through requirement categories in the order below, scaled by tier:

| Category | T1 | T2 | T3 | What It Captures |
|----------|:---:|:---:|:---:|-----------------|
| Functional | Required | Required | Required | Capabilities, workflows, user stories |
| Data Model | Optional | Required | Required | Entities, relationships, lifecycle states |
| Integration | Optional | Required | Required | External systems, protocols, auth handshakes |
| User Experience | Skip | Required (if UI) | Required | Interaction patterns, navigation model |
| Security & Access | Skip | Optional | Required | Auth model, roles, trust boundaries |
| Operational | Skip | Optional | Required | Deployment, scaling, monitoring, upgrade path |
| Commercial | Skip | Optional | Required | Licensing, billing, marketplace |
| Compliance | Skip | Skip | If applicable | Regulatory frameworks, audit, data residency |

**Progressive depth pattern** — for each category:
1. Start with an open-ended question ("How should users authenticate?")
2. Based on the answer, present 2–3 concrete approaches with trade-offs
3. Record the chosen approach as a decision
4. Tag downstream documents that need this decision

#### 2.2 Decision Recording

Every significant decision follows this format:

```markdown
### D-{number}: {Question}

**Context:** Why this decision matters.

**Options Considered:**
1. {Option A} — {trade-offs}
2. {Option B} — {trade-offs}
3. {Option C} — {trade-offs}

**Decision:** {Which option, with reasoning}

**Distribute To:** {List of downstream document types that need this decision}

**Version Gate:** {v1.0 | future | N/A}
```

The **"Distribute To"** field is critical — it creates a traceable chain from decision to document section. When writing documents in P3, each section references the decisions that informed it (e.g., "Per D-7, we use JWT with opaque API key fallback").

#### 2.3 Domain Knowledge Capture

During P2, watch for domain-specific patterns — questions, constraints, terminology, or architectural decisions that would apply to other products in the same vertical.

At the end of P2, package these as a draft domain pack. See [PAF-Domain-Pack-Specification.md](PAF-Domain-Pack-Specification.md) for the schema and capture process.

### Artifacts

- **Requirements Log** — structured requirements by category
- **Decision Record** — all decisions in D-{number} format
- **Domain Pack Draft** — captured domain knowledge (if new domain)

### Exit Gate

- [ ] All required requirement categories explored (per tier table above)
- [ ] Every decision has rationale and "Distribute To" targets
- [ ] No critical open questions remaining
- [ ] Version gates assigned (v1 vs. future)
- [ ] Domain pack draft reviewed (if new domain knowledge captured)

---

## P3: Design Authoring

**Purpose:** Write the design document corpus — the right documents in the right order at the right depth for the product's complexity tier.

**Entry criteria:** P2 exit gate passed.

### Activities

#### 3.1 Document List

Determine which documents to write using the taxonomy table in [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md). Filter by the assigned complexity tier.

#### 3.2 Scaffolding

For each document, generate a skeleton from three inputs:
1. **Taxonomy template** — default sections for that document type (from `templates/` directory)
2. **Domain pack additions** — domain-specific sections injected by the matching pack
3. **Tier adjustments** — T1 gets minimal sections, T3 gets full depth

The scaffold includes: YAML frontmatter (pre-filled), section headings, and placeholder notes for what each section should cover.

#### 3.3 Tiered Authoring

Write documents in dependency layer order (see [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md) §Authoring Order). A document at Layer N must reach `draft` before any Layer N+1 document begins.

For each document:
1. **Fill sections** — reference decisions from the P2 Decision Record. Cite decisions explicitly (e.g., "Per D-7, we use...").
2. **Cross-reference** — set the `depends_on` frontmatter field. If this document references a concept from another document, that document is a dependency.
3. **Verify bidirectional references** — if document A depends on B, check whether B should also reference A. Justify one-way references.
4. **Self-check** — read the document end-to-end. Are there contradictions? Placeholders? Undefined terms?
5. **Set status** — mark as `draft` and move to the next document in layer order.

#### 3.4 Shared Contracts

Certain artifacts are defined once and referenced everywhere. Track these as you author:

| Contract Type | Defined In | Referenced By |
|--------------|-----------|--------------|
| Design tokens | UI Design | Frontend Architecture, all UI documents |
| Database schema | Backend Architecture | API Specification, Functional Processes |
| API schemas | API Specification / Canonical Specs | Frontend Architecture, Backend Architecture |
| Auth model | Security Architecture | API Spec, Backend Architecture, Frontend Architecture |
| Config schema | Backend Architecture | Functional Processes, Operational documents |

When you define a shared contract in one document, note which other documents will reference it. This becomes a consistency check target in P4.

#### 3.5 Multi-Domain Authoring (T3)

If the product has multiple domains:
1. Author the primary domain's documents first (Layers 0–4)
2. Author secondary domain documents, explicitly referencing primary domain mirrors
3. Author shared/cross-cutting documents last

See [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md) §Multi-Domain Rule for naming conventions and cross-domain dependency tables.

#### 3.6 Continuous Decision Log Updates

The Decision Log is updated throughout P3 — new decisions will arise during authoring that weren't anticipated in P2. Record them in the same D-{number} format with "Distribute To" targets.

### Artifacts

- **Design document corpus** — all required documents at `draft` or `review` status
- **Updated Decision Log** — with any new decisions made during authoring

### Exit Gate

- [ ] All required documents for the assigned tier exist and are ≥ `draft`
- [ ] All `depends_on` fields populated and verified
- [ ] All P2 decisions distributed to their target documents
- [ ] Shared contracts identified and noted for P4 consistency checking
- [ ] Decision Log updated with any P3 decisions

---

## P4: Quality Assurance

**Purpose:** Verify the design corpus is internally consistent, complete, and free of contradictions. No new content is authored in this phase — it is purely verification and resolution.

**Entry criteria:** P3 exit gate passed.

### Activities

#### 4.1 Run Quality Gates

Execute each gate in order (see [PAF-Quality-Gates.md](PAF-Quality-Gates.md) for full gate definitions and procedures):

| Gate | T1 | T2 | T3 |
|------|:---:|:---:|:---:|
| G1: Metadata Integrity | Required | Required | Required |
| G2: Decision Coverage | Required | Required | Required |
| G3: Taxonomy Completeness | Required | Required | Required |
| G4: Internal Consistency | Skip | Required | Required |
| G5: Cross-Document Consistency | Skip | Required | Required |
| G6: Bidirectional References | Skip | Optional | Required |
| G7: Version Gate Alignment | Skip | Optional | Required |
| G8: Gap Analysis | Skip | Required | Required |

#### 4.2 Record Findings

Create a Completeness Report documenting gate results, consistency findings, identified gaps, and status promotion recommendations. See [PAF-Quality-Gates.md](PAF-Quality-Gates.md) for the report template.

#### 4.3 Resolution Loop

For each finding:
1. Assign the finding to a specific document and section
2. Resolve the issue (fix the document)
3. Re-run the affected gate
4. Repeat until the gate passes

### Artifacts

- **Completeness Report** — gate results, findings, resolutions, status promotions

### Exit Gate

- [ ] All required gates pass for the assigned tier
- [ ] Zero critical gaps remaining
- [ ] Zero contradictions in cross-document consistency checks
- [ ] Completeness report finalized
- [ ] All documents promoted to `review` or `approved`
- [ ] **Declaration: "Design corpus is implementation-ready"**

---

## P5: Implementation Planning

**Purpose:** Transform the approved design corpus into an executable, task-level implementation plan.

**Entry criteria:** P4 exit gate passed.

### Activities

#### 5.1 Component Extraction

Walk every design document and extract every buildable component:
- Each endpoint group from the API Specification
- Each UI screen from the UI Design
- Each database table from the Backend Architecture
- Each service boundary from the Root Architecture
- Each integration from the Functional Processes
- Each security mechanism from the Security Architecture

This becomes the raw task list.

#### 5.2 Dependency Mapping

Using the document dependency graph and shared contracts from P3, determine component build order:

```
Foundation (scaffold, config, database schema)
  → Core domain logic (processes, state machines)
    → API layer (endpoints, middleware, auth)
      → Frontend (design system, components, pages)
        → Integration testing (cross-component E2E)
          → Operational (CI/CD, observability, release pipeline)
```

#### 5.3 Sprint Grouping

Group components by dependency tier into phases. Each phase maps to 1–3 sprints (2-week cycles):

| Tier | Typical Phases | Typical Tasks | Sprint Count |
|------|---------------|---------------|-------------|
| T1 | 2–3 | 15–30 | 3–5 |
| T2 | 5–8 | 40–80 | 6–12 |
| T3 | 10–20 | 150–300+ | 15–40 |

#### 5.4 Task Decomposition

Break each task into bite-sized steps (2–5 minutes each). Use TDD by default:
1. Write the failing test
2. Run it, confirm it fails
3. Write minimal implementation to pass
4. Run it, confirm it passes
5. Commit

Every step includes: exact file paths, code snippets or clear descriptions, and expected command output.

#### 5.5 Cross-Referencing

Every task references the design document section(s) it implements:

```markdown
### Task S3-T4: Agent Lifecycle State Machine
**Ref:** Functional-Processes §2.1, Backend-Architecture §4.3, API-Spec §3.2
```

Verify bidirectional coverage:
- Every design section is referenced by ≥1 task
- Every task references ≥1 design section

#### 5.6 Tooling Recommendations

Include a tooling section in the plan recommending tools based on tech stack and complexity tier. See [PAF-Tooling-Guide.md](PAF-Tooling-Guide.md) for the capability matrix and synergy map.

#### 5.7 Estimation

Produce an effort estimation appendix covering:
- LOC estimates by component
- Person-month ranges (pure human vs. AI-assisted)
- Timeline by team size
- Cost ranges by market

### Artifacts

- **Implementation Plan** — task-level plan with sprint structure, dependency graph, cross-references
- **Estimation Appendix** — LOC, effort, timeline, cost analysis

### Exit Gate

- [ ] Every design document section referenced by ≥1 task
- [ ] Every task references ≥1 design document section
- [ ] Dependency graph has no cycles
- [ ] Sprint grouping reviewed by stakeholder
- [ ] Estimation reviewed (sanity check)
- [ ] **Declaration: "Plan is execution-ready"**

---

## P6: Execution & Delivery

**Purpose:** Build the product. PAF doesn't prescribe a single execution methodology — it defines guardrails, quality checkpoints, and completion criteria.

**Entry criteria:** P5 exit gate passed.

### Activities

#### 6.1 Select Execution Mode

| Mode | Description | Best For |
|------|-------------|----------|
| **Solo + AI** | Single developer with AI-assisted development | T1–T2, founder-led |
| **Small Team** | 2–4 developers with defined ownership | T2–T3, early-stage |
| **Full Team** | 5+ developers with tech lead, PR workflow | T3, established teams |

#### 6.2 Follow Execution Principles

Regardless of mode:
1. **Isolated workspaces** — feature branches or worktrees, never directly on main
2. **TDD by default** — tests before implementation
3. **Frequent commits** — each task produces ≥1 commit
4. **Design traceability** — commit messages reference task IDs which reference design sections
5. **Review before merge** — every completed task gets at least one review pass

#### 6.3 Per-Task Quality Checks

| Check | Solo+AI | Small Team | Full Team |
|-------|:-------:|:----------:|:---------:|
| Tests pass | Required | Required | Required |
| Spec compliance | AI reviewer | Peer or AI | Peer review |
| Code quality | AI reviewer | Peer or AI | Peer review |

#### 6.4 Per-Phase Quality Checks

At the end of each sprint/phase:
- [ ] All phase tasks complete
- [ ] Integration tests pass across components built so far
- [ ] Design drift check — has implementation deviated from spec?
- [ ] Plan adjustment — do remaining tasks need re-scoping?

#### 6.5 Design Feedback Loop

During execution, discoveries will require design changes:

1. **Minor adjustments** (implementation detail differs from spec) — update the design document inline, note in commit message
2. **Significant deviations** (architectural change, new component, dropped feature) — create a new Decision Record entry, update affected design documents, re-run relevant P4 quality gate
3. **Scope changes** (new requirements discovered) — return to P2 for that requirement category, flow through P3→P4→P5 for affected documents only

#### 6.6 Pre-Release Quality Checks

| Check | T1 | T2 | T3 |
|-------|:---:|:---:|:---:|
| All tasks complete | Required | Required | Required |
| Full test suite passes | Required | Required | Required |
| Security review | Skip | Basic | Full audit |
| Performance benchmarks | Skip | Optional | Required |
| User-facing documentation | Optional | Required | Required |
| Release checklist | Optional | Required | Required |

#### 6.7 Domain Knowledge Harvest

At the end of P6:
1. Review all Decision Record entries created during execution
2. Extract patterns useful for future products in this domain
3. Update the domain pack with new questions, constraints, quality checks, terminology
4. Version the domain pack

### Artifacts

- **Working software** — built, tested, deployable
- **Updated design documents** — reflecting any implementation deviations
- **Updated domain pack** — with execution learnings
- **Release artifacts** — binaries, containers, packages as applicable

### Exit Gate

- [ ] All plan tasks marked done
- [ ] Full test suite passes (unit + integration + E2E as applicable)
- [ ] Per-phase and pre-release reviews passed
- [ ] Design documents updated to reflect implementation deviations
- [ ] All design documents at `approved` status
- [ ] Release artifacts produced
- [ ] Domain pack updated with execution learnings
- [ ] **Declaration: "Product is deliverable"**
