# Continuous Integration Task Force

Status: proposed

Linked proposal: [CI](../proposals/ci.md)

Owner: @torrentius, @matrixdev, @SerhiiShchudlo _(Rust RDKm team)_

CMF owner: @sbarre01

Review date: 15-10-2026

## Purpose

Define, validate, and prepare for adoption a reusable GitHub Actions CI baseline for Rust repositories in the RDK-B stack. The task force will turn the CI proposal and guideline into workflow templates and supporting configuration that can be transferred to the CMF team.

## Scope

- Define mandatory baseline checks for formatting, linting, host-side testing, and dependency vulnerability scanning.
- Define how pull requests report compliance status, resource impact, and unit-test coverage as review signals.
- Create reusable GitHub Actions workflows and supporting configuration examples.
- Validate the workflow package against representative RDK-B Rust repositories (currently only ieee1905-rs).
- Document adoption, customization, exceptions, permissions, and target-only validation.
- Prepare the approved workflow package and guidance for transfer to the CMF team.

## Out of Scope

- Replacing target-device, integration, or platform-specific validation with host-side CI.
- Requiring existing C or C++ RDK-B components to adopt Rust tooling.
- Defining component-specific test environments, feature policies, or deployment pipelines.
- Mandating optional checks where repository constraints or CI cost make them impractical.

## Expected Benefits

The task force will provide consistent review signals across RDK-B Rust repositories, reduce duplicated workflow maintenance, improve dependency security coverage, and shorten onboarding for new repositories. A reusable baseline will also make CI behavior easier for the SIG, CMF team, and component teams to review and maintain.

## Applicability

- Target RDK-B components or domains: Rust libraries, services, tools, and mixed-language repositories containing Rust code.
- Target platforms or device classes: Repositories using GitHub Actions for host-side validation; target-device validation remains separate where required.
- Required operating system or Linux interfaces: Standard Cargo tooling on portable Linux GitHub-hosted or compatible runners.
- Known limitations or exclusions: Platform-bound FFI, device services, SoC integrations, and cross-compilation behavior may require additional target-specific validation.

## Deliverables

- Reusable GitHub Actions workflow implementing the agreed mandatory CI checks.
- Optional workflow jobs or configuration for `cargo deny`, `cargo hack`, and `cargo llvm-cov`.
- Supporting configuration examples, including dependency policy and workflow permissions where applicable.
- Adoption guide covering prerequisites, branch triggers, customization, exceptions, and target-only validation.
- Validation report from representative RDK-B Rust repositories, including CI runtime and compatibility findings.
- CMF handoff package identifying ownership, versioning, maintenance, and rollout expectations.

## Timeline

| Milestone | Target Date | Output | Status |
| --- | --- | --- | --- |
| Kickoff | TBD | Scope, ownership, and validation repositories confirmed | proposed |
| Baseline design review | TBD | Mandatory and optional checks agreed | proposed |
| Workflow prototype | TBD | Reusable workflow and configuration examples available | proposed |
| Repository validation | TBD | Validation findings and required adjustments recorded | proposed |
| Final review and handoff | TBD | Approved CI package and CMF adoption guidance complete | proposed |

## Workforce and Resources

- Task force lead: TBD
- Contributors: Rust SIG members, CMF representatives, and maintainers of validation repositories
- Reviewers: Rust SIG, CMF, repository maintainers, and security or licensing reviewers where required
- Estimated effort: TBD after validation repositories and optional checks are selected
- Required test devices or labs: None for the host-side baseline; component teams provide target validation where applicable
- Required CI, build, or repository access: GitHub Actions access and permission to test the workflows in representative RDK-B Rust repositories

## RDK-B Integration

The CI package will provide a common host-side baseline for Rust component repositories. Repositories will document required files, branch triggers, workflow permissions, enabled optional checks, and any validation that can only run on target hardware or platform infrastructure.

After SIG approval, the workflow templates, supporting files, and adoption guidance will be transferred to the CMF team for use as the default CI baseline for new Rust repositories. The handoff must identify whether CMF or the Rust SIG owns subsequent releases and maintenance.

## Security Considerations

The mandatory baseline will run `cargo audit` against the resolved dependency graph. Optional `cargo deny` checks will enforce agreed policies for licenses, banned crates, and dependency sources. Exceptions must be narrow, documented, reviewed, assigned to an owner, and given a target resolution date.

Workflow permissions will default to read-only. Additional permissions must be justified and documented for jobs that publish results or comment on pull requests.

## Resource Impact

Formatting, linting, host-side testing, and vulnerability scanning are expected to fit normal pull request CI budgets. Feature-matrix testing and coverage instrumentation can substantially increase runtime and compute use, so repositories may narrow these checks based on workspace size, risk, and available CI capacity.

The validation report will record representative execution times and identify opportunities for caching or job parallelism without weakening required checks.

## Portability Considerations

The workflow package will use standard Cargo tooling and portable Linux runners where practical. It will avoid vendor-specific build or device assumptions in the shared baseline.

Repositories with cross-compilation, platform-specific FFI, SoC integration, or device-service dependencies will retain host-side checks and document their separate target validation.

## Success Criteria

- The SIG agrees which checks are mandatory and which are optional.
- The reusable workflow and supporting configuration pass in the selected validation repositories.
- Adoption guidance documents permissions, customization, exceptions, and target-only validation.
- CI resource impact and known compatibility limitations are recorded.
- Ownership and maintenance responsibilities are agreed with the CMF team.
- The final CI package is reviewed and ready for CMF adoption.

## Decisions

No task-force decisions have been recorded. Decisions must link to the meeting record or proposal update where they were agreed.

## Action Items

All action items must be created and tracked in the [RDK Central Rust SIG GitHub project](https://github.com/orgs/rdkcentral/projects/119). Do not maintain a separate action-item list in this document.

## Open Questions

- Which checks must be enabled for every Rust repository created through CMF?
- Will CMF own the deployed workflow, or will the Rust SIG publish approved versions for CMF to consume?
- How will repositories request and review exceptions from mandatory checks?
- Will coverage remain advisory, or will a minimum threshold be considered later?
- How will target-device validation be associated with the GitHub CI result?
- Which repositories will be used to validate the baseline and optional checks?

## References

- [CI proposal](../proposals/ci.md)
- [Continuous Integration Guidelines](../guidelines/ci.md)
- [RDK-B Rust SIG meeting notes, 2026-09-09](../meetings/2026-09-09.md)
- [RDK Central Rust SIG GitHub project](https://github.com/orgs/rdkcentral/projects/119)
