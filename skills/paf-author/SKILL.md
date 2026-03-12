---
name: paf-author
description: >
  PAF Phase 3: Design Authoring. Use after paf-requirements completes.
  Scaffolds and authors the design document corpus, writing documents in
  dependency layer order. Cross-references decisions from P2 and tracks
  shared contracts for P4 validation. Hands off to paf-qa.
risk: safe
source: paf
date_added: "2026-03-12"
category: product-design
tags: "paf, authoring, design-documents, scaffolding, design"
---

# PAF Design Authoring (P3)

**Methodology reference:** Read `docs/paf/methodology/PAF-Phase-Guide.md` §P3 and `docs/paf/methodology/PAF-Document-Taxonomy.md` before proceeding.

## Prerequisites

- P2 exit gate passed (decision log exists at `docs/design/decision-log.md`)
- Know the assigned tier and matched domain pack (if any)
- If a domain pack matched, load it from `docs/paf/domain-packs/{pack}.yaml`

## Procedure

### 1. Determine Document Set

Read the taxonomy table in `docs/paf/methodology/PAF-Document-Taxonomy.md`. Filter by the assigned tier to get the list of required documents.

Present the document list to the user and confirm. They may promote optional documents to required.

### 2. Scaffold Documents

For each required document, in dependency layer order:

1. Read the corresponding template from `docs/paf/templates/{category}.md`
2. Replace placeholder variables:
   - `{Product}` → product name
   - `{domain}` → domain identifier
   - `{date}` → today's date
   - `{root-architecture-filename}` → actual filename
   - Other `{filename}` references → actual filenames
3. If a domain pack matched, read its `document_section_templates` for this category and inject additional sections into the scaffold
4. Apply tier adjustments — remove sections marked for higher tiers (noted in template comments)
5. Save the scaffold to `docs/design/{product}-{document-type}.md`

### 3. Author Documents in Layer Order

Write documents following the dependency layers:

```
Layer 0: Root Architecture (always first)
Layer 1: Functional Processes, UI Design
Layer 2: Frontend Architecture, Security, Backend Architecture
Layer 3: API Specification, Canonical Specs
Layer 4: Operational (Testing, Observability, Release)
```

**A document at Layer N must reach draft before starting any Layer N+1 document.**

For each document:

**a. Fill sections** — Reference decisions from the P2 Decision Log. Cite them explicitly (e.g., "Per D-7, we use JWT with opaque API key fallback"). Each section should be traceable to a decision or requirement.

**b. Cross-reference** — Set `depends_on` in the YAML frontmatter. If this document references a concept from another document, that document is a dependency.

**c. Verify bidirectional references** — If document A depends on B, check whether B should also reference A. One-way is fine for foundational documents (Root Architecture doesn't need to list every dependent).

**d. Track shared contracts** — Note when you define something that other documents will reference:
  - Design tokens (colors, spacing) → will be referenced by Frontend Architecture
  - Database schema → will be referenced by API Specification
  - Auth model → will be referenced by API, Backend, Frontend
  - Config schema → will be referenced by Functional Processes, Operational

**e. Self-check** — Read the document end-to-end. Fix contradictions, fill placeholders, resolve undefined terms.

**f. Set status** — Mark as `draft` in the frontmatter.

### 4. Present Each Document for Review

After completing each document (or each layer), present it to the user. Ask:
- Does this look right?
- Any sections that need more detail?
- Any missing topics?

Incorporate feedback before proceeding to the next layer.

### 5. Update Decision Log

New decisions will arise during authoring. Record them in the Decision Log using the same D-{number} format with "Distribute To" targets.

### 6. Operational Document Split (T3)

Per the taxonomy, T3 products split the Operational document into 2–3 separate documents:
- `{product}-testing-strategy.md` — test pyramid, CI/CD, coverage targets, performance benchmarks
- `{product}-observability.md` — logging, metrics, tracing, alerting, SLI/SLO, dashboards
- `{product}-release-process.md` — branching, quality gates, artifact signing, deployment checklist

Scaffold each from the relevant sections of `docs/paf/templates/operational.md`. Each gets its own YAML frontmatter with `category: operational` and a distinguishing `tags` entry.

T1–T2 products use a single `{product}-operational.md` document.

### 7. Multi-Domain Authoring (T3)

If the product has multiple domains:
1. Author the primary domain's documents first (Layers 0–4)
2. Author secondary domain documents, referencing primary domain mirrors
3. Author shared/cross-cutting documents last
4. Maintain a cross-domain dependency table

### 8. Exit Gate Checklist

- [ ] All required documents for the assigned tier exist at ≥ `draft`
- [ ] All `depends_on` fields populated
- [ ] All P2 decisions distributed to their target documents
- [ ] Shared contracts identified and noted (for P4)
- [ ] Decision Log updated with any P3 decisions
- [ ] User has reviewed and approved each document/layer

### 9. Handoff

Announce: "P3 complete." Present a summary of documents authored and shared contracts identified.
Ask: "Ready to proceed to P4 (Quality Assurance)? If you need time to review, you can resume later with `/paf-qa`."
Invoke `paf-qa` only after confirmation.

## When Things Go Wrong

- **Template file missing:** Warn the user and scaffold manually from the document type description in PAF-Document-Taxonomy.md. Note the gap.
- **Decision Log references a document type that doesn't exist at this tier:** Record the decision in the most relevant existing document. Note the mismatch.
- **Contradictions found during self-check:** Fix immediately. If the contradiction stems from a P2 decision that needs revision, record a new decision (D-{next}) and update affected documents.
- **Document too large for a single authoring pass:** Split into sections. Complete Layer N sections before moving to Layer N+1. Present each section for user review.
- **Domain pack sections don't fit the product's architecture:** Skip irrelevant sections. Note which were skipped and why.

## Related Skills

Works well with: `paf-requirements`, `paf-qa`
