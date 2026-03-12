# PAF Plan 1: Methodology Documents — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the 6 portable Markdown documents that define the PAF methodology — readable and followable by anyone without tooling.

**Architecture:** Each document covers one aspect of the methodology (overview, taxonomy, phases, quality gates, tooling, domain packs). Documents cross-reference each other but each stands alone as a reference. Content is extracted and refined from the approved design spec at `docs/paf/2026-03-12-paf-design-spec.md`.

**Tech Stack:** Markdown only. No code, no tooling dependencies.

**Spec:** `docs/paf/2026-03-12-paf-design-spec.md` (all section references below refer to this file)

---

## File Structure

```
docs/paf/
├── 2026-03-12-paf-design-spec.md          # EXISTS — approved spec (input)
├── plans/
│   └── 2026-03-12-paf-plan-1-methodology.md  # THIS PLAN
└── methodology/
    ├── PAF-Overview.md                     # CREATE — §1-3 of spec
    ├── PAF-Document-Taxonomy.md            # CREATE — §5 of spec
    ├── PAF-Phase-Guide.md                  # CREATE — §6-11 of spec
    ├── PAF-Quality-Gates.md                # CREATE — §9 of spec (expanded)
    ├── PAF-Tooling-Guide.md               # CREATE — §12 of spec (expanded)
    └── PAF-Domain-Pack-Specification.md    # CREATE — §7 domain capture (expanded)
```

Each file has one clear responsibility:
- **PAF-Overview.md** — "What is PAF?" entry point. Architecture, layers, tiers, lifecycle summary.
- **PAF-Document-Taxonomy.md** — "What documents do I write?" Full taxonomy, tier requirements, authoring order, metadata standard.
- **PAF-Phase-Guide.md** — "How do I run each phase?" P1–P6 procedures, entry/exit gates, checklists.
- **PAF-Quality-Gates.md** — "How do I verify quality?" Gate definitions, consistency checks, completeness report template.
- **PAF-Tooling-Guide.md** — "What tools should I use?" Capability matrix, synergy map, selection guidance.
- **PAF-Domain-Pack-Specification.md** — "How do domain packs work?" Schema, capture process, versioning, usage.

---

## Chunk 1: Foundation Documents

### Task 1: PAF-Overview.md

**Files:**
- Create: `docs/paf/methodology/PAF-Overview.md`

**Context:** This is the entry point document. Someone reading PAF for the first time starts here. It must explain the 3-layer architecture, the 6 phases, and the 3 complexity tiers — enough to understand the system without reading anything else. It then points readers to the other 5 documents for detail.

**Source sections from spec:** §1 (Purpose), §2 (Architecture), §3 (Lifecycle Phases), §4 (Complexity Tiers)

- [ ] **Step 1: Create the document with this content**

```markdown
# Product Architecture Framework (PAF)

## What PAF Is

PAF is a reusable system for designing software products at enterprise depth. It takes a product idea from initial intake through delivered software, producing a comprehensive, internally consistent design corpus, implementation plan, and execution framework.

PAF auto-scales — a CLI tool gets 5 documents and 3 quality gates. An enterprise platform gets 25+ documents, 8 quality gates, and machine-readable canonical specs. The system determines the right level of process based on complexity signals detected during intake.

### The Three Layers

**Layer 1 — The Methodology**
Portable, human-readable Markdown documents describing the complete product design lifecycle. Anyone can follow these manually without tooling. This is what gets published, handed to teams, or sold.

**Layer 2 — The Engine**
A chain of skills that automate the methodology. Includes a complexity assessor, document scaffolder, and quality gate runners. The reference implementation targets Claude Code, but the methodology is tool-agnostic.

**Layer 3 — Domain Packs**
YAML files encoding domain-specific questions, constraints, templates, and quality checks. Captured automatically during each design engagement. Each product designed with PAF makes the system smarter for the next one.

---

## Lifecycle Phases

PAF defines 6 phases. Each has entry criteria, prescribed activities, artifacts, and an exit gate that must pass before proceeding.

| Phase | Name | What It Produces | Exit Gate |
|-------|------|-----------------|-----------|
| **P1** | Intake & Scoping | Intake brief, complexity score | Tier assigned, scope confirmed |
| **P2** | Requirements & Decisions | Requirements log, decision record, domain knowledge draft | All critical questions answered, decisions distributed |
| **P3** | Design Authoring | Design document corpus (right-sized to tier) | All docs pass internal consistency check |
| **P4** | Quality Assurance | Completeness report, cross-reference audit | Zero contradictions, all dependencies satisfied |
| **P5** | Implementation Planning | Task-level plan with sprint structure | Every task ↔ design section, dependencies mapped |
| **P6** | Execution & Delivery | Working software, test results, release artifacts | All tasks complete, quality reviews passed |

```
P1 Intake → P2 Requirements → P3 Authoring → P4 QA → P5 Planning → P6 Execution
```

Each phase is detailed in [PAF-Phase-Guide.md](PAF-Phase-Guide.md).

---

## Complexity Tiers

The system assesses complexity during P1 and assigns a tier. The tier determines how much process to apply — which document types are required, which quality gates are enforced, and what team size is recommended.

### Tier Definitions

| Tier | Name | Description | Doc Count |
|------|------|-------------|-----------|
| **T1** | Utility | Single concern, 1 binary/service, <10 endpoints, no auth | 3–5 |
| **T2** | Application | Multiple concerns, 1–2 services, auth, persistent state, UI | 8–12 |
| **T3** | Platform | Multi-plane/multi-tenant, >50 endpoints, security model, governance | 15–25+ |

### Complexity Signals

| Signal | T1 | T2 Threshold | T3 Threshold |
|--------|:---:|:---:|:---:|
| User roles | 1 | 2–3 | 4+ |
| UI screens | 0 | 3–10 | 11+ |
| Data stores | 0–1 | 1–2 | 3+ |
| External integrations | 0–1 | 2–4 | 5+ |
| Multi-tenancy / billing | No | No | Yes |
| Security/compliance depth | Basic | Auth + RBAC | Threat model + audit + encryption |
| Deployment components | 1 | 1–2 | 3+ |

The highest tier triggered by any signal becomes the assigned tier. Stakeholders can override up (never down) if they know the product will grow into higher complexity.

---

## How the Layers Work Together

**Without the engine (manual process):**
1. Read this overview to understand PAF
2. Use [PAF-Phase-Guide.md](PAF-Phase-Guide.md) to walk through P1–P6
3. Use [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md) to know which documents to write
4. Use [PAF-Quality-Gates.md](PAF-Quality-Gates.md) to verify quality
5. Use [PAF-Tooling-Guide.md](PAF-Tooling-Guide.md) to select tools
6. Use [PAF-Domain-Pack-Specification.md](PAF-Domain-Pack-Specification.md) to capture and reuse domain knowledge

**With the engine (automated):**
1. Invoke `paf-intake` skill — it runs P1, assesses complexity, assigns tier
2. Each subsequent skill runs its phase, producing artifacts and verifying gates
3. Domain knowledge is captured automatically and saved as packs
4. The methodology documents are the engine's source of truth — skills read them, not the other way around

---

## Proof Case

PAF was extracted from a real enterprise ITSM platform design process, which produced:

- **22 design documents** across 2 architectural planes (Data Plane + Control Plane)
- **61,000+ lines** of internally consistent specification
- **2 canonical OpenAPI 3.1 specs** (155 endpoints, 138 schemas)
- **294-task implementation plan** across 37 sprints
- **Zero cross-document contradictions** verified by 8 targeted consistency checks
- **64/64 design tokens** consistent across all UI documents

A worked example is available in `examples/itsm-platform/` as a reference for what each phase produces.

---

## Document Map

| Document | Purpose |
|----------|---------|
| **This file** | Entry point — architecture, tiers, lifecycle |
| [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md) | What documents to write, when, in what order |
| [PAF-Phase-Guide.md](PAF-Phase-Guide.md) | How to run each phase (P1–P6) |
| [PAF-Quality-Gates.md](PAF-Quality-Gates.md) | How to verify quality — gates, checks, templates |
| [PAF-Tooling-Guide.md](PAF-Tooling-Guide.md) | What tools to use — capabilities, synergies, selection |
| [PAF-Domain-Pack-Specification.md](PAF-Domain-Pack-Specification.md) | How domain packs work — schema, capture, reuse |
```

- [ ] **Step 2: Verify the document reads correctly**

Run: `wc -l docs/paf/methodology/PAF-Overview.md`
Expected: ~120 lines

- [ ] **Step 3: Commit**

```bash
git add docs/paf/methodology/PAF-Overview.md
git commit -m "docs(paf): add PAF-Overview.md — entry point and architecture"
```

---

### Task 2: PAF-Document-Taxonomy.md

**Files:**
- Create: `docs/paf/methodology/PAF-Document-Taxonomy.md`

**Context:** This document answers "what documents do I need to write?" It defines every document type PAF supports, which tiers require each type, the authoring order (dependency layers), the metadata standard, and the multi-domain rule. A practitioner uses this as a checklist during P3.

**Source sections from spec:** §5 (Document Taxonomy)

- [ ] **Step 1: Create the document with this content**

```markdown
# PAF Document Taxonomy

This document defines the complete set of document types in a PAF design corpus — what each one is, when it's required, and what order to write them in.

---

## Document Types

| Type | Purpose | T1 (Utility) | T2 (Application) | T3 (Platform) |
|------|---------|:---:|:---:|:---:|
| **Root Architecture** | Product identity, deployment model, key decisions, component overview | Required | Required | Required |
| **Functional Processes** | Operational flows, state machines, lifecycle, data flows | Optional | Required | Required |
| **UI Design** | Design system tokens, component specs, screen layouts, interaction patterns | Skip | Required (if UI) | Required |
| **Frontend Architecture** | Framework decisions, build pipeline, state management, testing approach | Skip | Required (if UI) | Required |
| **Backend Architecture** | Binary/service structure, database schema, package layout, IPC | Optional | Required | Required |
| **API Specification** | Endpoint definitions, request/response schemas, auth, versioning | Optional | Required | Required |
| **Security Architecture** | Threat model, auth flows, RBAC, trust boundaries, data protection | Skip | Optional | Required |
| **Operational** | Testing strategy, observability, release process, runbooks | Skip | Optional | Required |
| **Decision Log** | Q&A-style decision record with rationale and distribution targets | Optional | Required | Required |
| **Completeness Report** | Cross-document consistency audit, gap analysis, status promotion | Skip | Optional | Required |
| **Design Library** | Document governance: frontmatter spec, naming convention, dependency graph, index | Skip | Skip | Required |
| **Canonical Specs** | Machine-readable source of truth (OpenAPI, JSON Schema, DDL) | Skip | Optional | Required |

### Reading the Table

- **Required** — must exist and pass quality gates before P4
- **Optional** — recommended but not gated; include if the product warrants it
- **Skip** — unnecessary at this tier; adding it is overhead without value
- **Required (if UI)** — required only when the product has a user interface

---

## Authoring Order

Documents are written in **dependency layers** (distinct from complexity tiers T1–T3). A document at Layer N must reach `draft` status before any Layer N+1 document begins. This prevents writing backend architecture before understanding the processes it implements.

```
Layer 0: Root Architecture
    ↓ must reach draft
Layer 1: Functional Processes, UI Design
    ↓ must reach draft
Layer 2: Frontend Architecture, Security Architecture, Backend Architecture
    ↓ must reach draft
Layer 3: API Specification, Canonical Specs
    ↓ must reach draft
Layer 4: Operational (Testing, Observability, Release)
```

**The Decision Log is not layered** — it is written continuously throughout P2 and P3 as decisions are made. It accumulates rather than completing at a specific layer.

### Why This Order

Each layer depends on the layers above it:

| Layer | Depends On | Because |
|-------|-----------|---------|
| Layer 1 (Processes, UI) | Root Architecture | Processes implement the architecture; UI visualizes it |
| Layer 2 (Frontend, Security, Backend) | Layer 1 | Backend implements processes; security constrains them; frontend renders UI design |
| Layer 3 (API, Canonical Specs) | Layer 2 | API is the interface between frontend and backend, constrained by security |
| Layer 4 (Operational) | All above | Testing, observability, and release concern the entire system |

---

## Document Type Details

### Root Architecture
The first document written for any product. Covers: product identity, problem statement, target users, deployment model, component overview, key architectural decisions, technology choices, and release roadmap. This is the single document someone reads to understand what the product is and how it works at a high level.

**Typical length:** T1: 200–500 lines. T2: 500–1,000 lines. T3: 800–1,500 lines.

### Functional Processes
Describes how the system behaves — not what components exist, but what they do. Covers: installation flows, configuration, runtime processes, lifecycle management (start/stop/upgrade), user-facing workflows, state machines, data flows, and error recovery paths.

**Why Layer 1:** Everything downstream depends on understanding the processes. You cannot design an API without knowing what operations the system supports. You cannot write security constraints without knowing what actions need governing.

**Typical length:** T2: 500–2,000 lines. T3: 2,000–6,000 lines.

### UI Design
Defines the visual language and component specifications. Covers: color system, typography, spacing, motion, icon system, surface hierarchy, component specs (buttons, forms, tables, cards, modals), screen layouts, responsive breakpoints, and accessibility requirements.

**Typical length:** T2: 400–800 lines. T3: 1,000–2,000 lines.

### Frontend Architecture
Architectural decisions for the frontend. Covers: framework choice (with rationale), project structure, build pipeline, component library, state management, API client approach, real-time strategy, testing approach, bundle targets, and accessibility compliance.

**Typical length:** T2: 300–600 lines. T3: 600–1,200 lines.

### Backend Architecture
Implementation architecture for the backend. Covers: language/runtime choice, binary/service structure, database schema (DDL or ERD), package/module layout, IPC protocol (if multi-process), configuration loading, error handling strategy, and performance targets.

**Typical length:** T1: 200–400 lines. T2: 500–1,500 lines. T3: 1,500–3,000 lines.

### API Specification
Complete API definition. Covers: authentication scheme, endpoint inventory (organized by domain), request/response schemas, error handling (RFC 7807 recommended), pagination strategy, filtering conventions, rate limiting, versioning strategy, and WebSocket/SSE event specifications.

**Typical length:** T2: 500–2,000 lines. T3: 2,000–5,000 lines.

### Security Architecture
Security design. Covers: threat model, trust boundaries, authentication flows (local, SSO, MFA), authorization model (RBAC, ABAC), data classification, encryption strategy (at rest, in transit), key management, audit logging, prompt injection defense (if AI), and compliance mapping.

**Typical length:** T2: 300–800 lines. T3: 1,000–2,000 lines.

### Operational
Cross-cutting operational concerns. May be a single document or split into separate Testing Strategy, Observability Strategy, and Release Process documents depending on complexity:

- **Testing Strategy:** Test pyramid, CI/CD pipeline, unit/integration/E2E test approach, coverage targets, performance benchmarks.
- **Observability Strategy:** Logging, metrics, tracing, alerting, SLI/SLO definitions, dashboard inventory.
- **Release Process:** Branching strategy, quality gates, artifact signing, container images, deployment checklist.

**Split rule:** T2 uses a single Operational document. T3 splits into 2–3 separate documents.

**Typical length:** T2: 300–600 lines (combined). T3: 500–1,000 lines per document.

### Decision Log
A chronological record of design decisions made during P2 and P3. Each entry follows a structured format (see [PAF-Phase-Guide.md](PAF-Phase-Guide.md) §P2 for the format). The "Distribute To" field in each entry creates a traceable chain from decision to document section.

**Typical length:** Scales with product complexity. T2: 10–20 entries. T3: 25–50+ entries.

### Completeness Report
The P4 quality gate artifact. Records which gates passed/failed, consistency check results, gaps identified, and status promotion recommendations. See [PAF-Quality-Gates.md](PAF-Quality-Gates.md) for the full template.

**Typical length:** T2: 100–300 lines. T3: 200–500 lines.

### Design Library
The governance document — defines frontmatter spec, naming conventions, dependency graph, and document index for the corpus. Only needed at T3 where the document count justifies formalized governance.

**Typical length:** T3: 150–300 lines.

### Canonical Specs
Machine-readable source of truth files — OpenAPI 3.1 YAML for APIs, JSON Schema for configurations, SQL DDL for database schemas. These live alongside the Markdown documents but are not Markdown themselves. If there's a conflict between a canonical spec and a Markdown document, the canonical spec governs types/schemas and the Markdown governs intent/rationale.

**Typical length:** Varies widely. An OpenAPI spec for 80 endpoints runs 4,000–6,000 lines.

---

## Metadata Standard

Every design document carries YAML frontmatter as its first block:

```yaml
---
title: string              # Human-readable document title
domain: string             # Product, plane, or module identifier
category: enum             # One of the document types above (lowercase, hyphenated)
status: draft | review | approved
version: "x.y"            # Semantic version, quoted to prevent YAML coercion
date: YYYY-MM-DD          # Date of last significant update
parent: filename | ~       # Parent document filename, or ~ for root
depends_on: []             # Filenames this document depends on
tags: []                   # Freeform keywords, lowercase, hyphenated
---
```

### Field Notes

- **domain**: Works for architectural planes, bounded contexts, services, or modules. Examples: `"core"`, `"data-plane"`, `"billing-service"`, `"shared"`.
- **category**: Must match one of the document types from the taxonomy table (lowercase, hyphenated: `root-architecture`, `functional-processes`, `ui-design`, `frontend-architecture`, `backend-architecture`, `api-specification`, `security-architecture`, `operational`, `decision-log`, `completeness-report`, `design-library`, `canonical-specs`).
- **parent**: The filename (not path) of this document's parent in the dependency hierarchy. Root Architecture documents use `~` (YAML null).
- **depends_on**: List of filenames this document explicitly references or depends on for context. Keep accurate — this powers cross-reference validation in P4.
- **status**: Documents start as `draft`, progress to `review` when complete, and reach `approved` after passing P4 quality gates.

---

## Multi-Domain Rule (T3 Only)

When a product has multiple architectural domains (e.g., Data Plane / Control Plane, or microservice boundaries), each domain gets its own set of documents mirroring the taxonomy.

### Naming Convention

```
{Product}-[{Domain}-]{Document-Type}.md

Examples:
  Primary domain:    Product-Backend-Architecture.md
  Secondary domain:  Product-CP-Backend-Architecture.md
  Shared:            Product-Testing-Strategy.md
```

### Cross-Domain Dependencies

Maintain a cross-domain dependency table (in the Design Library document) showing:
- Which secondary domain documents mirror which primary domain documents
- Cross-domain references (e.g., CP-Security → DP-Security for shared auth model)
- Shared documents that apply to all domains

### Authoring Order with Multiple Domains

1. Author the primary domain's documents first (Tiers 0–4)
2. Author secondary domain documents, explicitly referencing primary domain mirrors
3. Author shared/cross-cutting documents last (they depend on all domains)
```

- [ ] **Step 2: Verify the document reads correctly**

Run: `wc -l docs/paf/methodology/PAF-Document-Taxonomy.md`
Expected: ~180 lines

- [ ] **Step 3: Commit**

```bash
git add docs/paf/methodology/PAF-Document-Taxonomy.md
git commit -m "docs(paf): add PAF-Document-Taxonomy.md — document types, tiers, authoring order"
```

---

### Task 3: PAF-Phase-Guide.md

**Files:**
- Create: `docs/paf/methodology/PAF-Phase-Guide.md`

**Context:** The longest methodology document. Contains the full procedure for each of the 6 phases — what to do, in what order, with what inputs, producing what outputs, and passing what gate. A practitioner follows this step by step. Content is drawn from spec §6–§11 but expanded with procedural detail and checklists.

**Source sections from spec:** §6 (P1), §7 (P2), §8 (P3), §9 (P4), §10 (P5), §11 (P6)

- [ ] **Step 1: Create the document with this content**

```markdown
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

If yes, decompose into sub-products. Each gets its own P1–P6 cycle. Define the sequencing (which sub-product is designed first).

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
1. Author the primary domain's documents first (Tiers 0–4)
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
```

- [ ] **Step 2: Verify the document reads correctly**

Run: `wc -l docs/paf/methodology/PAF-Phase-Guide.md`
Expected: ~310 lines

- [ ] **Step 3: Commit**

```bash
git add docs/paf/methodology/PAF-Phase-Guide.md
git commit -m "docs(paf): add PAF-Phase-Guide.md — P1-P6 procedures and checklists"
```

---

### Task 4: PAF-Quality-Gates.md

**Files:**
- Create: `docs/paf/methodology/PAF-Quality-Gates.md`

**Context:** Expanded quality gate reference. Contains full gate definitions, step-by-step procedures for each consistency check, and the completeness report template. A practitioner uses this during P4 as a checklist. Also used by the `paf-qa` skill as its source of truth.

**Source sections from spec:** §9 (P4), expanded with procedural detail

- [ ] **Step 1: Create the document with this content**

```markdown
# PAF Quality Gates

This document defines every quality gate PAF uses to verify a design corpus. Gates are run during P4 (Quality Assurance) and scaled by complexity tier.

For phase context, see [PAF-Phase-Guide.md](PAF-Phase-Guide.md) §P4.

---

## Gate Summary

| Gate | What It Verifies | T1 | T2 | T3 |
|------|-----------------|:---:|:---:|:---:|
| **G1** | Metadata Integrity | Required | Required | Required |
| **G2** | Decision Coverage | Required | Required | Required |
| **G3** | Taxonomy Completeness | Required | Required | Required |
| **G4** | Internal Consistency | Skip | Required | Required |
| **G5** | Cross-Document Consistency | Skip | Required | Required |
| **G6** | Bidirectional References | Skip | Optional | Required |
| **G7** | Version Gate Alignment | Skip | Optional | Required |
| **G8** | Gap Analysis | Skip | Required | Required |

**Passing:** A gate passes when all its checks succeed with evidence recorded. A gate fails when any check fails. Failed gates must be resolved before P4 can exit.

---

## Gate Definitions

### G1: Metadata Integrity

**Purpose:** Ensure every document has valid, complete YAML frontmatter.

**Procedure:**
1. List all documents in the design corpus
2. For each document, verify:
   - [ ] YAML frontmatter block exists and parses without error
   - [ ] `title` field present and non-empty
   - [ ] `domain` field present and matches a known domain
   - [ ] `category` field present and matches the taxonomy enum
   - [ ] `status` field present and is one of: `draft`, `review`, `approved`
   - [ ] `version` field present and is a quoted string
   - [ ] `date` field present and is valid ISO 8601
   - [ ] `parent` field present (filename or `~`)
   - [ ] `depends_on` field present (list, may be empty)
   - [ ] `tags` field present (list, may be empty)
3. Verify `parent` references point to documents that exist
4. Verify `depends_on` references point to documents that exist

**Evidence:** Table of documents with pass/fail per field.

### G2: Decision Coverage

**Purpose:** Ensure every decision from P2 has been distributed to at least one design document.

**Procedure:**
1. Extract all D-{number} entries from the Decision Log
2. For each decision, read its "Distribute To" list
3. Verify that each target document contains a reference to the decision (explicit citation or the decision's content is reflected)

**Evidence:** Table of decisions with distribution status (distributed / missing).

### G3: Taxonomy Completeness

**Purpose:** Ensure all required document types for the assigned tier exist and are non-empty.

**Procedure:**
1. Look up the tier's required document types in [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md)
2. Verify each required type has a corresponding document in the corpus
3. Verify each document has substantive content (not just frontmatter and headings)

**Evidence:** Table of required types with existence and substantiveness check.

### G4: Internal Consistency

**Purpose:** Ensure no individual document contradicts itself.

**Procedure:** For each document:
1. Read the document end-to-end
2. Check for:
   - [ ] Prose that contradicts a table in the same document
   - [ ] Numbers or values that differ between sections (e.g., "30-minute timeout" in §2, "60-minute timeout" in §5)
   - [ ] Terms used inconsistently within the document
   - [ ] Section references that point to non-existent sections

**Evidence:** List of contradictions found (or "none") per document.

### G5: Cross-Document Consistency

**Purpose:** Ensure shared contracts match across all documents that reference them.

**Checks to run:**

| Check | Documents to Compare | What Must Match |
|-------|---------------------|----------------|
| Schema ↔ API | Backend Architecture ↔ API Specification | Every database column maps to an API response field |
| Auth ↔ Everything | Security Architecture ↔ API, Backend, Frontend | Role names, permission sets, token claims, lockout rules |
| Process ↔ API | Functional Processes ↔ API Specification | Every user-facing process has corresponding API endpoints |
| Tokens ↔ Frontend | UI Design ↔ Frontend Architecture ↔ secondary UI docs | Hex color values, spacing values, typography specs, motion curves |
| Config ↔ Docs | Backend Architecture ↔ Functional Processes ↔ Operational | Config key names, default values, environment variable names |
| Terminology ↔ All | Domain pack glossary (if exists) ↔ every document | Same term has same meaning everywhere |

**Procedure** for each check:
1. Extract the contract values from the defining document
2. Search all referencing documents for the same values
3. Flag any discrepancy (different value, missing reference, or conflicting definition)

Domain packs may inject additional checks specific to the domain.

**Evidence:** Table of checks with match/mismatch details.

### G6: Bidirectional References

**Purpose:** Ensure dependency relationships are acknowledged from both sides.

**Procedure:**
1. For each document, read its `depends_on` list
2. For each dependency A → B:
   - Check if B's `depends_on` includes A (bidirectional)
   - If not, verify the one-way relationship is justified (B is a foundational document that many things depend on — it doesn't need to know about each consumer)
3. Flag unjustified one-way references

**Evidence:** Table of dependency pairs with bidirectional status.

### G7: Version Gate Alignment

**Purpose:** Ensure features marked as "future" or deferred are consistently marked across all documents.

**Procedure:**
1. Extract all version-gated features from the Decision Log (entries with `Version Gate: future` or specific future version)
2. Search all documents for references to these features
3. Verify each reference is marked as deferred/future (not described as v1 functionality)
4. Verify no document depends on a deferred feature without noting the dependency is future-gated

**Evidence:** Table of deferred features with consistency status across documents.

### G8: Gap Analysis

**Purpose:** Ensure no critical architectural questions are left unanswered and no sections contain placeholder content.

**Procedure:**
1. Search all documents for:
   - [ ] TODO, TBD, FIXME, placeholder markers
   - [ ] Empty sections (heading with no content)
   - [ ] Questions posed but not answered
   - [ ] References to documents or sections that don't exist
2. Classify each finding as Critical (blocks implementation) or Minor (can be resolved during execution)
3. Critical findings must be resolved before P4 exits

**Evidence:** Table of gaps with severity and resolution status.

---

## Completeness Report Template

The P4 exit artifact. Create this document using the template below:

```markdown
# {Product Name} Design Completeness Report

**Date:** {YYYY-MM-DD}
**Tier:** {T1 | T2 | T3}
**Document Count:** {N}
**Assessor:** {name or "automated"}

---

## 1. Gate Results

| Gate | Required | Status | Evidence |
|------|:--------:|:------:|----------|
| G1: Metadata Integrity | {yes/no} | {✅/❌} | {summary} |
| G2: Decision Coverage | {yes/no} | {✅/❌} | {summary} |
| G3: Taxonomy Completeness | {yes/no} | {✅/❌} | {summary} |
| G4: Internal Consistency | {yes/no} | {✅/❌/⏭️} | {summary} |
| G5: Cross-Document Consistency | {yes/no} | {✅/❌/⏭️} | {summary} |
| G6: Bidirectional References | {yes/no} | {✅/❌/⏭️} | {summary} |
| G7: Version Gate Alignment | {yes/no} | {✅/❌/⏭️} | {summary} |
| G8: Gap Analysis | {yes/no} | {✅/❌/⏭️} | {summary} |

**Legend:** ✅ Pass | ❌ Fail | ⏭️ Skipped (not required for tier)

## 2. Consistency Findings

| Check | Documents Compared | Status | Detail |
|-------|-------------------|:------:|--------|
| Schema ↔ API | {docs} | {✅/⚠️/❌} | {detail} |
| Auth ↔ Everything | {docs} | {✅/⚠️/❌} | {detail} |
| Process ↔ API | {docs} | {✅/⚠️/❌} | {detail} |
| Tokens ↔ Frontend | {docs} | {✅/⚠️/❌} | {detail} |
| Config ↔ Docs | {docs} | {✅/⚠️/❌} | {detail} |
| Terminology ↔ All | {docs} | {✅/⚠️/❌} | {detail} |

**Legend:** ✅ Consistent | ⚠️ Minor discrepancy | ❌ Contradiction

## 3. Gaps Identified

| ID | Severity | Document | Section | Description | Resolution |
|----|:--------:|----------|---------|-------------|------------|
| GAP-1 | {Critical/Minor} | {doc} | {§N} | {description} | {resolution or "pending"} |

## 4. Status Promotion Recommendation

| Document | Current Status | Recommended | Blocker |
|----------|:--------------:|:-----------:|---------|
| {doc} | {draft/review} | {review/approved} | {blocker or "none"} |

## 5. Overall Assessment

**Implementation Ready:** {Yes / No — pending resolution of {N} critical gaps}

{Summary paragraph with key observations}
```

---

## Resolution Workflow

When a gate fails:

1. **Identify** — which gate, which check, which documents
2. **Assign** — each finding to a specific document and section for resolution
3. **Resolve** — fix the document (update content, add missing references, correct values)
4. **Re-run** — execute only the affected gate(s), not the full suite
5. **Record** — update the Completeness Report with resolution details
6. **Repeat** — until all required gates pass

**Escalation:** If a finding cannot be resolved without architectural changes, it becomes a new Decision Record entry and flows back through P3 (affected documents only) → P4 (re-run gates).
```

- [ ] **Step 2: Verify the document reads correctly**

Run: `wc -l docs/paf/methodology/PAF-Quality-Gates.md`
Expected: ~200 lines

- [ ] **Step 3: Commit**

```bash
git add docs/paf/methodology/PAF-Quality-Gates.md
git commit -m "docs(paf): add PAF-Quality-Gates.md — gate definitions and report template"
```

---

## Chunk 2: Tooling & Domain Packs

### Task 5: PAF-Tooling-Guide.md

**Files:**
- Create: `docs/paf/methodology/PAF-Tooling-Guide.md`

**Context:** The tooling reference document. Defines capabilities needed by phase, the reference tooling stack (Claude Code binding), the synergy map, and selection guidance for alternative stacks. A practitioner uses this when setting up their development environment or choosing tools for a new product.

**Source sections from spec:** §12 (Tooling Guide & Synergy Model)

- [ ] **Step 1: Create the document with this content**

```markdown
# PAF Tooling Guide

PAF is tool-agnostic in process but opinionated in recommendations. This document defines what capabilities are needed, recommends tools per capability, and maps synergies between tool pairings.

For phase procedures, see [PAF-Phase-Guide.md](PAF-Phase-Guide.md).

---

## Capabilities by Phase

| Phase | Capabilities Needed |
|-------|-------------------|
| P1 Intake | Conversational Q&A, structured data capture |
| P2 Requirements | Decision recording, domain knowledge storage, requirement categorization |
| P3 Authoring | Document generation, template scaffolding, cross-reference validation |
| P4 QA | Consistency checking, gap analysis, report generation |
| P5 Planning | Task decomposition, dependency graphing, estimation |
| P6 Execution | Code generation, testing, review, CI/CD, version control |

---

## Reference Tooling Stack

This is the concrete implementation validated by a real enterprise platform build. It targets Claude Code as the AI engine.

### Design & Authoring (P1–P5)

| Capability | Tool | Why |
|-----------|------|-----|
| AI-assisted design | Claude Code (Opus) | Powers conversational intake, document generation, quality checks |
| Skill orchestration | Superpowers skills | Chains PAF phases as executable, repeatable skills |
| Subagent dispatch | Claude Code Agent tool | Fresh context per task, parallel research |
| Version control | Git + GitHub | Document versioning, branch isolation, PR-based review |
| Document format | Markdown + YAML frontmatter | Portable, diffable, metadata-rich, tooling-friendly |

### API & Types

| Capability | Tool | Why |
|-----------|------|-----|
| API specification | OpenAPI 3.1 (YAML) | Industry standard, machine-readable, broad tooling support |
| Spec validation | Redocly CLI | Lint, bundle, validate on every PR |
| Type generation | openapi-typescript + openapi-fetch | Compile-time API type safety from spec |

### Frontend

| Capability | Tool | Why |
|-----------|------|-----|
| Framework | Svelte 5 + SvelteKit | Compiler-first, zero runtime, static adapter for embedding |
| Component library | Bits UI 2.x | Headless accessible primitives, native Svelte 5 runes |
| Icons | phosphor-svelte | Tree-shaken SVG, consistent weight system |
| Styling | CSS Custom Properties | Design system ownership, no utility framework dependency |

### Backend

| Capability | Tool | Why |
|-----------|------|-----|
| Language | Go 1.24+ | Static binaries, strong stdlib, go:embed for assets |
| Database (embedded) | SQLCipher | Encrypted SQLite, zero infrastructure, single-node |
| Database (multi-tenant) | PostgreSQL | Row-level security, proven at scale |

### Testing

| Capability | Tool | Why |
|-----------|------|-----|
| Unit testing (frontend) | Vitest (browser mode) | Real DOM, same engine as E2E |
| E2E testing | Playwright | Cross-browser, reliable selectors, screenshots |
| Unit testing (backend) | Go testing + testify | Standard Go tooling |
| API contract testing | Redocly CLI + custom Go tests | Route registration matches spec paths |

### Operations

| Capability | Tool | Why |
|-----------|------|-----|
| Containers | Docker | Reproducible builds, CI environments |
| CI/CD | GitHub Actions | Integrated with Git, free for open source |
| MCP servers | context7, docker, godoc, playwright, svelte | AI-accessible tool integrations |

---

## Synergy Map

Tools aren't chosen in isolation. The highest-value tool decisions are **pairings** where the combination is more valuable than the sum.

| Pair | Synergy Effect |
|------|---------------|
| **OpenAPI + openapi-typescript** | Change the YAML spec → regenerate types → compiler catches every frontend mismatch automatically |
| **OpenAPI + Redocly CLI** | Every PR touching the spec gets lint-checked. Broken refs, missing descriptions caught before merge |
| **Svelte 5 + Bits UI** | Both use runes (`$props()`, `$state()`). No adapter layer, no framework mismatch, minimal bundle |
| **Go + go:embed** | Frontend builds to static files, Go embeds them. Single binary serves everything — zero deployment dependencies |
| **Git worktrees + Subagents** | Each AI agent works in its own worktree. No file conflicts, true parallelism |
| **Claude Code + MCP servers** | AI can query docs (context7), run containers (docker), check language docs (godoc), test in browser (playwright) — all without leaving the conversation |
| **Vitest browser mode + Playwright** | Same browser engine for unit and E2E tests. Consistent behavior, shared utilities |
| **YAML frontmatter + Grep/Glob** | Any tool can search and filter design documents by metadata fields. Enables automated quality gates |
| **CSS Custom Properties + design tokens** | Tokens defined once in CSS variables, referenced everywhere. Runtime themeable, no build step for color changes |
| **Markdown + Git** | Documents are diffable, reviewable in PRs, blame-able. Design decisions have the same version history as code |

---

## Choosing Alternative Tools

When PAF is used with a different tech stack, select tools by capability using these criteria:

### Frontend Framework Selection

**PAF requires:**
- Component model that supports design system tokens (CSS custom properties or equivalent)
- Ability to produce static assets embeddable in a backend binary (if applicable)
- Headless component library available (for accessibility)
- Type-safe API client available (for OpenAPI integration)

**Options with synergy notes:**
| Framework | Best When | Synergy |
|-----------|-----------|---------|
| Svelte 5 + SvelteKit | Embedded/air-gapped products, single-binary deployment | static adapter + go:embed = zero deployment deps |
| React 19 + Next.js | Ecosystem breadth, large existing team | Largest component ecosystem, most hiring pool |
| Vue 3 + Nuxt | Middle ground, progressive adoption | Good DX, smaller bundle than React |

### Backend Language Selection

**PAF requires:**
- Static or easily-deployable binaries (for on-prem/embedded products)
- Strong standard library (reduce dependency count)
- Embeddable asset serving (for single-binary products)
- Good concurrency model (for real-time features)

**Options with synergy notes:**
| Language | Best When | Synergy |
|----------|-----------|---------|
| Go | Single-binary deployment, minimal runtime, cloud-native | go:embed + static builds, excellent for CLI + API |
| Rust | Maximum performance, memory safety critical | Single binary, but steeper learning curve |
| TypeScript/Node.js | Team already knows JS, SSR-heavy product | Shared language with frontend, but runtime dependency |
| Java/Kotlin | Enterprise ecosystem, Spring Boot familiarity | JVM maturity, but larger deployment footprint |

### Database Selection

**PAF requires:**
- Schema matches design documents (DDL in Backend Architecture)
- Encryption at rest (for security-conscious products)
- Migration tooling available

**Options with synergy notes:**
| Database | Best When | Synergy |
|----------|-----------|---------|
| SQLCipher | Single-node, embedded, air-gapped | Zero infrastructure, encrypted, ships with binary |
| PostgreSQL | Multi-tenant, cloud-hosted, needs RLS | Row-level security, proven at scale, rich ecosystem |
| SQLite | Single-node, simple persistence | Zero infrastructure, but no encryption by default |
| MySQL/MariaDB | Existing infrastructure, team familiarity | Widely deployed, but weaker RLS story |

---

## Domain Pack Tooling Extensions

Domain packs can recommend domain-specific tools beyond the base stack:

```yaml
# Example from domain-packs/itsm.yaml
tooling_recommendations:
  - capability: governance_policy_engine
    recommended: OPA/Rego
    rationale: "Non-Turing-complete, additive-only policies"
    synergy: "Embedded as Go library — no network hop for policy evaluation"
  - capability: audit_trail
    recommended: "Append-only table with hash chains"
    rationale: "Tamper-evident, no additional infrastructure"
    synergy: "Same DB engine as primary data store"
  - capability: sandbox_execution
    recommended: "Deno isolates (single-node) / Firecracker (distributed)"
    rationale: "Capability-based security, warm pool for latency"
    synergy: "Deno runs TypeScript/JavaScript — matches skill authoring language"
```

When the engine detects a domain pack match during P1, it includes these recommendations in the P5 tooling section.
```

- [ ] **Step 2: Verify the document reads correctly**

Run: `wc -l docs/paf/methodology/PAF-Tooling-Guide.md`
Expected: ~170 lines

- [ ] **Step 3: Commit**

```bash
git add docs/paf/methodology/PAF-Tooling-Guide.md
git commit -m "docs(paf): add PAF-Tooling-Guide.md — capabilities, synergies, selection guidance"
```

---

### Task 6: PAF-Domain-Pack-Specification.md

**Files:**
- Create: `docs/paf/methodology/PAF-Domain-Pack-Specification.md`

**Context:** Defines how domain packs work — the schema, how they're captured during design engagements, how they're versioned and reused, and how they plug into each phase. This is the specification that makes PAF self-improving.

**Source sections from spec:** §7 (domain knowledge capture), §12 (domain pack tooling extensions)

- [ ] **Step 1: Create the document with this content**

```markdown
# PAF Domain Pack Specification

Domain packs are PAF's learning mechanism. Each design engagement captures domain-specific knowledge — questions, constraints, terminology, quality checks — and packages it for reuse in future engagements.

This document defines the domain pack schema, capture process, versioning, and how packs integrate with each PAF phase.

---

## What Domain Packs Contain

A domain pack encodes knowledge that is **specific to a vertical or problem domain** but **generic across products in that domain**. Examples:

- An ITSM pack knows to ask about ITIL alignment, change authority models, and autonomy tiers
- A fintech pack knows to ask about regulatory compliance, transaction auditing, and PCI-DSS
- An IoT pack knows to ask about device provisioning, firmware updates, and edge computing

Domain packs do NOT contain:
- Product-specific decisions (those belong in the Decision Log)
- Code or implementation details (those belong in the implementation plan)
- Tool configurations (those belong in the Tooling Guide)

---

## Schema

Domain packs are YAML files following this schema:

```yaml
# domain-packs/{domain-name}.yaml

name: string                    # Human-readable domain name
version: "x.y"                 # Semantic version, quoted
captured_from: string           # Product name where this knowledge was first captured
date: YYYY-MM-DD               # Date of last update
description: string             # One-paragraph description of the domain

# P1: Additional intake questions for this domain
intake_questions:
  - "Question text?"
  - "Question text with context?"

# P2: Additional requirement prompts by category
requirement_prompts:
  functional:
    - "Domain-specific functional question?"
  security:
    - "Domain-specific security question?"
  operational:
    - "Domain-specific operational question?"
  # ... any category from the taxonomy

# P3: Additional section templates for specific document types
document_section_templates:
  functional_processes:
    - "Section heading or topic to include"
    - "Another section heading"
  backend_architecture:
    - "Domain-specific backend section"

# P4: Additional quality checks beyond the standard gates
quality_checks:
  - description: "What to check"
    documents: ["doc-type-1", "doc-type-2"]
    check: "What must be consistent"

# Domain glossary — terms with specific meaning in this domain
terminology:
  - term: "Term Name"
    definition: "What it means in this domain"
  - term: "Another Term"
    definition: "Definition"

# P5/P6: Domain-specific tooling recommendations
tooling_recommendations:
  - capability: "what_it_does"
    recommended: "Tool Name"
    rationale: "Why this tool for this domain"
    synergy: "How it pairs with other tools"
```

All fields except `name`, `version`, and `date` are optional. A domain pack can start with just intake questions and grow over time.

---

## Capture Process

Domain knowledge is captured at two points in the PAF lifecycle:

### During P2 (Requirements & Decisions)

As the practitioner works through requirement categories, the engine (or human) watches for patterns that would apply to other products in the same vertical:

1. **Questions that surprised** — questions the practitioner hadn't considered but proved important
2. **Constraints that recurred** — regulatory, operational, or architectural constraints specific to the domain
3. **Terminology that needed definition** — domain-specific terms that caused confusion
4. **Architectural patterns that emerged** — patterns like "governance as separate process" or "hash-chained audit trail" that are domain-typical

At the end of P2, the captured knowledge is assembled into a draft domain pack and presented for review.

### During P6 (Execution & Delivery)

As the product is built, the team discovers things the design missed or got wrong. Some of these are product-specific, but others are domain-general:

1. **Quality checks that would have caught issues earlier** — add to `quality_checks`
2. **Tooling that proved essential for the domain** — add to `tooling_recommendations`
3. **New terminology that emerged during implementation** — add to `terminology`
4. **Document sections that were missing from templates** — add to `document_section_templates`

At the end of P6, the domain pack is updated and versioned.

---

## Versioning

Domain packs use semantic versioning:

- **Patch (x.y.Z):** Typo fixes, clarification of existing entries
- **Minor (x.Y.0):** New questions, checks, or terminology added
- **Major (X.0.0):** Structural changes, removed entries, changed semantics

Version history is tracked via git (the pack files live in the repository).

---

## Integration with PAF Phases

### P1: Intake & Scoping

The engine matches the product description against known domain packs using keyword signals. If a match is found:
1. Load the pack's `intake_questions`
2. Append them to the standard intake questionnaire (after the complexity signals)
3. Note the matched domain pack in the intake brief

**Matching heuristics:**
- Keywords in the product description
- Industry vertical mentioned
- Similar complexity signals to the pack's `captured_from` product
- Explicit user selection ("this is an ITSM product")

### P2: Requirements & Decisions

If a domain pack matched:
1. Load `requirement_prompts` for each category
2. Inject domain-specific questions into the progressive depth exploration
3. Pre-populate the terminology glossary for consistency checking

### P3: Design Authoring

If a domain pack matched:
1. Load `document_section_templates`
2. Inject domain-specific sections into document scaffolds
3. Reference terminology definitions in section templates

### P4: Quality Assurance

If a domain pack matched:
1. Load `quality_checks`
2. Add them to the G5 (Cross-Document Consistency) check suite
3. Run domain-specific checks alongside standard checks

### P5: Implementation Planning

If a domain pack matched:
1. Load `tooling_recommendations`
2. Include domain-specific tools in the plan's tooling section
3. Note synergies with the base stack

### P6: Execution & Delivery

1. Capture new domain knowledge throughout execution
2. At the end, update the domain pack
3. Increment version

---

## Template

An empty domain pack template is provided at `domain-packs/_template.yaml`:

```yaml
name: ""
version: "0.1"
captured_from: ""
date: YYYY-MM-DD
description: ""

intake_questions: []

requirement_prompts: {}

document_section_templates: {}

quality_checks: []

terminology: []

tooling_recommendations: []
```

---

## Example: ITSM Domain Pack

The first domain pack, captured from an enterprise ITSM platform design process:

```yaml
name: IT Service Management
version: "1.0"
captured_from: enterprise ITSM platform
date: 2026-03-12
description: >
  Knowledge pack for AI-powered ITSM products — autonomous agents
  that handle incidents, problems, changes, and service requests
  within enterprise IT environments.

intake_questions:
  - "Does the product align with ITIL process areas? Which ones?"
  - "What change authority model applies? (advisory / approval / autonomous)"
  - "Are there maturity-gated capabilities (features unlocked by demonstrated competence)?"
  - "Does the product need to integrate with existing ITSM tools? (ServiceNow, Jira, etc.)"
  - "Is there a human-in-the-loop requirement for high-risk actions?"

requirement_prompts:
  functional:
    - "How does agent autonomy evolve over time? Is there a tier/maturity model?"
    - "What triggers agent actions? (events, schedules, human requests, other agents)"
    - "How are agent skills/capabilities packaged and distributed?"
  security:
    - "How does the system defend against prompt injection in AI-generated actions?"
    - "Is there an immutable audit trail requirement? Append-only? Hash-chained?"
    - "Does the governance layer need to be architecturally separate from the runtime?"
  operational:
    - "What is the agent lifecycle? (install → configure → learn → operate → retire)"
    - "How does the system handle agent-to-agent communication?"
    - "What sandbox model is used for code execution? (Deno, Firecracker, containers)"

document_section_templates:
  functional_processes:
    - "Agent Lifecycle Management (install → configure → activate → learn → operate → retire)"
    - "Escalation Flows (autonomous → supervised → human-required)"
    - "Skill Hierarchy (runbooks → actions → subskills → skills → workflows → playbooks)"
  backend_architecture:
    - "Governance Layer (separate process, IPC protocol)"
    - "Sandbox Pool (warm pool, execution isolation, resource limits)"
    - "Memory System (role, organizational, episodic, collaborative, procedural)"
  security_architecture:
    - "Prompt Injection Defense (governance backstop is deterministic, not AI-dependent)"
    - "Zero-Knowledge Architecture (operator cannot access client operational data)"
    - "Action Risk Classification (observe → inform → suggest → act-low → act-high)"

quality_checks:
  - description: "Autonomy tier consistency"
    documents: ["security-architecture", "api-specification", "backend-architecture"]
    check: "Tier definitions (A/B/C or equivalent) match across all documents"
  - description: "Agent memory layer consistency"
    documents: ["root-architecture", "backend-architecture", "functional-processes"]
    check: "Memory layer names and count match everywhere"
  - description: "Governance separation enforcement"
    documents: ["backend-architecture", "security-architecture"]
    check: "Governance is documented as a separate process, not embedded in runtime"

terminology:
  - term: "Autonomy Tier"
    definition: "Classification of agent decision-making authority. Tier A = full autonomous, Tier B = supervised, Tier C = maturity-gated."
  - term: "Decision Trace"
    definition: "Immutable record of an agent's reasoning chain, stored for audit replay."
  - term: "Skill"
    definition: "A packaged unit of agent capability, installable from a marketplace."
  - term: "Governance Layer"
    definition: "Architecturally separate process that enforces policies, records audit trails, and manages human escalation."
  - term: "Warm Pool"
    definition: "Pre-initialized sandbox instances kept ready for immediate use, reducing execution latency."

tooling_recommendations:
  - capability: governance_policy_engine
    recommended: OPA/Rego
    rationale: "Non-Turing-complete, additive-only — policies can restrict but never override tier rules"
    synergy: "Embedded as Go library — no network hop for policy evaluation"
  - capability: audit_trail
    recommended: "Append-only table with hash chains"
    rationale: "Tamper-evident without external dependencies"
    synergy: "Same DB engine as primary data store"
  - capability: sandbox_execution
    recommended: "Deno isolates (single-node) / Firecracker (distributed)"
    rationale: "Capability-based security model, warm pool pattern for latency"
    synergy: "Deno runs JS/TS — matches common skill authoring language"
  - capability: channel_adapters
    recommended: "Slack API, Microsoft Graph API, SMTP/IMAP"
    rationale: "Enterprise collaboration platforms where ITSM agents interact with humans"
    synergy: "Adapter pattern — same internal interface, different external protocols"
```
```

- [ ] **Step 2: Verify the document reads correctly**

Run: `wc -l docs/paf/methodology/PAF-Domain-Pack-Specification.md`
Expected: ~240 lines

- [ ] **Step 3: Commit**

```bash
git add docs/paf/methodology/PAF-Domain-Pack-Specification.md
git commit -m "docs(paf): add PAF-Domain-Pack-Specification.md — schema, capture, ITSM example"
```

---

## Final Step

### Task 7: Verify Complete Methodology and Final Commit

**Files:**
- Verify: `docs/paf/methodology/PAF-Overview.md`
- Verify: `docs/paf/methodology/PAF-Document-Taxonomy.md`
- Verify: `docs/paf/methodology/PAF-Phase-Guide.md`
- Verify: `docs/paf/methodology/PAF-Quality-Gates.md`
- Verify: `docs/paf/methodology/PAF-Tooling-Guide.md`
- Verify: `docs/paf/methodology/PAF-Domain-Pack-Specification.md`

- [ ] **Step 1: Verify all 6 files exist**

Run: `ls -la docs/paf/methodology/`
Expected: 6 `.md` files

- [ ] **Step 2: Verify cross-references**

Check that all relative links between documents resolve:
- PAF-Overview.md references all 5 other docs ✓
- PAF-Phase-Guide.md references Taxonomy, Quality Gates, Tooling, Domain Packs ✓
- PAF-Quality-Gates.md references Phase Guide ✓
- PAF-Tooling-Guide.md references Phase Guide ✓
- PAF-Domain-Pack-Specification.md is self-contained ✓

- [ ] **Step 3: Final verification commit**

Run: `git log --oneline -7` to verify all commits are clean.

---

**Plan complete. Ready to execute?**

Execution will use superpowers:subagent-driven-development — fresh subagent per task with two-stage review (spec compliance + code quality).
