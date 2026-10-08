# RDK Observer Task Force

Status: proposed

Linked proposal: [Adopt rdk-observer-rs as an RDK-B Rust Observer Reference Architecture](../proposals/rdk-observer-rs.md)

Owner: Jose Diaz Martinez (proposed; assigned to coordinate formation)

Review date: 2026-10-15 (proposed; formation and scope review)

## Purpose

Execute the evaluation and prototype work defined by the approved RDK Observer proposal. The task force will validate a reusable observer architecture for RDK-B, compare it with existing observers, and present measured results before recommending production adoption. Proposal approval does not establish production readiness.

## Scope

- Build a simplified prototype that collects regular global resource snapshots directly from procfs.
- Evaluate bounded adaptive sampling and finer sampling granularity using a task per selected process where device budgets permit.
- Evaluate Aya/BPF lifecycle events for short-lived processes, Linux process connector events over netlink, and periodic procfs discovery as the final fallback.
- Define a versioned protobuf observation contract with units, optional fields, compatibility rules, and explicit collection-error reporting.
- Compare capabilities, metric correctness, overhead, portability, security, and integration with `meminsight` and `cpuprocanalyzer`.
- Evaluate T2-compatible JSON adapters and USP exposure requirements without coupling the common record contract to either integration.
- Validate resource use, event loss, and degraded operation on selected RDK-ARM generic-compatible reference devices.

## Out of Scope

- Mandating replacement or Rust rewrites of existing C or C++ observers.
- Production rollout before resource, security, and integration validation.
- Requiring BPF support or vendor-specific interfaces on every device.
- Implementing a backend telemetry platform or changing T2 and USP specifications.

## Expected Benefits

A shared observer architecture can reduce duplicated process discovery, sampling, and record handling. Direct Linux interface access and validated parsing can improve portability and reliability, while measured resource budgets will establish whether the implementation is suitable for constrained RDK-B devices.

## Applicability

- Target RDK-B components or domains: System and process resource observation, diagnostics, and telemetry integration.
- Target platforms or device classes: Selected RDK-ARM generic-compatible devices, with additional architectures subject to validation.
- Required operating system or Linux interfaces: Linux procfs; optional BPF tracepoints and ring buffers, or process connector events over netlink, where supported and permitted.
- Known limitations or exclusions: Periodic discovery can miss short-lived processes; event sources depend on kernel configuration and permissions. Hardware exception coverage requires separate capability review.

## Deliverables

- A reviewed architecture and capability matrix covering sampling, event sources, fallback behavior, and observer health reporting.
- A procfs prototype in the separate `rdk-observer-rs` implementation repository, followed by evaluated per-process sampling and lifecycle-event support.
- A versioned protobuf schema with sample records, parsing and compatibility tests, and documented T2 and USP adapter requirements.
- A reproducible comparison with `meminsight` and `cpuprocanalyzer`, including metric meanings, collection failures, and resource measurements.
- Reference-device validation results for CPU, DRAM, storage, startup and shutdown, sampling latency, event loss, record volume, and encoding cost.
- An RDK-B integration and security plan covering packaging, startup, configuration, permissions, transport, and maintenance ownership.
- A prototype presentation and recommendation documenting acceptance criteria, remaining gaps, and readiness for further adoption review.

## Timeline

Remaining milestone dates will be agreed once device access and resource budgets are confirmed.

| Milestone | Target Date | Output | Status |
| --- | --- | --- | --- |
| Formation review | 2026-10-15 | Owner, contributors, scope, devices, and initial budgets confirmed | proposed |
| Architecture review | 2026-10-22 | Sampling, schema, fallback, and integration approach reviewed | planned |
| Global snapshot prototype | 2026-10-29 | Procfs collection and initial resource measurements available | planned |
| Process sampling and event evaluation | To be agreed | Sampling granularity, lifecycle tracking, and fallback results recorded | proposed |
| Reference-device comparison | To be agreed | Reproducible validation and existing-observer comparison complete | proposed |
| Prototype presentation and final review | To be agreed | Findings, remaining gaps, and adoption recommendation presented | proposed |

## Workforce and Resources

- Formation coordinator and proposed owner: Jose Diaz Martinez, following the October 7 meeting action.
- Initial contributors: @torrentius, @matrixdev, @SerhiiShchudlo _(Rust RDKm team)_
- Reviewers: SIG members and relevant platform, security, and backend maintainers; participation to be confirmed.
- Estimated effort: To be agreed after prototype scope and contributor capacity are confirmed.
- Required test devices or labs: RDK-ARM generic platforms such as RBPI4 and Qemu-ARM, with documented kernel capabilities and representative process workloads.
- Required access: The `rdk-observer-rs` repository, host-side CI, RDK-B build and packaging environments, and reference-device diagnostics.

## RDK-B Integration

Implementation code will remain in the separate `rdk-observer-rs` repository. The task force will document independent service packaging and startup, process-selection policy, sampling configuration, schema ownership, transport interfaces, and operational health reporting.

The integration plan must expose collection errors, dropped events, overload, and the active fallback mode. T2-compatible JSON and USP exposure will be evaluated as explicit adapters to the common observation contract. Device-side maintenance, backend ingestion, and platform validation ownership must be agreed before recommending deployment.

## Security Considerations

Document least-privilege access to procfs, netlink, and BPF, including required capabilities and service isolation. Validate collected input, bound records and buffers, and report malformed or unavailable data explicitly. Limit process details to approved observations and define which data may leave the device. Any transport must provide device authentication, integrity, and confidentiality.

## Resource Impact

Measure CPU, DRAM, storage, network volume, startup and shutdown costs, and encoding overhead under representative workloads. Include per-process task, event handling, buffering, and observer self-overhead. Agree sampling bounds, backpressure behavior, and device budgets before adoption; no production resource savings are assumed without measurements.

## Portability Considerations

Prefer direct procfs reads and standard, maintained Rust crates over shell pipelines or vendor-specific dependencies. Detect event-source capabilities at runtime and preserve periodic procfs operation when BPF or process connector events are unavailable. Record tested kernels, architectures, toolchains, permissions, and device configurations, including limitations of each fallback mode.

## Success Criteria

- Ownership, contributor capacity, reference devices, and acceptance budgets are agreed.
- The global snapshot prototype runs on selected devices with documented metric units and collection-error behavior.
- Per-process sampling and lifecycle-event approaches have measured overhead, latency, event loss, and fallback results.
- The observation schema and integration requirements are reviewed, with USP as the primary focus and T2 compatibility considered where required.
- Existing-observer comparisons use documented workloads and reproducible measurements.
- Security, portability, resource limits, and remaining production gaps are recorded.
- The SIG reviews the prototype and records a recommendation and any further work required before adoption.

## Decisions

- [2026-10-7 SIG meeting](../meetings/2026-10-7.md#decisions): The RDK Observer proposal was accepted and an initial task force is to be formed. Jose was assigned to propose its formation during the following week.

No further task-force decisions have been recorded. Proposed ownership, review dates, and milestones require confirmation; subsequent decisions must link to the meeting record or proposal update where they were agreed.

## Action Items

All action items must be created and tracked in the [RDK Central Rust SIG GitHub project](https://github.com/orgs/rdkcentral/projects/119). Do not maintain a separate action-item list in this document.

## Open Questions

- Which reference devices, kernels, workloads, and resource budgets define acceptance?
- Which observations can be missed during sampling, fallback, or overload, and what loss is acceptable?
- Which observations may leave the device, and what privacy and retention controls are required?
- What failure-event and hardware-exception coverage is feasible through the selected interfaces?
- Who will own device packaging, schema evolution, backend integration, and ongoing maintenance?

## References

- [RDK Observer proposal](../proposals/rdk-observer-rs.md)
- [RDK-B Rust SIG meeting notes, 2026-10-7](../meetings/2026-10-7.md)
- [rdk-observer-rs architecture discussions](https://github.com/rdkcentral/rdk-observer-rs/tree/adr-discussions)
- [meminsight](https://github.com/rdkcentral/meminsight)
- [cpuprocanalyzer](https://github.com/rdkcentral/cpuprocanalyzer)
- [meta-rdk-bsp-arm Hardware Support Table](https://github.com/rdkcentral/meta-rdk-bsp-arm/wiki/Hardware-Support-Table)
- [RDK Central Rust SIG GitHub project](https://github.com/orgs/rdkcentral/projects/119)
