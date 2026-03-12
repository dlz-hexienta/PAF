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

- P2 exit gate passed (decision log exists, requirements captured)
- Know the assigned tier and matched domain pack (if any)

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
5. Save the scaffold to the product's design directory

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

### 6. Multi-Domain Authoring (T3)

If the product has multiple domains:
1. Author the primary domain's documents first (Layers 0–4)
2. Author secondary domain documents, referencing primary domain mirrors
3. Author shared/cross-cutting documents last
4. Maintain a cross-domain dependency table

### 7. Exit Gate Checklist

- [ ] All required documents for the assigned tier exist at ≥ `draft`
- [ ] All `depends_on` fields populated
- [ ] All P2 decisions distributed to their target documents
- [ ] Shared contracts identified and noted (for P4)
- [ ] Decision Log updated with any P3 decisions
- [ ] User has reviewed and approved each document/layer

### 8. Handoff

Announce: "P3 complete. Invoking paf-qa for Phase 4."
Invoke the `paf-qa` skill.

## Related Skills

Works well with: `paf-requirements`, `paf-qa`
