# Engineering Standards

## Purpose

This document defines the engineering standards adopted by Vakzor Enterprise Lab (VEL).

Every implementation, regardless of size, should follow these standards.

---

# Engineering Principles

1. Learn by understanding.
2. Design before implementation.
3. Everything important must be documented.
4. Every change starts with a ticket.
5. Prefer automation over manual work.
6. Infrastructure is code.
7. Documentation is code.
8. Tests are part of development.
9. Small incremental improvements.
10. Avoid technical debt whenever possible.

---

# Git Workflow

Future workflow:

RFC

↓

Plane Issue

↓

Feature Branch

↓

Implementation

↓

Tests

↓

Documentation

↓

Pull Request

↓

Review

↓

Merge

↓

Release

---

# Branch Strategy

main

Production-ready state.

develop

Integration branch.

feature/*

Feature development.

hotfix/*

Production fixes.

docs/*

Documentation changes.

---

# Commit Convention

feat:

New feature.

fix:

Bug fix.

docs:

Documentation.

refactor:

Internal improvements.

test:

Tests.

build:

Infrastructure.

ci:

Pipelines.

---

# Definition of Done

A task is Done only if:

- Ticket updated.
- Code implemented.
- Tests passing.
- Documentation updated.
- Journal entry created.
- ADR added (if architecture changed).
- Code reviewed.
- No critical issues remain.

---

# Documentation Standard

All official documentation must be written in English.

---

# Coding Standard

Prefer readability over cleverness.

Avoid unnecessary abstractions.

Keep functions small.

Follow SOLID principles.

Use Clean Architecture whenever applicable.

---

# Infrastructure Standard

Everything possible should be reproducible.

Manual steps should eventually become automation.

---

# Learning Standard

Every Sprint must produce:

New knowledge.

Interview preparation.

Evidence of implementation.

Lessons learned.

---

# Engineering Mindset

VEL is not a homelab.

VEL is treated as a production engineering platform.