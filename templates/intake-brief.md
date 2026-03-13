---
title: "{Product} Intake Brief"
domain: "{domain}"
category: intake-brief
status: draft
version: "0.1"
date: {date}
parent: ~
depends_on: []
tags: []
---

# {Product} Intake Brief

## 1. Core Identity

**Name:** {Product}
**Description:** {One paragraph — what this product does}
**Target Users:** {Who uses it and who buys it}
**Problem Solved:** {What problem it solves that isn't solved today}
**Deployment Model:** {SaaS / on-prem / embedded / CLI / library}
**Commercial Model:** {open source / freemium / licensed / internal}

---

## 2. Complexity Assessment

| Signal | Value | Tier Triggered |
|--------|-------|:--------------:|
| User roles | {number} | {T1/T2/T3} |
| UI screens | {number} | {T1/T2/T3} |
| Data stores | {number} | {T1/T2/T3} |
| External integrations | {number} | {T1/T2/T3} |
| Multi-tenancy / billing | {Yes/No} | {T1/T2/T3} |
| Security/compliance | {description} | {T1/T2/T3} |
| Deployment components | {number} | {T1/T2/T3} |

**Assigned Tier:** {T1 / T2 / T3}
**Rationale:** {Highest signal was X, triggering TN}
**Override:** {None / Overridden to TN because...}

---

## 3. Scope Boundaries

### v1.0 (Must Have)
- {feature/capability}

### Future (Deferred)
- {feature} — {rationale for deferral}

### Out of Scope
- {feature} — {rationale for exclusion}

---

## 4. Stakeholders

<!-- [T2+] Key stakeholders and their concerns. Feeds Root Architecture §2. -->

| Role | Key Concern | Reviews |
|------|------------|---------|
| {e.g., Product Owner} | {e.g., feature completeness} | {e.g., Root Architecture, Functional Processes} |

---

## 5. Domain Pack Match

**Matched Pack:** {pack name or "None"}
**Domain Questions Asked:** {count or "N/A"}

---

<!-- TIER GUIDANCE:
T1: Sections 1-3, 5 required. Section 4 skipped.
T2: All sections required.
T3: All sections required. If decomposition check triggers, add §6 with sub-product breakdown.
-->
