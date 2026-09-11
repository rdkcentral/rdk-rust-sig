# Governance

Status: partially approved operating model. Adopted points are recorded in [`meetings/2026-09-09.md`](meetings/2026-09-09.md); unresolved items remain listed as open approval points.

## Purpose

The RDK-B Rust SIG coordinates discussion, proposals, and engineering guidance for using Rust within RDK-B.

Recorded SIG decision, 2026-09-09:

- The SIG is a technical coordination and recommendation body.
- The SIG is not intended to be a mandatory approval gate for every Rust-related implementation.
- The SIG should improve consistency, traceability, and reuse across Rust-related work in RDK-B.
- The SIG may define stronger rules in the future when those rules are reviewed and approved by the SIG.


## Responsibilities

Recorded SIG decision, 2026-09-09:

- Review and refine SIG goals, scope, and priorities.
- Discuss Rust-related technical proposals and integration patterns.
- Publish engineering guidance and recommended reusable approaches.
- Track meeting decisions, action items, and task-force outcomes.
- Coordinate with related architecture, platform, and security work where needed.


## Membership And Participation

Recorded SIG decision, 2026-09-09:

- Participation is open to contributors interested in Rust in the RDK-B ecosystem.
- Contributors who need access to the `rust-developers` GitHub group should open a `CMF_support` ticket requesting inclusion.
- The SIG may maintain an informal active-contributor set based on recent meeting participation, repository activity, or proposal review.
- The technical project manager should collect a list of interested participants and contact relevant companies.
- Contributors should keep discussion technical, concise, and traceable.
- Decisions, proposals, and action items should be recorded in this repository.

## Roles

Recorded SIG decision, 2026-09-09:

- A chair is responsible for meeting facilitation, agenda coordination, and decision confirmation.
- A co-chair provides backup coverage and helps maintain meeting continuity and logistics.
- A technical project manager arranges meetings, helps the co-chair with logistics, and formalizes communications across the group.
- The chair, co-chair, and technical project manager serve as repository maintainers and are responsible for keeping agendas, minutes, proposals, and governance documents current.
- Chair and co-chair roles should be ongoing positions that continue until the current role holder steps down or is no longer able to serve.
- These roles support the SIG process and do not replace technical consensus.

Initial role assignments:

| Role | Assignee |
| --- | --- |
| Chair | Justin |
| Co-chair | Jose |
| Technical Project Manager | Danny |

Open approval points:

- How the SIG records a role transition when a chair or co-chair steps down (PSC consultation with a proposal).

## Meetings

Recorded SIG decision, 2026-09-09:

- The SIG meets biweekly for 60 minutes during the initial operating period.
- Meetings may be cancelled when there is no agenda content.
- The meeting cadence should be reviewed next year.
- Meetings should have a published agenda.
- Meeting participants must state or display their affiliation and GitHub ID during SIG calls using the format `[affiliation] @github-id` or `[affiliation] name`.
- Meeting records should be stored under `meetings/`.
- Minutes should capture agenda, attendees, discussion summary, decisions, action items, open questions, and references.

## Agenda Preparation

Recorded SIG decision, 2026-09-09:

- The chair or co-chair publishes a draft agenda at least five business days before the meeting.
- Contributors may propose additions or changes until two business days before the meeting.
- Contributors may raise agenda topics through Slack or the proposal flow.
- Agenda items should be framed as discussion, decision, or status topics.
- Topics requiring a decision should include a short written problem statement before the meeting when practical.

## Decision-Making

Recorded SIG decision, 2026-09-09:

- The SIG should prefer rough consensus based on technical merit, portability, resource impact, and deployment practicality.
- Decisions may be made in meetings or asynchronously in Pull Requests when the context is fully documented.
- The recorded result becomes the authoritative SIG decision once it is written in meeting notes or in an approved Pull Request.
- Members of the `rust-sig-maintainers` GitHub group may approve Pull Requests based on documented SIG consensus.
- Important decisions may use a formal vote or agenda straw ballot when rough consensus is not sufficient.


## Meeting Minutes And Action Tracking

Recorded SIG decision, 2026-09-09:

- Meeting minutes should be tracked in Git.
- Meeting minutes should summarize decisions and reasoning rather than provide a transcript.
- Minutes should be published promptly after the meeting.
- Action items should include an owner, a target date when possible, and a status.
- Open questions should remain visible until resolved, deferred, or withdrawn.


## Technical Proposals

Recorded SIG decision, 2026-09-09:

- Proposals belong under `proposals/`.
- A proposal should be used for topics that introduce shared guidance, reusable infrastructure, or cross-team architectural impact.
- Proposals are the trigger documents for new technical projects to be discussed in SIG meetings and for starting task-force groups.
- The SIG should use a standard template for new proposals `proposals/TEMPLATE.md`.
- Proposal content should follow the repository guidance for problem statement, motivation, proposed approach, alternatives, integration, security, resource impact, portability, and open questions.
- A proposal should not be described as accepted until that outcome is recorded by the SIG.


## Guidelines

Recorded SIG decision, 2026-09-09:

- Agreed engineering guidance belongs under `guidelines/`.
- Guidelines should reflect accepted SIG decisions or stable implementation experience.
- Guidelines should be revised through Pull Requests that preserve traceability to discussions, proposals, or meeting decisions.


## Task Forces

Recorded SIG decision, 2026-09-09:

- Focused initiatives may be organized under `task-forces/`.
- Task forces should report outcomes back to the SIG for review and decision.
- The SIG should prepare a standard template for task-force records.
- A task force may be created when triggered by a SIG proposal or by work already initiated in one or more components of the RDK-B stack.
- A task force may document resource needs and request reserved capacity from RDKM or any other contributor where needed.
- Task-force work should be managed through a GitHub Project created ad hoc for the SIG task-force group.
- A task force must have a named owner, a defined scope, an expected deliverable, and a review date.
- A task force may own dedicated repositories when its work includes component development or other implementation artifacts that do not belong in the SIG coordination repository.
- A task force is concluded when its defined work and expected deliverables are complete.

## Communication And GitHub Workflow

Recorded SIG decision, 2026-09-09:

- Use Slack for exploratory topics and agenda input.
- Use email as a secondary communication channel.
- Use Issues for action items, tracked work, and follow-up tasks.
- Use Pull Requests for governance updates, meeting records, guidelines, and proposals.
- Keep `main` as the canonical branch and use short-lived branches for changes.


## Repository Changes

Recorded SIG decision, 2026-09-09:

- Follow the contribution process in `CONTRIBUTING.md`.
- Keep repository content concise, technical, and linked to decisions where possible.
- Preserve traceability between discussions, decisions, proposals, and action items.
