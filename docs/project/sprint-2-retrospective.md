# Sprint 2 Retrospective — Project & Work Item Management

## Sprint Summary

Sprint 2 delivered the core Project and Work Item management
workflows and the initial Next.js Project browser.

All eight planned work items were completed, including the
frontend stretch goal.

## What Went Well

### Vertical Slice Delivery

Features were implemented incrementally through the Domain,
Application, Infrastructure, API, and frontend layers.

Each completed backend vertical slice was validated before
moving to the next one.

### Test-Driven Development

Application tests helped define expected behavior before
implementing the List Work Items handler.

The development process exposed missing interfaces, incomplete
test doubles, and asynchronous test configuration issues.

### Transactional Consistency

Work Item number allocation and persistence were implemented
within the same transaction.

An integration test verified rollback behavior when an
operation fails after allocating a number.

### Read-Side Architecture

The introduction of `IWorkItemQueries` enabled optimized
read projections without modifying domain entities solely
for presentation requirements.

### API Regression Coverage

Bruno scenarios transformed manual API checks into
version-controlled regression tests.

Runtime variables allow Project and Work Item scenarios
to execute without relying on existing database records.

### Full-Stack Integration

The initial Project browser established the first
user-facing workflow between PostgreSQL, the .NET API,
and Next.js.

The frontend displays real Project information and handles
loading, empty, success, and error states.

## What Could Be Improved

### Frontend Automated Testing

Frontend validation currently relies on TypeScript checks,
production builds, and manual browser testing.

Automated end-to-end regression tests have not yet been
implemented.

### UI/UX Consistency

The initial interface fulfills the functional requirements,
but a product-specific visual identity and design system
are still needed.

Future screens should use consistent typography, spacing,
navigation, components, and interaction patterns.

### Test Design

An earlier testing step demonstrated that empty tests can
pass without verifying meaningful behavior.

Tests should verify observable results and should not be
considered complete merely because the test runner reports
a successful execution.

### Shared Layout Design

The initial frontend implementation duplicated the sidebar
across multiple route components.

The application shell was subsequently moved into the root
layout, reducing duplication and maintaining consistent
navigation during loading and error states.

### Development Environment

The frontend and backend run as separate development
processes.

API availability and frontend recovery require explicit
validation during integration testing.

## Action Items

### Establish Frontend E2E Testing

Introduce Playwright to cover the main Project browser
workflows using deterministic test data.

Add the test suite to CI once the local workflow is stable.

### Establish a UI/UX Design System

Define a distinctive visual identity for VEL Workbench.

Document reusable design tokens, typography, spacing,
navigation patterns, and component standards.

### Strengthen Test Quality

Continue using behavior-focused tests with explicit
assertions and meaningful failure conditions.

### Maintain Architectural Boundaries

Preserve the separation between domain behavior,
application use cases, persistence, read models,
and API contracts.

Avoid introducing additional infrastructure without
a documented requirement.

## Sprint Outcome

The Sprint 2 goal was achieved, including the frontend
stretch goal.

The next sprint should build upon the completed
full-stack foundation while addressing product usability
and automated frontend quality.
