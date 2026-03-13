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
- Skip targets for documents not produced at this tier (record as "skipped — document not in scope")
- For each remaining target, verify it contains the decision content

**G3: Taxonomy Completeness** (all tiers)
- Look up required document types for the tier
- Exclude P5 artifacts (Implementation Plan) — validated at P5 exit, not during P4
- Verify each remaining required type exists and has substantive content

**G4: Internal Consistency** (T2+)
- Read each document end-to-end
- Check for self-contradictions (prose vs. tables, numbers that differ between sections)

**G5: Cross-Document Consistency** (T2+)
- For each check below, first verify all referenced documents exist. **Skip the check if a referenced document was not produced at this tier** (e.g., skip Auth ↔ Everything if Security Architecture doesn't exist). Record skipped checks as "skipped — document not in scope."
- Run each applicable check from the consistency check table in PAF-Quality-Gates.md:
  - Schema ↔ API
  - Auth ↔ Everything
  - Process ↔ API
  - Tokens ↔ Frontend
  - Config ↔ Docs
  - Terminology ↔ All
  - Glossary ↔ All — product-specific terms from Decision Log glossary used consistently; no synonyms or variant spellings
  - Quality Attrs ↔ Impl — Root Architecture §8 targets match Backend Architecture §9 performance/scaling specs
  - Principles ↔ Decisions — no decision or design pattern contradicts a stated architecture principle (Root Architecture §6)
  - Cross-Cutting ↔ Refs — each concern in Root Architecture §11 is addressed in every document listed in its "Defined In" / "Enforced By" columns
- If a domain pack has `quality_checks`, run those too

**G6: Bidirectional References** (T3 required; T2 ask user: "Would you like to run optional reference checks?")
- For each depends_on relationship A → B, verify B acknowledges A

**G7: Version Gate Alignment** (T3 required; T2 ask user: "Would you like to verify version gate consistency?")
- Verify deferred features are consistently marked across all documents

**G8: Gap Analysis** (T2+)
- Search all documents for TODO, TBD, FIXME, empty sections, unanswered questions

### 3. Record Findings

**T1:** Record G1–G3 results inline (no separate completeness report). Present pass/fail summary to the user directly.

**T2+:** Save to `docs/design/completeness-report.md` using the template from `docs/paf/templates/completeness-report.md`. Fill in:
- Gate results table
- Consistency findings table (mark skipped checks for documents not in scope)
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
- [ ] Results recorded (T1: inline summary; T2+: completeness report finalized and saved)
- [ ] All documents promoted to `review` or `approved`
- [ ] **Declaration: "Design corpus is implementation-ready"**

### 7. Handoff

Announce: "P4 complete — design corpus is implementation-ready." Present a summary of gate results and any findings resolved.
Ask: "Ready to proceed to P5 (Implementation Planning)? If you need time to review, you can resume later with `/paf-plan`."
Invoke `paf-plan` only after confirmation.

## When Things Go Wrong

- **Gate fails repeatedly on the same finding:** After 3 attempts, escalate to the user. The finding may require an architectural decision that should be recorded as a new D-{number} entry and flowed back through P3.
- **Cross-document consistency check (G5) exceeds context:** For large corpora (15+ documents), batch G5 by shared contract type. Run Schema↔API first, then Auth↔Everything, etc. Don't attempt all checks in a single pass.
- **Domain pack quality checks reference documents that don't exist:** Skip the check and note it in the completeness report. The domain pack may need updating.
- **Finding requires scope change:** Pause P4, return to P2 for the affected requirement category, flow through P3 for affected documents only, then resume P4.

## Related Skills

Works well with: `paf-author`, `paf-plan`
