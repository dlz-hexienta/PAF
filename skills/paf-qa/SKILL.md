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
