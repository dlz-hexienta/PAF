---
title: "{Product} API Specification"
domain: "{domain}"
category: api-specification
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}", "{functional-processes-filename}", "{security-filename}"]
tags: []
---

# {Product} API Specification

## 1. API Conventions

### Base URL
**Production:** `{base_url}`
**Development:** `{dev_url}`

### Authentication
<!-- Reference Security Architecture. Summarize auth mechanism. -->

### Response Envelope
```json
{
  "data": {},
  "meta": {
    "request_id": "uuid",
    "timestamp": "ISO-8601"
  }
}
```

### Error Format
<!-- RFC 7807 ProblemDetail recommended -->

### Pagination
<!-- Cursor-based or offset-based? Parameters? -->

### Filtering
<!-- Filter syntax convention -->

### Rate Limiting
<!-- Headers, limits per endpoint group -->

---

## 2. Endpoints: {Domain Group 1}

### `{METHOD} {path}`
**Description:** {what it does}
**Auth:** {required role/permission}

**Request:**
```json
{request body or "N/A"}
```

**Response (200):**
```json
{response body}
```

**Errors:** {400, 401, 403, 404, 409, 422 as applicable}

<!-- Repeat for each endpoint in this group -->

---

## 3. Endpoints: {Domain Group 2}

<!-- Repeat group structure for each API domain -->

---

## N. Schemas

<!-- Shared schema definitions referenced across endpoints -->

### {SchemaName}
```json
{
  "field": "type — description"
}
```

---

## N+1. WebSocket / Real-Time

<!-- [T2+] If applicable: WebSocket endpoint, event types, subscription model -->

---

## N+2. CLI Mapping

<!-- [T2+] If CLI exists: map each CLI command to its API endpoint -->

| CLI Command | API Endpoint | Description |
|------------|-------------|-------------|
| `{command}` | `{METHOD} {path}` | {description} |

---

<!-- TIER GUIDANCE:
T1: Sections 1-2, N required (if optional doc is included).
T2: All relevant sections required.
T3: All sections required. Consider separate canonical OpenAPI spec.
-->
