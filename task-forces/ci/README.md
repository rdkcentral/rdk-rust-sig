# CI Task Force

## Purpose

Define reusable CI patterns for Rust component repositories in RDK-B.

## Scope

- Rust formatting, linting, testing, and dependency security checks
- GitHub Actions workflow templates
- `cargo audit` integration
- `cargo deny` dependency policy checks
- coverage and feature-combination checks
- embedded Linux and cross-compilation considerations
- recommendations for updates to `guidelines/ci.md`

## Deliverables

- reusable GitHub Actions workflow examples
- explanation of each workflow and when to use it
- adoption checklist for Rust component repositories
- proposed updates to the Rust SIG CI guidelines

## Workflow Templates

Workflow YAML files should be stored under [`github-actions/`](github-actions/).

Generic Rust workflow templates are stored under [`github-actions/workflows/`](github-actions/workflows/).

See [`github-actions/workflows/README.md`](github-actions/workflows/README.md) for an explanation of each workflow, including its purpose, triggers, required repository files, checks performed, failure behavior, and portability considerations.

## References

- [`guidelines/ci.md`](../../guidelines/ci.md)
- [`meetings/2026-09-09.md`](../../meetings/2026-09-09.md)
