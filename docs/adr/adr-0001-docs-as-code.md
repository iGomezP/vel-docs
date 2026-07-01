# ADR-0001 — Docs as Code

## Status

Accepted

---

## Date

2026-07-01

---

## Context

Vakzor Enterprise Lab (VEL) aims to become an enterprise engineering platform rather than a traditional homelab.

The project involves infrastructure, software architecture, DevOps, observability, cloud, artificial intelligence, networking, Linux administration, and security.

As the platform grows, documentation becomes one of the most valuable engineering assets.

Traditional documentation stored in Word documents or personal notes tends to become outdated, difficult to review, and disconnected from the actual implementation.

---

## Decision

VEL adopts the **Docs as Code** philosophy.

All documentation will:

- live inside Git repositories
- be version controlled
- follow Pull Request reviews
- evolve together with the source code
- be published automatically
- be written in Markdown

The official documentation platform will be:

- MkDocs
- Material for MkDocs
- GitHub
- GitHub Actions (future)
- GitHub Pages (future)

---

## Consequences

### Positive

- Documentation evolves with the project.

- Complete version history.

- Easy collaboration.

- Pull Request reviews.

- Searchable documentation.

- Professional portfolio.

- Easy automation.

---

### Negative

Requires engineering discipline.

Documentation becomes part of the Definition of Done for every Sprint.

---

## Alternatives Considered

### Word Documents

Rejected.

Not version controlled.

---

### Notion

Rejected.

Vendor lock-in.

External dependency.

Not infrastructure-as-code friendly.

---

### Wiki

Rejected.

Limited review workflow.

---

## References

The Pragmatic Programmer

Docs as Code

Microsoft Engineering Playbook

Spotify Engineering Culture