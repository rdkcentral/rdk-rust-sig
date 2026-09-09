# GitHub Actions Workflow Templates

This directory contains GitHub Actions material reviewed by the CI task force.

## Workflow Files

Generic Rust workflow templates are stored under [`workflows/`](workflows/).

See [`workflows/README.md`](workflows/README.md) for an explanation of each workflow and adaptation notes for RDK-B Rust component repositories.

## Supporting Files

- [`supporting-files/deny.toml`](supporting-files/deny.toml) - starter `cargo-deny` policy used by the Rust license policy workflow.

When adopting [`workflows/rust-license-policy.yml`](workflows/rust-license-policy.yml), copy `supporting-files/deny.toml` to the root of the target repository as `deny.toml` and review the accepted licenses, dependency-source policy, and exceptions before enabling the workflow as a required check.
