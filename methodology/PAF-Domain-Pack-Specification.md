# PAF Domain Pack Specification

Domain packs are PAF's learning mechanism. Each design engagement captures domain-specific knowledge — questions, constraints, terminology, quality checks — and packages it for reuse in future engagements.

This document defines the domain pack schema, capture process, versioning, and how packs integrate with each PAF phase.

---

## What Domain Packs Contain

A domain pack encodes knowledge that is **specific to a vertical or problem domain** but **generic across products in that domain**. Examples:

- An ITSM pack knows to ask about ITIL alignment, change authority models, and autonomy tiers
- A fintech pack knows to ask about regulatory compliance, transaction auditing, and PCI-DSS
- An IoT pack knows to ask about device provisioning, firmware updates, and edge computing

Domain packs do NOT contain:
- Product-specific decisions (those belong in the Decision Log)
- Code or implementation details (those belong in the implementation plan)
- Tool configurations (those belong in the Tooling Guide)

---

## Schema

Domain packs are YAML files following this schema:

```yaml
# domain-packs/{domain-name}.yaml

name: string                    # Human-readable domain name
version: "x.y"                 # Semantic version, quoted
captured_from: string           # Product name where this knowledge was first captured
date: YYYY-MM-DD               # Date of last update
description: string             # One-paragraph description of the domain

# P1: Additional intake questions for this domain
intake_questions:
  - "Question text?"
  - "Question text with context?"

# P2: Additional requirement prompts by category
requirement_prompts:
  functional:
    - "Domain-specific functional question?"
  security:
    - "Domain-specific security question?"
  operational:
    - "Domain-specific operational question?"
  # ... any category from the taxonomy

# P3: Additional section templates for specific document types
document_section_templates:
  functional_processes:
    - "Section heading or topic to include"
    - "Another section heading"
  backend_architecture:
    - "Domain-specific backend section"

# P4: Additional quality checks beyond the standard gates
quality_checks:
  - description: "What to check"
    documents: ["doc-type-1", "doc-type-2"]
    check: "What must be consistent"

# Domain glossary — terms with specific meaning in this domain
terminology:
  - term: "Term Name"
    definition: "What it means in this domain"
  - term: "Another Term"
    definition: "Definition"

# P5/P6: Domain-specific tooling recommendations
tooling_recommendations:
  - capability: "what_it_does"
    recommended: "Tool Name"
    rationale: "Why this tool for this domain"
    synergy: "How it pairs with other tools"
```

All fields except `name`, `version`, and `date` are optional. A domain pack can start with just intake questions and grow over time.

---

## Capture Process

Domain knowledge is captured at two points in the PAF lifecycle:

### During P2 (Requirements & Decisions)

As the practitioner works through requirement categories, the engine (or human) watches for patterns that would apply to other products in the same vertical:

1. **Questions that surprised** — questions the practitioner hadn't considered but proved important
2. **Constraints that recurred** — regulatory, operational, or architectural constraints specific to the domain
3. **Terminology that needed definition** — domain-specific terms that caused confusion
4. **Architectural patterns that emerged** — patterns like "governance as separate process" or "hash-chained audit trail" that are domain-typical

At the end of P2, the captured knowledge is assembled into a draft domain pack and presented for review.

### During P6 (Execution & Delivery)

As the product is built, the team discovers things the design missed or got wrong. Some of these are product-specific, but others are domain-general:

1. **Quality checks that would have caught issues earlier** — add to `quality_checks`
2. **Tooling that proved essential for the domain** — add to `tooling_recommendations`
3. **New terminology that emerged during implementation** — add to `terminology`
4. **Document sections that were missing from templates** — add to `document_section_templates`

At the end of P6, the domain pack is updated and versioned.

---

## Versioning

Domain packs use semantic versioning:

- **Patch (x.y.Z):** Typo fixes, clarification of existing entries
- **Minor (x.Y.0):** New questions, checks, or terminology added
- **Major (X.0.0):** Structural changes, removed entries, changed semantics

Version history is tracked via git (the pack files live in the repository).

---

## Integration with PAF Phases

### P1: Intake & Scoping

The engine matches the product description against known domain packs using keyword signals. If a match is found:
1. Load the pack's `intake_questions`
2. Append them to the standard intake questionnaire (after the complexity signals)
3. Note the matched domain pack in the intake brief

**Matching heuristics:**
- Keywords in the product description
- Industry vertical mentioned
- Similar complexity signals to the pack's `captured_from` product
- Explicit user selection ("this is an ITSM product")

### P2: Requirements & Decisions

If a domain pack matched:
1. Load `requirement_prompts` for each category
2. Inject domain-specific questions into the progressive depth exploration
3. Pre-populate the terminology glossary for consistency checking

### P3: Design Authoring

If a domain pack matched:
1. Load `document_section_templates`
2. Inject domain-specific sections into document scaffolds
3. Reference terminology definitions in section templates

### P4: Quality Assurance

If a domain pack matched:
1. Load `quality_checks`
2. Add them to the G5 (Cross-Document Consistency) check suite
3. Run domain-specific checks alongside standard checks

### P5: Implementation Planning

If a domain pack matched:
1. Load `tooling_recommendations`
2. Include domain-specific tools in the plan's tooling section
3. Note synergies with the base stack

### P6: Execution & Delivery

1. Capture new domain knowledge throughout execution
2. At the end, update the domain pack
3. Increment version

---

## Validation

Before committing or sharing a domain pack, verify the following:

**Schema compliance:**
- [ ] `name`, `version`, and `date` fields are present and non-empty
- [ ] `version` is a quoted semver string (e.g., `"1.0"`, `"0.3"`)
- [ ] `date` is valid ISO 8601 (YYYY-MM-DD)
- [ ] YAML parses without error

**Content quality:**
- [ ] `intake_questions` entries are phrased as questions (end with `?`)
- [ ] `requirement_prompts` entries are organized under valid category keys (functional, security, operational, etc.)
- [ ] `quality_checks` entries have `description`, `documents`, and `check` fields, and checks are falsifiable (pass/fail, not subjective)
- [ ] `terminology` entries have both `term` and `definition` fields

**Anonymization (required before sharing/committing):**
- [ ] `captured_from` does not contain client names — use generic descriptions (e.g., "enterprise ITSM platform")
- [ ] No entries contain proprietary product names, internal codenames, or client-specific details
- [ ] Terminology definitions describe domain-general concepts, not product-specific implementations
- [ ] Quality checks reference document types, not specific document filenames

**Completeness (optional but recommended):**
- [ ] At least 3 intake questions that surface domain-specific complexity
- [ ] Requirement prompts cover at least 2 categories
- [ ] At least 1 quality check that catches a domain-specific consistency issue
- [ ] At least 3 terminology entries for terms that have domain-specific meaning

---

## Template

An empty domain pack template is provided at `domain-packs/_template.yaml`:

```yaml
name: ""
version: "0.1"
captured_from: ""
date: YYYY-MM-DD
description: ""

intake_questions: []

requirement_prompts: {}

document_section_templates: {}

quality_checks: []

terminology: []

tooling_recommendations: []
```

---

## Example: ITSM Domain Pack

The first domain pack, captured from an enterprise ITSM platform design process:

```yaml
name: IT Service Management
version: "1.0"
captured_from: enterprise ITSM platform
date: 2026-03-12
description: >
  Knowledge pack for AI-powered ITSM products — autonomous agents
  that handle incidents, problems, changes, and service requests
  within enterprise IT environments.

intake_questions:
  - "Does the product align with ITIL process areas? Which ones?"
  - "What change authority model applies? (advisory / approval / autonomous)"
  - "Are there maturity-gated capabilities (features unlocked by demonstrated competence)?"
  - "Does the product need to integrate with existing ITSM tools? (ServiceNow, Jira, etc.)"
  - "Is there a human-in-the-loop requirement for high-risk actions?"

requirement_prompts:
  functional:
    - "How does agent autonomy evolve over time? Is there a tier/maturity model?"
    - "What triggers agent actions? (events, schedules, human requests, other agents)"
    - "How are agent skills/capabilities packaged and distributed?"
  security:
    - "How does the system defend against prompt injection in AI-generated actions?"
    - "Is there an immutable audit trail requirement? Append-only? Hash-chained?"
    - "Does the governance layer need to be architecturally separate from the runtime?"
  operational:
    - "What is the agent lifecycle? (install → configure → learn → operate → retire)"
    - "How does the system handle agent-to-agent communication?"
    - "What sandbox model is used for code execution? (Deno, Firecracker, containers)"

document_section_templates:
  functional_processes:
    - "Agent Lifecycle Management (install → configure → activate → learn → operate → retire)"
    - "Escalation Flows (autonomous → supervised → human-required)"
    - "Skill Hierarchy (runbooks → actions → subskills → skills → workflows → playbooks)"
  backend_architecture:
    - "Governance Layer (separate process, IPC protocol)"
    - "Sandbox Pool (warm pool, execution isolation, resource limits)"
    - "Memory System (role, organizational, episodic, collaborative, procedural)"
  security_architecture:
    - "Prompt Injection Defense (governance backstop is deterministic, not AI-dependent)"
    - "Zero-Knowledge Architecture (operator cannot access client operational data)"
    - "Action Risk Classification (observe → inform → suggest → act-low → act-high)"

quality_checks:
  - description: "Autonomy tier consistency"
    documents: ["security-architecture", "api-specification", "backend-architecture"]
    check: "Tier definitions (A/B/C or equivalent) match across all documents"
  - description: "Agent memory layer consistency"
    documents: ["root-architecture", "backend-architecture", "functional-processes"]
    check: "Memory layer names and count match everywhere"
  - description: "Governance separation enforcement"
    documents: ["backend-architecture", "security-architecture"]
    check: "Governance is documented as a separate process, not embedded in runtime"

terminology:
  - term: "Autonomy Tier"
    definition: "Classification of agent decision-making authority. Tier A = full autonomous, Tier B = supervised, Tier C = maturity-gated."
  - term: "Decision Trace"
    definition: "Immutable record of an agent's reasoning chain, stored for audit replay."
  - term: "Skill"
    definition: "A packaged unit of agent capability, installable from a marketplace."
  - term: "Governance Layer"
    definition: "Architecturally separate process that enforces policies, records audit trails, and manages human escalation."
  - term: "Warm Pool"
    definition: "Pre-initialized sandbox instances kept ready for immediate use, reducing execution latency."

tooling_recommendations:
  - capability: governance_policy_engine
    recommended: OPA/Rego
    rationale: "Non-Turing-complete, additive-only — policies can restrict but never override tier rules"
    synergy: "Embedded as Go library — no network hop for policy evaluation"
  - capability: audit_trail
    recommended: "Append-only table with hash chains"
    rationale: "Tamper-evident without external dependencies"
    synergy: "Same DB engine as primary data store"
  - capability: sandbox_execution
    recommended: "Deno isolates (single-node) / Firecracker (distributed)"
    rationale: "Capability-based security model, warm pool pattern for latency"
    synergy: "Deno runs JS/TS — matches common skill authoring language"
  - capability: channel_adapters
    recommended: "Slack API, Microsoft Graph API, SMTP/IMAP"
    rationale: "Enterprise collaboration platforms where ITSM agents interact with humans"
    synergy: "Adapter pattern — same internal interface, different external protocols"
```
