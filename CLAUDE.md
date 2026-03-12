# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What is PAF

Product Architecture Framework — a reusable system for designing software products at enterprise depth. It produces internally consistent design document corpora through a 6-phase lifecycle with automated quality gates. The framework is auto-scaling: complexity tier (T1/T2/T3) is determined during intake and controls which documents, quality gates, and effort levels apply.

## Three-Layer Architecture

1. **Methodology** (`methodology/`) — Source of truth. Markdown docs defining phases, document taxonomy, quality gates, and tier system. Skills reference these; never the reverse.
2. **Engine** (`skills/`) — Six Claude Code skills that automate each phase. Each skill reads methodology docs, scaffolds from templates, loads domain packs, and enforces exit gates.
3. **Domain Packs** (`domain-packs/`) — YAML files encoding domain-specific knowledge (intake questions, requirement prompts, template sections, quality checks, terminology). Captured during P2/P6 and reused across future projects in the same domain.

## Phase Chain

```
P1 Intake → P2 Requirements → P3 Authoring → P4 QA → P5 Planning → P6 Execution
skill:       paf-intake  paf-requirements  paf-author  paf-qa  paf-plan  paf-execute
```

Each skill produces artifacts, verifies exit gates, and hands off to the next. The phase guide (`methodology/PAF-Phase-Guide.md`) is the authoritative reference for each phase's procedures.

## Complexity Tiers

- **T1 (Utility):** Single concern, <10 endpoints, no auth. 3–5 docs, gates G1–G3.
- **T2 (Application):** Multiple concerns, auth, persistent state, UI. 8–12 docs, gates G1–G5 + G8.
- **T3 (Platform):** Multi-plane/multi-tenant, >50 endpoints, full security. 15–25+ docs, all 8 gates.

Tier is assigned during P1 intake based on complexity signals (user roles, UI screens, data stores, integrations, tenancy, security depth, deployment components).

## Document Authoring Order

P3 documents must be authored in dependency layer order:

- **Layer 0:** Root Architecture
- **Layer 1:** Functional Processes, UI Design
- **Layer 2:** Frontend Architecture, Security Architecture, Backend Architecture
- **Layer 3:** API Specification, Canonical Specs
- **Layer 4:** Operational

A document at Layer N must reach draft before any Layer N+1 document begins. Templates in `templates/` provide the scaffolding for each document type.

## Key Relationships

- **Skills → Methodology:** Skills reference methodology as source of truth for procedures and rules
- **Skills → Templates:** P3 skill (`paf-author`) scaffolds documents from `templates/`
- **Skills → Domain Packs:** Loaded at P1, injected throughout P1–P6 (extra questions, prompts, sections, checks)
- **Quality Gates → Documents:** P4 skill (`paf-qa`) runs gates G1–G8 against the document corpus; gate definitions in `methodology/PAF-Quality-Gates.md`

## Shared Contracts (P3/P4 Concept)

These must stay consistent across documents — verified by gate G5:
- Design tokens (UI Design → Frontend)
- Database schema (Backend → API)
- API schemas (API Spec → Frontend/Backend)
- Auth model (Security → API/Backend/Frontend)
- Config schema (Backend → Functional/Operational)

## Domain Pack Lifecycle

1. P1: Match product description to domain pack keywords; load pack's intake questions
2. P2: Inject requirement prompts; capture new domain patterns into draft pack
3. P3: Inject document section templates
4. P4: Run domain-specific quality checks
5. P6: Harvest new knowledge, update pack, increment version

Pack schema defined in `methodology/PAF-Domain-Pack-Specification.md`. Template at `domain-packs/_template.yaml`.

## Decision Format

Decisions use `D-{number}` IDs with: Context, Options Considered, Decision (with reasoning), Distribute To (target documents), Version Gate (v1.0/future/out-of-scope).

## Reference Example

`examples/itsm-platform/` — T3 platform that produced 22 documents, 155 API endpoints, 312-task plan with zero cross-document contradictions.
