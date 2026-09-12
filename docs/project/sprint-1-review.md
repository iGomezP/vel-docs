# Sprint 1 Review — VEL Workbench Foundation

## Sprint Goal

Establish the initial foundation for VEL Workbench as the internal work management platform for Vakzor Enterprise Lab.

## Outcome

Sprint 1 was completed successfully.

The sprint established the initial technical and product foundation for VEL Workbench using a modular monolith architecture, PostgreSQL persistence, vertical slices, automated .NET tests, and a version-controlled HTTP API test suite.

All planned Sprint 1 work items were completed.

## Completed Work

- VW-001 — Repository bootstrap
- VW-002 — Initial domain model
- VW-003 — .NET API skeleton
- VW-004 — Next.js frontend skeleton
- VW-005 — PostgreSQL schema draft
- VW-006 — Configure EF Core and PostgreSQL persistence
- VW-007 — Implement Project creation vertical slice
- VW-008 — Add version-controlled API test suite with Bruno
- VW-009 — Implement Get Project by ID vertical slice

## Delivered Capabilities

### Repository and Solution Foundation

VEL Workbench now has an initial repository structure for:

- Backend services
- Frontend application
- Infrastructure assets
- Automated tests
- API tests
- Documentation

The backend targets .NET 10 and uses a solution organized around domain, application, contracts, infrastructure, API, and test projects.

### Frontend Foundation

The frontend was bootstrapped using Next.js with a feature-first structure.

The initial organization includes feature areas for:

- Projects
- Work Items
- Sprints
- Activity

Shared components, utilities, and types are kept separate from feature-specific code.

### Persistence

PostgreSQL was selected as the source of truth for VEL Workbench.

Entity Framework Core migrations are the authority for schema evolution.

The initial schema includes the core data structures required for:

- Projects
- Work Items
- Work Item States
- Labels
- Sprints
- Comments
- Activity Events
- Evidence Links

Project records include a `next_work_item_number` field intended to support concurrency-safe human-readable Work Item identifiers in future vertical slices.

### Project Creation Vertical Slice

VEL Workbench supports Project creation through the HTTP API.

The implementation includes:

- Request contract
- Application command
- Application handler
- Domain validation
- Repository abstraction
- PostgreSQL persistence
- HTTP endpoint
- Conflict handling
- Automated tests

The endpoint returns:

- `201 Created` for successful creation
- `400 Bad Request` for invalid Project data
- `409 Conflict` for duplicate Project keys

Project identifiers use UUIDv7.

### Project Retrieval Vertical Slice

VEL Workbench supports retrieving a Project by ID.

The implementation includes:

- Application query
- Query handler
- Repository lookup
- HTTP endpoint
- Automated tests

The endpoint returns:

- `200 OK` when the Project exists
- `404 Not Found` when a valid Project identifier is unknown
- `404 Not Found` at the routing layer when the route identifier is invalid

### Automated Testing

The backend currently has:

```text
17 automated .NET tests
17 passing
0 failing
```

The solution builds successfully.

### API Testing

Bruno was introduced as the version-controlled HTTP API testing tool for VEL Workbench.

The Project API suite currently covers:

- Successful Project creation
- Duplicate Project key rejection
- Invalid Project data rejection
- Successful Project retrieval
- Unknown Project ID
- Invalid Project route identifier

Current result:

```text
6 requests
14 assertions
0 failures
```

The suite uses runtime-generated Project keys and runtime variables to support repeatable execution without hardcoded database identifiers.

## Engineering Decisions Reinforced

Sprint 1 reinforced the following engineering decisions:

- Modular monolith before distributed architecture
- PostgreSQL as the source of truth
- EF Core migrations as schema authority
- Vertical slices for application features
- Domain validation close to the domain model
- UUIDv7 for entity identifiers
- Version-controlled API tests
- No private infrastructure addresses or credentials committed to the repository
- Avoid adding infrastructure before a concrete requirement exists

## Scope Deliberately Excluded

The following areas remain outside the current MVP scope:

- Authentication
- Authorization
- Multi-tenancy
- Custom workflow designer
- Advanced reporting
- Notifications
- AI capabilities
- Mobile applications
- Realtime collaboration
- File uploads
- External integrations

These were intentionally excluded to keep the initial Workbench foundation small and evolvable.

## Evidence

Relevant VEL Workbench commits include:

```text
09b07ed feat: implement project creation vertical slice
caa5500 feat: implement project retrieval by id
a3844a9 test: bootstrap Bruno API test suite
c361d4e test: add Project API scenarios to Bruno suite
2b2ebd5 docs: document Bruno API test workflow
```

At Sprint 1 completion:

```text
VEL Workbench repository: clean
VEL Workbench main branch: synchronized with origin/main
.NET build: successful
.NET tests: 17/17 passing
Bruno requests: 6/6 passing
Bruno assertions: 14/14 passing
```

## Sprint Result

Sprint 1 achieved its goal.

VEL Workbench now has a working engineering foundation and the first complete Project workflows implemented through the HTTP boundary.

The platform is ready to move from foundation work into the first usable work management workflows in Sprint 2.
