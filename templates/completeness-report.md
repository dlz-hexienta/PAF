---
title: "{Product} Design Completeness Report"
domain: "{domain}"
category: completeness-report
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: []
tags: []
---

# {Product} Design Completeness Report

**Date:** {YYYY-MM-DD}
**Tier:** {T1 | T2 | T3}
**Document Count:** {N}
**Assessor:** {name or "automated"}

---

## 1. Gate Results

| Gate | Required | Status | Evidence |
|------|:--------:|:------:|----------|
| G1: Metadata Integrity | {yes/no} | {pass/fail/skip} | {summary} |
| G2: Decision Coverage | {yes/no} | {pass/fail/skip} | {summary} |
| G3: Taxonomy Completeness | {yes/no} | {pass/fail/skip} | {summary} |
| G4: Internal Consistency | {yes/no} | {pass/fail/skip} | {summary} |
| G5: Cross-Document Consistency | {yes/no} | {pass/fail/skip} | {summary} |
| G6: Bidirectional References | {yes/no} | {pass/fail/skip} | {summary} |
| G7: Version Gate Alignment | {yes/no} | {pass/fail/skip} | {summary} |
| G8: Gap Analysis | {yes/no} | {pass/fail/skip} | {summary} |

---

## 2. Consistency Findings

| Check | Documents Compared | Status | Detail |
|-------|-------------------|:------:|--------|
| Schema ↔ API | {docs} | {result} | {detail} |
| Auth ↔ Everything | {docs} | {result} | {detail} |
| Process ↔ API | {docs} | {result} | {detail} |
| Tokens ↔ Frontend | {docs} | {result} | {detail} |
| Config ↔ Docs | {docs} | {result} | {detail} |
| Terminology ↔ All | {docs} | {result} | {detail} |
| Glossary ↔ All | {docs} | {result} | {detail} |
| Quality Attrs ↔ Impl | {docs} | {result} | {detail} |
| Principles ↔ Decisions | {docs} | {result} | {detail} |
| Cross-Cutting ↔ Refs | {docs} | {result} | {detail} |

---

## 3. Gaps Identified

| ID | Severity | Document | Section | Description | Resolution |
|----|:--------:|----------|---------|-------------|------------|
| GAP-1 | {Critical/Minor} | {doc} | {section} | {description} | {resolution} |

---

## 4. Status Promotion Recommendation

| Document | Current Status | Recommended | Blocker |
|----------|:--------------:|:-----------:|---------|
| {doc} | {status} | {recommendation} | {blocker or "none"} |

---

## 5. Overall Assessment

**Implementation Ready:** {Yes / No — pending resolution of N critical gaps}

{Summary paragraph}
