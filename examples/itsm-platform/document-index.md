# ITSM Platform Design Document Index

This example shows the document corpus produced during P3 for a T3 enterprise ITSM platform with two architectural domains (Data Plane + Control Plane).

## Document Summary

| # | Document | Domain | Category | Layer | Lines |
|---|----------|--------|----------|:-----:|------:|
| 1 | Product-Architecture.md | shared | root-architecture | 0 | ~960 |
| 2 | Product-Functional-Processes.md | data-plane | functional-processes | 1 | ~5,630 |
| 3 | Product-UI-Design.md | data-plane | ui-design | 1 | ~1,510 |
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
| 20 | Design-Completeness-Report.md | shared | completeness-report | — | — |
| 21 | Design-Library.md | shared | design-library | — | ~240 |
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
