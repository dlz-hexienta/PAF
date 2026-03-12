---
title: "{Product} Architecture"
domain: "{domain}"
category: root-architecture
status: draft
version: "0.1"
date: {date}
parent: ~
depends_on: []
tags: []
---

# {Product} Architecture

## 1. Product Identity

<!-- What is this product? One paragraph. -->

**Name:** {Product}
**Tagline:** {One sentence value proposition}
**Target Users:** {Who uses it and who buys it}

### Problem Statement
<!-- What problem does this solve? Why do existing solutions fall short? -->

### Product Vision
<!-- Where is this product going? What does success look like in 2 years? -->

---

## 2. Architecture Overview

<!-- High-level component diagram. What are the major pieces and how do they connect? -->

### Components

| Component | Responsibility | Deployment |
|-----------|---------------|------------|
| {name} | {what it does} | {how it runs} |

### Component Interaction
<!-- How do components communicate? (HTTP, gRPC, IPC, message queue, shared DB) -->

---

## 3. Deployment Model

<!-- How is this deployed? Single binary? Multiple services? Container? -->

**Primary:** {e.g., single binary, Docker container, Kubernetes}
**Development:** {e.g., local binary, docker-compose}

### Deployment Topology
<!-- [T2+] Diagram showing deployment components and their relationships -->

### Scaling Strategy
<!-- [T3] How does deployment evolve? (single node → modular → distributed) -->

---

## 4. Technology Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Backend | {language/framework} | {why} |
| Frontend | {framework} | {why} |
| Database | {engine} | {why} |
| {other} | {tool} | {why} |

---

## 5. Key Architectural Decisions

<!-- Major decisions that shape the system. Each should have rationale. -->

### Decision 1: {Title}
**Choice:** {what was decided}
**Rationale:** {why}
**Alternatives considered:** {what was rejected and why}

<!-- Repeat for each major decision. Reference Decision Log entries where applicable. -->

---

## 6. Data Model Overview

<!-- [T2+] High-level entities and relationships. Not full schema — that's in Backend Architecture. -->

| Entity | Description | Key Relationships |
|--------|-------------|-------------------|
| {name} | {what it represents} | {relates to...} |

---

## 7. Security Model

<!-- [T2+] High-level security posture. Full detail in Security Architecture. -->

**Authentication:** {method}
**Authorization:** {model — RBAC, ABAC, etc.}
**Data Protection:** {encryption strategy}

---

## 8. Integration Points

<!-- [T2+] External systems this product connects to. -->

| System | Protocol | Purpose |
|--------|----------|---------|
| {name} | {REST/gRPC/SMTP/etc.} | {what it does} |

---

## 9. Release Roadmap

| Version | Scope | Key Features |
|---------|-------|-------------|
| v1.0 | {scope} | {features} |
| v1.1 | {scope} | {features} |

---

## 10. Constraints & Assumptions

<!-- What constraints shape this architecture? What assumptions are we making? -->

### Constraints
- {constraint with rationale}

### Assumptions
- {assumption — document so it can be validated}

---

<!-- TIER GUIDANCE:
T1 (Utility): Sections 1, 2, 4, 5 required. Others optional.
T2 (Application): Sections 1-8 required. 9-10 recommended.
T3 (Platform): All sections required. Add subsections as needed.
-->
