# ITSM Platform Decision Record (Sample Entries)

The source project captured 7 formal decision entries plus decisions embedded throughout the design documents. Below are representative entries showing the PAF Decision Record format.

---

### D-1: What frontend framework should the product use?

**Date:** 2026-03-08

**Context:** The product UI must be embeddable in a Go binary (go:embed), work air-gapped (zero CDN), and produce a sub-300KB bundle. The design system uses CSS Custom Properties.

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
