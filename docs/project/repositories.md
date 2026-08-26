# Repository Catalog

This page tracks the official repositories used by Vakzor Enterprise Lab.

## Active Repositories

| Repository | Purpose | Status | Primary Technology | Notes |
| --- | --- | --- | --- | --- |
| `vel-docs` | Engineering documentation portal for Vakzor Enterprise Lab. | Active | MkDocs, Markdown | Contains ADRs, RFCs, runbooks, capabilities, engineering standards, and journals. |
| `vel-workbench` | Internal work management platform for Vakzor Enterprise Lab. | Foundation | .NET, React, PostgreSQL | Created as part of ADR-0002 and Sprint 1 — VEL Workbench Foundation. |

## Repository Rules

All VEL repositories should follow these rules:

- Use English for code, documentation, commits, and technical artifacts.
- Include a clear `README.md`.
- Include a `.gitignore`.
- Keep generated artifacts out of Git.
- Use meaningful commit messages.
- Link important work back to Plane work items when applicable.
- Produce documentation evidence for significant engineering decisions.

## Related Decisions

- ADR-0001 — Docs as Code.
- ADR-0002 — Build VEL Workbench as the Internal Work Management Platform.

## Related Work

- `[VW-001] Create VEL Workbench repository`