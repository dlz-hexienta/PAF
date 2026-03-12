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

## When Things Go Wrong

- **Test fails and fix requires design change:** Classify as Level 1 (minor), Level 2 (significant), or Level 3 (scope change) per §4 above. Don't skip the design feedback loop — silent drift compounds.
- **Implementation reveals missing design section:** Pause the task. Author the missing section following P3 procedures, run relevant P4 gate, update the plan, then resume.
- **Domain pack harvest produces no new knowledge:** This is fine — not every engagement teaches something new. Only add entries that would genuinely help the next product in this domain.
- **Pre-release check fails:** Do not ship. Identify the failing check, trace it to the responsible task(s), fix, and re-run. Use `superpowers:systematic-debugging` if the issue is unclear.
- **Context window limits during large corpus checks:** Batch design drift checks by domain or document layer. Check Layer 0–1 docs first, then Layer 2–3, then Layer 4.

## Related Skills

Works well with: `paf-plan`, `superpowers:subagent-driven-development`, `superpowers:executing-plans`, `superpowers:using-git-worktrees`, `superpowers:finishing-a-development-branch`, `superpowers:verification-before-completion`
