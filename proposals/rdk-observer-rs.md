# Adopt rdk-observer-rs as an RDK-B Rust Observer Reference Architecture

Status: proposed

Type: technical

## Problem

RDK-B needs a consistent way to observe process and system resource consumption,
failure events, and hardware exceptions across target devices. Without a shared
architecture, individual components may implement different process discovery,
sampling, event capture, serialization, and backend integration approaches.
That increases duplication and makes resource usage, portability, and operational
behavior difficult to compare.

The `rdk-observer-rs` project provides a Rust-based starting point for a system
process observer. Its current repository is a project scaffold, so the SIG must
review and validate the proposed architecture before recommending adoption.

## Motivation

A common observer architecture could provide reusable process and system
observations for diagnostics, monitoring, and performance analysis while using
memory-safe Rust for new implementation work. The project also provides a
concrete basis for evaluating standard Linux interfaces and for aligning Rust
components with RDK-B resource and portability constraints.

Leaving this uncoordinated would risk multiple incompatible observers, repeated
platform integration work, inconsistent record schemas, and uneven handling of
kernel capabilities across devices.

## Proposed Approach

Evaluate `rdk-observer-rs` as a reference architecture for RDK-B observability.
It would collect system metrics from procfs, track selected processes with
bounded adaptive sampling, use Aya/BPF lifecycle events when available, and
fall back to periodic procfs discovery. Observation records would use a
versioned CBOR schema with defined units, optional fields, and compatibility
rules.

Validate the prototype on representative RDK-B devices, measuring resource
overhead, sampling latency, event loss, record volume, and encoding cost before
recommending production adoption.

## Alternatives Considered

During the most recent RTAB meeting, two existing projects were proposed as
alternative approaches to the `rdk-observer-rs` architecture:

- [`meminsight`](https://github.com/rdkcentral/meminsight)
- [`cpuprocanalyzer`](https://github.com/rdkcentral/cpuprocanalyzer)

The SIG should compare these projects with this proposal for supported
capabilities and metrics, integration requirements, resource overhead,
portability, security, production readiness, maintainability, testing, and
sound coding practices before making a recommendation.

### Continue with current independent observers

The current `meminsight` and `cpuprocanalyzer` approaches appear to overlap
with each other in resource observation and analysis. Continuing to develop
them independently could duplicate implementation and platform validation
work, and lead to inconsistent records, interfaces, and operational behavior.

### Use periodic procfs scans only

This is broadly portable and avoids a BPF dependency, but it can miss short-lived
processes and delays process lifecycle handling until the next scan. It should
remain the fallback path, not the only target architecture.

### Collect full process data in BPF

This could reduce user-space discovery latency, but it increases kernel-side
complexity, portability constraints, permissions requirements, and the risk of
placing policy and snapshot logic in the wrong layer. BPF should emit compact
lifecycle events while procfs remains the source of process snapshots.

### Use a text-based observation format

The current approaches appear to rely on logging records in a text-based format.
Although text is easy to inspect, it can be less efficient to encode and process,
and can increase storage, payload size, and transmission requirements. CBOR
should be compared with the current text-based format on target hardware before
its benefits are treated as proven.

## RDK-B Integration

The observer should run as an independently deployable RDK-B service or library
with a documented interface for process-selection policy, configuration,
transport, and lifecycle reporting. Integration should identify ownership for:

- device-side observer packaging and startup;
- backend ingestion and schema version management;
- platform-specific process-selection rules;
- kernel capability detection and BPF permissions;
- diagnostic access to health, dropped-event, and fallback status; and
- target-device performance and portability validation.

The implementation should use standard Linux interfaces through standard,
well-maintained Rust crates where available, avoid assuming a fixed device
inventory, and preserve operation with periodic procfs discovery when Aya,
required tracepoints, or the BPF ring buffer cannot be used.

## Security Considerations

Use least privilege and document BPF capabilities, ownership, and service
isolation. BPF should emit only minimal lifecycle events; transport must provide
device authentication, integrity, and confidentiality. Validate procfs input,
bound record sizes, handle malformed data, and avoid exposing sensitive process
details without approval.

## Resource Impact

Measure CPU, DRAM, storage, network, startup, and shutdown costs, including
worker, procfs, BPF, buffering, and record-encoding overhead. Configure sampling
bounds, event coalescing, and backpressure, and establish device budgets before
production adoption.

## Portability Considerations

Target Linux RDK-B devices using standard procfs interfaces and Rust crates.
Detect unsupported kernel, BPF, Aya, and ring-buffer features and fall back to
periodic procfs discovery. Validate supported SoCs, kernels, architectures,
toolchains, process counts, and storage layouts; do not infer filesystem space
from `/proc/diskstats`.

## Resulting Repository Changes

If approved, define the implementation plan, versioned schema, backend contract,
RDK-B packaging and service integration, capability documentation, target-device
benchmarks, and Rust/Yocto guidance. Record the decision and action items in the
relevant SIG meeting record.

## Next Steps

- Start a prototype of the proposed observer architecture.
- Analyze and compare it with `meminsight` and `cpuprocanalyzer`, including
	capabilities, security, resource use, portability, and production readiness.
- Prepare conclusions and recommendations for presentation at the next SIG
	meeting.

## Open Questions

- Who owns the observer, backend schema, and device integration?
- Which process-selection modes, kernel versions, and BPF capabilities are required?
- Which observations may leave the device?
- What resource budgets, transport, target devices, and workloads define acceptance?

## References

- [rdk-observer-rs](https://github.com/rdkcentral/rdk-observer-rs)
- [ADR-0001: Collect System Resource Snapshots from procfs](https://github.com/rdkcentral/rdk-observer-rs/blob/main/docs/architecture/adr/0001-collect-system-resource-snapshots-from-procfs.md)
- [ADR-0002: Schedule Per-Process Sampling Workers](https://github.com/rdkcentral/rdk-observer-rs/blob/main/docs/architecture/adr/0002-schedule-per-process-sampling-workers.md)
- [ADR-0003: Trigger Process Capture with Aya and BPF Events](https://github.com/rdkcentral/rdk-observer-rs/blob/main/docs/architecture/adr/0003-trigger-process-capture-with-bpf-events.md)
- [ADR-0004: Use CBOR for Observation Records](https://github.com/rdkcentral/rdk-observer-rs/blob/main/docs/architecture/adr/0004-use-cbor-for-observation-records.md)
- [RDK-B Rust SIG README](../README.md)
