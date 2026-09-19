# RFC-0002: VEL Workbench MCP Integration

- Status: Proposed
- Date: 2026-09-18
- Project: VEL Workbench
- Category: AI Integration
- Target: Future sprint

## 1. Context

VEL Workbench is an internal work management platform built using
.NET, PostgreSQL, and Next.js.

The platform currently provides APIs for managing Projects and
Work Items.

We want AI clients such as Cursor and ChatGPT to interact with
Workbench through standardized interfaces.

The Model Context Protocol (MCP) provides a mechanism for
exposing application capabilities to compatible AI clients.

## 2. Objective

Enable authorized AI clients to retrieve Workbench information
through an MCP server without direct database access.

The initial implementation will be read-only.

## 3. Proposed Architecture

```text
AI Client
    |
    | MCP
    v
VEL Workbench MCP Server
    |
    | Authorized requests
    v
VEL Workbench API
    |
    v
Application Layer
    |
    v
Infrastructure / PostgreSQL
```

The MCP server must not access PostgreSQL directly.

Business rules and authorization must remain enforced by
the existing application.

## 4. Initial Scope

Expose two MCP tools:

### vel_list_projects

Retrieve the Projects visible to the authenticated user.

Expected output:

- Project ID
- Project key
- Project name
- Project description
- Project status

### vel_list_work_items

Retrieve Work Items belonging to a specified Project.

Input:

- Project ID

Expected output:

- Work Item ID
- Project ID
- Number
- Human-readable code
- Title
- Description
- Status

An unknown Project must produce an explicit error.

## 5. Technology

Proposed stack:

- .NET 10
- Official MCP C# SDK
- ASP.NET Core hosting
- Streamable HTTP transport
- Existing VEL Workbench API

The implementation must verify SDK compatibility with the
selected MCP protocol revision.

The MCP server may be hosted independently from the main API
to maintain clear deployment and security boundaries.

## 6. Security Requirements

- Authentication is required for remote access.
- Authorization must be enforced per user and Project.
- No database credentials may be exposed to AI clients.
- No unrestricted access to internal APIs.
- Validate HTTP origins according to MCP transport requirements.
- Use HTTPS for remotely accessible deployments.
- Apply request limits and structured logging.
- Do not log access tokens or sensitive Work Item content.
- Treat data returned through MCP as untrusted model context.
- Prevent cross-user and cross-Project data exposure.

An authentication and authorization design must be approved
before exposing the server outside the development environment.

## 7. Out of Scope

The initial implementation will not support:

- Creating or modifying Work Items.
- Updating Project status.
- Deleting resources.
- Automatic sprint planning.
- Autonomous execution of workflow changes.
- Direct PostgreSQL access.
- Public internet deployment.

## 8. Future Capabilities

Potential future integrations include:

- Sprint summaries.
- Dashboard analytics.
- Work Item creation.
- Status updates.
- Comments and activity history.
- GitHub commit and pull request correlation.
- Automated Sprint Review preparation.

Write operations will require separate authorization,
validation, auditing, and user confirmation policies.

## 9. Acceptance Criteria

- A compatible MCP client can connect to the server.
- The client can discover the available tools.
- The client can retrieve real Projects.
- The client can retrieve Work Items for a specific Project.
- Existing application authorization is respected.
- No direct database connection is used.
- Error responses are explicit and predictable.
- Integration tests validate successful and unauthorized access.
- Setup instructions are documented.
- The implementation is reproducible in the VEL environment.

## 10. Open Questions

- Should the MCP server run as an independent ASP.NET Core service?
- Which identity provider and authorization flow should be used?
- How will user identity propagate to the Workbench API?
- Which MCP clients and protocol versions will be supported initially?
- Should development start with a local transport before remote HTTP?
- How should tool usage be audited?

These questions must be resolved before implementation.

## 11. Decision

Pending research and architectural review.

No implementation is approved by this RFC alone.
