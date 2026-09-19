# Sprint 2 Review — Project & Work Item Management

## Sprint Goal

Deliver the first usable work management workflow in VEL Workbench
by allowing users to browse Projects and create, retrieve, and list
Work Items within a Project.

## Sprint Outcome

The core scope and stretch goal were implemented.

Eight work items were completed:

- VW-010 — Standardize repository line endings
- VW-011 — Implement List Projects vertical slice
- VW-012 — Implement concurrency-safe Work Item numbering
- VW-013 — Implement Create Work Item vertical slice
- VW-014 — Implement Get Work Item by ID vertical slice
- VW-015 — Implement List Work Items by Project vertical slice
- VW-016 — Extend Bruno Work Item API suite
- VW-017 — Implement initial Project browser in frontend

## Delivered Capabilities

### 1. Repository Consistency

Standardized repository line endings to improve cross-platform
development between Fedora and Windows.

### 2. Project Discovery

Implemented:

`GET /projects`

Projects can be retrieved through the API and displayed in the frontend.

### 3. Concurrency-Safe Work Item Numbering

Implemented PostgreSQL-backed Project-scoped number allocation.

The implementation uses an atomic database operation rather than
calculating the next number with MAX(number) + 1.

Number allocation and Work Item persistence participate in the same
transaction.

### 4. Work Item Creation

Implemented:

`POST /projects/{projectId}/work-items`

The implementation includes:

- Domain validation.
- UUIDv7 identifiers.
- Project-scoped numbering.
- Human-readable Work Item codes.
- EF Core persistence and migrations.
- Transactional number allocation and persistence.
- Application and domain tests.
- Integration testing for transaction rollback.

### 5. Work Item Retrieval

Implemented:

`GET /work-items/{id}`

Introduced a read-side abstraction to retrieve Work Item data
together with its Project key.

The API can construct human-readable Work Item codes without
adding presentation-specific data to the domain entity.

### 6. Project Work Item Listing

Implemented:

`GET /projects/{projectId}/work-items`

The endpoint provides:

- Project-scoped results.
- Deterministic ordering by Work Item number.
- An empty collection for an existing Project without Work Items.
- Not Found responses for unknown Projects.

### 7. API Regression Testing

Extended the Bruno collection with Work Item scenarios covering:

- Successful creation.
- Invalid creation data.
- Unknown Project.
- Successful retrieval.
- Unknown Work Item.
- Project-scoped listing.

Runtime variables connect Project creation to Work Item scenarios.

The existing Project scenarios remained part of the regression suite.

### 8. Initial Project Browser

Implemented the first user-facing workflow in Next.js.

Delivered:

- Real API integration.
- Project cards displaying key, name, description, and status.
- Loading, empty, success, and error states.
- Error recovery.
- Shared application sidebar.
- Navigation between Overview and Projects.
- Dynamic Project count in Overview.
- Configurable API base URL.

The frontend no longer depends on hardcoded Project data.

## Architecture Decisions

### Read and Write Separation

Write operations use domain entities and repository abstractions.

Read operations can use query-specific projections to retrieve
the data required by API consumers.

This separation does not require introducing a full CQRS framework.

### Transactional Consistency

Work Item numbering and creation share the same database
transaction to prevent numbers from being permanently consumed
by failed creation operations.

### Server-Side Rendering

The initial Project browser uses Next.js Server Components
to retrieve data from the Workbench API.

The shared application shell is owned by the root layout.

### API Tooling

Scalar provides interactive API documentation during development.

Bruno provides version-controlled API regression scenarios.

## Validation Evidence

### Backend

- Full .NET build completed successfully during VW-015 validation.
- The .NET suite reached 40 passing tests.
- Create, Get, and List Work Item endpoints were manually validated.
- Transaction rollback behavior was covered by an integration test.

### API Regression

- Work Item Bruno scenarios passed.
- Project and Work Item workflows were exercised together.

### Frontend

- TypeScript validation completed successfully.
- The Project browser displayed real Project data.
- The Overview displayed the corresponding dynamic Project count.
- API failure and recovery were exercised.
- The sidebar remained visible during navigation and loading.

## Git Evidence

The implementation is traceable through the following commits
in the `vel-workbench` repository.

| Work Item | Commit    | Description                                    |
| --------- | --------- | ---------------------------------------------- |
| VW-010    | `573bf1e` | Standardize repository line endings            |
| VW-011    | `2c07210` | Implement Project listing                      |
| VW-012    | `9cc8e12` | Implement concurrency-safe Work Item numbering |
| VW-013    | `4cdc757` | Implement Work Item creation                   |
| VW-014    | `11bb751` | Implement Work Item retrieval by ID            |
| VW-015    | `59d6239` | Implement Project Work Item listing            |
| VW-016    | `595ebc0` | Add Work Item API regression scenarios         |
| VW-017    | `a20c17e` | Implement initial Project browser              |

Additional technical improvements:

- `98e1e9a` — Add Scalar API documentation.

All listed commits were pushed to the remote `main` branch.

## Scope Exclusions

The following capabilities were not part of Sprint 2:

- Work Item updates and deletion.
- Sprint management workflows.
- Authentication and authorization.
- Advanced search and filtering.
- Pagination.
- Production deployment.

## Sprint Goal Assessment

The requested Project browsing and Work Item management
read/create workflows were delivered.

The stretch goal was also implemented, establishing the first
user-facing integration between PostgreSQL, the .NET API,
and Next.js.

## Quality Gaps and Future Improvements

### Frontend Automated Testing

The initial Project Browser was validated using TypeScript checks,
a production build, and manual browser verification.

However, automated frontend end-to-end tests were not implemented
during Sprint 2.

A future backlog item will introduce Playwright to validate:

- Navigation between Overview and Projects.
- Project rendering and dynamic counts.
- Loading, empty, success, and error states.
- Recovery from API failures.
- Integration with CI.

This work remains outside the completed Sprint 2 scope.

## Follow-Up

- Conduct the Sprint 2 Retrospective.
- Plan Sprint 3 using the remaining product backlog.
- Address UI/UX refinement as a separate work item.
- Keep MCP integration research separate from the completed
  Sprint 2 scope.
