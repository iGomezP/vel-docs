# VEL Inception Journal - 2026-07-01

## Context

Vakzor Enterprise Lab started as a personal homelab project and evolved into a structured engineering platform.

The original goal was to build and learn with real infrastructure services such as Docker, databases, messaging, observability, DevOps, and backend APIs.

During the initial work, it became clear that the project needed more than tools and containers. It needed an operating model.

VEL was created to transform the homelab into a production-like engineering environment with documentation, standards, decisions, work tracking, runbooks, and continuous learning.

---

## Name

Official name:

```text
Vakzor Enterprise Lab
```

Acronym:

```text
VEL
```

Technical naming convention:

```text
vel-
vel_
```

Examples:

```text
vel-docs
vel-api
vel-platform
vel-infrastructure
```

---

## Vision

VEL is a production-like engineering platform for learning, experimentation, architecture, DevOps, observability, security, cloud, networking, backend engineering, and artificial intelligence.

The goal is not only to install tools, but to understand how professional engineering platforms are designed, operated, documented, and improved.

---

## Mission

Build a self-hosted engineering platform that supports:

- Backend engineering.
- Infrastructure operations.
- DevOps workflows.
- Observability.
- Security practices.
- Cloud readiness.
- AI experimentation.
- Architecture documentation.
- Portfolio evidence.
- Interview preparation.
- Continuous learning.

---

## Operating Principles

VEL will follow these principles:

- Every important decision must leave evidence.
- Documentation is part of the product.
- Work should be tracked.
- Runbooks are required for operational services.
- Architecture decisions should be captured in ADRs.
- Research should be captured in RFCs.
- Generated artifacts should not be committed.
- Public documentation should avoid exposing private infrastructure details.
- Infrastructure should become reproducible over time.
- Learning should be intentional and documented.

---

## Documentation Strategy

VEL documentation will use Docs as Code.

The documentation platform is based on:

```text
Markdown
MkDocs
Material for MkDocs
Git
GitHub
```

Repository:

```text
vel-docs
```

The documentation repository is the source of truth for:

- Engineering standards.
- Engineering principles.
- Roadmap.
- Platform capabilities.
- Architecture decisions.
- Research documents.
- Knowledge base.
- Runbooks.
- Engineering journal.

---

## Work Management Strategy

VEL will use a project management platform to track engineering work.

Current candidate:

```text
Plane
```

Current status:

```text
Under evaluation
```

Reason:

Plane works for work items, labels, states, documentation pages, views, and cycles, but some advanced features such as epics or initiatives may be limited depending on edition or plan.

VEL will evaluate project management tools before making a final platform decision.

Expected output:

```text
ADR-0002 — Project Management Platform Selection
```

Candidate tools:

- Plane
- OpenProject
- Taiga
- GitHub Projects

---

## Initial Folder Structure

The initial VEL folder structure is:

```text
~/vel
├── diagrams
├── infrastructure
├── repositories
├── runbooks
├── temp
└── tools
```

Purpose of each folder:

| Folder | Purpose |
|--------|---------|
| `diagrams` | Architecture diagrams, Draw.io files, exported diagrams |
| `infrastructure` | Future infrastructure as code, compose files, automation |
| `repositories` | Git repositories used by VEL |
| `runbooks` | Operational procedures outside repo when needed |
| `temp` | Temporary files and experiments |
| `tools` | CLI tools, binaries, helper scripts |

The main documentation repository is:

```text
~/vel/repositories/vel-docs
```

---

## Initial Documentation Structure

The initial `vel-docs` structure includes:

```text
docs
├── adr
├── capabilities
├── knowledge
├── project
├── research
└── runbooks
```

Purpose of each documentation section:

| Section | Purpose |
|---------|---------|
| `adr` | Architecture Decision Records |
| `capabilities` | Platform capability documentation |
| `knowledge` | Learning notes and technical knowledge base |
| `project` | Standards, principles, roadmap, health, journal |
| `research` | RFCs and tool evaluations |
| `runbooks` | Operational procedures |

---

## Initial Platform Capabilities

VEL will be organized around platform capabilities instead of isolated tools.

Initial capabilities include:

| Capability | Example Implementation |
|-----------|------------------------|
| Documentation Platform | MkDocs Material |
| Project Management Platform | Plane / Under Evaluation |
| Source Control Platform | GitHub |
| Container Platform | Docker |
| Database Platform | PostgreSQL / SQL Server / MongoDB |
| Cache Platform | Redis |
| Object Storage Platform | MinIO |
| Messaging Platform | RabbitMQ |
| Observability Platform | Prometheus / Grafana / Loki / Tempo |
| Security Platform | Vault / Keycloak |
| DevOps Platform | GitHub Actions |
| Networking Platform | Reverse proxy / DNS / routing |
| AI Platform | Future local and cloud AI tooling |

---

## Initial Engineering Workflow

The expected VEL workflow is:

```text
Idea
  → Research / RFC
  → ADR if needed
  → Work Item
  → Implementation
  → Validation
  → Documentation
  → Runbook if operational
  → Commit
  → Journal
  → Review
```

This workflow is intended to make every meaningful change traceable.

---

## Initial Completed Milestones

The first completed milestones were:

- Created VEL name and identity.
- Created base folder structure.
- Created `vel-docs` repository.
- Installed MkDocs and Material for MkDocs.
- Created the documentation portal.
- Created engineering standards.
- Created engineering principles.
- Created platform capabilities.
- Created ADR structure.
- Created RFC structure.
- Created knowledge base structure.
- Created platform health page.
- Created first operational runbook.
- Created first engineering journal entry.

---

## First ADR

The first ADR was:

```text
ADR-0001 — Docs as Code
```

Decision:

```text
VEL documentation will be stored as Markdown in Git and published through MkDocs.
```

---

## First RFC

The first RFC was:

```text
RFC-0001 — Project Management Tools
```

Purpose:

```text
Evaluate project management platforms for VEL before selecting the official ALM/work tracking platform.
```

---

## First Operational Runbook

The first real operational runbook was:

```text
Epson L4360 Printer Service Runbook
```

This runbook documented a real infrastructure issue involving Fedora, CUPS, Epson drivers, Windows clients, phone printing, scanning, and lab network forwarding.

This established the expected VEL operating model:

```text
Problem
  → Investigation
  → Implementation
  → Validation
  → Documentation
  → Work Item
  → Commit
  → Journal
```

---

## Initial Sprint

Initial sprint:

```text
Sprint 0.5 — Engineering Foundation
```

Purpose:

```text
Create the engineering foundation before building more infrastructure.
```

Initial sprint goals:

- Documentation platform.
- Engineering standards.
- Engineering principles.
- ADR process.
- RFC process.
- Work tracking.
- Platform capabilities.
- Runbook structure.
- Journal structure.
- Tool evaluation process.

---

## Current Status at Inception

| Area | Status |
|------|--------|
| VEL identity | Created |
| Folder structure | Created |
| Documentation platform | Active |
| MkDocs portal | Active |
| Git repository | Active |
| ADR process | Started |
| RFC process | Started |
| Runbook process | Started |
| Journal process | Started |
| Project management platform | Under Evaluation |
| Infrastructure platform | Existing but not fully documented |
| Sprint 1 | Not started |

---

## Open Decisions

The following decisions are still open:

- Final project management platform.
- Reverse proxy platform.
- DNS strategy.
- TLS/certificate strategy.
- Network routing strategy between home and lab networks.
- Observability standard.
- Security platform architecture.
- AI platform architecture.
- Infrastructure as Code strategy.

---

## Next Strategic Step

The next strategic step is:

```text
[VF-002] Evaluate Project Management Platforms
```

Expected output:

```text
ADR-0002 — Project Management Platform Selection
```

This decision should be completed before starting Sprint 1.

---

## Inception Summary

VEL began as a homelab, but it is now being shaped into a professional engineering platform.

The purpose is not to accumulate tools.

The purpose is to build engineering capability.

VEL should become:

- A learning environment.
- A portfolio.
- A platform engineering lab.
- A DevOps practice environment.
- A backend architecture playground.
- A place to document decisions and lessons.
- A long-term system that grows with discipline.

The project officially starts with this operating belief:

```text
Every engineering decision must leave evidence.
```
