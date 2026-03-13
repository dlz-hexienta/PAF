---
title: "{Product} Backend Architecture"
domain: "{domain}"
category: backend-architecture
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}", "{functional-processes-filename}"]
tags: []
---

# {Product} Backend Architecture

## 1. Technology Stack

| Component | Technology | Version | Rationale |
|-----------|-----------|---------|-----------|
| Language | {lang} | {ver} | {why} |
| Database | {db} | {ver} | {why} |
| {other} | {tool} | {ver} | {why} |

---

## 2. Binary / Service Structure

<!-- What binaries or services are produced? What does each one do? -->

| Binary/Service | Responsibility | Ports |
|---------------|---------------|-------|
| {name} | {what it does} | {ports} |

---

## 3. Package / Module Layout

```
{project-root}/
├── cmd/
│   └── {binary}/             # Entrypoint
├── internal/
│   ├── api/                  # HTTP handlers
│   ├── {domain}/             # Business logic
│   └── db/                   # Database layer
└── pkg/                      # Shared types
```

---

## 4. Database Schema

<!-- DDL or ERD for each table. Include indexes, constraints, relationships. -->

### Table: {name}

```sql
CREATE TABLE {name} (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- columns
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

<!-- Repeat for each table -->

---

## 5. API Layer

<!-- How are HTTP routes organized? Middleware chain? Auth enforcement? -->

### Middleware Chain
<!-- Order matters: logging → auth → rate limit → handler -->

### Route Organization
<!-- How are routes grouped? By domain? By version? -->

---

## 6. Configuration

### Loading Order
<!-- config file → env vars → CLI flags. Which wins? -->

### Schema
<!-- Key configuration sections with types and defaults -->

---

## 7. Error Handling

<!-- Error types, propagation strategy, user-facing error format -->

**Error format:** {e.g., RFC 7807 ProblemDetail}

---

## 8. IPC Protocol

<!-- [T3] If multiple processes: how do they communicate? -->

**Transport:** {gRPC, Unix socket, HTTP, message queue}
**Protocol:** {request/response, streaming, pub/sub}

---

## 9. Performance & Scaling

### Performance Targets

| Metric | Target | Measured By |
|--------|--------|-------------|
| API response (p95) | {ms} | {e.g., middleware timer, APM} |
| Startup time | {seconds} | {e.g., readiness probe} |
| Memory usage | {MB} | {e.g., runtime metrics} |

### Scaling Strategy

<!-- [T2+] How does each component scale? What triggers scaling? -->

| Component | Strategy | Trigger | Ceiling |
|-----------|----------|---------|---------|
| {e.g., API server} | {horizontal / vertical} | {e.g., CPU > 70%, queue depth > 100} | {e.g., 8 replicas} |
| {e.g., Database} | {read replicas / sharding / vertical} | {e.g., connection pool > 80%} | {e.g., 1 primary + 3 read replicas} |

### Capacity Planning

<!-- [T3] Expected load and growth assumptions. Feeds operational monitoring thresholds. -->

| Resource | v1.0 Target | Growth Assumption |
|----------|-------------|-------------------|
| {e.g., Concurrent users} | {number} | {e.g., 2x per quarter} |
| {e.g., Storage} | {GB} | {e.g., +50GB/month} |
| {e.g., Requests/sec} | {number} | {e.g., proportional to user growth} |

---

## 10. Migration Strategy

<!-- How are database schema changes managed? -->

**Tool:** {migration tool}
**Direction:** {forward-only or reversible}

---

<!-- TIER GUIDANCE:
T1: Sections 1-4, 6-7, 9 (Performance Targets only) required (if optional doc is included).
T2: Sections 1-7, 9 (Performance Targets + Scaling Strategy) required. 8, 10 recommended.
T3: All sections and subsections required.
-->
