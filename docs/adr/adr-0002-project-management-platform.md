# ADR-0002 — Build VEL Workbench as the Internal Work Management Platform

## Status

Accepted

## Date

2026-08-25

## Context

Vakzor Enterprise Lab needs a way to manage engineering work, documentation, runbooks, ADRs, RFCs, sprints, commits, and delivery evidence.

During Sprint 0.5, Plane was used as a temporary work tracking tool. It helped bootstrap the lab with work items, labels, states, and basic project organization.

However, VEL is not only a homelab or a collection of installed tools. VEL is intended to become a production-like engineering platform and a long-term learning and portfolio environment.

Because of that, limitations in third-party project management tools should not only be treated as blockers. They can also become opportunities to build internal engineering products.

## Decision

VEL will use Plane temporarily as a bootstrap work tracking tool, but the long-term direction is to build VEL Workbench as the internal work management platform for Vakzor Enterprise Lab.

The working name is:

```text
VEL Workbench
```

VEL Workbench will manage:

- Projects.
- Work items.
- States.
- Labels.
- Sprints.
- Comments.
- Activity history.
- Links to documentation.
- Links to ADRs and RFCs.
- Links to runbooks.
- Links to commits.
- Delivery evidence.

The goal is not to clone Jira, Azure DevOps, Plane, or OpenProject.

The goal is to build a focused internal platform that supports the VEL engineering workflow.

## Technology Direction

Initial stack:

| Area               | Technology                   |
| ------------------ | ---------------------------- |
| Backend            | .NET                         |
| Frontend           | Next.js / React / TypeScript |
| Database           | PostgreSQL                   |
| Cache              | Redis                        |
| Messaging          | RabbitMQ                     |
| Object Storage     | MinIO                        |
| Documentation      | MkDocs                       |
| Future AI services | Python                       |
| Future CLI tooling | Go                           |

The first version will be a modular monolith.

Microservices are out of scope for the initial version.

## Learning Goals

VEL Workbench will be used to practice concepts from:

- Designing Data-Intensive Applications.
- The Pragmatic Programmer.
- Clean Architecture.
- Clean Code.
- Python.
- Go.

The project should help practice real engineering topics such as:

- Domain modeling.
- Database design.
- API design.
- Frontend architecture.
- Testing.
- CI/CD.
- Observability.
- Product development.
- Engineering documentation.

## Initial MVP

The first version should support:

- Create projects.
- Create work items.
- Update work item status.
- Assign labels.
- Group work items by sprint.
- Add comments.
- Track activity history.
- Link work items to documentation.
- Link work items to ADRs.
- Link work items to RFCs.
- Link work items to runbooks.
- Link work items to commits.
- Close work items with evidence.

## Out of Scope

The following items are out of scope for the first version:

- Full Jira replacement.
- Complex permission model.
- Multi-tenant SaaS.
- Mobile application.
- Advanced reporting.
- AI automation.
- Microservices.
- Public user registration.
- Complex workflow designer.

## Consequences

### Positive

- VEL gains a real product development track.
- The project becomes stronger as portfolio evidence.
- The tool will be designed around VEL's real workflow.
- VEL avoids being blocked by third-party tool limitations.
- The platform creates practical experience across backend, frontend, data, DevOps, and observability.

### Negative

- Building the tool takes time.
- Plane is still needed temporarily.
- There is risk of overbuilding.
- There is risk of building features before they are needed.
- The project may distract from infrastructure work if scope is not controlled.

### Mitigation

- Start with a small MVP.
- Build only what VEL needs.
- Use Plane only as a temporary bootstrap tool.
- Keep the first version focused on work items and evidence.
- Review scope at the end of each sprint.

## Alternatives Considered

### Keep Plane as the Final Platform

Plane is useful and already available in the lab.

Rejected as the final direction because VEL can gain more learning value by building a focused internal platform.

Plane remains useful as a temporary bootstrap solution.

### Adopt OpenProject

OpenProject provides a more enterprise-oriented project management experience.

Rejected for now because it would move VEL toward tool administration instead of product development.

### Adopt Taiga

Taiga provides agile project management features.

Rejected for now because it does not provide enough additional value compared with building a focused VEL-specific platform.

### Use GitHub Projects

GitHub Projects integrates well with repositories.

Rejected as the primary platform because VEL aims to practice internal platform engineering and self-hosted operations.

## Initial Work Items

The first VEL Workbench work items should be:

```text
[VW-001] Create VEL Workbench repository
[VW-002] Define initial domain model
[VW-003] Create .NET API skeleton
[VW-004] Create PostgreSQL schema
[VW-005] Create React app skeleton
[VW-006] Implement Work Items CRUD
[VW-007] Add state transitions
[VW-008] Link work items to documentation paths
[VW-009] Add activity log
[VW-010] Add initial tests
```

## Decision Outcome

VEL will continue using Plane temporarily while building VEL Workbench.

VEL Workbench becomes the strategic internal product for work tracking and engineering evidence management.

This decision turns a tooling limitation into a product development and learning opportunity.

## Follow-up Actions

- Create the `vel-workbench` repository.
- Define the initial domain model.
- Create the .NET API skeleton.
- Create the PostgreSQL schema.
- Create the React frontend skeleton.
- Plan Sprint 1 around VEL Workbench Foundation.
- Update the VEL roadmap.
- Update the Project Management Platform capability page.
