# Product Architecture Framework (PAF) — Design Specification

**Date:** 2026-03-12
**Status:** Approved
**Working Title:** PAF (Product Architecture Framework)
**Proof Case:** Enterprise ITSM platform (22 documents, 61K lines, 312-task plan, zero contradictions)

---

## 1. Purpose

PAF is a reusable system for designing software products at enterprise depth. It takes a product idea from initial intake through delivered software, producing a comprehensive, internally consistent design corpus, implementation plan, and execution framework.

PAF was reverse-engineered from a real enterprise ITSM platform design process, which produced 22 interconnected design documents across 2 architectural planes, 2 canonical OpenAPI specs (155 endpoints), and a 312-task implementation plan — with zero cross-document contradictions and 64/64 design tokens consistent.

The goal is to make that level of quality **repeatable, scalable, and learnable**.

---

## 2. Architecture

### Three Layers

**Layer 1 — The Methodology (portable, human-readable)**
A set of Markdown documents describing the complete product design lifecycle. Anyone can read these and follow the process manually. This is what gets published, handed to teams, or sold.

**Layer 2 — The Engine (reference implementation)**
A chain of skills that automate the methodology. Includes a complexity assessor that right-sizes the process per product, a document scaffolder that generates tailored skeletons, and quality gate runners that verify cross-document consistency. The reference implementation targets Claude Code but the methodology is tool-agnostic.

**Layer 3 — Domain Packs (learned, pluggable knowledge)**
YAML/Markdown files encoding domain-specific questions, constraints, document section templates, and quality checks. Captured automatically during each design engagement. Reusable, shareable, and independently valuable.

### Audiences

| Audience | What They Use | Format |
|----------|--------------|--------|
| Founder + AI | Full package (methodology + skills + templates + domain packs) | Git repo, Claude Code skills |
| Team members | Methodology + templates + domain packs | Markdown docs |
| External users | Methodology + templates + example | Published docs or PDF |
| Marketplace (future) | Domain packs | YAML files, purchasable add-ons |

### Design Principles

- **Tool-aware, tool-agnostic** — describes the process in terms of roles and capabilities, with a Claude Code reference implementation as one concrete binding
- **Auto-scaling** — right-sizes from a 5-doc utility to a 25-doc enterprise platform based on complexity signals
- **Self-improving** — captures domain knowledge with each engagement, building reusable packs
- **Full lifecycle** — requirements → design → plan → execution, not just documentation

---

## 3. Lifecycle Phases

PAF defines 6 phases, each with entry criteria, activities, artifacts, and exit gates:

| Phase | Name | Key Artifacts | Exit Gate |
|-------|------|---------------|-----------|
| P1 | Intake & Scoping | Intake brief, complexity score | Tier assigned, scope confirmed |
| P2 | Requirements & Decisions | Requirements log, decision record, domain knowledge draft | All critical questions answered, decisions distributed |
| P3 | Design Authoring | Design document corpus (right-sized) | All docs pass internal consistency |
| P4 | Quality Assurance | Completeness report, cross-reference audit | Zero contradictions, all dependencies satisfied |
| P5 | Implementation Planning | Task-level plan with sprint structure | Every task ↔ design section, dependencies mapped |
| P6 | Execution & Delivery | Working software, test results, release artifacts | All tasks complete, reviews passed |

---

## 4. Complexity Tiers

The engine assesses complexity during P1 and assigns a tier that determines how much process to apply:

| Tier | Name | Signals | Doc Count | Example |
|------|------|---------|-----------|---------|
| T1 | Utility | Single concern, 1 binary/service, <10 endpoints, no auth | 3–5 | CLI tool, library, webhook handler |
| T2 | Application | Multiple concerns, 1–2 services, auth, persistent state, UI | 8–12 | SaaS app, internal tool, API service |
| T3 | Platform | Multi-plane/multi-tenant, >50 endpoints, security model, governance | 15–25+ | enterprise platform, marketplace, multi-plane system |

Each tier prescribes which document types are required vs. optional, which quality gates are enforced, minimum team size for execution, and recommended tooling.

### Complexity Scoring

| Signal | T1 | T2 Threshold | T3 Threshold |
|--------|:---:|:---:|:---:|
| User roles | 1 | 2–3 | 4+ |
| UI screens | 0 | 3–10 | 11+ |
| Data stores | 0–1 | 1–2 | 3+ |
| External integrations | 0–1 | 2–4 | 5+ |
| Multi-tenancy / billing | No | No | Yes |
| Security/compliance depth | Basic | Auth + RBAC | Threat model + audit + encryption |
| Deployment components | 1 | 1–2 | 3+ |

The highest tier triggered by any signal becomes the assigned tier. The user can override up (never down).

---

## 5. Document Taxonomy

### Document Types by Tier

| Type | Purpose | T1 | T2 | T3 |
|------|---------|:---:|:---:|:---:|
| Root Architecture | Product identity, deployment model, key decisions | Required | Required | Required |
| Functional Processes | Operational flows, state machines, lifecycle, data flows | Optional | Required | Required |
| UI Design | Design system, component specs, screen layouts | Skip | Required (if UI) | Required |
| Frontend Architecture | Stack decisions, build pipeline, state management | Skip | Required (if UI) | Required |
| Backend Architecture | Binary/service structure, DB schema, package layout | Optional | Required | Required |
| API Specification | Endpoints, request/response schemas, auth, versioning | Optional | Required | Required |
| Security Architecture | Threat model, auth flows, RBAC, trust boundaries | Skip | Optional | Required |
| Operational | Testing strategy, observability, release process | Skip | Optional | Required |
| Decision Log | Q&A-style decision record with rationale + distribution | Optional | Required | Required |
| Completeness Report | Cross-document consistency audit, gap analysis | Skip | Optional | Required |
| Design Library | Document governance: frontmatter, naming, dependency graph | Skip | Skip | Required |
| Canonical Specs | Machine-readable source of truth (OpenAPI, JSON Schema) | Skip | Optional | Required |

### Authoring Order Within P3

Documents are authored in **dependency layers** (distinct from complexity tiers T1–T3). A document at Layer N must reach draft before any Layer N+1 document begins:

```
Layer 0: Root Architecture (always first)
Layer 1: Functional Processes, UI Design (depend on root only)
Layer 2: Frontend Architecture, Security, Backend Architecture (depend on Layer 1)
Layer 3: API Specification, Canonical Specs (depend on Layer 2)
Layer 4: Operational — Testing, Observability, Release (depend on all above)
```

The Decision Log is written continuously throughout P2 and P3.

### Multi-Domain Rule (T3)

When a product has multiple architectural domains (e.g., Data Plane / Control Plane), each domain gets its own document set mirroring the taxonomy. Cross-domain dependencies are explicitly declared via a naming convention (e.g., `CP-` prefix) and cross-dependency table.

### Metadata Standard

Every document carries YAML frontmatter:

```yaml
---
title: string
domain: string           # product/plane/module identifier
category: enum           # root-architecture | functional-processes | ui-design | frontend-architecture | backend-architecture | api-specification | security-architecture | operational | decision-log | completeness-report | design-library | canonical-specs
status: draft | review | approved
version: "x.y"
date: YYYY-MM-DD
parent: filename | ~
depends_on: []
tags: []
---
```

---

## 6. Phase P1 — Intake & Scoping

### Intake Brief

A structured questionnaire capturing enough about the idea to assess complexity:

**Core Identity (always asked):**
1. What does this product do?
2. Who is it for?
3. What problem does it solve that isn't solved today?
4. How will it be deployed? (SaaS / on-prem / embedded / CLI / library)
5. What's the commercial model? (open source / freemium / licensed / internal)

**Complexity Signals (asked to determine tier):**
6. How many distinct user roles?
7. Does it have a UI? How many primary screens?
8. Does it need persistent state? What kind?
9. How many external systems does it integrate with?
10. Multi-tenancy, billing, or marketplace concerns?
11. Security/compliance requirements?
12. Multiple deployment components?

**Domain Probing (if a domain pack exists):**
13–N. Injected from matching domain pack.

### Decomposition Check (T3 Only)

If the product scores T3, assess whether it is actually multiple independent products. Decomposition signals:
- Two components can be deployed and used independently
- Components have different user bases or buyers
- Components could be on different release schedules
- Components have no shared runtime state (only API contracts between them)

If 2+ signals are true, decompose into sub-products. Each gets its own P1–P6 cycle. Define sequencing (which sub-product is designed first).

### P1 Exit Gate

- Intake brief complete
- Complexity tier assigned and confirmed
- Scope boundaries documented (v1 vs. deferred)
- If T3: decomposition check passed (single product confirmed or sub-products defined)

---

## 7. Phase P2 — Requirements & Decisions

### Requirement Categories by Tier

| Category | T1 | T2 | T3 | What It Captures |
|----------|:---:|:---:|:---:|-----------------|
| Functional | Required | Required | Required | Capabilities, workflows, user stories |
| Data Model | Optional | Required | Required | Entities, relationships, lifecycle states |
| Integration | Optional | Required | Required | External systems, protocols, auth handshakes |
| User Experience | Skip | Required (if UI) | Required | Interaction patterns, navigation model |
| Security & Access | Skip | Optional | Required | Auth model, roles, trust boundaries |
| Operational | Skip | Optional | Required | Deployment, scaling, monitoring, upgrade |
| Commercial | Skip | Optional | Required | Licensing, billing, marketplace |
| Compliance | Skip | Skip | If applicable | Regulatory, audit, data residency |

### Progressive Depth Pattern

Within each category:
1. Start with open-ended question
2. Present 2–3 concrete approaches with trade-offs
3. Record decision with rationale
4. Tag downstream documents for distribution

### Decision Record Format

```markdown
### D-{number}: {Question}

**Context:** Why this decision matters.

**Options Considered:**
1. {Option A} — {trade-offs}
2. {Option B} — {trade-offs}

**Decision:** {Which option, with reasoning}

**Distribute To:** {List of downstream document types}

**Version Gate:** {v1.0 | future | N/A}
```

### Domain Knowledge Capture

During P2, the engine watches for domain-specific patterns and packages them as a draft domain pack at the end of the phase:

```yaml
name: string
version: "1.0"
captured_from: string
date: YYYY-MM-DD

intake_questions: []
requirement_prompts: {}
document_section_templates: {}
quality_checks: []
terminology: []
tooling_recommendations: []
```

### P2 Exit Gate

- All required requirement categories explored (per tier)
- Every decision has rationale and "Distribute To" targets
- No critical open questions remaining
- Version gates assigned
- Domain pack draft reviewed

---

## 8. Phase P3 — Design Authoring

### Authoring Workflow

For each document in the taxonomy (filtered by tier):

1. **Scaffold** — generate skeleton from taxonomy template + domain pack
2. **Author** — fill sections, referencing P2 decisions
3. **Cross-reference** — wire up `depends_on`, verify bidirectional references
4. **Self-check** — validate internal consistency
5. **Status** — mark as draft, move to next document in tier order

### Document Scaffolding

Each skeleton is generated from three inputs:
1. **Taxonomy template** — default sections for that document type
2. **Domain pack additions** — domain-specific sections injected
3. **Tier adjustments** — T1 gets minimal sections, T3 gets full depth

### Cross-Reference Rules

1. If document A references a concept from document B, B must appear in A's `depends_on`
2. Bidirectional verification — if A depends on B, B should acknowledge A or justify one-way
3. Decision traceability — every major section references its P2 decision(s)
4. Terminology consistency — terms from domain pack glossary must match usage everywhere

### Shared Contracts

Artifacts defined once and referenced everywhere:

| Contract Type | Defined In | Referenced By |
|--------------|-----------|--------------|
| Design tokens | UI Design | Frontend Arch, all UI docs |
| Database schema | Backend Architecture | API Spec, Functional Processes |
| API schemas | API Spec / Canonical Specs | Frontend Arch, Backend Arch |
| Auth model | Security Architecture | API Spec, Backend, Frontend |
| Config schema | Backend Architecture | Processes, Operational docs |

### P3 Exit Gate

- All required documents at `draft` or `review` status
- All `depends_on` fields populated and verified
- All P2 decisions distributed to target documents
- Shared contracts identified and consistent within each document
- Decision Log updated with any new decisions

---

## 9. Phase P4 — Quality Assurance

### Quality Gate Framework

| Gate | What It Verifies | T1 | T2 | T3 |
|------|-----------------|:---:|:---:|:---:|
| G1: Metadata Integrity | Valid frontmatter, correct parent/depends_on | Required | Required | Required |
| G2: Decision Coverage | Every P2 decision distributed to ≥1 document | Required | Required | Required |
| G3: Taxonomy Completeness | All required document types exist and non-empty | Required | Required | Required |
| G4: Internal Consistency | No contradictions within individual documents | Skip | Required | Required |
| G5: Cross-Document Consistency | Shared contracts match across all documents | Skip | Required | Required |
| G6: Bidirectional References | A↔B dependencies verified or justified | Skip | Optional | Required |
| G7: Version Gate Alignment | Deferred features consistently marked everywhere | Skip | Optional | Required |
| G8: Gap Analysis | No critical unanswered questions or placeholders | Skip | Required | Required |

### Consistency Check Targets (G5)

| Check | Documents Compared | What Must Match |
|-------|-------------------|----------------|
| Schema ↔ API | Backend ↔ API Spec | DB columns map to API response fields |
| Auth ↔ Everything | Security ↔ API, Backend, Frontend | Roles, permissions, token claims |
| Process ↔ API | Processes ↔ API Spec | Every process has corresponding endpoints |
| Tokens ↔ Frontend | UI Design ↔ Frontend ↔ all UI docs | Hex values, spacing, typography |
| Config ↔ Docs | Backend ↔ Processes ↔ Operational | Config keys, defaults, env vars |
| Terminology ↔ All | Domain pack glossary ↔ every document | Same term = same meaning |

Domain packs can inject additional checks.

### Completeness Report

```markdown
# {Product} Design Completeness Report

## 1. Gate Results
| Gate | Status | Evidence |

## 2. Consistency Findings
| Check | Status | Detail |

## 3. Gaps Identified
| ID | Severity | Description | Resolution |

## 4. Status Promotion Recommendation
| Document | Current | Recommended | Blocker |
```

### Resolution Loop

1. Each finding assigned to a specific document and section
2. Author resolves the finding
3. Affected gate re-runs
4. Repeat until all gates pass

### P4 Exit Gate

- All required gates pass for the assigned tier
- Zero critical gaps remaining
- Zero cross-document contradictions
- Completeness report finalized
- All documents promoted to `review` or `approved`
- **Declaration: "Design corpus is implementation-ready"**

---

## 10. Phase P5 — Implementation Planning

### Plan Generation Process

**Step 1: Extract Components**
Walk every design document, extract every buildable component — endpoint groups, UI screens, DB tables, service boundaries, integrations.

**Step 2: Map Dependencies**
Using the document dependency graph and shared contracts:

```
Foundation (scaffold, config, DB)
  → Core domain logic
    → API layer
      → Frontend
        → Integration & E2E testing
          → Operational (CI/CD, observability, release)
```

**Step 3: Group into Phases/Sprints**
Components grouped by dependency tier into phases (2-week sprints):

| Tier | Typical Phases | Typical Tasks | Sprint Count |
|------|---------------|---------------|-------------|
| T1 | 2–3 | 15–30 | 3–5 |
| T2 | 5–8 | 40–80 | 6–12 |
| T3 | 10–20 | 150–300+ | 15–40 |

**Step 4: Decompose into Bite-Sized Steps**
Each task broken into 2–5 minute steps. TDD by default:
1. Write failing test
2. Confirm failure
3. Write minimal implementation
4. Confirm pass
5. Commit

### Cross-Referencing

Every task references its design document section(s):

```markdown
### Task S3-T4: Agent Lifecycle State Machine
**Ref:** Functional-Processes §2.1, Backend-Architecture §4.3, API-Spec §3.2
```

Full traceability: idea → decision → design section → task → code.

### Tooling Recommendations Section

The plan includes a tooling section recommending tools based on tech stack and complexity tier, with synergy annotations:

```markdown
| Purpose | Tool | Synergy |
|---------|------|---------|
| Type generation | openapi-typescript | Pairs with OpenAPI spec → compile-time safety |
| CI validation | Redocly CLI | Pairs with OpenAPI spec → lint on every PR |
```

### Estimation Appendix

Every plan includes an effort estimation:
- LOC estimates by component
- Person-month ranges (pure human vs. AI-assisted)
- Timeline by team size
- Cost ranges by market

### P5 Exit Gate

- Every design section referenced by ≥1 task
- Every task references ≥1 design section
- Dependency graph has no cycles
- Sprint grouping reviewed by stakeholder
- Estimation reviewed (sanity check)
- **Declaration: "Plan is execution-ready"**

---

## 11. Phase P6 — Execution & Delivery

### Execution Modes

| Mode | Description | Best For |
|------|-------------|----------|
| Solo + AI | Single developer with AI-assisted development | T1–T2, founder-led |
| Small Team | 2–4 developers with defined ownership | T2–T3, early-stage |
| Full Team | 5+ developers with tech lead, PR workflow, CI gates | T3, established teams |

### Execution Principles (All Modes)

1. **Isolated workspaces** — branches or worktrees, never directly on main
2. **TDD by default** — tests before implementation
3. **Frequent commits** — each task produces ≥1 commit
4. **Design traceability** — commits reference task IDs referencing design sections
5. **Review before merge** — every task gets at least one review pass

### Quality Checkpoints

**Per-Task:**

| Check | Solo+AI | Small Team | Full Team |
|-------|:-------:|:----------:|:---------:|
| Tests pass | Required | Required | Required |
| Spec compliance | AI reviewer | Peer or AI | Peer review |
| Code quality | AI reviewer | Peer or AI | Peer review |

**Per-Phase:**

| Check | All Modes |
|-------|-----------|
| All phase tasks complete | Required |
| Integration tests pass | Required |
| Design drift check | Required |
| Plan adjustment if needed | Required |

**Pre-Release:**

| Check | T1 | T2 | T3 |
|-------|:---:|:---:|:---:|
| All tasks complete | Required | Required | Required |
| Full test suite passes | Required | Required | Required |
| Security review | Skip | Basic | Full audit |
| Performance benchmarks | Skip | Optional | Required |
| User-facing documentation | Optional | Required | Required |
| Release checklist | Optional | Required | Required |

### Design Feedback Loop

1. **Minor adjustments** — update design doc inline, note in commit message
2. **Significant deviations** — new Decision Record entry, update affected docs, re-run relevant P4 gate
3. **Scope changes** — return to P2 for that requirement category, flow through P3→P4→P5 for affected docs only

### Domain Knowledge Harvest

At the end of P6:
1. Review Decision Record entries created during execution
2. Extract patterns useful for future products in the domain
3. Update domain pack (new questions, constraints, checks, terminology)
4. Version the domain pack

### P6 Completion Criteria

- All plan tasks marked done
- Full test suite passes
- Per-phase and pre-release reviews passed
- Design documents updated to reflect implementation deviations
- All design documents at `approved` status
- Release artifacts produced
- Domain pack updated with execution learnings
- **Declaration: "Product is deliverable"**

---

## 12. Tooling Guide & Synergy Model

### Capability Requirements by Phase

| Phase | Capabilities Needed |
|-------|-------------------|
| P1 Intake | Conversational Q&A, structured data capture |
| P2 Requirements | Decision recording, domain knowledge storage |
| P3 Authoring | Document generation, template scaffolding, cross-reference validation |
| P4 QA | Consistency checking, gap analysis, report generation |
| P5 Planning | Task decomposition, dependency graphing, estimation |
| P6 Execution | Code generation, testing, review, CI/CD, version control |

### Reference Tooling Stack (Claude Code Binding)

| Capability | Tool | Role |
|-----------|------|------|
| AI-assisted design | Claude Code (Opus) | Powers P1–P5 |
| Skill orchestration | Superpowers skills | Chains PAF phases |
| Subagent dispatch | Claude Code Agent tool | Parallel tasks in P3–P6 |
| Version control | Git + GitHub | Document + code versioning |
| Document format | Markdown + YAML frontmatter | Portable, diffable, metadata-rich |
| API specification | OpenAPI 3.1 (YAML) | Machine-readable contracts |
| API validation | Redocly CLI | Lint + bundle on every PR |
| Type generation | openapi-typescript + openapi-fetch | Compile-time API type safety |
| Frontend framework | Svelte 5 + SvelteKit | Compiler-first, zero runtime |
| Component library | Bits UI | Headless accessible primitives |
| Backend language | Go | Static binaries, strong stdlib |
| Testing | Vitest + Playwright | Unit + E2E |
| Containers | Docker | Reproducible builds |
| MCP servers | context7, docker, godoc, playwright, svelte | AI tool integrations |

### Synergy Map

| Pair | Synergy |
|------|---------|
| OpenAPI + openapi-typescript | Spec → Types: change YAML, regenerate, compiler catches mismatches |
| OpenAPI + Redocly CLI | Spec → Validation: PR-level lint checking |
| Svelte 5 + Bits UI | Same reactivity model ($props, $state), no adapter needed |
| Go + go:embed | Single binary serves UI — zero deployment deps |
| Git worktrees + Subagents | Each agent works in isolation, no file conflicts |
| Claude Code + MCP servers | AI queries docs, runs containers, checks APIs directly |
| Vitest browser + Playwright | Same browser engine for unit and E2E |
| YAML frontmatter + Grep/Glob | Any tool can search/filter documents by metadata |

### Tooling Selection Guidance

When PAF is used with a different tech stack, the engine provides selection criteria per capability area with synergy considerations. Example:

```markdown
## Choosing a Frontend Framework

**PAF Requirements:**
- Must support design system tokens (CSS custom properties)
- Must produce static assets embeddable in backend binary (if applicable)
- Must have headless component library (accessibility)
- Type-safe API client must be available

**Options:**
1. Svelte 5 + SvelteKit (validated by proof-case) — best for embedded/air-gapped
2. React 19 + Next.js — best for ecosystem breadth
3. Vue 3 + Nuxt — middle ground

**Synergy: if Go backend → Svelte (static adapter + go:embed = single binary)**
```

### Domain Pack Tooling Extensions

Domain packs can recommend domain-specific tools:

```yaml
tooling_recommendations:
  - capability: governance_policy_engine
    recommended: OPA/Rego
    synergy: "Embedded Go library, non-Turing-complete, additive-only"
  - capability: audit_trail
    recommended: Append-only DB with hash chains
    synergy: "Same DB engine as primary store, no additional infra"
```

---

## 13. File Structure

```
paf/
├── methodology/                          # Layer 1: Portable, human-readable
│   ├── PAF-Overview.md                   # Architecture, phases, tiers, lifecycle
│   ├── PAF-Document-Taxonomy.md          # Document types, tier requirements, authoring order
│   ├── PAF-Phase-Guide.md               # P1–P6 detailed procedures, entry/exit gates
│   ├── PAF-Quality-Gates.md             # Gate definitions, consistency checks, report template
│   ├── PAF-Tooling-Guide.md             # Capability matrix, synergy map, selection guidance
│   └── PAF-Domain-Pack-Specification.md  # Domain pack schema, capture process, versioning
│
├── templates/                            # Scaffolding templates per document type
│   ├── root-architecture.md
│   ├── functional-processes.md
│   ├── ui-design.md
│   ├── frontend-architecture.md
│   ├── backend-architecture.md
│   ├── api-specification.md
│   ├── security-architecture.md
│   ├── operational.md
│   ├── decision-log.md
│   ├── completeness-report.md
│   ├── design-library.md
│   └── implementation-plan.md
│
├── domain-packs/                         # Layer 3: Learned knowledge
│   ├── _template.yaml                    # Empty domain pack template
│   └── itsm.yaml                         # First pack (captured from proof-case)
│
├── skills/                               # Layer 2: Claude Code reference implementation
│   ├── paf-intake/SKILL.md               # P1 skill
│   ├── paf-requirements/SKILL.md         # P2 skill
│   ├── paf-author/SKILL.md               # P3 skill
│   ├── paf-qa/SKILL.md                   # P4 skill
│   ├── paf-plan/SKILL.md                 # P5 skill
│   └── paf-execute/SKILL.md              # P6 skill
│
└── examples/                             # Proof by example
    └── itsm-platform/                    # Proof-case reference implementation
        ├── intake-brief.md
        ├── decision-record.md
        ├── document-index.md
        ├── completeness-report.md
        ├── implementation-plan.md
        └── estimation.md
```

---

## 14. Success Criteria

PAF is successful when:

1. A new product can go from idea to implementation-ready design in **days, not weeks**
2. The design corpus has **zero contradictions** verified by automated quality gates
3. Every design decision is **traceable** from intake question → decision → document section → task → code
4. The system gets **smarter with each engagement** via domain pack learning
5. A developer with **zero project context** can pick up the implementation plan and start building
6. The methodology is **readable and followable** without the Claude Code engine
7. The engine **right-sizes automatically** — a CLI tool doesn't get 22 documents
