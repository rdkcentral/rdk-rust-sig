# Adopt rdk-observer-rs as an RDK-B Rust Observer Reference Architecture

Status: Approved

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
fall back to Linux process connector events over netlink when BPF is unavailable.
If neither event source is usable, it would fall back to periodic procfs
discovery. Observation records would use a
versioned protobuf schema with defined units, optional fields, and compatibility
rules.

eBPF adds programmable filtering, richer event data, and access to additional
tracing hooks. Both eBPF and netlink provide event-driven lifecycle tracking.
eBPF could improve efficiency by filtering for selected processes before emitting
records, reducing event traffic and userspace processing. A memory-mapped
could also reduce event transfer overhead. 

The reference devices for prototype validation will be RDK-ARM
generic-compatible devices listed in the
[meta-rdk-bsp-arm Hardware Support Table](https://github.com/rdkcentral/meta-rdk-bsp-arm/wiki/Hardware-Support-Table).
Validate the prototype on these devices, measuring resource overhead, sampling
latency, event loss, record volume, and encoding cost before recommending
production adoption.

### BPF-to-netlink fallback

Evaluate Linux process connector events over `NETLINK_CONNECTOR` as the
intermediate fallback between Aya/BPF and periodic procfs discovery. Select
the event source through runtime capability checks and successful subscription,
with the following proposed order:

1. Aya/BPF lifecycle events when the required kernel features and permissions
   are available.
2. Process connector events when BPF cannot be loaded or attached, or its event
   source fails during operation.
3. Periodic procfs discovery when neither event source is usable.

Netlink support must be validated on each target kernel, including
`CONFIG_CONNECTOR`, `CONFIG_PROC_EVENTS`, subscription permissions, and coverage
of fork, exec, and exit events. It should not be assumed to provide the richer
observations or programmable filtering available through BPF. See the
[Linux process connector configuration](https://github.com/torvalds/linux/blob/master/drivers/connector/Kconfig).

Keep periodic procfs reconciliation active with either event source to discover
existing processes and repair tracking after lost events or source transitions.
Netlink messages can be lost under memory pressure or receive-buffer overflow,
as described in the
[Linux connector documentation](https://docs.kernel.org/driver-api/connector.html#reliability).
Report the active source, fallback reason, and detected loss; account for PID
reuse when reconciling records. Validate startup failures, runtime source
failures, event bursts, transition gaps, and resource overhead on reference
devices. Procfs reconciliation cannot recover the lifecycle of a process that
starts and exits between scans.

## Alternatives Considered

During the most recent RTAB meeting, two existing projects were proposed as
alternative approaches to the `rdk-observer-rs` architecture:

- [`meminsight`](https://github.com/rdkcentral/meminsight)
- [`cpuprocanalyzer`](https://github.com/rdkcentral/cpuprocanalyzer)

The SIG should compare these projects with this proposal for supported
capabilities and metrics, integration requirements, resource overhead,
portability, security, production readiness, maintainability, testing, and
sound coding practices before making a recommendation.

The analysis should include the issues observed in the current command-driven
implementations:

- every sample can create extra child processes;
- results depend on the shell, `PATH`, and the exact BusyBox version;
- parsers depend on the text output of external commands;
- errors from child commands are not always checked; and
- commands built from input can be unsafe.

These issues can make CPU overhead much higher than the cost of a small
single-purpose binary. They also make behavior harder to reproduce across
RDK-B devices with different BusyBox builds, shell behavior, and available
command options.

The review should also account for memory-correctness issues found during
analysis of the existing implementations:

- `ReadProcessName()` looks for `(` without checking that it was found, so a
  malformed string can cause an out-of-bounds access.
- `ReadProcStat()` checks `fscanf()` only for `EOF`, rather than checking the
  number of fields read successfully.
- `GetMemParams()` and `GetUsedMemory()` can continue with zero or incomplete
  values when expected fields are missing.
- `GetValuesFromFile()` checks `strValue[strlen(strValue)]`, which is already
  the terminating `\0`, not a newline.
- `OutFilename()` passes the `sizeof` of another local buffer instead of the
  actual destination buffer size.
- `monitorSysLevel` can remain `true` after the first missing process list and
  change the behavior of later iterations.
- the `Idle%` field in `loadandmem.data` is filled by `GetUsedPercent()`, so
  the field name may not match its value.
- `MonitorAllProcess=1` is not compatible with BusyBox `ps` syntax, but this
  condition is not returned to the user as a failure.

A Rust implementation can improve memory correctness by parsing procfs and
system files with bounded string handling, typed results, explicit error
propagation, checked optional fields, and tests for malformed input. It should
prefer direct reads from stable Linux interfaces over shell pipelines, and it
should treat missing fields, unsupported BusyBox syntax, child-process
failures, and malformed records as explicit errors or degraded-mode states.

### Continue with current independent observers

The current `meminsight` and `cpuprocanalyzer` approaches appear to overlap
with each other in resource observation and analysis. Continuing to develop
them independently could duplicate implementation and platform validation
work, and lead to inconsistent records, interfaces, and operational behavior.

Based on the analysis above, deploying the current implementations unchanged
could also harm production reliability. Repeated child-process creation adds
CPU and transient memory overhead on constrained devices and can distort the
resource measurements themselves. Shell and BusyBox dependencies can cause
device-specific failures, while unchecked command failures, incomplete parsing,
and persistent sampling state can silently produce missing or misleading
telemetry. Buffer-handling defects can cause invalid memory access or observer
crashes, and commands constructed from input can introduce command-injection
risks when that input is not trusted.

Continuing with these observers would therefore require fixing the identified
defects, reporting collection failures explicitly, validating metric meanings,
and measuring overhead under representative production workloads. Direct procfs
reads and validated parsing could improve the existing implementations; a Rust
implementation could additionally use safe buffer handling and typed errors to
address memory-safety risks. Rust alone does not correct metric semantics or
sampling-state errors, so these behaviors still require explicit validation and
tests before production adoption.

### Use a text-based observation format

The current approaches appear to rely on logging records in text-based formats.
For example, `meminsight` supports CSV reports and optional JSON output for
memory and CPU records. The JSON representation may be useful for compatibility
with the existing T2 telemetry component, but it should be treated as an
integration-specific format rather than the common observer contract if it is
not compatible with USP data modeling, transport, or encoding requirements.

Although text and JSON are easy to inspect, they can be less efficient to encode
and process, and can increase storage, payload size, and transmission
requirements. The proposal should move to a versioned protobuf observation
format as the internal and transport-neutral record contract, then define
explicit adapters for T2-compatible JSON and USP-compatible exposure where
required.

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
inventory, and evaluate process connector events over netlink when Aya,
required tracepoints, or the BPF ring buffer cannot be used. Preserve operation
with periodic procfs discovery when neither event source is usable.

## Security Considerations

Use least privilege and document BPF and process connector permissions,
ownership, and service isolation. BPF should emit only minimal lifecycle events;
transport must provide device authentication, integrity, and confidentiality. Validate procfs input,
bound record sizes, handle malformed data, and avoid exposing sensitive process
details without approval.

## Resource Impact

Measure CPU, DRAM, storage, network, startup, and shutdown costs, including
worker, procfs, BPF, buffering, and record-encoding overhead. Configure sampling
bounds, event coalescing, and backpressure, and establish device budgets before
adoption.

## Portability Considerations

Target Linux RDK-B devices using standard procfs interfaces and Rust crates.
Detect unsupported kernel, BPF, Aya, and ring-buffer features and evaluate
process connector events over netlink before falling back to periodic procfs
discovery. Validate connector support and permissions as well as supported
SoCs, kernels, architectures, toolchains, and process counts.

## Next Steps

1. Start with a simplified prototype that collects regular global snapshots
   through procfs.
2. Evaluate finer sampling granularity using a task per selected process, where
   feasible within target-device resource budgets.
3. Add BPF lifecycle events to detect short-lived processes and evaluate process
   connector events over netlink as the intermediate fallback, retaining
   periodic procfs discovery and reconciliation in all modes.
4. Compare `meminsight` output with the prototype record contract, including
   T2-compatible JSON output and USP integration requirements.
5. Present prototype.

## Open Questions

- Which observations can be missed during sampling, fallback operation, or
  overload, and what loss is acceptable?
- Which observations may leave the device, in terms of privacy and data protection?
- What resource budgets and workloads define acceptance on the reference devices?
