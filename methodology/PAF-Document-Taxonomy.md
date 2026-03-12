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
| **Decision Log** | Q&A-style decision record with rationale and distribution targets | Required | Required | Required |
| **Completeness Report** | Cross-document consistency audit, gap analysis, status promotion | Skip | Optional | Required |
| **Design Library** | Document governance: frontmatter spec, naming convention, dependency graph, index | Skip | Skip | Required |
| **Canonical Specs** | Machine-readable source of truth (OpenAPI, JSON Schema, DDL) | Skip | Optional | Required |

### Reading the Table

- **Required** — must exist and pass quality gates before P4
- **Optional** — recommended but not gated; include if the product warrants it
- **Skip** — unnecessary at this tier; adding it is overhead without value
- **Required (if UI)** — required only when the product has a user interface

**T1 note:** T1 products skip the Completeness Report. The P4 exit gate is satisfied by passing G1–G3 inline — no separate report document is needed. The Decision Log is required at all tiers because the D-{number} traceability chain is foundational to PAF's consistency guarantees.

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

1. Author the primary domain's documents first (Layers 0–4)
2. Author secondary domain documents, explicitly referencing primary domain mirrors
3. Author shared/cross-cutting documents last (they depend on all domains)
