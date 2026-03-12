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

## 9. Performance Targets

| Metric | Target |
|--------|--------|
| API response (p95) | {ms} |
| Startup time | {seconds} |
| Memory usage | {MB} |

---

## 10. Migration Strategy

<!-- How are database schema changes managed? -->

**Tool:** {migration tool}
**Direction:** {forward-only or reversible}

---

<!-- TIER GUIDANCE:
T1: Sections 1-4, 6-7 required (if optional doc is included).
T2: Sections 1-7 required. 8-10 recommended.
T3: All sections required.
-->
