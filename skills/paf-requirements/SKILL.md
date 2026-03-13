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

Save decisions to `docs/design/decision-log.md` using the template at `docs/paf/templates/decision-log.md`.

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

**SMART targets:** When a decision involves a measurable target (performance, capacity, availability, timeline), express it as Specific, Measurable, Achievable, Relevant, Time-bound. Example: instead of "the API should be fast", record "API p95 latency < 200ms for v1.0". These feed Root Architecture §8 (Quality Attributes) and Backend Architecture §9 (Performance & Scaling).

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

### 5. Principles & Glossary Capture

As decisions accumulate, two additional artifacts emerge:

**Architecture Principles (T2+):** Look for recurring themes across decisions — rules that guided multiple choices. Capture 3–5 principles as a table:

| Principle | Rationale |
|-----------|-----------|
| {e.g., Single binary deployment} | {e.g., Reduces operational complexity} |

These feed Root Architecture §6. Don't force principles — if only 1–2 emerge naturally, that's fine.

**Product Glossary (T2+):** When a product-specific term is defined or clarified during discussion, add it to the glossary section of the Decision Log:

| Term | Definition | Used In |
|------|-----------|---------|
| {term} | {definition in this product's context} | {target documents} |

This is distinct from domain pack terminology (which is domain-general). G5 checks glossary terms for consistent usage.

### 6. Exit Gate Checklist

- [ ] All required requirement categories explored (per tier)
- [ ] Every decision has rationale and "Distribute To" targets
- [ ] Measurable targets expressed as SMART criteria (if any)
- [ ] Architecture principles captured (T2+, if they emerged)
- [ ] Product glossary populated (T2+, if terms were defined)
- [ ] No critical open questions remaining
- [ ] Version gates assigned (v1 vs. future)
- [ ] Domain pack draft reviewed (if new domain knowledge)

### 7. Handoff

Announce: "P2 complete." Present a summary of decision count and categories explored.
Ask: "Ready to proceed to P3 (Design Authoring)? If you need time to review, you can resume later with `/paf-author`."
Invoke `paf-author` only after confirmation.

## When Things Go Wrong

- **User can't decide between options:** Record both options with trade-offs as a "pending" decision. Continue with other categories. Return before exit gate.
- **Domain pack prompts don't fit the product:** Skip irrelevant prompts. Note the mismatch — it may indicate the wrong pack matched.
- **Too many open questions remain:** Group them by criticality. Critical = blocks architecture. Non-critical = can be resolved during P3 authoring. Only critical questions block the exit gate.
- **New complexity signals emerge:** If answers reveal higher complexity than P1 assessed, re-score the tier. Update intake brief.

## Related Skills

Works well with: `paf-intake`, `paf-author`
