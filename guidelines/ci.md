# Continuous Integration Guidelines

## Recommendation

Rust component repositories should run automated CI checks for build health, formatting, linting, testing, and dependency security.

At minimum, Rust CI should include:

- `cargo fmt --check`
- `cargo clippy`
- `cargo test`
- `cargo audit`

## Cargo Audit

Rust component repositories should run `cargo audit` in CI to detect known vulnerable crate dependencies.

The SIG approved documenting this recommendation in the 2026-09-09 meeting notes. Where practical, repositories should base their workflow on the existing IEEE1905 GitHub Actions `cargo audit` pattern.

The workflow should:

- install or use `cargo-audit`
- run `cargo audit`
- fail CI when vulnerable dependencies are detected
- allow temporary exceptions only when they are documented and reviewed

## Handling Findings

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
- RustSec Advisory Database
