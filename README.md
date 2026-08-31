# RDK-B Rust SIG

The **RDK-B Rust Special Interest Group (Rust SIG)** provides a collaborative space for discussing, defining, and promoting the use of Rust within RDK-B.

The SIG is intended to bring together developers, architects, platform teams, and other contributors interested in using Rust for RDK-B system software.

The objective is not to replace existing C/C++ components by default, but to establish a clear and consistent approach for using Rust where it provides technical value.

## Scope

The Rust SIG provides a collaborative space to define how Rust can be introduced and used consistently across RDK projects.

The scope of the SIG includes:

* Define recommended Rust architecture, coding practices, and project structure.
* Identify existing RDK components and new functionality that can benefit from Rust.
* Define integration patterns between Rust and existing C/C++ RDK components.
* Establish common approaches for IPC, systemd integration, logging, configuration, telemetry, and Linux APIs.
* Recommend reusable crates and shared Rust libraries for RDK.
* Define Yocto/Cargo build, packaging, dependency management, and security practices.
* Promote the use of standard Linux interfaces such as Netlink, procfs, sysfs, D-Bus, and sockets.
* Share experience, prototypes, reference implementations, and lessons learned across teams and platforms.
* Coordinate with existing RDK architecture, security, and platform activities.
* Provide a forum for reviewing Rust-related architectural proposals and identifying opportunities for common solutions.

## Goals

The primary goal of the Rust SIG is to use Rust as an additional tool to improve the **efficiency, reliability, security, portability, and maintainability** of RDK-B software.

Specific goals include:

* Establish a clear and supported path for developing Rust components in RDK-B.
* Reduce duplication by creating reusable Rust libraries and common infrastructure.
* Encourage memory-safe implementations for new system services and platform components.
* Use Rust to build lightweight components that interact directly with standard Linux kernel and userspace interfaces.
* Demonstrate practical RDK-B use cases through reference implementations and prototypes.
* Improve portability across SoCs by reducing dependency on vendor-specific userspace interfaces where standard Linux interfaces are available.
* Define best practices for integrating Rust components with the existing RDK-B ecosystem.
* Evaluate where Rust can simplify or modernize existing RDK-B architecture without requiring unnecessary rewrites of existing components.
* Build shared knowledge and experience around Rust development within the RDK-B community.
* Provide guidance that enables different teams to build Rust components using consistent architectural and engineering practices.

## Areas of Interest

Topics discussed by the SIG may include:

* Rust architecture and component design
* Async programming and runtime selection
* Linux networking
* IPC interoperability
* systemd integration
* Logging and telemetry
* Observability
* Security and cryptographic services
* FFI and C/C++ interoperability
* Yocto and Cargo integration
* Crate selection and dependency management
* Cross-compilation and platform portability
* Testing and CI
* Resource-constrained embedded systems
* Standard Linux kernel and userspace interfaces

## Reference Projects and Task Forces

The SIG may create focused **task forces** or reference projects to explore specific use cases and demonstrate recommended approaches.

These activities can be used to validate architecture, libraries, build integration, performance, resource consumption, and portability before recommending broader adoption.

Examples may include:

* RDK observability and system resource monitoring
* Linux networking and platform management
* Common Rust infrastructure and libraries
* Yocto/Cargo integration
* Security and cryptographic service integration

Implementation projects should normally live in their own repositories, while this repository is used for SIG coordination, architecture discussions, proposals, guidelines, and meeting records.

## Meetings

SIG meetings are intended to be open technical discussions around Rust usage in RDK-B.

This repository can be used to track:

* Meeting agendas
* Meeting notes
* Technical proposals
* Architecture discussions
* Decisions and recommendations
* Task forces and reference projects
* Action items

Meeting material can be maintained under the `meetings/` directory.

## Repository Structure

```text
rdk-rust-sig/
├── README.md
├── meetings/
├── proposals/
├── guidelines/
└── task-forces/
```

## Guiding Principle

Rust should be introduced where it provides a clear engineering benefit.

The SIG should favor solutions that:

* Use standard Linux interfaces
* Minimize platform-specific dependencies
* Remain lightweight enough for embedded systems
* Integrate cleanly with existing RDK-B components
* Improve software safety and maintainability
* It can be reused across different RDK-B platforms

```
```
