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
versioned protobuf schema with defined units, optional fields, and compatibility
rules.

The reference devices for prototype validation will be RDK-ARM
generic-compatible devices listed in the
[meta-rdk-bsp-arm Hardware Support Table](https://github.com/rdkcentral/meta-rdk-bsp-arm/wiki/Hardware-Support-Table).
Validate the prototype on these devices, measuring resource overhead, sampling
latency, event loss, record volume, and encoding cost before recommending
production adoption.

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
and can increase storage, payload size, and transmission requirements. The
proposal should move to a protobuf observation format to improve portability to
USP integrations and provide a more efficient representation than JSON or plain
text.

## RDK-B Integration

The observer should run as an independently deployable RDK-B service
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
 adoption.

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

1. Start with a simplified prototype that collects regular global snapshots
   through procfs.
2. Evaluate finer sampling granularity using a task per selected process, where
   feasible within target-device resource budgets.
3. Add BPF lifecycle events to detect short-lived processes, retaining periodic
   procfs discovery as the fallback.
4. Analyze and compare the prototype with `meminsight` and `cpuprocanalyzer`,
   including capabilities, security, resource use, portability, and production
   readiness.
5. Prepare conclusions and recommendations for presentation at the next SIG
   meeting.

## Open Questions

- Who owns the observer, backend schema, and device integration?
- Which process-selection modes, kernel versions, and BPF capabilities are required?
- Which observations can be missed during sampling, fallback operation, or
  overload, and what loss is acceptable?
- Which observations may leave the device, in terms of privacy and data protection?
- What resource budgets and workloads define acceptance on the reference devices?

## References
