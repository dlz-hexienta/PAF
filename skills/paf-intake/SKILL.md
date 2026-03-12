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

Create the intake brief as a Markdown document. Save to `docs/design/intake-brief.md`.

Structure: Core Identity answers, Complexity Signals table with tier scoring, Scope Boundaries (v1/future/out-of-scope), and Domain Pack match (if any).

### 7. Exit Gate Checklist

Before handing off, verify:
- [ ] Intake brief complete (all questions answered)
- [ ] Complexity tier assigned and confirmed by user
- [ ] Scope boundaries documented (v1 vs. deferred)
- [ ] If T3: decomposition check passed

### 8. Handoff

Announce: "P1 complete." Present a summary of tier assignment and scope boundaries.
Ask: "Ready to proceed to P2 (Requirements)? If you need time to review, you can resume later with `/paf-requirements`."
Invoke `paf-requirements` only after confirmation.

## When Things Go Wrong

- **User can't answer a question:** Record as "TBD — needs stakeholder input" and continue. Flag at exit gate.
- **Complexity signals conflict:** Use the highest tier triggered. If ambiguous, present the conflict and ask the user to decide.
- **No domain pack matches:** Proceed without domain-specific questions. A pack may be captured during P2.
- **User disagrees with tier assignment:** Allow upward override. If they want downward, explain the risk (missing documents → gaps caught late in P4 or execution).

## Related Skills

Works well with: `paf-requirements`, `superpowers:brainstorming`
