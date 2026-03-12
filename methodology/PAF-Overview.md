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
- **312-task implementation plan** across 37 sprints
- **Zero cross-document contradictions** verified by 8 targeted consistency checks
- **64/64 design tokens** consistent across all UI documents

This proof case validates that PAF's quality gates catch contradictions that would otherwise surface mid-sprint.

---

## Security of the Design Process

The design corpus itself may contain sensitive information — threat models, authentication flows, database schemas, API keys patterns, and trust boundaries. Treat it accordingly.

### For Proprietary Products

- **Access control:** Restrict access to the design corpus repository. Security Architecture and Backend Architecture documents contain the most sensitive content.
- **No secrets in documents:** Design documents describe security *mechanisms*, not credentials. Never include actual API keys, passwords, connection strings, or tokens. Use placeholders (e.g., `${DB_PASSWORD}`).
- **Encrypted storage:** For regulated industries (healthcare, finance, government), store the design corpus in an encrypted repository or use encrypted-at-rest storage.

### For Domain Packs

- **Anonymize before sharing:** Domain packs must not contain client names, proprietary product names, or internal codenames. Use generic descriptions (e.g., "enterprise ITSM platform" not "Acme Corp HIVE").
- **Review before committing:** Treat domain pack commits like code reviews. Check `terminology`, `quality_checks`, and `document_section_templates` for inadvertent proprietary details.
- **Separate proprietary packs:** If a domain pack contains knowledge that cannot be anonymized, keep it in a private repository. Only share packs with domain-general knowledge.

### Audit Trail

For products in regulated industries:
- Use git signed commits for all design document changes
- Tag phase completion points (`p1-complete`, `p2-complete`, etc.) for audit traceability
- Decision Log entries provide the rationale chain — ensure "Distribute To" links are maintained so auditors can trace from requirement → decision → design → implementation

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
