# PAF Quality Gates

This document defines every quality gate PAF uses to verify a design corpus. Gates are run during P4 (Quality Assurance) and scaled by complexity tier.

For phase context, see [PAF-Phase-Guide.md](PAF-Phase-Guide.md) §P4.

---

## Gate Summary

| Gate | What It Verifies | T1 | T2 | T3 |
|------|-----------------|:---:|:---:|:---:|
| **G1** | Metadata Integrity | Required | Required | Required |
| **G2** | Decision Coverage | Required | Required | Required |
| **G3** | Taxonomy Completeness | Required | Required | Required |
| **G4** | Internal Consistency | Skip | Required | Required |
| **G5** | Cross-Document Consistency | Skip | Required | Required |
| **G6** | Bidirectional References | Skip | Optional | Required |
| **G7** | Version Gate Alignment | Skip | Optional | Required |
| **G8** | Gap Analysis | Skip | Required | Required |

**Passing:** A gate passes when all its checks succeed with evidence recorded. A gate fails when any check fails. Failed gates must be resolved before P4 can exit.

---

## Gate Definitions

### G1: Metadata Integrity

**Purpose:** Ensure every document has valid, complete YAML frontmatter.

**Procedure:**
1. List all documents in the design corpus
2. For each document, verify:
   - [ ] YAML frontmatter block exists and parses without error
   - [ ] `title` field present and non-empty
   - [ ] `domain` field present and matches a known domain
   - [ ] `category` field present and matches the taxonomy enum
   - [ ] `status` field present and is one of: `draft`, `review`, `approved`
   - [ ] `version` field present and is a quoted string
   - [ ] `date` field present and is valid ISO 8601
   - [ ] `parent` field present (filename or `~`)
   - [ ] `depends_on` field present (list, may be empty)
   - [ ] `tags` field present (list, may be empty)
3. Verify `parent` references point to documents that exist
4. Verify `depends_on` references point to documents that exist

**Evidence:** Table of documents with pass/fail per field.

### G2: Decision Coverage

**Purpose:** Ensure every decision from P2 has been distributed to at least one design document.

**Procedure:**
1. Extract all D-{number} entries from the Decision Log
2. For each decision, read its "Distribute To" list
3. **Skip targets for documents not produced at this tier** (e.g., if a decision targets Backend Architecture but it was not included). Record as "skipped — document not in scope."
4. Verify that each remaining target document contains a reference to the decision (explicit citation or the decision's content is reflected)

**Evidence:** Table of decisions with distribution status (distributed / missing / skipped).

### G3: Taxonomy Completeness

**Purpose:** Ensure all required document types for the assigned tier exist and are non-empty.

**Procedure:**
1. Look up the tier's required document types in [PAF-Document-Taxonomy.md](PAF-Document-Taxonomy.md)
2. **Exclude P5 artifacts** (Implementation Plan) — these are validated at P5 exit, not during P4
3. Verify each remaining required type has a corresponding document in the corpus
4. Verify each document has substantive content (not just frontmatter and headings)

**Evidence:** Table of required types with existence and substantiveness check.

### G4: Internal Consistency

**Purpose:** Ensure no individual document contradicts itself.

**Procedure:** For each document:
1. Read the document end-to-end
2. Check for:
   - [ ] Prose that contradicts a table in the same document
   - [ ] Numbers or values that differ between sections (e.g., "30-minute timeout" in §2, "60-minute timeout" in §5)
   - [ ] Terms used inconsistently within the document
   - [ ] Section references that point to non-existent sections

**Evidence:** List of contradictions found (or "none") per document.

### G5: Cross-Document Consistency

**Purpose:** Ensure shared contracts match across all documents that reference them.

**Checks to run:**

| Check | Documents to Compare | What Must Match |
|-------|---------------------|----------------|
| Schema ↔ API | Backend Architecture ↔ API Specification | Every database column maps to an API response field |
| Auth ↔ Everything | Security Architecture ↔ API, Backend, Frontend | Role names, permission sets, token claims, lockout rules |
| Process ↔ API | Functional Processes ↔ API Specification | Every user-facing process has corresponding API endpoints |
| Tokens ↔ Frontend | UI Design ↔ Frontend Architecture ↔ secondary UI docs | Hex color values, spacing values, typography specs, motion curves |
| Config ↔ Docs | Backend Architecture ↔ Functional Processes ↔ Operational | Config key names, default values, environment variable names |
| Terminology ↔ All | Domain pack glossary (if exists) ↔ every document | Same term has same meaning everywhere |
| Glossary ↔ All | Decision Log (Product Glossary) ↔ every document | Product-specific terms used consistently; no synonyms or variant spellings |
| Quality Attrs ↔ Impl | Root Architecture §8 ↔ Backend Architecture §9 | Performance targets, availability goals, and security targets match between high-level and detailed specs |
| Principles ↔ Decisions | Root Architecture §6 ↔ Decision Log, all architecture docs | No decision or design pattern contradicts a stated architecture principle |
| Cross-Cutting ↔ Refs | Root Architecture §11 ↔ documents listed in "Defined In" / "Enforced By" | Each cross-cutting concern is addressed in every document that claims to enforce it |

**Procedure** for each check:
1. Verify all referenced documents exist — **skip the check if any referenced document was not produced at this tier** (e.g., skip Auth ↔ Everything if Security Architecture doesn't exist). Record as "skipped — document not in scope" in the completeness report.
2. Extract the contract values from the defining document
3. Search all referencing documents for the same values
4. Flag any discrepancy (different value, missing reference, or conflicting definition)

Domain packs may inject additional checks specific to the domain.

**Evidence:** Table of checks with match/mismatch details.

### G6: Bidirectional References

**Purpose:** Ensure dependency relationships are acknowledged from both sides.

**Procedure:**
1. For each document, read its `depends_on` list
2. For each dependency A → B:
   - Check if B's `depends_on` includes A (bidirectional)
   - If not, verify the one-way relationship is justified (B is a foundational document that many things depend on — it doesn't need to know about each consumer)
3. Flag unjustified one-way references

**Evidence:** Table of dependency pairs with bidirectional status.

### G7: Version Gate Alignment

**Purpose:** Ensure features marked as "future" or deferred are consistently marked across all documents.

**Procedure:**
1. Extract all version-gated features from the Decision Log (entries with `Version Gate: future` or specific future version)
2. Search all documents for references to these features
3. Verify each reference is marked as deferred/future (not described as v1 functionality)
4. Verify no document depends on a deferred feature without noting the dependency is future-gated

**Evidence:** Table of deferred features with consistency status across documents.

### G8: Gap Analysis

**Purpose:** Ensure no critical architectural questions are left unanswered and no sections contain placeholder content.

**Procedure:**
1. Search all documents for:
   - [ ] TODO, TBD, FIXME, placeholder markers
   - [ ] Empty sections (heading with no content)
   - [ ] Questions posed but not answered
   - [ ] References to documents or sections that don't exist
2. Classify each finding as Critical (blocks implementation) or Minor (can be resolved during execution)
3. Critical findings must be resolved before P4 exits

**Evidence:** Table of gaps with severity and resolution status.

---

## Completeness Report Template

The P4 exit artifact. Create this document using the template below:

```markdown
# {Product Name} Design Completeness Report

**Date:** {YYYY-MM-DD}
**Tier:** {T1 | T2 | T3}
**Document Count:** {N}
**Assessor:** {name or "automated"}

---

## 1. Gate Results

| Gate | Required | Status | Evidence |
|------|:--------:|:------:|----------|
| G1: Metadata Integrity | {yes/no} | {pass/fail} | {summary} |
| G2: Decision Coverage | {yes/no} | {pass/fail} | {summary} |
| G3: Taxonomy Completeness | {yes/no} | {pass/fail} | {summary} |
| G4: Internal Consistency | {yes/no} | {pass/fail/skipped} | {summary} |
| G5: Cross-Document Consistency | {yes/no} | {pass/fail/skipped} | {summary} |
| G6: Bidirectional References | {yes/no} | {pass/fail/skipped} | {summary} |
| G7: Version Gate Alignment | {yes/no} | {pass/fail/skipped} | {summary} |
| G8: Gap Analysis | {yes/no} | {pass/fail/skipped} | {summary} |

## 2. Consistency Findings

| Check | Documents Compared | Status | Detail |
|-------|-------------------|:------:|--------|
| Schema ↔ API | {docs} | {consistent/minor/contradiction} | {detail} |
| Auth ↔ Everything | {docs} | {consistent/minor/contradiction} | {detail} |
| Process ↔ API | {docs} | {consistent/minor/contradiction} | {detail} |
| Tokens ↔ Frontend | {docs} | {consistent/minor/contradiction} | {detail} |
| Config ↔ Docs | {docs} | {consistent/minor/contradiction} | {detail} |
| Terminology ↔ All | {docs} | {consistent/minor/contradiction} | {detail} |
| Glossary ↔ All | {docs} | {consistent/minor/contradiction} | {detail} |
| Quality Attrs ↔ Impl | {docs} | {consistent/minor/contradiction} | {detail} |
| Principles ↔ Decisions | {docs} | {consistent/minor/contradiction} | {detail} |
| Cross-Cutting ↔ Refs | {docs} | {consistent/minor/contradiction} | {detail} |

## 3. Gaps Identified

| ID | Severity | Document | Section | Description | Resolution |
|----|:--------:|----------|---------|-------------|------------|
| GAP-1 | {Critical/Minor} | {doc} | {section} | {description} | {resolution or "pending"} |

## 4. Status Promotion Recommendation

| Document | Current Status | Recommended | Blocker |
|----------|:--------------:|:-----------:|---------|
| {doc} | {draft/review} | {review/approved} | {blocker or "none"} |

## 5. Overall Assessment

**Implementation Ready:** {Yes / No — pending resolution of {N} critical gaps}

{Summary paragraph with key observations}
```

---

## Resolution Workflow

When a gate fails:

1. **Identify** — which gate, which check, which documents
2. **Assign** — each finding to a specific document and section for resolution
3. **Resolve** — fix the document (update content, add missing references, correct values)
4. **Re-run** — execute only the affected gate(s), not the full suite
5. **Record** — update the Completeness Report with resolution details
6. **Repeat** — until all required gates pass

**Escalation:** If a finding cannot be resolved without architectural changes, it becomes a new Decision Log entry and flows back through P3 (affected documents only) → P4 (re-run gates).
