---
title: "{Product} Frontend Architecture"
domain: "{domain}"
category: frontend-architecture
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}", "{ui-design-filename}"]
tags: []
---

# {Product} Frontend Architecture

## 1. Framework Decision

**Choice:** {framework + version}
**Rationale:** {why this framework}
**Alternatives considered:** {what was rejected}

---

## 2. Project Structure

```
{project-dir}/
├── src/
│   ├── lib/
│   │   ├── components/      # Shared UI components
│   │   ├── api/              # API client, types
│   │   └── stores/           # State management
│   ├── routes/               # Pages/routes
│   └── app.{ext}             # Entry point
├── static/                   # Static assets (fonts, icons)
├── tests/                    # Test files
└── {config files}
```

---

## 3. Build Pipeline

| Stage | Tool | Output |
|-------|------|--------|
| Dev server | {tool} | {localhost:port} |
| Build | {tool} | {output format} |
| Bundle target | — | {size target} |

---

## 4. Component Library

**Library:** {headless library or custom}
**Rationale:** {why}

### Component Architecture
<!-- How are components structured? Props interface, composition pattern. -->

---

## 5. API Client

**Approach:** {e.g., openapi-typescript + openapi-fetch, axios, fetch wrapper}
**Type safety:** {how types are generated/maintained}

---

## 6. State Management

<!-- How is application state managed? (stores, context, URL state) -->

---

## 7. Real-Time

<!-- [T2+] WebSocket, SSE, or polling strategy for live updates -->

---

## 8. Styling Strategy

**Approach:** {CSS modules, custom properties, utility classes, CSS-in-JS}
**Design tokens:** {how tokens from UI Design are consumed}

---

## 9. Testing Strategy

| Type | Tool | Coverage Target |
|------|------|----------------|
| Unit | {tool} | {target} |
| E2E | {tool} | {target} |

---

## 10. Accessibility

**Target:** {WCAG level}
**Approach:** {how accessibility is implemented}

---

## 11. Performance

| Metric | Target |
|--------|--------|
| Bundle size (gzipped) | {target} |
| First Contentful Paint | {target} |
| Time to Interactive | {target} |

---

## 12. Deployment

<!-- How is the frontend deployed? (static hosting, embedded in binary, CDN) -->

**Path prefix:** {e.g., /ui/, /, /app/}
**Hosting:** {strategy}

---

<!-- TIER GUIDANCE:
T2: Sections 1-6, 8-9 required. Others recommended.
T3: All sections required.
-->
