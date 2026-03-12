---
title: "{Product} Operational Guide"
domain: "{domain}"
category: operational
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}", "{backend-filename}", "{security-filename}"]
tags: []
---

# {Product} Operational Guide

<!-- At T3, consider splitting into separate documents:
     - {Product} Testing Strategy
     - {Product} Observability Strategy
     - {Product} Release Process -->

## 1. Testing Strategy

### Test Pyramid

| Level | Tool | Coverage Target | What It Tests |
|-------|------|----------------|---------------|
| Unit | {tool} | {%} | {scope} |
| Integration | {tool} | {count} | {scope} |
| E2E | {tool} | {count} | {scope} |

### CI/CD Pipeline
<!-- What runs on each PR? On merge to main? On release? -->

---

## 2. Observability

### Logging
**Format:** {structured JSON, plain text}
**Levels:** {debug, info, warn, error}
**Retention:** {duration}

### Metrics
| Metric | Type | Description |
|--------|------|-------------|
| {name} | {counter/gauge/histogram} | {what it measures} |

### Tracing
<!-- [T3] Distributed tracing implementation -->

### Alerting
| Alert | Condition | Severity | Response |
|-------|-----------|----------|----------|
| {name} | {condition} | {level} | {action} |

---

## 3. Release Process

### Versioning
**Scheme:** {semver, calver}

### Build Artifacts
| Artifact | Format | Distribution |
|----------|--------|-------------|
| {name} | {binary/container/package} | {how distributed} |

### Release Checklist
- [ ] {step}
- [ ] {step}

---

## 4. Runbook

<!-- [T3] Common operational procedures -->

### {Procedure Name}
**When:** {trigger condition}
**Steps:**
1. {step}
2. {step}

---

<!-- TIER GUIDANCE:
T2: Sections 1-3 required (if optional doc is included).
T3: All sections required. Consider splitting into 3 documents.
-->
