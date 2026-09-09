# Continuous Integration Guidelines

## Recommendation

Rust component repositories should run automated CI checks for build health, formatting, linting, testing, and dependency security.

At minimum, Rust CI should include:

- `cargo fmt --check`
- `cargo clippy`
- `cargo test`
- `cargo audit`

Repositories may add broader checks such as dependency policy, feature-matrix testing, and coverage reporting when those checks fit the component's risk profile and CI budget.

## Cargo Command Guidance

The CI recommendation and reusable workflow templates under [`task-forces/ci/github-actions/workflows/`](../task-forces/ci/github-actions/workflows/) use the following Cargo commands and Cargo subcommands.

### `cargo fmt --check`

Runs `rustfmt` in check mode and fails CI when committed Rust source is not formatted according to the repository's formatting policy.

Advantages:

- keeps formatting mechanical and consistent across contributors
- reduces review noise by separating style changes from technical changes
- avoids formatting drift between local development and CI

### `cargo clippy --workspace --all-targets --all-features -- -D warnings`

Runs Clippy lints across the workspace, including libraries, binaries, tests, benches, examples, and all feature-enabled code paths. The `-D warnings` flag makes warnings fail CI.

Advantages:

- catches common correctness, maintainability, and API-usage issues before review
- checks code that may not be built by a default `cargo build`
- keeps the warning baseline clean so new warnings are visible immediately

Repositories with an existing warning backlog may temporarily run Clippy without `-D warnings`, but should restore warning failures after the baseline is clean.

### `cargo test --workspace --all-targets`

Builds and runs tests for every workspace crate and builds all testable targets.

Advantages:

- verifies that workspace crates continue to compile and pass their host-side tests together
- catches integration breakage across crates, examples, binaries, and test targets
- provides a fast default signal for pull requests even when target-device validation happens elsewhere

Repositories with target-only crates, platform-bound FFI, or tests requiring device services should split host-side tests from target validation instead of disabling tests entirely.

### `cargo audit`

Checks the resolved dependency graph against the RustSec Advisory Database and fails CI when known vulnerable dependencies are present.

Advantages:

- detects vulnerable direct and transitive crate dependencies
- gives repositories a repeatable security gate tied to the committed `Cargo.lock`
- supports documented temporary exceptions when an immediate upgrade is not practical

Binary and service repositories should normally commit `Cargo.lock` so CI audits the dependency set that is actually built.

### `cargo deny check licenses bans sources`

Checks dependency licenses, explicitly banned crates, and allowed dependency sources against the repository's `deny.toml` policy.

Advantages:

- enforces license policy before dependencies enter the component
- prevents known-unwanted crates or duplicate dependency patterns from spreading
- limits dependency sources to reviewed registries or repositories, improving supply-chain traceability

Use this check when the repository has an agreed license and dependency-source policy. Keep exceptions narrow, reviewed, and traceable.

### `cargo hack test --workspace --each-feature`

Runs tests across feature combinations using `cargo-hack`. The reusable workflow exposes the exact flags through `CARGO_HACK_FLAGS` so repositories can tune the matrix.

Advantages:

- catches missing feature gates and accidental assumptions about default features
- verifies optional code paths that ordinary `cargo test` may skip
- improves confidence that crates remain reusable across different RDK-B component configurations

Feature-matrix testing can be expensive on large workspaces. Repositories may narrow the package or feature set when full coverage is too slow for every pull request.

### `cargo llvm-cov --workspace --summary-only --output-path coverage_report.txt`

Runs tests with coverage instrumentation through `cargo-llvm-cov` and writes a coverage summary for CI reporting.

Advantages:

- shows whether pull requests are exercising changed Rust code
- helps identify untested logic before it becomes embedded in component behavior
- provides a trendable signal for test quality without requiring target hardware for every check

Coverage should be treated as an engineering signal, not as the only measure of test quality. Repositories that accept forked pull requests should confirm their GitHub Actions permission model before posting coverage comments.

## Cargo Audit

Rust component repositories should run `cargo audit` in CI to detect known vulnerable crate dependencies.

This recommendation should be reviewed as part of the full CI guideline approval in the 2026-09-09 meeting notes. Where practical, repositories should base their workflow on the existing IEEE1905 GitHub Actions `cargo audit` pattern.

The workflow should:

- install or use `cargo-audit`
- run `cargo audit`
- fail CI when vulnerable dependencies are detected
- allow temporary exceptions only when they are documented and reviewed

How to Handle Findings:

Security advisories should be resolved by upgrading affected dependencies where practical.

If an advisory cannot be resolved immediately, the repository should document:

- affected crate
- advisory ID
- reason for the temporary exception
- owner
- target resolution date

## Embedded System Considerations

CI checks should account for RDK-B platform constraints such as CPU, memory, storage, startup time, and cross-compilation requirements.

Where host-side CI cannot fully represent the target platform, repositories should still run host-side Rust checks and document any target-only validation that must happen elsewhere.

## References

- [RDK-B Rust SIG meeting notes, 2026-09-09](../meetings/2026-09-09.md)
- IEEE1905 GitHub Actions `cargo audit` workflow
- `cargo-audit`
- `cargo-deny`
- `cargo-hack`
- `cargo-llvm-cov`
- RustSec Advisory Database
