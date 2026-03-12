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
