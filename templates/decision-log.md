---
title: "{Product} Decision Log"
domain: "{domain}"
category: decision-log
status: draft
version: "0.1"
date: {date}
parent: ~
depends_on: []
# NOTE: Update parent and depends_on to point to the Root Architecture filename
# once it is created during P3 (e.g., parent: "acme-root-architecture.md")
tags: []
---

# {Product} Decision Log

<!-- This document accumulates throughout P2 and P3. Each entry records a
     design decision with rationale and distribution targets. The "Distribute To"
     field creates traceability from decision to document section. -->

---

### D-1: {Question}

**Date:** {YYYY-MM-DD}

**Context:** {Why this decision matters}

**Options Considered:**
1. {Option A} — {trade-offs}
2. {Option B} — {trade-offs}
3. {Option C} — {trade-offs}

**Decision:** {Which option was chosen, with reasoning}

**Distribute To:** {List of document types that need this decision}

**Version Gate:** {v1.0 | future | N/A}

---

### D-2: {Question}

<!-- Repeat the format above for each decision -->

---

## Product Glossary

<!-- [T2+] Define product-specific terms used across the design corpus. This is distinct from domain pack terminology (which is domain-general). Terms here ensure consistent naming in all documents. G5 checks terminology consistency against this table. -->

| Term | Definition | Used In |
|------|-----------|---------|
| {term} | {precise definition in this product's context} | {list of documents using this term} |

<!-- USAGE NOTES:
- Number decisions sequentially (D-1, D-2, D-3...)
- Always include "Distribute To" — this is what makes decisions traceable
- The Version Gate field prevents scope creep by explicitly deferring features
- Reference decisions from target documents: "Per D-7, we use..."
- New decisions discovered during P3 authoring are added here too
- Decisions made during P6 execution (Design Feedback Loop) also go here
- Glossary: add terms during P2 as they emerge; verify coverage during P4 (G5)
-->
