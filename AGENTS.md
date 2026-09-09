# AGENTS.md

## Repository Purpose

This repository is the coordination and documentation space for the RDK-B Rust Special Interest Group.

The repository contains:
- SIG scope and goals
- governance and contribution rules
- meeting agendas and minutes
- technical proposals
- engineering guidelines
- task-force coordination

Implementation code for individual RDK-B components should normally live in separate repositories.

## Repository Structure

- `README.md` - SIG purpose, scope, and goals
- `meetings/` - agendas and meeting records
- `proposals/` - technical proposals
- `guidelines/` - agreed Rust engineering guidance
- `task-forces/` - focused SIG initiatives
- `GOVERNANCE.md` - SIG governance
- `CONTRIBUTING.md` - contribution process

## Working Principles

When modifying this repository:

- Keep documentation concise and technical.
- Prefer standard Linux interfaces and portable solutions.
- Treat Rust as an additional engineering tool, not as a mandate to rewrite C/C++ components.
- Consider embedded-system constraints such as CPU, DRAM, storage, and startup time.
- Favor reusable infrastructure over duplicated implementations.
- Keep vendor-specific dependencies to a minimum where standard Linux interfaces are available.
- Preserve traceability between discussions, decisions, proposals, and actions.

## Meetings

Meeting files should use:

1. Agenda
2. Attendees
3. Discussion
4. Decisions
5. Action Items
6. Open Questions
7. References

Meeting minutes should summarize decisions and reasoning rather than reproduce the discussion as a transcript.

Use filenames such as:

`meetings/2026-09-21.md`

## Proposals

Technical proposals should clearly describe:

- Problem
- Motivation
- Proposed approach
- Alternatives considered
- RDK-B integration
- Security considerations
- Resource impact
- Portability considerations
- Open questions

Do not describe a proposal as an accepted SIG decision unless that decision is recorded in the repository.

## Git Workflow

- `main` is the canonical state of the SIG.
- Use short-lived branches.
- Use Pull Requests for changes.
- Prefer names such as:
  - `meeting/2026-09-21`
  - `proposal/rust-logging`
  - `guideline/crate-selection`

## Contributions

Repository content is governed by the repository license and standard RDK contribution process.

Do not add separate licensing or copyright headers to Markdown files unless required by repository policy or tooling.
