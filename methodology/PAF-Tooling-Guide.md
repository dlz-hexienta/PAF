# PAF Tooling Guide

PAF is tool-agnostic in process but opinionated in recommendations. This document defines what capabilities are needed, recommends tools per capability, and maps synergies between tool pairings.

For phase procedures, see [PAF-Phase-Guide.md](PAF-Phase-Guide.md).

---

## Capabilities by Phase

| Phase | Capabilities Needed |
|-------|-------------------|
| P1 Intake | Conversational Q&A, structured data capture |
| P2 Requirements | Decision recording, domain knowledge storage, requirement categorization |
| P3 Authoring | Document generation, template scaffolding, cross-reference validation |
| P4 QA | Consistency checking, gap analysis, report generation |
| P5 Planning | Task decomposition, dependency graphing, estimation |
| P6 Execution | Code generation, testing, review, CI/CD, version control |

---

## Reference Tooling Stack

This is the concrete implementation validated by a real enterprise platform build. It targets Claude Code as the AI engine.

### Design & Authoring (P1–P5)

| Capability | Tool | Why |
|-----------|------|-----|
| AI-assisted design | Claude Code (Opus) | Powers conversational intake, document generation, quality checks |
| Skill orchestration | Superpowers skills | Chains PAF phases as executable, repeatable skills |
| Subagent dispatch | Claude Code Agent tool | Fresh context per task, parallel research |
| Version control | Git + GitHub | Document versioning, branch isolation, PR-based review |
| Document format | Markdown + YAML frontmatter | Portable, diffable, metadata-rich, tooling-friendly |

### API & Types

| Capability | Tool | Why |
|-----------|------|-----|
| API specification | OpenAPI 3.1 (YAML) | Industry standard, machine-readable, broad tooling support |
| Spec validation | Redocly CLI | Lint, bundle, validate on every PR |
| Type generation | openapi-typescript + openapi-fetch | Compile-time API type safety from spec |

### Frontend

| Capability | Tool | Why |
|-----------|------|-----|
| Framework | Svelte 5 + SvelteKit | Compiler-first, zero runtime, static adapter for embedding |
| Component library | Bits UI 2.x | Headless accessible primitives, native Svelte 5 runes |
| Icons | phosphor-svelte | Tree-shaken SVG, consistent weight system |
| Styling | CSS Custom Properties | Design system ownership, no utility framework dependency |

### Backend

| Capability | Tool | Why |
|-----------|------|-----|
| Language | Go 1.24+ | Static binaries, strong stdlib, go:embed for assets |
| Database (embedded) | SQLCipher | Encrypted SQLite, zero infrastructure, single-node |
| Database (multi-tenant) | PostgreSQL | Row-level security, proven at scale |

### Testing

| Capability | Tool | Why |
|-----------|------|-----|
| Unit testing (frontend) | Vitest (browser mode) | Real DOM, same engine as E2E |
| E2E testing | Playwright | Cross-browser, reliable selectors, screenshots |
| Unit testing (backend) | Go testing + testify | Standard Go tooling |
| API contract testing | Redocly CLI + custom Go tests | Route registration matches spec paths |

### Operations

| Capability | Tool | Why |
|-----------|------|-----|
| Containers | Docker | Reproducible builds, CI environments |
| CI/CD | GitHub Actions | Integrated with Git, free for open source |
| MCP servers | context7, docker, godoc, playwright, svelte | AI-accessible tool integrations |

---

## Synergy Map

Tools aren't chosen in isolation. The highest-value tool decisions are **pairings** where the combination is more valuable than the sum.

| Pair | Synergy Effect |
|------|---------------|
| **OpenAPI + openapi-typescript** | Change the YAML spec → regenerate types → compiler catches every frontend mismatch automatically |
| **OpenAPI + Redocly CLI** | Every PR touching the spec gets lint-checked. Broken refs, missing descriptions caught before merge |
| **Svelte 5 + Bits UI** | Both use runes (`$props()`, `$state()`). No adapter layer, no framework mismatch, minimal bundle |
| **Go + go:embed** | Frontend builds to static files, Go embeds them. Single binary serves everything — zero deployment dependencies |
| **Git worktrees + Subagents** | Each AI agent works in its own worktree. No file conflicts, true parallelism |
| **Claude Code + MCP servers** | AI can query docs (context7), run containers (docker), check language docs (godoc), test in browser (playwright) — all without leaving the conversation |
| **Vitest browser mode + Playwright** | Same browser engine for unit and E2E tests. Consistent behavior, shared utilities |
| **YAML frontmatter + Grep/Glob** | Any tool can search and filter design documents by metadata fields. Enables automated quality gates |
| **CSS Custom Properties + design tokens** | Tokens defined once in CSS variables, referenced everywhere. Runtime themeable, no build step for color changes |
| **Markdown + Git** | Documents are diffable, reviewable in PRs, blame-able. Design decisions have the same version history as code |

---

## Choosing Alternative Tools

When PAF is used with a different tech stack, select tools by capability using these criteria:

### Frontend Framework Selection

**PAF requires:**
- Component model that supports design system tokens (CSS custom properties or equivalent)
- Ability to produce static assets embeddable in a backend binary (if applicable)
- Headless component library available (for accessibility)
- Type-safe API client available (for OpenAPI integration)

**Options with synergy notes:**
| Framework | Best When | Synergy |
|-----------|-----------|---------|
| Svelte 5 + SvelteKit | Embedded/air-gapped products, single-binary deployment | static adapter + go:embed = zero deployment deps |
| React 19 + Next.js | Ecosystem breadth, large existing team | Largest component ecosystem, most hiring pool |
| Vue 3 + Nuxt | Middle ground, progressive adoption | Good DX, smaller bundle than React |

### Backend Language Selection

**PAF requires:**
- Static or easily-deployable binaries (for on-prem/embedded products)
- Strong standard library (reduce dependency count)
- Embeddable asset serving (for single-binary products)
- Good concurrency model (for real-time features)

**Options with synergy notes:**
| Language | Best When | Synergy |
|----------|-----------|---------|
| Go | Single-binary deployment, minimal runtime, cloud-native | go:embed + static builds, excellent for CLI + API |
| Rust | Maximum performance, memory safety critical | Single binary, but steeper learning curve |
| TypeScript/Node.js | Team already knows JS, SSR-heavy product | Shared language with frontend, but runtime dependency |
| Java/Kotlin | Enterprise ecosystem, Spring Boot familiarity | JVM maturity, but larger deployment footprint |

### Database Selection

**PAF requires:**
- Schema matches design documents (DDL in Backend Architecture)
- Encryption at rest (for security-conscious products)
- Migration tooling available

**Options with synergy notes:**
| Database | Best When | Synergy |
|----------|-----------|---------|
| SQLCipher | Single-node, embedded, air-gapped | Zero infrastructure, encrypted, ships with binary |
| PostgreSQL | Multi-tenant, cloud-hosted, needs RLS | Row-level security, proven at scale, rich ecosystem |
| SQLite | Single-node, simple persistence | Zero infrastructure, but no encryption by default |
| MySQL/MariaDB | Existing infrastructure, team familiarity | Widely deployed, but weaker RLS story |

---

## Domain Pack Tooling Extensions

Domain packs can recommend domain-specific tools beyond the base stack:

```yaml
# Example from domain-packs/itsm.yaml
tooling_recommendations:
  - capability: governance_policy_engine
    recommended: OPA/Rego
    rationale: "Non-Turing-complete, additive-only policies"
    synergy: "Embedded as Go library — no network hop for policy evaluation"
  - capability: audit_trail
    recommended: "Append-only table with hash chains"
    rationale: "Tamper-evident, no additional infrastructure"
    synergy: "Same DB engine as primary data store"
  - capability: sandbox_execution
    recommended: "Deno isolates (single-node) / Firecracker (distributed)"
    rationale: "Capability-based security, warm pool for latency"
    synergy: "Deno runs TypeScript/JavaScript — matches skill authoring language"
```

When the engine detects a domain pack match during P1, it includes these recommendations in the P5 tooling section.
