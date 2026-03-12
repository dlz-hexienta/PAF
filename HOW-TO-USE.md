# How to Use PAF

This guide walks you through using PAF with Claude Code to design and build a software product. The automated path is the primary workflow — PAF skills chain through all 6 phases, asking questions, producing artifacts, and handing off automatically.

---

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- A project directory where your product will live (can be empty or existing)
- A product idea — can be a sentence, a paragraph, or a full brief

---

## Installation

PAF skills need two things in your project: the skill files (so Claude Code can run them) and the PAF source docs (so skills can read methodology, templates, and domain packs at runtime).

From your **project root**:

```bash
# 1. Copy the skills into Claude Code's skill directory
mkdir -p .claude/skills
cp -r /path/to/paf/skills/paf-* .claude/skills/

# 2. Copy the PAF source docs where skills expect them
mkdir -p docs/paf
cp -r /path/to/paf/methodology docs/paf/
cp -r /path/to/paf/templates docs/paf/
cp -r /path/to/paf/domain-packs docs/paf/
```

Your project should now have:

```
your-project/
├── .claude/skills/
│   ├── paf-intake/SKILL.md
│   ├── paf-requirements/SKILL.md
│   ├── paf-author/SKILL.md
│   ├── paf-qa/SKILL.md
│   ├── paf-plan/SKILL.md
│   └── paf-execute/SKILL.md
├── docs/
│   ├── paf/                      # PAF source (installed)
│   │   ├── methodology/          # Source of truth for all phases
│   │   ├── templates/            # Document scaffolds
│   │   └── domain-packs/         # Domain knowledge (YAML)
│   └── design/                   # ← Created by skills, your artifacts go here
```

**For proprietary products:** Treat `docs/design/` as confidential — it will contain threat models, auth flows, and database schemas. See `docs/paf/methodology/PAF-Overview.md` §Security for guidance.

---

## Starting a Design Engagement

Open Claude Code in your project directory and run:

```
/paf-intake
```

That's it. The skill chain handles everything from here. Each phase asks questions, produces artifacts, verifies exit gates, and asks for your confirmation before proceeding to the next phase.

All design artifacts are saved to `docs/design/` in your project (created automatically by the skills).

---

## What Happens at Each Phase

### Phase 1: Intake (`/paf-intake`)

**What it does:** Asks you 12 structured questions about your product — 5 about identity (what it does, who it's for, how deployed) and 7 complexity signals (user roles, UI screens, data stores, integrations, tenancy, security, deployment components).

**If a domain pack matches** your product description, it loads additional domain-specific questions.

**What it produces:**
- Intake brief (`docs/design/intake-brief.md`)
- Complexity score and tier assignment (T1, T2, or T3)
- Scope boundaries (v1 vs. future vs. out-of-scope)

**What you review:** The tier assignment. You can override upward (never downward). If T3, it asks whether your product should be decomposed into sub-products.

**Then:** Asks for confirmation, then invokes `paf-requirements`.

---

### Phase 2: Requirements (`paf-requirements`)

**What it does:** Explores requirements category by category, scaled to your tier. Uses a progressive depth pattern: open-ended question → 2–3 concrete approaches with trade-offs → records your decision.

**Categories explored (by tier):**

| Category | T1 | T2 | T3 |
|----------|:---:|:---:|:---:|
| Functional | Yes | Yes | Yes |
| Data Model | Optional | Yes | Yes |
| Integration | Optional | Yes | Yes |
| User Experience | — | Yes (if UI) | Yes |
| Security & Access | — | Optional | Yes |
| Operational | — | Optional | Yes |
| Commercial | — | Optional | Yes |
| Compliance | — | — | If applicable |

**What it produces:**
- Decision log (`docs/design/decision-log.md`) with `D-{number}` entries — each recording context, options, rationale, and which documents need this decision
- Domain pack draft (if new domain-specific patterns were captured)

**What you review:** Each decision as it's recorded. The "Distribute To" field on each decision — this is what makes decisions traceable through the design corpus.

**Then:** Asks for confirmation, then invokes `paf-author`.

---

### Phase 3: Authoring (`paf-author`)

**What it does:** Determines which documents your tier requires, scaffolds them from templates, then authors them in dependency layer order:

```
Layer 0: Root Architecture (always first)
Layer 1: Functional Processes, UI Design
Layer 2: Frontend Architecture, Security, Backend Architecture
Layer 3: API Specification, Canonical Specs
Layer 4: Operational
```

Each document cites decisions from P2 (e.g., "Per D-7, we use JWT with opaque API key fallback") and tracks shared contracts (design tokens, DB schema, API schemas, auth model, config) for cross-document consistency.

**What it produces:** The full design document corpus in `docs/design/` — right-sized to your tier (3–5 docs for T1, 8–12 for T2, 15–25+ for T3). T3 products split the Operational document into separate testing, observability, and release docs.

**What you review:** Each document or layer as it completes. This is the most interactive phase — you'll confirm architecture decisions, UI layouts, API shapes, and security models as they're written.

**Then:** Asks for confirmation, then invokes `paf-qa`.

---

### Phase 4: QA (`paf-qa`)

**What it does:** Runs quality gates against your design corpus, scaled by tier:

| Gate | Check | T1 | T2 | T3 |
|------|-------|:---:|:---:|:---:|
| G1 | Metadata integrity (YAML frontmatter) | Yes | Yes | Yes |
| G2 | Decision coverage (decisions → target docs) | Yes | Yes | Yes |
| G3 | Taxonomy completeness (required docs exist) | Yes | Yes | Yes |
| G4 | Internal consistency (no self-contradictions) | — | Yes | Yes |
| G5 | Cross-document consistency (schema↔API, auth↔everything) | — | Yes | Yes |
| G6 | Bidirectional references | — | — | Yes |
| G7 | Version gate alignment | — | — | Yes |
| G8 | Gap analysis (no TODOs, empty sections) | — | Yes | Yes |

If a domain pack matched, its domain-specific quality checks run as part of G5.

**Resolution loop:** When a gate fails, the skill identifies the issue, fixes it, and re-runs the affected gate until it passes. Architectural changes get recorded as new decisions.

**What it produces:** Completeness report (`docs/design/completeness-report.md`) with gate results, findings, and status promotions. For large corpora (T3, 15+ docs), G5 checks are batched by shared contract type to stay within context limits.

**What you review:** The completeness report. Any findings that required architectural changes. Document status promotions (draft → review → approved).

**Then:** Asks for confirmation, then invokes `paf-plan`.

---

### Phase 5: Planning (`paf-plan`)

**What it does:** Walks every design document and extracts buildable components — endpoints, screens, database tables, services, security controls, CI/CD pipelines. Maps dependencies, groups into sprints, and decomposes into TDD tasks.

**Task format:**
```markdown
### Task {Phase}-{Number}: {Component Name}
Files:
- Create: exact/path/to/file
- Test: tests/path/to/test
Ref: {Design-Document §Section}
- [ ] Write failing test
- [ ] Run test → verify it fails
- [ ] Write minimal implementation
- [ ] Run test → verify it passes
- [ ] Commit
```

**Expected scale:**

| Tier | Tasks | Sprints |
|------|-------|---------|
| T1 | 15–30 | 3–5 |
| T2 | 40–80 | 6–12 |
| T3 | 150–300+ | 15–40 |

**What it produces:** Implementation plan (`docs/design/implementation-plan.md`) with sprint structure, task decomposition, effort estimation (LOC, person-months, timelines, costs).

**What you review:** Sprint grouping, dependency ordering, effort estimates. Every design section should map to at least one task and vice versa.

**Then:** Asks for confirmation, then invokes `paf-execute`.

---

### Phase 6: Execution (`paf-execute`)

**What it does:** Selects an execution mode based on team size, sets up an isolated workspace, and delegates to existing execution skills with PAF-specific guardrails.

**Execution modes:**

| Mode | Team | Delegates to |
|------|------|-------------|
| Solo + AI | 1 developer | `superpowers:subagent-driven-development` |
| Small Team | 2–4 developers | `superpowers:subagent-driven-development` or `superpowers:executing-plans` |
| Full Team | 5+ developers | `superpowers:executing-plans` (parallel sessions) |

**PAF guardrails on top of execution:**
- Per-task: tests pass, code matches design spec, no silent drift
- Per-phase: integration tests, design drift check, plan adjustment review
- Pre-release: security review, performance benchmarks, user docs (scaled by tier)

**Design feedback loop:** When implementation reveals design issues:
- Minor adjustments → update design doc inline
- Significant deviations → new Decision Record entry, update affected docs, re-run relevant QA gate
- Scope changes → pause, return to P2 for that category, flow through P3→P4→P5, resume

**Domain knowledge harvest:** At the end, the skill extracts domain-general patterns from all execution decisions and updates the domain pack — making PAF smarter for the next product in this domain.

**What you review:** Per-phase integration results, any design drift findings, the final domain pack update.

---

## Domain Packs

### Using an Existing Pack

Domain packs are loaded automatically. During P1 intake, the skill checks if your product description matches a domain pack in `docs/paf/domain-packs/`. If it matches, the pack's questions, prompts, template sections, and quality checks are injected throughout P1–P6.

Available packs ship with PAF. Check `docs/paf/domain-packs/` for `.yaml` files (ignore `_template.yaml`).

### Creating a New Pack

**Option A (automatic):** Run a full PAF engagement. During P2, the skill captures domain-specific patterns. During P6, it harvests execution learnings. The result is a draft domain pack you can review and commit.

**Option B (manual):** Copy `docs/paf/domain-packs/_template.yaml`, rename it to your domain, and fill in:

| Field | What to add |
|-------|------------|
| `intake_questions` | Questions specific to this domain that surface complexity |
| `requirement_prompts` | Guided exploration prompts by category |
| `document_section_templates` | Sections to inject into standard templates |
| `quality_checks` | Domain-specific consistency checks for G5 |
| `terminology` | Canonical definitions to enforce across documents |
| `tooling_recommendations` | Domain-relevant tools with synergies |

---

## Running Individual Phases

You don't have to start from P1 every time. If you have existing artifacts, you can invoke any skill directly:

| Command | When to use |
|---------|-------------|
| `/paf-intake` | Starting fresh |
| `/paf-requirements` | You have an intake brief, need to explore requirements |
| `/paf-author` | You have a decision log, need to write design documents |
| `/paf-qa` | You have design documents, need to verify consistency |
| `/paf-plan` | You have approved documents, need an implementation plan |
| `/paf-execute` | You have an approved plan, ready to build |

Each skill checks its prerequisites (prior phase artifacts must exist) before proceeding.

---

## Re-entering Mid-Project

If you paused after a phase and return later:

1. Open Claude Code in your project directory
2. The skills will find your existing artifacts in `docs/design/`
3. Invoke the next phase skill directly (e.g., `/paf-author` if you completed P2 last)
4. The skill loads your intake brief, decision log, and tier assignment from the existing artifacts

If requirements changed since you last worked:

1. Run `/paf-requirements` to explore the changed category
2. New decisions get added to your existing Decision Log
3. Run `/paf-author` for affected documents only
4. Run `/paf-qa` to re-verify consistency
5. Continue from P5

---

## What You Get at the End

After a full P1–P6 engagement:

| Artifact | Description |
|----------|-------------|
| **Intake brief** | Product identity, complexity assessment, scope boundaries |
| **Decision log** | Every architectural decision with rationale and traceability |
| **Design corpus** | Internally consistent design documents (scaled to tier) |
| **Completeness report** | Quality gate results proving zero contradictions |
| **Implementation plan** | Sprint-structured TDD tasks with effort estimates |
| **Working software** | Code matching the design, with tests passing |
| **Updated domain pack** | Domain knowledge captured for reuse |

---

## Tips

- **Answer intake questions honestly.** The tier system only works if complexity signals are accurate. Over-simplifying leads to missing documents; over-inflating leads to unnecessary work.
- **Review each phase's output before the next proceeds.** The exit gate confirmation is your checkpoint. Catching a wrong assumption in P2 is cheap; catching it in P6 is expensive.
- **Decisions drive everything.** The `D-{number}` format with "Distribute To" targets is what makes PAF's consistency guarantees work. If a decision can't identify which documents it affects, it may not be significant enough to record.
- **Domain packs compound.** Your first product in a domain runs the full questionnaire. Your second product in the same domain gets domain-specific questions, pre-loaded requirement prompts, and additional quality checks automatically.
- **Tier determines effort, not quality.** A T1 utility with 3 documents and 3 quality gates has the same consistency guarantee as a T3 platform with 25 documents and 8 gates — just at a different scale.
