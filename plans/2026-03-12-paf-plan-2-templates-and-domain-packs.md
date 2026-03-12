# PAF Plan 2: Templates & Domain Packs — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the 12 document templates, the domain pack template, the ITSM domain pack (captured from the proof-case product), and the proof-case example stubs.

**Architecture:** Templates are Markdown scaffolds with placeholder variables, section headings, tier indicators, and guidance notes. Domain packs are YAML files following the schema defined in PAF-Domain-Pack-Specification.md. Example stubs point back to the source product's design corpus as a reference implementation.

**Tech Stack:** Markdown, YAML. No code dependencies.

**Spec:** `docs/paf/2026-03-12-paf-design-spec.md` §5 (taxonomy), §13 (file structure)
**Depends on:** Plan 1 (methodology documents — for cross-references in templates)

---

## File Structure

```
docs/paf/
├── templates/                            # CREATE all
│   ├── root-architecture.md              # Layer 0
│   ├── functional-processes.md           # Layer 1
│   ├── ui-design.md                      # Layer 1
│   ├── frontend-architecture.md          # Layer 2
│   ├── backend-architecture.md           # Layer 2
│   ├── security-architecture.md          # Layer 2
│   ├── api-specification.md              # Layer 3
│   ├── operational.md                    # Layer 4
│   ├── decision-log.md                   # Continuous
│   ├── completeness-report.md            # P4 artifact
│   ├── design-library.md                 # T3 governance
│   └── implementation-plan.md            # P5 artifact
│
├── domain-packs/                         # CREATE all
│   ├── _template.yaml                    # Empty pack scaffold
│   └── itsm.yaml                         # First real pack
│
└── examples/
    └── itsm-platform/                     # CREATE all
        ├── README.md                     # Overview + pointers
        ├── intake-brief.md               # What P1 produced
        ├── decision-record.md            # What P2 produced (sample entries)
        └── document-index.md             # What P3 produced (pointer to corpus)
```

---

## Chunk 1: Domain Packs

### Task 1: Domain Pack Template

**Files:**
- Create: `docs/paf/domain-packs/_template.yaml`

- [ ] **Step 1: Create the empty domain pack template**

```yaml
# PAF Domain Pack Template
# Copy this file and fill in domain-specific knowledge.
# See methodology/PAF-Domain-Pack-Specification.md for field definitions.

name: ""
version: "0.1"
captured_from: ""
date: YYYY-MM-DD
description: ""

# P1: Additional intake questions for this domain
intake_questions: []

# P2: Additional requirement prompts by category
# Valid categories: functional, data-model, integration, user-experience,
#   security, operational, commercial, compliance
requirement_prompts: {}
#  functional:
#    - "Question?"
#  security:
#    - "Question?"

# P3: Additional sections to inject into document scaffolds
# Keys are document types (lowercase, hyphenated)
document_section_templates: {}
#  functional_processes:
#    - "Section heading"
#  backend_architecture:
#    - "Section heading"

# P4: Additional quality checks for G5 (Cross-Document Consistency)
quality_checks: []
#  - description: "What to check"
#    documents: ["doc-type-1", "doc-type-2"]
#    check: "What must be consistent"

# Domain glossary
terminology: []
#  - term: "Term Name"
#    definition: "What it means in this domain"

# P5/P6: Domain-specific tooling recommendations
tooling_recommendations: []
#  - capability: "what_it_does"
#    recommended: "Tool Name"
#    rationale: "Why"
#    synergy: "How it pairs with other tools"
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/domain-packs/_template.yaml
git commit -m "docs(paf): add empty domain pack template"
```

---

### Task 2: ITSM Domain Pack

**Files:**
- Create: `docs/paf/domain-packs/itsm.yaml`

**Context:** This is the first real domain pack, captured from a real enterprise platform design process. It encodes ITSM-specific questions, constraints, section templates, quality checks, terminology, and tooling recommendations. Source: PAF-Domain-Pack-Specification.md §Example.

- [ ] **Step 1: Create the ITSM domain pack**

```yaml
name: IT Service Management
version: "1.0"
captured_from: enterprise ITSM platform
date: 2026-03-12
description: >
  Knowledge pack for AI-powered ITSM products — autonomous agents
  that handle incidents, problems, changes, and service requests
  within enterprise IT environments. Captured from a real platform design
  process (22 documents, 312-task plan).

intake_questions:
  - "Does the product align with ITIL process areas? Which ones? (Incident, Problem, Change, Service Request, Knowledge, CMDB)"
  - "What change authority model applies? (advisory / approval-required / autonomous)"
  - "Are there maturity-gated capabilities — features unlocked by demonstrated competence?"
  - "Does the product need to integrate with existing ITSM tools? (ServiceNow, Jira Service Management, BMC, Freshservice)"
  - "Is there a human-in-the-loop requirement for high-risk actions?"
  - "Does the product operate within client environments (on-prem/air-gapped) or as SaaS?"
  - "Is there a marketplace or skill/plugin ecosystem?"

requirement_prompts:
  functional:
    - "How does agent autonomy evolve over time? Is there a tier/maturity model?"
    - "What triggers agent actions? (events, schedules, human requests, other agents)"
    - "How are agent skills/capabilities packaged and distributed?"
    - "What is the agent lifecycle? (install → configure → learn → operate → retire)"
    - "How do agents communicate with each other? (direct, message bus, shared state)"
    - "What channels do agents use to interact with humans? (Slack, Teams, Email, Web UI)"
  data-model:
    - "What memory layers does an agent have? (identity, context, experience, relationships, capability)"
    - "How is agent state persisted? (embedded DB, external DB, hybrid)"
    - "What is the skill hierarchy? (atomic actions → composed skills → workflows → playbooks)"
  security:
    - "How does the system defend against prompt injection in AI-generated actions?"
    - "Is there an immutable audit trail requirement? Append-only? Hash-chained?"
    - "Does the governance layer need to be architecturally separate from the runtime?"
    - "What is the action risk classification model? (observe → inform → suggest → act)"
    - "Does the operator (vendor) have zero-knowledge of client operational data?"
  operational:
    - "What sandbox model is used for code execution? (Deno, Firecracker, containers, none)"
    - "How does the system handle agent-to-agent communication across trust boundaries?"
    - "What is the deployment model? (single binary, modular, distributed/Kubernetes)"
    - "How are upgrades handled? (all-at-once, rolling restart, blue-green)"
  commercial:
    - "Is there a license tier model? (essentials, professional, enterprise)"
    - "Is there a control plane / data plane separation for fleet management?"
    - "Does the marketplace support third-party publishers?"

document_section_templates:
  functional_processes:
    - "Agent Lifecycle Management (install → configure → activate → learn → operate → pause → retire)"
    - "Escalation Flows (autonomous → supervised → human-required, with timeout and fallback)"
    - "Skill Hierarchy (runbooks → actions → subskills → skills → workflows → playbooks)"
    - "Trigger Engine (event-driven, schedule-driven, human-initiated, agent-initiated)"
    - "Channel Interaction Flows (inbound routing, outbound formatting, thread state, identity)"
  backend_architecture:
    - "Governance Layer (separate process, IPC protocol, fail-closed enforcement)"
    - "Sandbox Pool (warm pool management, execution isolation, resource limits, cleanup)"
    - "Memory System (multi-layer: role, organizational, episodic, collaborative, procedural)"
    - "Agent Runtime (lifecycle state machine, LLM provider abstraction, tool registry)"
    - "Uplink Client (control plane communication, license refresh, telemetry, catalog sync)"
  security_architecture:
    - "Prompt Injection Defense (governance backstop is deterministic, not AI-dependent)"
    - "Zero-Knowledge Architecture (operator cannot access client operational data)"
    - "Action Risk Classification (observe → inform → suggest → act-low → act-high)"
    - "Autonomy Tier Enforcement (tier rules are hard constraints, not suggestions)"
    - "Key Management (KMS integration, key rotation, encrypted memory layers)"
  api_specification:
    - "Agent Management Endpoints (CRUD, lifecycle transitions, logs, config, memory, triggers)"
    - "Governance Endpoints (policies, audit trail, decision traces, replay, approvals)"
    - "Skill Management Endpoints (list, search, install, approve, reload, updates)"
    - "WebSocket Activity Stream (real-time events, subscription filters)"
  ui_design:
    - "Agent Dashboard (status cards, activity feed, health indicators)"
    - "Governance Views (policy editor, audit log, decision trace viewer, replay)"
    - "Skill Marketplace (catalog grid, detail view, install flow)"

quality_checks:
  - description: "Autonomy tier consistency"
    documents: ["security-architecture", "api-specification", "backend-architecture", "functional-processes"]
    check: "Tier definitions (A/B/C or equivalent) must match exactly across all documents — same names, same permissions, same constraints"
  - description: "Agent memory layer consistency"
    documents: ["root-architecture", "backend-architecture", "functional-processes"]
    check: "Memory layer names, count, and descriptions must match everywhere they appear"
  - description: "Governance separation enforcement"
    documents: ["backend-architecture", "security-architecture"]
    check: "Governance is documented as a separate process with its own binary, not embedded in the runtime"
  - description: "Action risk classification consistency"
    documents: ["security-architecture", "api-specification", "functional-processes"]
    check: "Risk levels (observe/inform/suggest/act) match across all documents"
  - description: "Skill hierarchy consistency"
    documents: ["functional-processes", "api-specification", "backend-architecture"]
    check: "Skill hierarchy levels match across all references"
  - description: "Channel adapter interface consistency"
    documents: ["backend-architecture", "functional-processes", "api-specification"]
    check: "Channel adapter interface (inbound/outbound) is described consistently"

terminology:
  - term: "Autonomy Tier"
    definition: "Classification of agent decision-making authority. Tier A = full autonomous, Tier B = supervised, Tier C = maturity-gated. Higher tiers unlock as the agent demonstrates competence."
  - term: "Decision Trace"
    definition: "Immutable record of an agent's reasoning chain — inputs, context, options considered, selected action, outcome. Stored for audit replay and governance review."
  - term: "Skill"
    definition: "A packaged unit of agent capability, installable from a marketplace or local registry. Contains instructions, required tools, and metadata."
  - term: "Governance Layer"
    definition: "Architecturally separate process that enforces policies (OPA/Rego), records audit trails (append-only, hash-chained), manages human escalation, and provides decision replay."
  - term: "Warm Pool"
    definition: "Pre-initialized sandbox instances kept ready for immediate use, reducing cold-start latency for agent code execution."
  - term: "Uplink"
    definition: "Outbound-only HTTPS polling connection from data plane to control plane. Carries telemetry, license refresh, catalog sync. Never exposes inbound ports."
  - term: "Zero-Knowledge Architecture"
    definition: "Design principle where the platform operator (vendor) architecturally cannot access client operational data — encryption keys are client-held, data never leaves client environment."
  - term: "Fail-Closed"
    definition: "Security posture where component failure defaults to denying actions rather than permitting them. If governance is unreachable, agents cannot act."
  - term: "Channel"
    definition: "Human interaction pathway (Slack, Teams, Email, Web UI). Distinct from MCP (tool integration) and A2A (agent-to-agent)."
  - term: "Role vs Instance"
    definition: "A Role is a marketplace blueprint (purchasable). An Agent Instance is a running deployment of a Role, configured for a specific organizational context."

tooling_recommendations:
  - capability: governance_policy_engine
    recommended: OPA/Rego
    rationale: "Non-Turing-complete, additive-only — policies can restrict but never override tier rules"
    synergy: "Embedded as Go library (no network hop). Rego policies are version-controlled alongside code."
  - capability: audit_trail
    recommended: "Append-only table with hash chains (SHA-256)"
    rationale: "Tamper-evident without external dependencies. Each record includes hash of previous record."
    synergy: "Same DB engine as primary data store — no additional infrastructure."
  - capability: sandbox_execution
    recommended: "Deno isolates (single-node) / Firecracker (distributed)"
    rationale: "Capability-based security model. Warm pool pattern for latency."
    synergy: "Deno runs JS/TS — matches common skill authoring language. Firecracker shares Linux kernel."
  - capability: channel_adapters
    recommended: "Slack Bolt SDK, Microsoft Graph API, SMTP/IMAP"
    rationale: "Enterprise collaboration platforms where ITSM agents interact with humans"
    synergy: "Adapter pattern with shared internal interface — add channels without changing core."
  - capability: agent_identity
    recommended: "Per-role visual identity system (avatars, color coding)"
    rationale: "Humans need to distinguish agent types in collaboration platforms"
    synergy: "SVG avatars embedded in binary — works in air-gapped environments."
  - capability: llm_provider_abstraction
    recommended: "Provider interface with pluggable backends (OpenAI, Anthropic, Azure, local)"
    rationale: "Enterprise clients have different AI infrastructure preferences and constraints"
    synergy: "Interface defined once in Go, implementations share connection pooling and retry logic."
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/domain-packs/itsm.yaml
git commit -m "docs(paf): add ITSM domain pack captured from proof-case product"
```

---

## Chunk 2: Document Templates (Layers 0–2)

### Task 3: Root Architecture Template (Layer 0)

**Files:**
- Create: `docs/paf/templates/root-architecture.md`

**Context:** Every product at every tier needs this. It's the most important template — must be comprehensive enough to stand alone as a product overview yet flexible enough for T1 (200 lines) through T3 (1500 lines).

- [ ] **Step 1: Create the template**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add docs/paf/templates/root-architecture.md
git commit -m "docs(paf): add root-architecture template (Layer 0)"
```

---

### Task 4: Layer 1 Templates (Functional Processes, UI Design)

**Files:**
- Create: `docs/paf/templates/functional-processes.md`
- Create: `docs/paf/templates/ui-design.md`

- [ ] **Step 1: Create functional-processes template**

```markdown
---
title: "{Product} Functional Processes"
domain: "{domain}"
category: functional-processes
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}"]
tags: []
---

# {Product} Functional Processes

<!-- This document describes HOW the system behaves — not what components exist,
     but what they do. Every downstream document (API, Backend, Security) depends
     on this for understanding the system's operational flows. -->

## 1. Installation & Setup

### Prerequisites
<!-- What must exist before installation? -->

### Installation Flow
<!-- Step-by-step: download → verify → configure → initialize → validate -->

### First-Run Experience
<!-- What happens when the system starts for the first time? -->

---

## 2. Configuration

### Configuration Sources
<!-- Where does config come from? (file, env vars, CLI flags, API) Priority order? -->

### Configuration Schema
<!-- Key configuration sections with defaults and descriptions -->

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| {key} | {type} | {default} | {description} |

### Configuration Validation
<!-- What happens with invalid config? When is config validated? -->

---

## 3. Core Processes

<!-- The primary operational flows. Each process should describe:
     - Trigger: what starts it
     - Flow: step-by-step sequence
     - State transitions: if applicable
     - Error handling: what happens when things go wrong
     - Output: what it produces -->

### Process 1: {Name}

**Trigger:** {what starts this process}

**Flow:**
1. {step}
2. {step}
3. {step}

**State Transitions:**
<!-- [T2+] State machine diagram if applicable -->

**Error Handling:**
<!-- What happens on failure? Retry? Escalate? Fail-closed? -->

<!-- Repeat for each core process -->

---

## 4. Lifecycle Management

<!-- [T2+] How are the system's managed entities created, updated, and removed? -->

### Entity Lifecycle: {Name}
**States:** {draft → active → paused → retired}
**Transitions:** {what triggers each transition}

---

## 5. Scheduled Operations

<!-- [T2+] What happens on a schedule? (maintenance, cleanup, sync, reporting) -->

| Schedule | Operation | Description |
|----------|-----------|-------------|
| {cron/interval} | {name} | {what it does} |

---

## 6. Data Flows

<!-- [T3] How data moves through the system. Input → processing → output → storage. -->

---

<!-- TIER GUIDANCE:
T1: Sections 1-3 only (if optional doc is included).
T2: Sections 1-5 required.
T3: All sections required. Expand §3 with detailed state machines.
-->
```

- [ ] **Step 2: Create ui-design template**

```markdown
---
title: "{Product} UI Design"
domain: "{domain}"
category: ui-design
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}"]
tags: []
---

# {Product} UI Design

## 1. Design Principles

<!-- What visual philosophy governs this product? (e.g., "dark-first precision",
     "clean minimalism", "data-dense operational") -->

- {principle}: {explanation}

---

## 2. Color System

### Surface Hierarchy
<!-- Background layers from deepest to most prominent -->

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-0` | {hex} | Page background |
| `--bg-1` | {hex} | Cards, panels |
| `--bg-2` | {hex} | Nested surfaces |
| `--bg-3` | {hex} | Hover states |
| `--bg-4` | {hex} | Active elements |

### Semantic Colors
<!-- Colors with meaning (accent, status, action) -->

| Token | Hex | Meaning |
|-------|-----|---------|
| `--accent-primary` | {hex} | {meaning} |
| `--accent-secondary` | {hex} | {meaning} |

### Status Colors
| Status | Hex | Usage |
|--------|-----|-------|
| Success/Green | {hex} | {when used} |
| Warning/Amber | {hex} | {when used} |
| Error/Red | {hex} | {when used} |

### Text Colors
| Token | Hex | Usage |
|-------|-----|-------|
| `--text-primary` | {hex} | Headings, important text |
| `--text-secondary` | {hex} | Body text |
| `--text-tertiary` | {hex} | Muted, labels |

### Border Colors
| Token | Hex | Usage |
|-------|-----|-------|
| `--border-primary` | {hex} | Dividers, card edges |
| `--border-subtle` | {hex} | Nested borders |

---

## 3. Typography

| Role | Font | Weight | Size |
|------|------|--------|------|
| Headings | {font} | {weight} | {sizes} |
| Body | {font} | {weight} | {size} |
| Code/Data | {font} | {weight} | {size} |

---

## 4. Spacing & Layout

### Spacing Scale
<!-- Base unit and scale (e.g., 4px base: 4, 8, 12, 16, 24, 32, 48) -->

### Grid System
<!-- Layout grid (e.g., sidebar + main content, responsive breakpoints) -->

### Responsive Breakpoints
| Breakpoint | Width | Layout Change |
|------------|-------|---------------|
| {name} | {px} | {what changes} |

---

## 5. Component Specifications

<!-- Each component the product uses. Include: visual spec, states, interaction. -->

### 5.1 Buttons
<!-- Variants (primary, secondary, ghost, danger), sizes, states (hover, active, disabled) -->

### 5.2 Forms & Inputs
<!-- Text inputs, selects, checkboxes, toggles, validation states -->

### 5.3 Cards & Panels
<!-- Surface components, padding, border radius -->

### 5.4 Tables
<!-- Data tables, sorting, pagination, row actions -->

### 5.5 Navigation
<!-- Sidebar, breadcrumbs, tabs -->

### 5.6 Modals & Dialogs
<!-- Overlay components, confirmation dialogs -->

### 5.7 Status Indicators
<!-- Badges, pills, progress bars, health indicators -->

---

## 6. Icons

**Library:** {icon library}
**Default weight:** {weight}
**Sizes:** {sidebar: Npx, inline: Npx}

---

## 7. Motion & Animation

| Property | Value | Usage |
|----------|-------|-------|
| Duration | {ms} | {standard transitions} |
| Easing | {curve} | {all transitions} |

---

## 8. Screen Layouts

<!-- [T2+] Each primary screen. Include: purpose, layout wireframe, key components. -->

### Screen: {Name}
**Purpose:** {what the user does here}
**Layout:** {description or wireframe reference}
**Key Components:** {which components from §5 appear here}

---

## 9. Design Anti-Patterns

<!-- What to explicitly avoid -->

- **NO:** {anti-pattern with rationale}

---

<!-- TIER GUIDANCE:
T2: Sections 1-7 required. §8 for primary screens only. §5 can be abbreviated.
T3: All sections required. §5 should be comprehensive. §8 for every screen.
-->
```

- [ ] **Step 3: Commit**

```bash
git add docs/paf/templates/functional-processes.md docs/paf/templates/ui-design.md
git commit -m "docs(paf): add Layer 1 templates (functional-processes, ui-design)"
```

---

### Task 5: Layer 2 Templates (Frontend, Backend, Security)

**Files:**
- Create: `docs/paf/templates/frontend-architecture.md`
- Create: `docs/paf/templates/backend-architecture.md`
- Create: `docs/paf/templates/security-architecture.md`

- [ ] **Step 1: Create frontend-architecture template**

```markdown
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
```

- [ ] **Step 2: Create backend-architecture template**

```markdown
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
```

- [ ] **Step 3: Create security-architecture template**

```markdown
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
```

- [ ] **Step 4: Commit**

```bash
git add docs/paf/templates/frontend-architecture.md docs/paf/templates/backend-architecture.md docs/paf/templates/security-architecture.md
git commit -m "docs(paf): add Layer 2 templates (frontend, backend, security)"
```

---

## Chunk 3: Remaining Templates & Examples

### Task 6: Layer 3–4 & Meta Templates

**Files:**
- Create: `docs/paf/templates/api-specification.md`
- Create: `docs/paf/templates/operational.md`
- Create: `docs/paf/templates/decision-log.md`
- Create: `docs/paf/templates/completeness-report.md`
- Create: `docs/paf/templates/design-library.md`
- Create: `docs/paf/templates/implementation-plan.md`

- [ ] **Step 1: Create api-specification template**

```markdown
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
```

- [ ] **Step 2: Create operational template**

```markdown
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
```

- [ ] **Step 3: Create decision-log template**

```markdown
---
title: "{Product} Decision Log"
domain: "{domain}"
category: decision-log
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}"]
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

<!-- USAGE NOTES:
- Number decisions sequentially (D-1, D-2, D-3...)
- Always include "Distribute To" — this is what makes decisions traceable
- The Version Gate field prevents scope creep by explicitly deferring features
- Reference decisions from target documents: "Per D-7, we use..."
- New decisions discovered during P3 authoring are added here too
- Decisions made during P6 execution (Design Feedback Loop) also go here
-->
```

- [ ] **Step 4: Create completeness-report template**

```markdown
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
```

- [ ] **Step 5: Create design-library template**

```markdown
---
title: "{Product} Design Document Library"
domain: "{domain}"
category: design-library
status: draft
version: "0.1"
date: {date}
parent: ~
depends_on: []
tags: [governance, standards, metadata]
---

# {Product} Design Document Library

<!-- T3 only. Defines document governance for the corpus. -->

## 1. Metadata Standard

<!-- Copy the YAML frontmatter spec from PAF-Document-Taxonomy.md, adapted
     for this product's domain values and any custom categories. -->

## 2. Naming Convention

```
{Product}-[{Domain}-]<Descriptive-Name>.md
```

| Component | Rule | Example |
|-----------|------|---------|
| Prefix | Always `{Product}-` | `{Product}-` |
| Domain tag | `{Domain}-` for secondary domains. Omitted for primary/shared. | `{Product}-{Domain}-Architecture.md` |
| Name | PascalCase with hyphens | `Frontend-Architecture.md` |

## 3. Document Dependency Graph

```
{Product}-Architecture.md (root)
├── {Product}-Functional-Processes.md
├── {Product}-UI-Design.md
│   ...
```

### Cross-Dependencies
| Document | Dependencies Beyond Parent |
|----------|--------------------------|
| {doc} | {list} |

## 4. Document Index

| File | Domain | Category | Status |
|------|--------|----------|--------|
| {filename} | {domain} | {category} | {status} |

## 5. Rules for New Documents

1. Frontmatter first
2. Register in this index
3. Declare dependencies
4. Follow naming convention
5. Start as draft
```

- [ ] **Step 6: Create implementation-plan template**

```markdown
---
title: "{Product} Implementation Plan"
domain: "{domain}"
category: implementation-plan
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: []
tags: [planning, execution]
---

# {Product} Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development
> (if subagents available) or superpowers:executing-plans to implement this plan.
> Steps use checkbox syntax for tracking.

**Goal:** {One sentence describing what this builds}

**Architecture:** {2-3 sentences about approach}

**Tech Stack:** {Key technologies/libraries}

---

## Module Dependency Graph

```
{Layer 0 components}
  → {Layer 1 components}
    → {Layer 2 components}
      → ...
```

---

## Phase 1: {Name}

### Task 1-1: {Component}

**Files:**
- Create: `{path}`
- Modify: `{path}`
- Test: `{path}`

**Ref:** {Design-Doc §N.N}

- [ ] **Step 1: Write the failing test**
- [ ] **Step 2: Run test to verify it fails**
- [ ] **Step 3: Write minimal implementation**
- [ ] **Step 4: Run test to verify it passes**
- [ ] **Step 5: Commit**

<!-- Repeat tasks for each component in this phase -->

---

## Estimation

### Code Volume
| Component | LOC Range |
|-----------|-----------|

### Effort
| Team Size | Duration | Cost Range |
|-----------|----------|------------|

---

<!-- PLAN CONVENTIONS:
- Every task references a design document section (Ref: field)
- Every step has exact file paths and expected output
- TDD by default: test → fail → implement → pass → commit
- Each task is one commit
-->
```

- [ ] **Step 7: Commit all templates**

```bash
git add docs/paf/templates/api-specification.md docs/paf/templates/operational.md docs/paf/templates/decision-log.md docs/paf/templates/completeness-report.md docs/paf/templates/design-library.md docs/paf/templates/implementation-plan.md
git commit -m "docs(paf): add Layer 3-4 and meta document templates"
```

---

### Task 7: Proof-Case Example Stubs

**Files:**
- Create: `docs/paf/examples/itsm-platform/README.md`
- Create: `docs/paf/examples/itsm-platform/intake-brief.md`
- Create: `docs/paf/examples/itsm-platform/decision-record.md`
- Create: `docs/paf/examples/itsm-platform/document-index.md`

**Context:** These are not full copies of the source project's documents — they're concise reference stubs that show what each PAF phase produced, with pointers to the actual documents in `docs/design/`.

- [ ] **Step 1: Create README**

```markdown
# ITSM Platform — PAF Reference Example

An enterprise agentic AI gateway is the first product designed using PAF.
It serves as the reference implementation showing what each phase produces.

## What PAF Produced

| Phase | Artifact | Location |
|-------|----------|----------|
| P1 | Intake Brief | [intake-brief.md](intake-brief.md) |
| P2 | Decision Record | [decision-record.md](decision-record.md) (sample entries) |
| P3 | Design Corpus | [document-index.md](document-index.md) (22 documents) |
| P4 | Completeness Report | `docs/design/DESIGN-COMPLETENESS-REPORT-v2.md` |
| P5 | Implementation Plan | (internal to source project) |
| P5 | Effort Estimation | (internal to source project) |

## Complexity Assessment

**Tier:** T3 (Platform)
**Signals:** 4+ user roles, 26 UI screens, 3 data stores, 5+ integrations, multi-tenancy, full security model, 3 deployment components.

## Result

- 22 design documents (61,000+ lines)
- 2 OpenAPI 3.1 specs (155 endpoints, 138 schemas)
- 312-task implementation plan across 37 sprints
- Zero cross-document contradictions
- 64/64 design tokens consistent
```

- [ ] **Step 2: Create intake-brief stub**

```markdown
# ITSM Platform Intake Brief

**Date:** 2026-03-06
**Tier Assigned:** T3 (Platform)

## Core Identity

1. **What it does:** Enterprise agentic AI gateway for autonomous ITSM agents. Deploys within client secure environments, governed by their policies, powered by their AI infrastructure.
2. **Who it's for:** Enterprise IT operations teams (buyers: CIOs, IT Directors). Users: IT operators, auditors, platform administrators.
3. **Problem it solves:** ITSM teams drown in repetitive incident/problem work. Existing automation is brittle scripts. AI agents can handle this but need governance, audit trails, and enterprise trust.
4. **Deployment:** On-prem single binary (v1.0), modular Docker (v1.1), distributed Kubernetes (v1.2). Air-gapped capable.
5. **Commercial model:** Licensed SaaS-managed (Control Plane) + on-prem runtime (Data Plane). Tiered: Essentials, Professional, Enterprise.

## Complexity Signals

| Signal | Value | Tier Triggered |
|--------|-------|:-:|
| User roles | 4 (Admin, Operator, Auditor, Viewer) | T3 |
| UI screens | 11 (DP) + 15 (CP) = 26 | T3 |
| Data stores | 3 (SQLCipher, PostgreSQL, in-memory) | T3 |
| External integrations | 5+ (ServiceNow, Jira, Slack, Teams, Email) | T3 |
| Multi-tenancy | Yes (Control Plane) | T3 |
| Security depth | Threat model + audit + encryption + zero-knowledge | T3 |
| Deployment components | 3 binaries + 3 containers | T3 |

## Scope Boundaries

**v1.0:** Single Node, Essentials license, Tier C autonomy, 2 agent roles (Incident Manager, Problem Analyst), Email adapter, full governance + audit.
**v1.1 (future):** Modular deployment, Professional tier, Tier B, Marketplace, Slack adapter.
**v1.2 (future):** Distributed, Enterprise tier, Tier A, SAML/OIDC, Teams adapter, Firecracker sandbox.

## Decomposition

Two domains identified: **Data Plane** (client-deployed runtime) and **Control Plane** (vendor-operated SaaS). Designed as a single product with two domains, not two products — they share the same release cadence and codebase (monorepo).
```

- [ ] **Step 3: Create decision-record stub (sample entries)**

```markdown
# ITSM Platform Decision Record (Sample Entries)

The full decision record was captured in the source project (7 formal entries) plus decisions embedded throughout the design documents. Below are representative entries showing the PAF Decision Record format.

---

### D-1: What frontend framework should the product use?

**Date:** 2026-03-08

**Context:** The product UI must be embeddable in a Go binary (go:embed), work air-gapped (zero CDN), and produce a sub-300KB bundle. The design system (Precision Design) uses CSS Custom Properties.

**Options Considered:**
1. React 19 + Next.js — Largest ecosystem, most hiring pool. But: runtime overhead, SSR unnecessary (SPA embedded in binary), bundle size challenging.
2. Svelte 5 + SvelteKit — Compiler-first (zero runtime), static adapter for go:embed, smallest bundles, CSS Custom Properties native.
3. Vue 3 + Nuxt — Middle ground. Good DX but smaller component ecosystem than React.

**Decision:** Svelte 5 + SvelteKit. Compiler-first approach means no framework runtime in the bundle. Static adapter produces files directly embeddable via go:embed. CSS Custom Properties work natively without a styling framework. Bits UI provides headless accessible components.

**Distribute To:** Frontend Architecture (§1 Framework Decision), Root Architecture (§4 Technology Stack)

**Version Gate:** v1.0

---

### D-5: How should governance policies be evaluated?

**Date:** 2026-03-08

**Context:** The platform needs custom governance policies that enterprise clients can write to constrain agent behavior. Policies must be non-Turing-complete (guaranteed termination), additive-only (can restrict but never override tier rules), and evaluable without network calls.

**Options Considered:**
1. Custom DSL — Full control but requires building parser, runtime, tooling from scratch.
2. OPA/Rego — Purpose-built for policy evaluation. Non-Turing-complete by design. Embeddable as Go library. Industry standard for cloud-native policy.
3. CEL (Common Expression Language) — Google's expression language. Lighter than Rego but less powerful for complex policies.

**Decision:** OPA/Rego embedded as Go library. Rego's declarative nature matches our needs — policies are data, not code. Embedding as a library means no network hop (critical for fail-closed enforcement). The OPA ecosystem provides testing and debugging tools.

**Distribute To:** Security Architecture (§Governance Policies), Backend Architecture (§Governance Layer), API Specification (§Governance Endpoints)

**Version Gate:** v1.0

---

<!-- See the source project for all 7 formal entries -->
```

- [ ] **Step 4: Create document-index stub**

```markdown
# ITSM Platform Design Document Index

This example shows the document corpus produced during P3 for an enterprise ITSM platform. The authoritative index is maintained in `docs/design/DESIGN-LIBRARY.md` §4.

## Document Summary

| # | Document | Domain | Category | Layer | Lines |
|---|----------|--------|----------|:-----:|------:|
| 1 | Product-Design-Document.md | shared | root-architecture | 0 | ~960 |
| 2 | Product-Functional-Processes.md | data-plane | functional-processes | 1 | ~5,630 |
| 3 | Product-UI-Design-Document.md | data-plane | ui-design | 1 | ~1,510 |
| 4 | Product-Frontend-Architecture.md | data-plane | frontend-architecture | 2 | ~930 |
| 5 | Product-Security.md | data-plane | security-architecture | 2 | ~1,510 |
| 6 | Product-Backend-Architecture.md | data-plane | backend-architecture | 2 | ~2,470 |
| 7 | Product-API-Specification.md | data-plane | api-specification | 3 | ~4,150 |
| 8 | Product-Email-Adapter-Spec.md | data-plane | functional-processes | 1 | ~840 |
| 9 | Product-CP-Architecture.md | control-plane | root-architecture | 0 | — |
| 10 | Product-CP-Functional-Processes.md | control-plane | functional-processes | 1 | — |
| 11 | Product-CP-Security.md | control-plane | security-architecture | 2 | — |
| 12 | Product-CP-API-Specification.md | control-plane | api-specification | 3 | — |
| 13 | Product-CP-Backend-Architecture.md | control-plane | backend-architecture | 2 | — |
| 14 | Product-CP-Frontend-Architecture.md | control-plane | frontend-architecture | 2 | — |
| 15 | Product-CP-UI-Design.md | control-plane | ui-design | 1 | — |
| 16 | Product-Testing-Strategy.md | shared | operational | 4 | ~980 |
| 17 | Product-Observability-Strategy.md | shared | operational | 4 | ~580 |
| 18 | Product-Release-Process.md | shared | operational | 4 | ~640 |
| 19 | Product-Design-QA.md | shared | decision-log | — | — |
| 20 | DESIGN-COMPLETENESS-REPORT-v2.md | shared | completeness-report | — | — |
| 21 | DESIGN-LIBRARY.md | shared | design-library | — | ~240 |
| 22 | Product-Visual-Asset-Strategy.md | shared | brand | — | — |

## Canonical Specs (Machine-Readable)

| File | Domain | Endpoints | Schemas |
|------|--------|:---------:|:-------:|
| product-management-api.yaml | data-plane | 75 | 66 |
| product-control-api.yaml | control-plane | 80 | 72 |

## Key Metrics

- **22 documents** across 2 domains
- **61,000+ lines** total
- **Zero contradictions** verified by 8 consistency checks
- **64/64 design tokens** consistent across all UI documents
```

- [ ] **Step 5: Commit**

```bash
git add docs/paf/examples/itsm-platform/
git commit -m "docs(paf): add proof-case example stubs (intake, decisions, index)"
```

---

### Task 8: Verify Complete Plan 2

- [ ] **Step 1: Verify all files exist**

```bash
ls -R docs/paf/templates/ docs/paf/domain-packs/ docs/paf/examples/
```

Expected:
- `templates/`: 12 `.md` files
- `domain-packs/`: `_template.yaml`, `itsm.yaml`
- `examples/itsm-platform/`: `README.md`, `intake-brief.md`, `decision-record.md`, `document-index.md`

- [ ] **Step 2: Verify template placeholders are consistent**

All templates use `{Product}`, `{domain}`, `{date}` as placeholder variables. No template references a specific product name.

- [ ] **Step 3: Final log check**

```bash
git log --oneline -8
```
