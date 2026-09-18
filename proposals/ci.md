# Continuous Integration Guideline

Status: proposed

Type: guideline

## Problem

The `ieee1905-rs` repository already has GitHub Actions workflows for Rust code quality, testing, dependency compliance, security, unit-test coverage, and process memory reporting. These workflows have not yet been reviewed by the SIG as a common baseline for other RDK-B Rust repositories.

Without a coordinated review, new Rust repositories may adopt different checks and reporting formats, duplicate workflow maintenance, or omit useful pull-request signals such as dependency compliance, known vulnerabilities, unit-test coverage, and resource impact.

## Motivation

The existing IEEE1905 workflows provide a practical starting point and reduce the implementation effort required to define a common CI approach. A focused task force can collect comments, make agreed adjustments, document limitations, and prepare the reviewed workflows for CMF adoption.

This work is expected to improve consistency, dependency governance, security visibility, test visibility, and resource awareness during pull-request review.

## Proposed Approach

Adopt a continuous integration guideline for Rust repositories in the RDK-B stack, using the existing `ieee1905-rs` workflows as the initial reference implementation.

The guideline will define:

- Common baseline checks and checks that remain optional or repository-specific.
- Pull-request reporting for compliance, security, unit-test coverage, and resource information.
- Configuration expectations for package selection, exclusions, feature flags, branch triggers, and workflow permissions.
- Portability boundaries between shared host-side CI and component-specific target validation.
- The review and exception process for repositories that cannot apply a baseline check.

A linked Continuous Integration Task Force will review the existing workflows, collect comments, apply agreed changes, and prepare the guideline and workflow package for final SIG review and CMF handoff.

Proposed task-force ownership and dates:

- Task-force owners: `@torrentius`, `@matrixdev`, and `@SerhiiShchudlo` from the Rust RDKM team
- CMF owner: `@sbarre01`
- Task-force review date: 2026-10-15
- Final review target: 2026-10-30
- CMF handoff target: 2026-10-30

The task force will:

- Review the existing workflows and collect comments from the Rust SIG, CMF, and relevant repository maintainers.
- Define which checks form the common baseline and which remain optional or repository-specific.
- Define how pull requests report compliance, security, unit-test coverage, and resource information.
- Apply agreed changes and document configuration, permissions, exceptions, and known limitations.
- Prepare the reviewed CI package and guidance for CMF handoff.

The initial reference implementation contains the following workflows:

### Code Quality and Testing

- `rust-cargo-clippy.yml` reports Rust lint and code-quality issues with `cargo clippy`.
- `rust-cargo-tests.yml` runs workspace unit tests, with IEEE1905-specific exclusions for RBUS crates that require platform dependencies.
- `rust-cargo-hack.yml` exercises supported feature combinations for the `ieee1905` package.

### Compliance

- `rust-cargo-deny.yml` enforces dependency license, banned-crate, allowed-source, and advisory policies defined in `deny.toml`.

### Security

- `rust-cargo-audit.yml` identifies known vulnerabilities in direct and transitive Rust dependencies.

### Pull Request Review Information

- `rust-cargo-coverage.yml` generates a unit-test coverage summary and posts it on the pull request.
- `rust-rss-usage-check.yml` runs the release binary, reads Linux process memory information from `/proc`, and posts an RSS report on the pull request.

The CLA and IEEE1905 on-demand functional-test workflows are not part of this proposal.

## Task-Force Mapping

| Guideline Requirement | Task-Force Work | Expected Output |
| --- | --- | --- |
| Define common and optional CI checks | Review the existing IEEE1905 code-quality, testing, compliance, and security workflows | Agreed classification of mandatory, optional, and repository-specific checks |
| Provide useful pull-request review signals | Review coverage, RSS, compliance, and vulnerability reporting | Documented PR checks and reporting behavior |
| Keep repository-specific settings configurable | Identify IEEE1905 package names, RBUS exclusions, feature flags, branches, and executable commands | Configuration and adoption guidance |
| Define portability boundaries | Record what shared host-side workflows do not test | Explicit exclusions for platform-specific FFI, dependencies, SoC integration, and device services |
| Support adoption by new Rust repositories | Coordinate final review with the SIG and CMF | Reviewed guideline and workflow package ready for CMF handoff |

## Expected Deliverables

- Reviewed versions of the listed Rust CI workflows.
- A decision identifying mandatory, optional, and repository-specific checks.
- Configuration guidance for package selection, exclusions, feature flags, branch triggers, workflow permissions, and pull-request reporting.
- Documentation of unsupported platform-specific validation and the responsibility of component repositories.
- A reviewed CI package and adoption guidance ready for CMF handoff.
- A task-force outcome reported to the SIG for final guideline review.

## Workforce and Resources

The workflows are already in place, so the remaining effort is expected to be limited. One task-force owner will coordinate comments, agreed changes, final review, and handoff. Other proposed owners, SIG members, CMF representatives, and repository maintainers will provide review input through the normal review process.

No dedicated test devices or labs are required. The work requires existing GitHub repository and GitHub Actions access.

## Requested Decisions

The SIG is asked to decide whether to:

- Approve the proposed continuous integration guideline direction.
- Use the linked Continuous Integration Task Force, with the proposed owners and dates, to review and prepare the guideline deliverables.
- Use the existing `ieee1905-rs` workflows as the initial reference implementation.
- Treat Clippy, host-side tests, dependency compliance, and vulnerability checks as common baseline candidates.
- Treat feature testing, unit-test coverage, and RSS reporting as configurable checks based on repository applicability and CI cost.
- Exclude platform-specific FFI, platform-bound dependencies, SoC integrations, and device-service behavior from the shared host-side workflow validation.
- Prepare the reviewed workflow package for CMF adoption after final SIG review.

## Alternatives Considered

Each Rust repository defines its own CI:

- maximizes local flexibility
- duplicates workflow maintenance
- produces inconsistent review, compliance, and security signals

Publish guidance without reviewing the existing workflows:

- requires less coordination
- leaves repository teams to interpret and reimplement the guidance
- does not benefit from the existing IEEE1905 implementation experience

Make every existing IEEE1905 check mandatory:

- maximizes consistency
- ignores differences in repository structure, feature use, executable availability, and CI budget
- may make package-specific checks such as RSS reporting unsuitable for libraries or platform-bound components

## RDK-B Integration

The proposed baseline applies to Rust libraries, services, tools, and mixed-language repositories that can run portable host-side checks in GitHub Actions. Repository adoption will identify required Cargo files, branch triggers, workflow permissions, package selection, optional checks, and excluded target-only validation.

After final SIG review, the approved workflow package and adoption guidance will be handed to CMF for use with new Rust repositories. Component teams will retain responsibility for repository-specific settings and target-platform validation.

## Security Considerations

`cargo audit` provides visibility into known vulnerable direct and transitive dependencies. `cargo deny` provides policy checks for dependency licenses, banned crates, sources, and advisories.

Workflow permissions should remain read-only by default. Workflows that post pull-request comments, including coverage and RSS reports, require documented pull-request write permission and must account for the restricted tokens used by pull requests from forks.

Exceptions must be narrow, documented, reviewed, assigned to an owner, and given a target resolution date.

## Resource Impact

The task-force effort is limited because the reference workflows already exist. Remaining work consists mainly of review coordination, agreed edits, documentation, and handoff.

For repositories adopting the workflows, Clippy, host-side tests, and dependency checks are expected to fit normal pull-request CI budgets. Feature testing and coverage instrumentation can increase CI runtime. RSS reporting requires a runnable host binary and is not applicable to every repository.

## Portability Considerations

The shared workflows will use standard Cargo tooling and portable Linux runners where practical.

They will not build, test, or validate platform-specific FFI, platform-bound dependencies, SoC integrations, or device-service behavior. Validation of those areas remains the responsibility of component-specific target environments.

The IEEE1905 package names, RBUS exclusions, feature flags, and executable commands must not be treated as universal defaults; repositories will configure these values according to their structure.

## Resulting Repository Changes

If approved:

- Activate the [Continuous Integration Task Force](../task-forces/ci.md) as the implementation and review record for this proposal.
- Update the task-force record with the SIG decision and a link to the authoritative meeting record.
- Update [Continuous Integration Guidelines](../guidelines/ci.md) with the agreed mandatory, optional, and repository-specific checks.
- Track implementation and handoff actions in the [RDK Central Rust SIG GitHub project](https://github.com/orgs/rdkcentral/projects/119).

## Open Questions

- Which checks must be enabled for every Rust repository created through CMF?
- Should coverage and RSS reports remain advisory, or should repositories be able to define blocking thresholds?
- How should repositories request and review exceptions from mandatory checks?
- Will CMF own the deployed workflows, or will the Rust SIG publish approved versions for CMF to consume?

## References

- [Draft Continuous Integration Task Force](../task-forces/ci.md)
- [Continuous Integration Guidelines](../guidelines/ci.md)
- [Existing `ieee1905-rs` GitHub Actions workflows](https://github.com/rdkcentral/ieee1905-rs/tree/main/.github/workflows)
- [RDK-B Rust SIG meeting notes, 2026-09-09](../meetings/2026-09-09.md)
