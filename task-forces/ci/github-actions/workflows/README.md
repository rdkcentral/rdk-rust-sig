# GitHub Actions Workflow Templates

This directory contains generic GitHub Actions workflow templates for Rust component repositories in RDK-B.

The templates are intended as starting points. Repositories should adapt branch names, Rust toolchain policy, package selection, and required checks to match their own release and integration workflow.

## Common Pattern

The templates generally:

- run on pull requests targeting `main` or `develop`
- run on pushes to `main` or `develop` where useful
- support manual execution with `workflow_dispatch`
- use `ubuntu-latest`
- check out the repository with `actions/checkout@v4`
- install the stable Rust toolchain with `dtolnay/rust-toolchain@stable`
- use read-only repository permissions unless a pull request comment is required

Repositories with a different branch policy should update the `branches` lists before adopting the templates.

## `rust-cargo-audit.yml`

Purpose:
Checks Rust dependencies for known security advisories using `cargo audit`.

Triggers:

- pull requests to `main` or `develop` when Cargo or audit configuration files change
- pushes to `main` or `develop` when Cargo or audit configuration files change
- weekly scheduled run
- manual run

Required repository files:

- `Cargo.toml`
- `Cargo.lock`

Checks performed:

- installs `cargo-audit`
- runs `cargo audit`

Expected failure behavior:
Fails when `cargo audit` reports a vulnerable dependency that is not ignored by the repository audit policy.

Adoption notes:
Binary and service repositories should normally commit `Cargo.lock` so CI audits the dependency set that is actually built. Temporary advisory exceptions should be documented with an owner and target resolution date.

## `rust-license-policy.yml`

Purpose:
Checks Rust dependency license policy using `cargo deny`.

Triggers:

- pull requests to `main` or `develop` when Cargo or `deny.toml` files change
- pushes to `main` or `develop` when Cargo or `deny.toml` files change
- manual run

Required repository files:

- `Cargo.toml`
- `Cargo.lock`
- `deny.toml`, copied from [`../supporting-files/deny.toml`](../supporting-files/deny.toml) and reviewed for the target repository

Checks performed:

- installs `cargo-deny`
- runs `cargo deny check licenses bans sources`

Expected failure behavior:
Fails when dependency licenses, banned crates, or dependency sources violate `deny.toml`.

Adoption notes:
Use this workflow when the repository has an agreed license and dependency-source policy. Keep exceptions narrow, reviewed, and traceable.

## `rust-cargo-clippy.yml`

Purpose:
Runs Rust lint checks with Clippy.

Triggers:

- pull requests to `main` or `develop` when Rust or Cargo files change
- pushes to `main` or `develop` when Rust or Cargo files change
- manual run

Required repository files:

- `Cargo.toml`

Checks performed:

- installs the stable Rust toolchain
- runs `cargo clippy --workspace --all-targets --all-features -- -D warnings`

Expected failure behavior:
Fails on Clippy warnings or errors.

Adoption notes:
Repositories with an existing warning backlog may initially remove `-- -D warnings`, then re-enable it after the lint baseline is clean.

## `rust-cargo-tests.yml`

Purpose:
Runs Rust tests for the workspace.

Triggers:

- pull requests to `main` or `develop` when Rust or Cargo files change
- pushes to `main` or `develop` when Rust or Cargo files change
- manual run

Required repository files:

- workspace or crate `Cargo.toml`

Checks performed:

- installs the stable Rust toolchain
- runs `cargo test --workspace --all-targets`

Expected failure behavior:
Fails when tests fail or when the workspace does not compile for the tested targets.

Adoption notes:
Repositories with target-only crates, platform-bound FFI, or tests requiring device services may need to split host-side tests from target validation.

## `rust-cargo-hack.yml`

Purpose:
Runs feature-combination testing with `cargo hack`.

Triggers:

- pull requests to `main` or `develop` when Rust or Cargo files change
- pushes to `main` or `develop` when Rust or Cargo files change
- manual run

Required repository files:

- `Cargo.toml`

Checks performed:

- installs `cargo-hack`
- runs `cargo hack test $CARGO_HACK_FLAGS`

Default configuration:

```yaml
env:
  CARGO_HACK_FLAGS: "--workspace --each-feature"
```

Expected failure behavior:
Fails when a feature combination does not build or test successfully.

Adoption notes:
Feature-matrix testing can be expensive on large workspaces. Repositories may narrow `CARGO_HACK_FLAGS` to specific packages or feature sets when full workspace coverage is too slow for every pull request.

## `rust-cargo-coverage.yml`

Purpose:
Generates a Rust test coverage summary and posts it as a pull request comment.

Triggers:

- pull requests to `main` or `develop` when Rust or Cargo files change
- manual run

Required repository files:

- `Cargo.toml`

Required permissions:

- `contents: read`
- `pull-requests: write`

Checks performed:

- installs stable Rust with `llvm-tools-preview`
- installs `cargo-llvm-cov`
- runs `cargo llvm-cov $CARGO_LLVM_COV_ARGS`
- formats the coverage summary
- posts or updates a sticky pull request comment when running on a pull request

Default configuration:

```yaml
env:
  CARGO_LLVM_COV_ARGS: "--workspace --summary-only --output-path coverage_report.txt"
```

Expected failure behavior:
Fails when coverage generation fails or tests fail during coverage execution.

Adoption notes:
Pull request comments require write permission. Repositories that accept forked pull requests should confirm their GitHub Actions permission model before making this a required check.
