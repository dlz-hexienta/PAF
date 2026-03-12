---
title: "{Product} Security Architecture"
domain: "{domain}"
category: security-architecture
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}", "{functional-processes-filename}"]
tags: []
---

# {Product} Security Architecture

## 1. Security Principles

<!-- What security philosophy governs this product? -->

- {principle}: {explanation}

---

## 2. Trust Boundaries

<!-- Where are the trust boundaries? What crosses them? -->

| Boundary | Inside | Outside | Crossing Mechanism |
|----------|--------|---------|-------------------|
| {name} | {what's trusted} | {what's not} | {how things cross} |

---

## 3. Threat Model

<!-- [T3] Key threats and mitigations. Can reference STRIDE, OWASP, etc. -->

| Threat | Category | Mitigation |
|--------|----------|-----------|
| {threat} | {category} | {how it's addressed} |

---

## 4. Authentication

### Auth Flows
<!-- How do users authenticate? (local password, SSO, API key, certificate) -->

### Token Management
<!-- JWT? Session? What's the lifetime, refresh mechanism, revocation? -->

### Multi-Factor Authentication
<!-- [T3] MFA implementation if applicable -->

---

## 5. Authorization

### Role-Based Access Control

| Role | Description | Permissions |
|------|-------------|-------------|
| {role} | {who has this role} | {what they can do} |

### Permission Model
<!-- How are permissions checked? Middleware? Decorator? Policy engine? -->

---

## 6. Data Protection

### Encryption at Rest
<!-- What's encrypted? What algorithm? Where are keys stored? -->

### Encryption in Transit
<!-- TLS version, certificate management -->

### Data Classification
<!-- [T3] What data categories exist? What protection level for each? -->

---

## 7. Audit & Logging

<!-- What security events are logged? Format? Retention? -->

### Audit Events
| Event | Trigger | Data Captured |
|-------|---------|---------------|
| {event} | {when} | {what's logged} |

### Log Security
<!-- How are logs protected from tampering? -->

---

## 8. API Security

<!-- Rate limiting, input validation, CORS, CSP, security headers -->

### Rate Limiting
| Endpoint Group | Limit | Window |
|---------------|-------|--------|
| {group} | {requests} | {period} |

### Input Validation
<!-- Where and how is input validated? -->

---

## 9. Key Management

<!-- [T3] KMS integration, key rotation schedule, key hierarchy -->

---

## 10. Compliance

<!-- [T3] Regulatory requirements and how they're met -->

| Requirement | Standard | Implementation |
|-------------|----------|---------------|
| {requirement} | {standard} | {how met} |

---

<!-- TIER GUIDANCE:
T2: Sections 1-2, 4-6, 8 required (if optional doc is included).
T3: All sections required.
-->
