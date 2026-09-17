# CI

Status: proposed

Type: guideline

## Problem

Rust projects in the RDK-B stack may adopt different CI checks, workflow layouts, and dependency policy gates unless a common default pipeline is defined.

This can lead to inconsistent review signals, duplicated workflow maintenance, uneven dependency security coverage, and extra onboarding work for new Rust repositories.

## Motivation

A common GitHub CI pipeline gives Rust component repositories a shared baseline for build health, formatting, linting, testing, dependency security, dependency policy, feature validation, and coverage reporting.

The pipeline should be suitable for transfer to the CMF team so it can be offered or applied by default when new Rust projects are created.

## Proposed Approach

Define a reusable GitHub Actions CI pipeline for Rust repositories based on standard Cargo tooling and portable GitHub Actions patterns.

The default pipeline should include:

- formatting checks with `cargo fmt --check`
- lint checks with `cargo clippy`
- host-side tests with `cargo test`
- dependency vulnerability checks with `cargo audit`

The pipeline should also provide optional checks that repositories can enable based on risk, repository maturity, and CI budget:

- dependency license, ban, and source checks with `cargo deny`
- feature-combination testing with `cargo hack`
- test coverage reporting with `cargo llvm-cov`

Once approved, the SIG should transfer the default CI package, adoption notes, and required configuration guidance to the CMF team.

The CMF team can then use the approved pipeline as the default CI baseline for Rust repositories, while component teams retain the ability to tune package selection, feature flags, target validation, and optional checks.

## Alternatives Considered

Each Rust repository defines its own CI:

- maximizes local flexibility
- increases duplicated maintenance
- makes review and security signals inconsistent across RDK-B Rust projects

Only document recommendations without reusable workflow templates:

- keeps the SIG guidance lightweight
- still leaves project teams to reimplement the same checks repeatedly
- increases the chance that checks drift from the guideline

Make every check mandatory for every Rust repository:

- maximizes consistency
- may be too expensive for large workspaces or target-bound components
- may block adoption where target services, FFI, or cross-compilation constraints require staged validation

## RDK-B Integration

The common pipeline should be designed for Rust component repositories in the RDK-B stack, including repositories that build host-side tests in GitHub Actions and perform target-device validation elsewhere.

Repository adoption should document:

- required files such as `Cargo.toml`, `Cargo.lock`, and optional `deny.toml`
- expected branch triggers
- required GitHub permissions
- optional checks enabled or deferred
- target-only validation that is not represented by host-side CI

The CMF team should receive the approved workflow templates and supporting files as the default Rust CI package for new Rust repositories.

## Security Considerations

The default pipeline should include `cargo audit` so vulnerable direct and transitive crate dependencies are detected in CI.

Repositories that enable `cargo deny` should define an explicit dependency policy for licenses, banned crates, and allowed dependency sources. Temporary exceptions should be narrow, documented, reviewed, and tied to a target resolution date.

Workflow permissions should remain read-only unless a workflow requires additional access, such as posting a pull request coverage comment.

## Resource Impact

The minimum checks are expected to be practical for normal pull request CI.

Optional checks may increase CI runtime:

- `cargo deny` adds dependency graph policy evaluation
- `cargo hack` can multiply test runs across feature combinations
- `cargo llvm-cov` reruns tests with coverage instrumentation

Repositories should tune optional checks for large workspaces, embedded constraints, target-only crates, and components with platform-bound dependencies.

## Portability Considerations

The default pipeline should rely on standard Cargo tooling and portable Linux CI runners where possible.

Host-side CI does not replace target validation for RDK-B components that depend on device services, platform-specific FFI, SoC integrations, or cross-compilation behavior. Such repositories should keep host-side Rust checks enabled and document target-only validation separately.

The workflow templates should avoid vendor-specific assumptions unless a repository explicitly opts into them.

## Open Questions

- Which parts of the pipeline should be mandatory for every Rust repository created through CMF?
- Should the CMF team own the deployed default workflow, or should the Rust SIG remain the source of truth with CMF consuming approved releases?
- How should repositories request exceptions from mandatory checks?
- Should coverage reporting be advisory only, or should any minimum threshold be defined later?
- How should target-device validation be linked back to the GitHub CI result?

## References

- [`guidelines/ci.md`](../guidelines/ci.md)
