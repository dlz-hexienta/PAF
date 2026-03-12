# PAF Plan 3: Skills Engine — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create 6 Claude Code skills that automate the PAF methodology — one skill per phase (P1–P6), forming a chain where each skill reads the methodology docs as its source of truth.

**Architecture:** Each skill is a `SKILL.md` file following the established skill format (YAML frontmatter + Markdown content). Skills reference the methodology documents in `docs/paf/methodology/` for procedures and use templates from `docs/paf/templates/` for scaffolding. The skill chain is: `paf-intake` → `paf-requirements` → `paf-author` → `paf-qa` → `paf-plan` → `paf-execute`.

**Tech Stack:** Markdown (SKILL.md format). Skills are Claude Code instructions, not executable code.

**Spec:** `docs/paf/2026-03-12-paf-design-spec.md` §6–§13
**Depends on:** Plan 1 (methodology docs) and Plan 2 (templates + domain packs)

---

## File Structure

```
docs/paf/
└── skills/
    ├── paf-intake/
    │   └── SKILL.md              # P1: Intake & Scoping
    ├── paf-requirements/
    │   └── SKILL.md              # P2: Requirements & Decisions
    ├── paf-author/
    │   └── SKILL.md              # P3: Design Authoring
    ├── paf-qa/
    │   └── SKILL.md              # P4: Quality Assurance
    ├── paf-plan/
    │   └── SKILL.md              # P5: Implementation Planning
    └── paf-execute/
        └── SKILL.md              # P6: Execution & Delivery
```

---

## Chunk 1: Discovery & Design Skills (P1–P3)

### Task 1: paf-intake Skill (P1)

**Files:**
- Create: `docs/paf/skills/paf-intake/SKILL.md`

**Context:** This skill runs P1 — Intake & Scoping. It asks the intake questionnaire, scores complexity, assigns a tier, checks for decomposition needs, and produces an intake brief. It hands off to `paf-requirements`.

- [ ] **Step 1: Create the skill file**

```markdown
---
name: paf-intake
description: >
  PAF Phase 1: Intake & Scoping. Use when starting a new product design.
  Captures product identity, assesses complexity, assigns a tier (T1/T2/T3),
  and produces an intake brief. Hands off to paf-requirements.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, intake, scoping, complexity, design"
---

# PAF Intake & Scoping (P1)

**Methodology reference:** Read `docs/paf/methodology/PAF-Phase-Guide.md` §P1 before proceeding. That document is the source of truth — this skill automates it.

## When to Use

- Starting a new product design from scratch
- Assessing whether an idea warrants a full PAF engagement
- Re-scoping an existing product after significant requirements change

## Procedure

### 1. Domain Pack Detection

Before asking questions, check if any domain packs exist at `docs/paf/domain-packs/`:
- Read available `.yaml` files (skip `_template.yaml`)
- Hold them ready — if the product description matches a domain, load its `intake_questions`

### 2. Intake Questionnaire

Ask questions **one at a time**, in this order. Adapt follow-ups based on answers.

**Core Identity (always ask all 5):**
1. What does this product do? (one paragraph)
2. Who is it for? (target users and buyers)
3. What problem does it solve that isn't solved today?
4. How will it be deployed? (SaaS / on-prem / embedded / CLI / library)
5. What's the commercial model? (open source / freemium / licensed / internal)

**Complexity Signals (ask all 7):**
6. How many distinct user roles interact with the system?
7. Does it have a UI? How many primary screens?
8. Does it need persistent state? What kind? (relational / document / file / in-memory)
9. How many external systems does it integrate with?
10. Does it have multi-tenancy, billing, or marketplace concerns?
11. Are there security/compliance requirements? (auth, encryption, audit, regulatory)
12. Does it have multiple deployment components? (separate binaries, services, planes)

**Domain Probing (if a domain pack matched):**
- Load the matched pack's `intake_questions`
- Ask each one after the standard questions
- Note which domain pack was matched in the intake brief

### 3. Complexity Scoring

Score each signal against the tier thresholds:

| Signal | T1 | T2 | T3 |
|--------|:---:|:---:|:---:|
| User roles | 1 | 2–3 | 4+ |
| UI screens | 0 | 3–10 | 11+ |
| Data stores | 0–1 | 1–2 | 3+ |
| External integrations | 0–1 | 2–4 | 5+ |
| Multi-tenancy / billing | No | No | Yes |
| Security/compliance | Basic | Auth + RBAC | Threat model + audit + encryption |
| Deployment components | 1 | 1–2 | 3+ |

**Rule:** Highest tier triggered by any signal wins. Present the score to the user and ask if they want to override upward (never downward).

### 4. Scope Boundaries

Ask: "What must be in v1, and what can be deferred?"
Document the answer as version gates (v1.0, future, out of scope).

### 5. Decomposition Check (T3 only)

If T3, ask: "Could any of these components be deployed and used independently, serve different user bases, or ship on different release schedules?"

If yes, suggest decomposition into sub-products. Each gets its own P1–P6 cycle.

### 6. Produce Intake Brief

Create the intake brief as a Markdown document. Save to the product's design directory.

Use the example at `docs/paf/examples/itsm-platform/intake-brief.md` as a formatting reference.

### 7. Exit Gate Checklist

Before handing off, verify:
- [ ] Intake brief complete (all questions answered)
- [ ] Complexity tier assigned and confirmed by user
- [ ] Scope boundaries documented (v1 vs. deferred)
- [ ] If T3: decomposition check passed

### 8. Handoff

Announce: "P1 complete. Invoking paf-requirements for Phase 2."
Invoke the `paf-requirements` skill.

## Related Skills

Works well with: `paf-requirements`, `superpowers:brainstorming`
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/skills/paf-intake/SKILL.md
git commit -m "docs(paf): add paf-intake skill (P1: Intake & Scoping)"
```

---

### Task 2: paf-requirements Skill (P2)

**Files:**
- Create: `docs/paf/skills/paf-requirements/SKILL.md`

- [ ] **Step 1: Create the skill file**

```markdown
---
name: paf-requirements
description: >
  PAF Phase 2: Requirements & Decisions. Use after paf-intake completes.
  Explores requirements by category (scaled to tier), records decisions with
  rationale and distribution targets, captures domain knowledge for reuse.
  Hands off to paf-author.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, requirements, decisions, domain-knowledge, design"
---

# PAF Requirements & Decisions (P2)

**Methodology reference:** Read `docs/paf/methodology/PAF-Phase-Guide.md` §P2 before proceeding.

## Prerequisites

- P1 exit gate passed (intake brief exists, tier assigned)
- Know the assigned tier (T1, T2, or T3) — it determines which categories to explore

## Procedure

### 1. Load Context

- Read the intake brief produced by P1
- Read the assigned complexity tier
- If a domain pack matched in P1, load it from `docs/paf/domain-packs/{pack}.yaml`
- Read the domain pack's `requirement_prompts` and `terminology` sections

### 2. Requirements Exploration

Work through requirement categories **in order**, asking questions one at a time. Use the progressive depth pattern: open-ended question → 2-3 concrete approaches → record decision.

**Categories to explore (scaled by tier):**

| Category | T1 | T2 | T3 |
|----------|:---:|:---:|:---:|
| Functional | Required | Required | Required |
| Data Model | Optional | Required | Required |
| Integration | Optional | Required | Required |
| User Experience | Skip | Required (if UI) | Required |
| Security & Access | Skip | Optional | Required |
| Operational | Skip | Optional | Required |
| Commercial | Skip | Optional | Required |
| Compliance | Skip | Skip | If applicable |

**For each category:**
1. Start with an open-ended question about the category
2. Based on the answer, present 2-3 approaches with trade-offs and your recommendation
3. If a domain pack has prompts for this category, weave them into the conversation
4. Record each significant decision in D-{number} format

### 3. Decision Recording

For every significant decision, record it using this format:

```markdown
### D-{number}: {Question}

**Date:** {today}
**Context:** {why this matters}
**Options Considered:**
1. {Option A} — {trade-offs}
2. {Option B} — {trade-offs}
**Decision:** {choice with reasoning}
**Distribute To:** {document types that need this}
**Version Gate:** {v1.0 | future | N/A}
```

The **"Distribute To"** field is mandatory — it's what makes decisions traceable. If you can't identify a target document, the decision may not be significant enough to record.

Save decisions to a Decision Log file in the product's design directory. Use the template at `docs/paf/templates/decision-log.md`.

### 4. Domain Knowledge Capture

Throughout the conversation, watch for patterns that are domain-specific (not product-specific):
- Questions that proved unexpectedly important
- Constraints specific to the vertical
- Terminology that needed definition
- Architectural patterns that emerged

At the end of P2, if new domain knowledge was captured:
1. If updating an existing pack: add new entries to the relevant sections
2. If creating a new pack: copy `docs/paf/domain-packs/_template.yaml` and fill in captured knowledge
3. Present the draft pack to the user for review

### 5. Exit Gate Checklist

- [ ] All required requirement categories explored (per tier)
- [ ] Every decision has rationale and "Distribute To" targets
- [ ] No critical open questions remaining
- [ ] Version gates assigned (v1 vs. future)
- [ ] Domain pack draft reviewed (if new domain knowledge)

### 6. Handoff

Announce: "P2 complete. Invoking paf-author for Phase 3."
Invoke the `paf-author` skill.

## Related Skills

Works well with: `paf-intake`, `paf-author`
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/skills/paf-requirements/SKILL.md
git commit -m "docs(paf): add paf-requirements skill (P2: Requirements & Decisions)"
```

---

### Task 3: paf-author Skill (P3)

**Files:**
- Create: `docs/paf/skills/paf-author/SKILL.md`

- [ ] **Step 1: Create the skill file**

```markdown
---
name: paf-author
description: >
  PAF Phase 3: Design Authoring. Use after paf-requirements completes.
  Scaffolds and authors the design document corpus, writing documents in
  dependency layer order. Cross-references decisions from P2 and tracks
  shared contracts for P4 validation. Hands off to paf-qa.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, authoring, design-documents, scaffolding, design"
---

# PAF Design Authoring (P3)

**Methodology reference:** Read `docs/paf/methodology/PAF-Phase-Guide.md` §P3 and `docs/paf/methodology/PAF-Document-Taxonomy.md` before proceeding.

## Prerequisites

- P2 exit gate passed (decision log exists, requirements captured)
- Know the assigned tier and matched domain pack (if any)

## Procedure

### 1. Determine Document Set

Read the taxonomy table in `docs/paf/methodology/PAF-Document-Taxonomy.md`. Filter by the assigned tier to get the list of required documents.

Present the document list to the user and confirm. They may promote optional documents to required.

### 2. Scaffold Documents

For each required document, in dependency layer order:

1. Read the corresponding template from `docs/paf/templates/{category}.md`
2. Replace placeholder variables:
   - `{Product}` → product name
   - `{domain}` → domain identifier
   - `{date}` → today's date
   - `{root-architecture-filename}` → actual filename
   - Other `{filename}` references → actual filenames
3. If a domain pack matched, read its `document_section_templates` for this category and inject additional sections into the scaffold
4. Apply tier adjustments — remove sections marked for higher tiers (noted in template comments)
5. Save the scaffold to the product's design directory

### 3. Author Documents in Layer Order

Write documents following the dependency layers:

```
Layer 0: Root Architecture (always first)
Layer 1: Functional Processes, UI Design
Layer 2: Frontend Architecture, Security, Backend Architecture
Layer 3: API Specification, Canonical Specs
Layer 4: Operational (Testing, Observability, Release)
```

**A document at Layer N must reach draft before starting any Layer N+1 document.**

For each document:

**a. Fill sections** — Reference decisions from the P2 Decision Log. Cite them explicitly (e.g., "Per D-7, we use JWT with opaque API key fallback"). Each section should be traceable to a decision or requirement.

**b. Cross-reference** — Set `depends_on` in the YAML frontmatter. If this document references a concept from another document, that document is a dependency.

**c. Verify bidirectional references** — If document A depends on B, check whether B should also reference A. One-way is fine for foundational documents (Root Architecture doesn't need to list every dependent).

**d. Track shared contracts** — Note when you define something that other documents will reference:
  - Design tokens (colors, spacing) → will be referenced by Frontend Architecture
  - Database schema → will be referenced by API Specification
  - Auth model → will be referenced by API, Backend, Frontend
  - Config schema → will be referenced by Functional Processes, Operational

**e. Self-check** — Read the document end-to-end. Fix contradictions, fill placeholders, resolve undefined terms.

**f. Set status** — Mark as `draft` in the frontmatter.

### 4. Present Each Document for Review

After completing each document (or each layer), present it to the user. Ask:
- Does this look right?
- Any sections that need more detail?
- Any missing topics?

Incorporate feedback before proceeding to the next layer.

### 5. Update Decision Log

New decisions will arise during authoring. Record them in the Decision Log using the same D-{number} format with "Distribute To" targets.

### 6. Multi-Domain Authoring (T3)

If the product has multiple domains:
1. Author the primary domain's documents first (Layers 0–4)
2. Author secondary domain documents, referencing primary domain mirrors
3. Author shared/cross-cutting documents last
4. Maintain a cross-domain dependency table

### 7. Exit Gate Checklist

- [ ] All required documents for the assigned tier exist at ≥ `draft`
- [ ] All `depends_on` fields populated
- [ ] All P2 decisions distributed to their target documents
- [ ] Shared contracts identified and noted (for P4)
- [ ] Decision Log updated with any P3 decisions
- [ ] User has reviewed and approved each document/layer

### 8. Handoff

Announce: "P3 complete. Invoking paf-qa for Phase 4."
Invoke the `paf-qa` skill.

## Related Skills

Works well with: `paf-requirements`, `paf-qa`
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/skills/paf-author/SKILL.md
git commit -m "docs(paf): add paf-author skill (P3: Design Authoring)"
```

---

## Chunk 2: Verification & Execution Skills (P4–P6)

### Task 4: paf-qa Skill (P4)

**Files:**
- Create: `docs/paf/skills/paf-qa/SKILL.md`

- [ ] **Step 1: Create the skill file**

```markdown
---
name: paf-qa
description: >
  PAF Phase 4: Quality Assurance. Use after paf-author completes.
  Runs quality gates (G1-G8) scaled to tier, verifies cross-document
  consistency, produces a completeness report, and resolves findings.
  Hands off to paf-plan.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, quality, consistency, completeness, verification, design"
---

# PAF Quality Assurance (P4)

**Methodology reference:** Read `docs/paf/methodology/PAF-Quality-Gates.md` before proceeding. That document defines every gate, check procedure, and the completeness report template.

## Prerequisites

- P3 exit gate passed (all required documents at ≥ draft)
- Know the assigned tier (determines which gates are required)
- If a domain pack matched, have it ready (it may add quality checks)

## Procedure

### 1. Load Gate Requirements

Read `docs/paf/methodology/PAF-Quality-Gates.md` §Gate Summary to determine which gates are required for the assigned tier.

### 2. Run Gates in Order

Execute each required gate following the procedures in PAF-Quality-Gates.md:

**G1: Metadata Integrity** (all tiers)
- List all design documents
- Verify each has valid YAML frontmatter with all required fields
- Verify `parent` and `depends_on` references resolve to existing documents

**G2: Decision Coverage** (all tiers)
- Read all D-{number} entries from the Decision Log
- For each, verify its "Distribute To" targets contain the decision content

**G3: Taxonomy Completeness** (all tiers)
- Look up required document types for the tier
- Verify each exists and has substantive content

**G4: Internal Consistency** (T2+)
- Read each document end-to-end
- Check for self-contradictions (prose vs. tables, numbers that differ between sections)

**G5: Cross-Document Consistency** (T2+)
- Run each check from the consistency check table in PAF-Quality-Gates.md:
  - Schema ↔ API
  - Auth ↔ Everything
  - Process ↔ API
  - Tokens ↔ Frontend
  - Config ↔ Docs
  - Terminology ↔ All
- If a domain pack has `quality_checks`, run those too

**G6: Bidirectional References** (T3)
- For each depends_on relationship A → B, verify B acknowledges A

**G7: Version Gate Alignment** (T3)
- Verify deferred features are consistently marked across all documents

**G8: Gap Analysis** (T2+)
- Search all documents for TODO, TBD, FIXME, empty sections, unanswered questions

### 3. Record Findings

Use the completeness report template from `docs/paf/templates/completeness-report.md`. Fill in:
- Gate results table
- Consistency findings table
- Gaps identified table
- Status promotion recommendations

### 4. Resolution Loop

For each finding:
1. Identify the specific document and section
2. Fix the issue
3. Re-run only the affected gate
4. Repeat until the gate passes
5. Update the completeness report

If a finding requires an architectural change, record it as a new Decision Log entry and flow back through P3 (for affected documents only) before returning to P4.

### 5. Status Promotion

Once all gates pass, promote document statuses:
- Documents verified as consistent → `review` or `approved`
- Present promotion recommendations to user for confirmation

### 6. Exit Gate Checklist

- [ ] All required gates pass for the assigned tier
- [ ] Zero critical gaps remaining
- [ ] Zero cross-document contradictions
- [ ] Completeness report finalized and saved
- [ ] All documents promoted to `review` or `approved`

### 7. Handoff

Announce: "P4 complete — design corpus is implementation-ready. Invoking paf-plan for Phase 5."
Invoke the `paf-plan` skill.

## Related Skills

Works well with: `paf-author`, `paf-plan`
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/skills/paf-qa/SKILL.md
git commit -m "docs(paf): add paf-qa skill (P4: Quality Assurance)"
```

---

### Task 5: paf-plan Skill (P5)

**Files:**
- Create: `docs/paf/skills/paf-plan/SKILL.md`

- [ ] **Step 1: Create the skill file**

```markdown
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

Use `docs/paf/examples/itsm-platform/` as a reference example.

### 8. Save the Plan

Save the implementation plan using the template at `docs/paf/templates/implementation-plan.md`.

Use the `superpowers:writing-plans` skill format for task structure — that skill is the execution standard.

### 9. Exit Gate Checklist

- [ ] Every design section → ≥1 task
- [ ] Every task → ≥1 design section
- [ ] Dependency graph has no cycles
- [ ] Sprint grouping reviewed by user
- [ ] Estimation reviewed (sanity check)
- [ ] Plan saved

### 10. Handoff

Announce: "P5 complete. Plan saved to {path}. Ready to execute?"

Invoke the `paf-execute` skill when the user confirms.

## Related Skills

Works well with: `paf-qa`, `paf-execute`, `superpowers:writing-plans`
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/skills/paf-plan/SKILL.md
git commit -m "docs(paf): add paf-plan skill (P5: Implementation Planning)"
```

---

### Task 6: paf-execute Skill (P6)

**Files:**
- Create: `docs/paf/skills/paf-execute/SKILL.md`

- [ ] **Step 1: Create the skill file**

```markdown
---
name: paf-execute
description: >
  PAF Phase 6: Execution & Delivery. Use after paf-plan completes.
  Orchestrates implementation: selects execution mode, enforces quality
  checkpoints (per-task, per-phase, pre-release), manages design feedback
  loop, and harvests domain knowledge. This skill wraps existing execution
  skills (subagent-driven-development, executing-plans) with PAF-specific
  guardrails.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, execution, delivery, review, domain-knowledge, design"
---

# PAF Execution & Delivery (P6)

**Methodology reference:** Read `docs/paf/methodology/PAF-Phase-Guide.md` §P6 before proceeding.

## Prerequisites

- P5 exit gate passed (implementation plan exists and is approved)
- All design documents at `approved` status
- Execution mode selected

## Procedure

### 1. Select Execution Mode

Based on team size and tier:

| Mode | When | Execution Skill |
|------|------|----------------|
| **Solo + AI** | 1 developer, T1–T2 | `superpowers:subagent-driven-development` |
| **Small Team** | 2–4 developers, T2–T3 | `superpowers:subagent-driven-development` or `superpowers:executing-plans` |
| **Full Team** | 5+ developers, T3 | `superpowers:executing-plans` (parallel sessions) |

### 2. Workspace Setup

Before starting execution:
1. Create an isolated workspace using `superpowers:using-git-worktrees`
2. Verify all design documents are accessible from the worktree
3. Verify the implementation plan is accessible

### 3. Execute with PAF Guardrails

Delegate execution to the appropriate skill (`subagent-driven-development` or `executing-plans`). PAF adds these guardrails on top:

**Per-Task Guardrails:**
- [ ] Tests pass before marking complete
- [ ] Code matches the design spec section referenced in the task's **Ref** field (spec compliance)
- [ ] No design drift (implementation doesn't silently diverge from spec)

**Per-Phase Guardrails (run at end of each sprint/phase):**
- [ ] All phase tasks complete
- [ ] Integration tests pass across components built so far
- [ ] **Design drift check:** Compare implementation against design docs. If divergence found:
  - Minor: update design doc inline, note in commit message
  - Significant: create new Decision Log entry (D-{number}), update affected docs
  - Scope change: pause execution, return to P2 for that category, flow through P3→P4→P5
- [ ] **Plan review:** Do remaining tasks need adjustment based on what was learned?

### 4. Design Feedback Loop

During execution, the team will discover things the design got wrong. Handle explicitly:

**Level 1 — Minor Adjustment:**
Implementation detail differs from spec (e.g., field name changed, slightly different API shape).
→ Update the design document inline. Note in commit message.

**Level 2 — Significant Deviation:**
Architectural change, new component, or dropped feature.
→ Create a new Decision Record entry (same D-{number} format).
→ Update all affected design documents.
→ Re-run the relevant P4 quality gate (just the affected checks, not full P4).

**Level 3 — Scope Change:**
New requirements discovered that weren't in P2.
→ Pause current phase.
→ Return to P2 (`paf-requirements`) for that specific requirement category.
→ Flow through P3 → P4 → P5 for affected documents only.
→ Resume execution with updated plan.

### 5. Pre-Release Quality Checks

Before declaring the product deliverable:

| Check | T1 | T2 | T3 |
|-------|:---:|:---:|:---:|
| All tasks complete | Required | Required | Required |
| Full test suite passes | Required | Required | Required |
| Security review | Skip | Basic | Full audit |
| Performance benchmarks | Skip | Optional | Required |
| User-facing docs | Optional | Required | Required |
| Release checklist | Optional | Required | Required |

Use `superpowers:verification-before-completion` before claiming any check passes.

### 6. Domain Knowledge Harvest

At the end of P6, before declaring completion:

1. Read all Decision Record entries created during execution (Level 1–3 feedback)
2. Identify patterns that are domain-general (not product-specific)
3. Update the domain pack:
   - New `intake_questions` or `requirement_prompts` discovered
   - New `quality_checks` that would have caught issues earlier
   - New `terminology` that emerged
   - New `tooling_recommendations` validated
   - New `document_section_templates` that were missing
4. Increment the domain pack version
5. Present the updated pack to the user for review

### 7. Completion

Use `superpowers:finishing-a-development-branch` to complete the work:
- Merge, PR, or cleanup as appropriate
- Ensure all design documents are at `approved` status
- Ensure domain pack is updated and committed

### 8. Exit Gate Checklist

- [ ] All plan tasks marked done
- [ ] Full test suite passes
- [ ] Per-phase and pre-release reviews passed
- [ ] Design documents updated to reflect implementation deviations
- [ ] All design documents at `approved` status
- [ ] Release artifacts produced (if applicable)
- [ ] Domain pack updated with execution learnings
- [ ] **Declaration: "Product is deliverable"**

## Related Skills

Works well with: `paf-plan`, `superpowers:subagent-driven-development`, `superpowers:executing-plans`, `superpowers:using-git-worktrees`, `superpowers:finishing-a-development-branch`, `superpowers:verification-before-completion`
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/skills/paf-execute/SKILL.md
git commit -m "docs(paf): add paf-execute skill (P6: Execution & Delivery)"
```

---

### Task 7: Verify Complete Skills Engine

- [ ] **Step 1: Verify all 6 skill files exist**

```bash
ls -R docs/paf/skills/
```

Expected: 6 directories, each with a `SKILL.md` file.

- [ ] **Step 2: Verify skill chain references**

Each skill should reference the next:
- paf-intake → invokes paf-requirements ✓
- paf-requirements → invokes paf-author ✓
- paf-author → invokes paf-qa ✓
- paf-qa → invokes paf-plan ✓
- paf-plan → invokes paf-execute ✓
- paf-execute → invokes finishing-a-development-branch ✓

- [ ] **Step 3: Verify methodology references**

Each skill should reference its methodology source:
- paf-intake → PAF-Phase-Guide.md §P1 ✓
- paf-requirements → PAF-Phase-Guide.md §P2 ✓
- paf-author → PAF-Phase-Guide.md §P3 + PAF-Document-Taxonomy.md ✓
- paf-qa → PAF-Quality-Gates.md ✓
- paf-plan → PAF-Phase-Guide.md §P5 + PAF-Tooling-Guide.md ✓
- paf-execute → PAF-Phase-Guide.md §P6 ✓

- [ ] **Step 4: Final commit and log check**

```bash
git log --oneline -10
```
