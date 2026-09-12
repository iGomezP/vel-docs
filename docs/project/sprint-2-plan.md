# Sprint 2 Plan — Project & Work Item Management

## Sprint Goal

Deliver the first usable work management workflow in VEL Workbench by allowing users to browse Projects and create, retrieve, and list Work Items within a Project.

## Context

Sprint 1 established the VEL Workbench engineering foundation and delivered the first Project API vertical slices.

Sprint 2 moves the product from foundation work toward an initial usable work management workflow.

The sprint focuses on Project discovery, Work Item creation and retrieval, Project-scoped Work Item numbering, HTTP validation, and potentially the first frontend integration.

## Core Scope

### VW-010 — Standardize Repository Line Endings

Define an explicit repository-level line-ending policy for consistent development across Windows and Linux environments.

This addresses the LF/CRLF warnings observed during Sprint 1.

### VW-011 — Implement List Projects Vertical Slice

Provide the read workflow required to retrieve available Projects through the HTTP API.

Expected capability:

```text
GET /projects
```

### VW-012 — Implement Concurrency-Safe Work Item Numbering

Implement atomic allocation of Project-scoped Work Item numbers using the existing:

```text
projects.next_work_item_number
```

Human-readable Work Item identifiers will combine the Project key and allocated number:

```text
VEL-1
VEL-2
VEL-3
```

The implementation must not derive identifiers using `MAX(number) + 1`.

### VW-013 — Implement Create Work Item Vertical Slice

Allow creation of a Work Item within an existing Project.

Expected capability:

```text
POST /projects/{projectId}/work-items
```

Creation must atomically persist the Work Item and its allocated Project-scoped number.

### VW-014 — Implement Get Work Item by ID Vertical Slice

Provide retrieval of an individual Work Item through the HTTP API.

The final route design will be confirmed during implementation rather than treated as an accidental architectural constraint during planning.

### VW-015 — Implement List Work Items by Project Vertical Slice

Allow retrieval of the Work Items belonging to a Project.

Expected capability:

```text
GET /projects/{projectId}/work-items
```

### VW-016 — Extend Bruno Work Item API Suite

Extend the version-controlled Bruno suite to validate the Sprint 2 Work Item HTTP workflows.

The suite should remain repeatable and avoid hardcoded database identifiers.

## Stretch Scope

### VW-017 — Implement Initial Project Browser in Frontend

Establish the first real API-to-UI workflow in VEL Workbench.

The frontend should retrieve and display Projects from the backend rather than using hardcoded Project data.

This is a stretch goal. Backend design and correctness will not be compromised solely to include frontend functionality in Sprint 2.

## Dependencies

```text
VW-010

VW-011
   |
   +--------------------> VW-017

VW-012
   |
   v
VW-013
   |
   +---------> VW-014
   |
   +---------> VW-015
                  |
VW-014 -----------+-----> VW-016
VW-015 -----------+
```

VW-011 and VW-012 are independent capabilities and can conceptually be developed in parallel.

Implementation will normally proceed sequentially because VEL Workbench currently has a single primary development stream.

## Engineering Constraints

Sprint 2 will preserve the engineering principles established during Sprint 1:

- Prefer small vertical slices.
- Keep PostgreSQL as the source of truth.
- Use EF Core migrations as schema authority.
- Keep domain rules close to the domain.
- Do not introduce infrastructure without a concrete requirement.
- Use automated tests to influence design.
- Validate HTTP behavior independently through Bruno.
- Keep private infrastructure addresses and credentials outside the repository.
- Leave engineering evidence through commits and documentation.

## Deliberately Excluded

Sprint 2 does not introduce:

- Authentication or authorization.
- Multi-tenancy.
- Redis-backed application behavior.
- RabbitMQ messaging.
- Realtime collaboration.
- Notifications.
- AI capabilities.
- Work Item editing or deletion.
- Advanced search or filtering.
- Advanced reporting.
- Custom workflows.

These capabilities require concrete product requirements before additional architecture is introduced.

## Expected Sprint Outcome

At the end of Sprint 2, the core backend workflow should support:

```text
Project
   |
   +--> List Projects
   |
   +--> Create Work Item
              |
              +--> Project-scoped number
              |       e.g. VEL-1
              |
              +--> Retrieve Work Item
              |
              +--> List Project Work Items
```

The workflow should be covered by automated .NET tests and version-controlled Bruno API tests.

If the core scope is completed without compromising design quality, the frontend Project browser will establish the first complete path:

```text
PostgreSQL
    ↓
EF Core
    ↓
Application
    ↓
HTTP API
    ↓
Next.js
    ↓
User
```

## Definition of Sprint Success

Sprint 2 is successful when:

- Core Work Item workflows are operational.
- Project-scoped numbering is concurrency-safe.
- The solution builds successfully.
- Automated .NET tests pass.
- Bruno validates the new HTTP workflows.
- Repository documentation reflects important engineering decisions.
- Sprint evidence is preserved in Git and Plane.

VW-017 remains a stretch goal and is not required to declare the core Sprint 2 goal successful.
