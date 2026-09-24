# Sprint 3 Plan — Work Item Lifecycle

## Sprint Goal

Enable users to manage the Work Item lifecycle through the API
and frontend, including updates, status transitions, and
automated regression coverage.

## Context

Sprint 2 delivered the initial Project and Work Item
management capabilities.

The backend currently supports:

- Project creation, retrieval, and listing.
- Work Item creation and retrieval.
- Project-scoped Work Item listing.
- Concurrency-safe Work Item numbering.

The frontend currently supports:

- Project browsing.
- Shared application navigation.
- Dynamic Project counts.
- Loading, empty, success, and error states.

Sprint 3 builds upon this foundation by introducing Work Item
updates, status transitions, frontend workflows, and automated
end-to-end testing.

## Sprint Backlog

| ID     | Work Item                                        | Area                    |
| ------ | ------------------------------------------------ | ----------------------- |
| VW-018 | Establish frontend E2E testing with Playwright   | Frontend / Testing      |
| VW-019 | Implement Update Work Item vertical slice        | Backend                 |
| VW-020 | Implement Work Item status transitions           | Backend                 |
| VW-021 | Extend Bruno Work Item regression suite          | Backend / Testing       |
| VW-022 | Implement Work Item browser                      | Frontend                |
| VW-023 | Implement Work Item editing                      | Frontend                |
| VW-024 | Automate Work Item frontend workflows            | Frontend / Testing      |
| VW-025 | Define Work Item deletion and archival semantics | Architecture / Research |

## Delivery Strategy

### Phase 1 — Frontend Testing Foundation

VW-018 establishes Playwright and automated regression coverage
for the existing Project Browser.

The initial implementation will use deterministic HTTP
responses to avoid modifying the development database.

The test infrastructure will support local execution,
diagnostics, documentation, and GitHub Actions integration.

### Phase 2 — Backend Work Item Lifecycle

VW-019 introduces updates to Work Item titles and descriptions.

VW-020 defines and implements supported Work Item status
transitions.

Both vertical slices must preserve immutable Work Item
properties and enforce domain validation.

### Phase 3 — API Regression

VW-021 extends the existing Bruno collection.

The suite must verify successful updates, validation failures,
status transitions, and persistence through subsequent reads.

Existing API scenarios must remain green.

### Phase 4 — Frontend Workflows

VW-022 introduces Project-scoped Work Item browsing.

VW-023 introduces Work Item editing and supported status
transitions through the frontend.

The frontend must consume existing API contracts and preserve
the shared application layout.

### Phase 5 — Frontend Regression

VW-024 extends Playwright coverage to Work Item browsing,
editing, validation, and status transitions.

Tests must remain independent of development and production
database contents.

### Phase 6 — Lifecycle Architecture

VW-025 defines Work Item deletion and archival semantics.

The investigation will address:

- Historical data preservation.
- Work Item number reuse.
- Referential integrity.
- Retrieval and listing behavior.
- Restoration requirements.
- Authorization implications.

This ticket produces a documented architectural decision and
follow-up implementation work.

It does not introduce destructive functionality.

## Dependencies

| Work Item | Dependencies                                    |
| --------- | ----------------------------------------------- |
| VW-018    | Sprint 2 frontend foundation                    |
| VW-019    | Sprint 2 Work Item capabilities                 |
| VW-020    | VW-019                                          |
| VW-021    | VW-019, VW-020                                  |
| VW-022    | VW-018 and Sprint 2 listing APIs                |
| VW-023    | VW-019, VW-020, VW-022                          |
| VW-024    | VW-018, VW-022, VW-023                          |
| VW-025    | Existing Work Item domain and persistence model |

VW-025 can proceed independently.

VW-022 does not depend on the update or status transition APIs,
although its completion requires the agreed automated frontend
testing foundation.

## Testing Strategy

### Backend

- Domain unit tests.
- Application tests using test doubles.
- PostgreSQL integration tests when persistence behavior
  requires verification.
- Complete .NET build and test suite.

### API

- Version-controlled Bruno regression scenarios.
- Runtime variables instead of hardcoded resource identifiers.
- Verification of successful and rejected operations.

### Frontend

- TypeScript validation.
- Next.js production build.
- Playwright E2E tests.
- Deterministic test fixtures.
- Controlled HTTP responses for isolated UI scenarios.
- GitHub Actions execution.

Frontend tests must not modify development or production data.

## Definition of Done

A development work item is complete when its applicable
acceptance criteria are satisfied and the following evidence
is available:

- Implementation committed and pushed.
- Relevant automated tests passing.
- Existing regression tests remaining green.
- Build validation successful.
- API contracts and behavior verified.
- Documentation updated where required.
- Plane ticket updated with implementation evidence.

Research work items require a completed and reviewed design
document rather than implementation tests.

## Risks

### Frontend Test Instability

Mitigation:

Use deterministic fixtures, isolated HTTP responses, and
reliable selectors instead of arbitrary delays.

### API and Frontend Contract Drift

Mitigation:

Maintain explicit contracts and validate API behavior before
implementing dependent frontend functionality.

### Uncontrolled Scope Expansion

Mitigation:

Keep deletion implementation, advanced UI redesign, and MCP
integration outside Sprint 3.

### Insufficient Regression Coverage

Mitigation:

Introduce automated tests as functionality is delivered,
rather than postponing testing until migration.

## Out of Scope

- Final UI/UX identity and design system.
- MCP integration implementation.
- Work Item deletion or archival implementation.
- Production deployment.
- Plane data migration.
- Advanced search and filtering.

## VW-018 Progress — 2026-09-24

Status: **In progress**. The first Chromium navigation smoke test is implemented and passing. Local execution, failure traces, and frontend run instructions are available; lint and production build validation passed.

The current test uses the real development API. Deterministic HTTP fixtures, database-independent regression coverage, GitHub Actions execution, and the Plane acceptance-criteria and evidence review remain outstanding. The Phase 1 scope and Definition of Done are unchanged.

See the [September 24 engineering journal](journal/2026-09-24.md) for implementation evidence and the next-session checklist.

## Next Steps

1. Complete VW-018 and establish Playwright.
2. Implement Work Item lifecycle operations.
3. Extend API regression coverage.
4. Deliver the frontend workflows.
5. Complete frontend E2E coverage.
6. Document deletion and archival semantics.
7. Conduct Sprint Review and Retrospective.

## Expected Outcome

Sprint 3 should deliver a tested Work Item lifecycle covering
browsing, editing, and supported status transitions.

The resulting automated regression foundation will support
future development and reduce migration risk when VEL Workbench
eventually replaces Plane.
