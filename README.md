# Product Architecture Framework (PAF)

**A reusable system for designing software products at enterprise depth.**

PAF takes a product idea from initial intake through delivered software, producing a comprehensive, internally consistent design corpus, implementation plan, and execution framework. It auto-scales — a CLI tool gets 5 documents and 3 quality gates; an enterprise platform gets 25+ documents, 8 quality gates, and machine-readable canonical specs.

---

## Why PAF Exists

Most software projects fail not because the code is bad, but because the design was incomplete, inconsistent, or never existed. Teams discover contradictions mid-sprint. API specs drift from database schemas. Security models described in one document contradict auth flows in another. The cost of fixing these issues grows exponentially the later they're found.

PAF was reverse-engineered from a real enterprise platform design process that produced 22 interconnected design documents (61,000+ lines), 2 canonical OpenAPI specs (155 endpoints, 138 schemas), and a 312-task implementation plan — with **zero cross-document contradictions**. The goal is to make that level of quality repeatable, scalable, and learnable.

---

## The Three Layers

### Layer 1 — Methodology
Portable, human-readable Markdown documents describing the complete product design lifecycle. Anyone can follow these manually without tooling. This is what gets published, handed to teams, or taught.

### Layer 2 — Engine
A chain of 6 Claude Code skills that automate the methodology. Each skill runs one phase, produces artifacts, verifies exit gates, and hands off to the next. The reference implementation targets Claude Code, but the methodology is tool-agnostic.

### Layer 3 — Domain Packs
YAML files encoding domain-specific questions, constraints, templates, and quality checks. Captured automatically during each design engagement. Each product designed with PAF makes the system smarter for the next one in that domain.

---

## Lifecycle Phases

PAF defines 6 phases. Each has entry criteria, prescribed activities, artifacts, and an exit gate that must pass before proceeding.

```
P1 Intake  →  P2 Requirements  →  P3 Authoring  →  P4 QA  →  P5 Planning  →  P6 Execution
```

| Phase | Name | What It Produces | Exit Gate |
|:-----:|------|-----------------|-----------|
| **P1** | Intake & Scoping | Intake brief, complexity score, tier assignment | Tier assigned, scope confirmed |
| **P2** | Requirements & Decisions | Decision record with rationale and distribution targets | All critical questions answered |
| **P3** | Design Authoring | Design document corpus (right-sized to tier) | All docs pass internal consistency |
| **P4** | Quality Assurance | Completeness report, cross-reference audit | Zero contradictions, all dependencies satisfied |
| **P5** | Implementation Planning | Task-level plan with sprint structure and effort estimates | Every task maps to a design section |
| **P6** | Execution & Delivery | Working software, test results, release artifacts | All tasks complete, quality reviews passed |

---

## Complexity Tiers

The system assesses complexity during P1 and assigns a tier. The tier determines how much process to apply — which document types are required, which quality gates are enforced, and what level of detail is expected.

| Tier | Name | When It Applies | Documents | Quality Gates |
|:----:|------|----------------|:---------:|:-------------:|
| **T1** | Utility | Single concern, 1 binary, <10 endpoints, no auth | 3–5 | G1–G3 |
| **T2** | Application | Multiple concerns, auth, persistent state, UI | 8–12 | G1–G5, G8 |
| **T3** | Platform | Multi-tenant, >50 endpoints, security model, governance | 15–25+ | G1–G8 |

**Complexity signals scored during intake:**

| Signal | T1 | T2 | T3 |
|--------|:---:|:---:|:---:|
| User roles | 1 | 2–3 | 4+ |
| UI screens | 0 | 3–10 | 11+ |
| Data stores | 0–1 | 1–2 | 3+ |
| External integrations | 0–1 | 2–4 | 5+ |
| Multi-tenancy / billing | No | No | Yes |
| Security / compliance | Basic | Auth + RBAC | Threat model + audit + encryption |
| Deployment components | 1 | 1–2 | 3+ |

The highest tier triggered by any signal wins. You can override upward (never downward).

---

## Quick Start

### Option A: Manual Process (No Tooling Required)

Read the methodology documents in order:

1. **[PAF-Overview.md](methodology/PAF-Overview.md)** — Understand the framework
2. **[PAF-Phase-Guide.md](methodology/PAF-Phase-Guide.md)** — Walk through P1–P6
3. **[PAF-Document-Taxonomy.md](methodology/PAF-Document-Taxonomy.md)** — Know which documents to write
4. **[PAF-Quality-Gates.md](methodology/PAF-Quality-Gates.md)** — Verify quality (G1–G8)
5. **[PAF-Tooling-Guide.md](methodology/PAF-Tooling-Guide.md)** — Select tools for your stack
6. **[PAF-Domain-Pack-Specification.md](methodology/PAF-Domain-Pack-Specification.md)** — Capture and reuse domain knowledge

Use the [templates](templates/) to scaffold your documents. Check the [ITSM example](examples/itsm-platform/) to see what each phase produces.

### Option B: Claude Code Skills (Automated)

If you use [Claude Code](https://docs.anthropic.com/en/docs/claude-code), copy the `skills/` directory into your project's `.claude/skills/` directory:

```bash
cp -r skills/paf-* /path/to/your-project/.claude/skills/
```

Then start a design engagement:

```
/paf-intake
```

The skills chain automatically: P1 intake → P2 requirements → P3 authoring → P4 QA → P5 planning → P6 execution. Each skill reads the methodology docs as its source of truth, asks questions one at a time, produces artifacts, and verifies exit gates before handing off.

---

## Repository Structure

```
paf/
├── README.md                          # This file
│
├── methodology/                       # Layer 1: The Methodology
│   ├── PAF-Overview.md                # Entry point — architecture, tiers, lifecycle
│   ├── PAF-Phase-Guide.md             # Complete P1–P6 procedures
│   ├── PAF-Document-Taxonomy.md       # Document types, tier requirements, authoring order
│   ├── PAF-Quality-Gates.md           # G1–G8 gate definitions and procedures
│   ├── PAF-Tooling-Guide.md           # Tool capabilities, synergies, selection guidance
│   └── PAF-Domain-Pack-Specification.md  # Domain pack schema, capture process, reuse
│
├── templates/                         # Document Templates (12 types)
│   ├── root-architecture.md           # Product identity, deployment, key decisions
│   ├── functional-processes.md        # Operational flows, state machines, lifecycle
│   ├── ui-design.md                   # Design system, components, screens
│   ├── frontend-architecture.md       # Stack, build pipeline, state management
│   ├── backend-architecture.md        # Binary structure, DB schema, packages
│   ├── security-architecture.md       # Threat model, auth, RBAC, trust boundaries
│   ├── api-specification.md           # Endpoints, schemas, WebSocket, CLI mapping
│   ├── operational.md                 # Testing, observability, release process
│   ├── decision-log.md                # D-{number} format decision record
│   ├── completeness-report.md         # Quality gate results, consistency audit
│   ├── design-library.md              # Document governance (T3 only)
│   └── implementation-plan.md         # Sprint structure, TDD task decomposition
│
├── skills/                            # Layer 2: The Engine (Claude Code skills)
│   ├── paf-intake/SKILL.md            # P1: Intake questionnaire, complexity scoring
│   ├── paf-requirements/SKILL.md      # P2: Requirements exploration, decision recording
│   ├── paf-author/SKILL.md            # P3: Document scaffolding, layer-order authoring
│   ├── paf-qa/SKILL.md                # P4: Quality gates G1–G8, completeness report
│   ├── paf-plan/SKILL.md              # P5: Component extraction, sprint grouping, TDD tasks
│   └── paf-execute/SKILL.md           # P6: Execution guardrails, design feedback loop
│
├── domain-packs/                      # Layer 3: Domain Knowledge
│   ├── _template.yaml                 # Empty scaffold for new domain packs
│   └── itsm.yaml                      # IT Service Management domain pack
│
└── examples/                          # Worked Examples
    └── itsm-platform/                 # T3 enterprise ITSM platform (proof case)
        ├── README.md                  # What PAF produced for this product
        ├── intake-brief.md            # P1 output: core identity + complexity signals
        ├── decision-record.md         # P2 output: sample decision entries
        └── document-index.md          # P3 output: 22-document corpus index
```

---

## Document Templates

Each template includes YAML frontmatter with placeholder variables (`{Product}`, `{domain}`, `{date}`), section headings with guidance comments, and tier annotations indicating which sections apply at which complexity level.

Templates are used during P3 (Design Authoring) — the `paf-author` skill reads them automatically, or you can copy and fill them manually.

| Template | Sections | Used By |
|----------|:--------:|:-------:|
| root-architecture | 10 | T1+ |
| functional-processes | 6 | T2+ |
| ui-design | 9 | T2+ (if UI) |
| frontend-architecture | 12 | T2+ (if UI) |
| backend-architecture | 10 | T2+ |
| security-architecture | 10 | T3 |
| api-specification | 5 | T2+ |
| operational | 4 | T3 |
| decision-log | — | T2+ |
| completeness-report | 5 | T3 |
| design-library | — | T3 |
| implementation-plan | 3 | T1+ |

---

## Quality Gates

PAF defines 8 quality gates, run during P4 and scaled by tier:

| Gate | Name | What It Checks | T1 | T2 | T3 |
|:----:|------|---------------|:---:|:---:|:---:|
| G1 | Metadata Integrity | YAML frontmatter, dependency references resolve | Yes | Yes | Yes |
| G2 | Decision Coverage | Every decision distributed to its target documents | Yes | Yes | Yes |
| G3 | Taxonomy Completeness | All required document types exist with content | Yes | Yes | Yes |
| G4 | Internal Consistency | No self-contradictions within a document | — | Yes | Yes |
| G5 | Cross-Document Consistency | Schema/API, auth/everything, tokens/frontend align | — | Yes | Yes |
| G6 | Bidirectional References | If A depends on B, B acknowledges A | — | — | Yes |
| G7 | Version Gate Alignment | Deferred features consistently marked everywhere | — | — | Yes |
| G8 | Gap Analysis | No TODO/TBD/FIXME/empty sections remain | — | Yes | Yes |

---

## Domain Packs

Domain packs are YAML files that encode domain-specific knowledge, making PAF smarter for repeat engagements in the same vertical. A pack contains:

| Field | Purpose |
|-------|---------|
| `intake_questions` | Domain-specific questions added to P1 |
| `requirement_prompts` | Guided prompts by category for P2 |
| `document_section_templates` | Extra sections injected into document templates during P3 |
| `quality_checks` | Domain-specific consistency checks for P4 |
| `terminology` | Canonical definitions to enforce consistency |
| `tooling_recommendations` | Domain-relevant tools and integrations |

### Creating a Domain Pack

1. Copy `domain-packs/_template.yaml`
2. Fill in knowledge from your domain expertise or a completed PAF engagement
3. The `paf-execute` skill (P6) automatically harvests new domain knowledge at the end of each project

### Available Packs

| Pack | Domain | Questions | Prompts | Checks |
|------|--------|:---------:|:-------:|:------:|
| [itsm.yaml](domain-packs/itsm.yaml) | IT Service Management | 7 | 17 | 6 |

---

## Worked Example: ITSM Platform

The [examples/itsm-platform/](examples/itsm-platform/) directory contains artifacts from the first product designed with PAF — a T3 enterprise agentic AI gateway for autonomous ITSM agents. This serves as the reference for what each phase produces.

**Results:**
- 22 design documents across 2 architectural planes (Data Plane + Control Plane)
- 61,000+ lines of internally consistent specification
- 2 canonical OpenAPI 3.1 specs (155 endpoints, 138 schemas)
- 312-task implementation plan across 37 sprints
- Zero cross-document contradictions (verified by 8 consistency checks)
- 64/64 design tokens consistent across all UI documents

**Included artifacts:**
- [Intake Brief](examples/itsm-platform/intake-brief.md) — P1 output showing complexity assessment
- [Decision Record](examples/itsm-platform/decision-record.md) — P2 output showing the decision format
- [Document Index](examples/itsm-platform/document-index.md) — P3 output showing the full corpus

---

## Adapting PAF

### For Different AI Tools

The methodology (Layer 1) is tool-agnostic. The skills (Layer 2) target Claude Code but can be ported to any AI coding assistant that supports:
- Reading files from the repository
- Asking the user questions interactively
- Creating and editing files
- Invoking other skills/commands

To port the skills, translate each `SKILL.md` into your tool's skill/plugin format. The methodology docs remain the source of truth — skills are just automation on top.

### For Different Domains

1. Run PAF on a product in your domain
2. During P6, the framework automatically captures domain knowledge
3. Export the captured knowledge as a new domain pack
4. Future products in the same domain benefit from domain-specific intake questions, requirement prompts, document sections, and quality checks

### For Teams Without AI

Read the methodology documents. Use the templates. Run the quality gates manually. PAF works as a structured design process with or without AI tooling — the engine just makes it faster.

---

## Design Principles

- **Tool-aware, tool-agnostic** — Describes the process in terms of roles and capabilities, not specific tools. The Claude Code skills are one concrete binding.
- **Auto-scaling** — Right-sizes from a 5-document utility to a 25-document enterprise platform based on complexity signals detected during intake.
- **Self-improving** — Captures domain knowledge with each engagement, building reusable packs that make subsequent designs faster and more thorough.
- **Full lifecycle** — Requirements through execution, not just documentation. The design feeds directly into an implementable plan.
- **Consistency over completeness** — A smaller set of documents that don't contradict each other is more valuable than an exhaustive set that does.

---

## Contributing

Contributions are welcome, particularly:

- **New domain packs** — If you've used PAF in a domain and captured useful knowledge, consider contributing the pack
- **Template improvements** — Better section guidance, missing sections, tier annotation fixes
- **Quality gate refinements** — New consistency checks, better procedures
- **Tool integrations** — Skills for AI tools other than Claude Code (Cursor, Windsurf, Copilot, etc.)
- **Methodology feedback** — If a phase's procedure didn't work well for your product, open an issue

### Guidelines

- Domain packs should be anonymized (no client names, proprietary details)
- Templates should use `{Product}` / `{domain}` / `{date}` placeholders
- Skills should reference methodology docs as source of truth (not duplicate them)
- Quality checks should be falsifiable (pass/fail, not subjective)

---

## License

Licensed under the [Apache License, Version 2.0](LICENSE). You may use, modify, and distribute PAF freely, provided you include attribution. Products designed using PAF are yours — the license applies to the framework itself, not to what you build with it.

---

## Author

**Dimitar Lazarov** — Founder of [Hexienta](https://hexienta.com)

PAF was born from a simple frustration: designing enterprise software is hard, and most of the difficulty isn't technical — it's organizational. Requirements live in someone's head. Decisions get made in meetings and never written down. API specs contradict database schemas because they were written by different people in different weeks. Security models described in one document don't match auth flows in another. By the time you discover these contradictions, you're mid-sprint and the cost of fixing them has multiplied.

I built PAF while designing [Hexienta HIVE](https://hexienta.com) — an enterprise agentic AI gateway for autonomous ITSM agents. The product required 22 interconnected design documents across 2 architectural planes, 2 canonical OpenAPI specs with 155 endpoints, and a 312-task implementation plan. The process I developed to keep all of that internally consistent — zero contradictions, 64/64 design tokens aligned, every decision traceable to the documents it affected — became PAF.

I'm releasing it as open source because I believe the biggest bottleneck in software isn't writing code. It's knowing exactly what to build before you write the first line. If PAF helps your team ship better software with fewer mid-sprint surprises, it's done its job.

**Hexienta** is an IT consultancy that helps organizations build and assess their technology capabilities. PAF is one of the tools we use internally and with clients. If you need help designing complex software or want a guided PAF engagement, [get in touch](https://hexienta.com).

---

*"Most software fails not because the code is bad, but because the design was incomplete."*
