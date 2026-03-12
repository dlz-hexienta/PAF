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
