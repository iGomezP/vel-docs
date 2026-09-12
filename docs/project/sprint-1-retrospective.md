# Sprint 1 Retrospective — VEL Workbench Foundation

## Context

Sprint 1 established the initial engineering foundation for VEL Workbench and delivered the first complete Project API vertical slices.

This retrospective captures the practices that worked well, the friction discovered during implementation, and the actions that should influence the next sprint.

## What Went Well

### Vertical Slices Kept the Work Focused

Implementing Project creation and retrieval as complete vertical slices provided a clear path from the HTTP boundary through the application and domain layers to PostgreSQL.

This avoided building abstractions without an immediate use case.

### Tests Influenced the Design

Automated tests were not treated only as final verification.

During Project creation, API testing exposed a null Project key path that produced an unexpected server error. The implementation was corrected by allowing domain validation to remain responsible for validating the input instead of performing premature normalization in the handler.

A regression test was added before completing the fix.

### API Tests Added a Different Level of Confidence

The .NET tests validated domain and application behavior, while Bruno validated the actual HTTP contract.

Using both exposed behavior that unit-level tests alone did not necessarily demonstrate.

### Infrastructure Was Added Only When Required

Existing VEL PostgreSQL infrastructure was reused instead of introducing additional databases or services.

Redis, RabbitMQ, and other available VEL capabilities were deliberately not introduced because Sprint 1 did not have requirements that justified them.

### Documentation Followed the Implementation

Repository structure, API test execution, engineering decisions, and daily progress were documented alongside the implementation rather than postponed indefinitely.

### Git History Provides Useful Evidence

Work was committed incrementally around meaningful engineering changes.

The repository history now provides evidence of how VEL Workbench evolved from its initial skeleton to tested vertical slices.

## What Could Improve

### Cross-Platform Repository Behavior Is Not Fully Standardized

Development now occurs from both Fedora and Windows.

Git reported LF/CRLF conversion warnings when Bruno files were edited from Windows.

The repository should explicitly define its line-ending policy instead of relying on workstation-specific Git defaults.

### API Test Data Accumulates

Bruno generates unique Project keys, which makes repeated executions reliable, but the created records remain in the development database.

This is acceptable at the current scale but will become increasingly inconvenient as the API suite grows.

A cleanup or isolated test-data strategy should be introduced when the cost becomes justified.

### Remote Development Assumptions Need to Stay Explicit

The backend commonly runs on Fedora while desktop development tools can run on Windows.

VS Code Remote SSH port forwarding made this workflow practical, but development documentation should continue avoiding assumptions that only one workstation topology exists.

### Ticket Ordering Changed During the Sprint

VW-009 was implemented before VW-008 was completed.

The change was reasonable because Get Project by ID completed the `Location` contract introduced by Project creation, but future sprint planning should make dependencies and preferred implementation order clearer.

### Small Repository Standards Are Emerging Organically

Formatting, line endings, test organization, local environment files, and cross-platform behavior are gradually becoming repository conventions.

These should be captured as explicit standards when repeated patterns emerge instead of remaining tribal knowledge.

## Actions for Sprint 2

### Define Repository Line Endings

Evaluate and introduce a repository-level `.gitattributes` policy for files shared between Windows and Linux development environments.

### Preserve Vertical Slice Delivery

Continue delivering functionality through small end-to-end slices instead of creating infrastructure or abstractions ahead of requirements.

### Keep Test Layers Complementary

Maintain the current separation:

```text
.NET tests
└── Domain and application behavior

Bruno
└── HTTP contracts and API workflows
```

Add tests at the level where they provide meaningful confidence rather than duplicating every scenario across every test layer.

### Monitor API Test Data

Continue using dynamically generated API test data during early development.

Introduce cleanup or isolated test environments only when accumulated data begins to affect development, debugging, or test reliability.

### Make Dependencies Visible During Planning

Sprint 2 work items should identify dependencies and be ordered around usable vertical capabilities.

## Key Learning

Sprint 1 demonstrated that a small walking skeleton can evolve into a credible platform foundation without prematurely introducing distributed architecture.

The most valuable engineering work was not the amount of code produced, but establishing a repeatable loop:

```text
Requirement
    ↓
Small vertical slice
    ↓
Automated tests
    ↓
HTTP validation
    ↓
Evidence
    ↓
Documentation
    ↓
Next decision
```

This loop should remain a core VEL Workbench engineering practice.
