# Governance

Status: draft proposed operating model. Nothing in this document should be treated as an adopted SIG rule until it is approved and recorded by the SIG.

## Purpose

The RDK-B Rust SIG coordinates discussion, proposals, and engineering guidance for using Rust within RDK-B.

Proposed operating model pending SIG approval:

- The SIG is a technical coordination and recommendation body.
- The SIG is not intended to be a mandatory approval gate for every Rust-related implementation.
- The SIG should improve consistency, traceability, and reuse across Rust-related work in RDK-B.

## Responsibilities

Proposed operating model pending SIG approval:

- Review and refine SIG goals, scope, and priorities.
- Discuss Rust-related technical proposals and integration patterns.
- Publish engineering guidance and recommended reusable approaches.
- Track meeting decisions, action items, and task-force outcomes.
- Coordinate with related architecture, platform, and security work where needed.

## Membership And Participation

Proposed operating model pending SIG approval:

- Participation is open to contributors interested in Rust in the RDK-B ecosystem.
- Contributors who need access to the `rust-developers` GitHub group should open a `CMF_support` ticket requesting inclusion.
- The SIG may maintain an informal active-contributor set based on recent meeting participation, repository activity, or proposal review.
- Contributors should keep discussion technical, concise, and traceable.
- Decisions, proposals, and action items should be recorded in this repository.

Open approval points:

- Whether the SIG wants a formal member list.
- How active-contributor status should be recognized.

## Roles

Proposed operating model pending SIG approval:

- A chair is responsible for meeting facilitation, agenda coordination, and decision confirmation.
- A co-chair provides backup coverage and helps maintain meeting continuity and logistics.
- A technical project manager arranges meetings, helps the co-chair with logistics, and formalizes communications across the group.
- The chair, co-chair, and technical project manager serve as repository maintainers and are responsible for keeping agendas, minutes, proposals, and governance documents current.
- For the initial operating period, chair and co-chair selection should prefer the main contributors and most senior technical members currently driving SIG formation, specifically Justin and Jose.
- Chair and co-chair roles should be ongoing positions that continue until the current role holder steps down or is no longer able to serve.
- These roles support the SIG process and do not replace technical consensus.

Open approval points:

- Whether Justin and Jose should serve as the initial chair and co-chair, or otherwise as the initial leadership pair for the SIG.
- How the SIG records a role transition when a chair or co-chair steps down (PSC consultation with a proposal).

## Meetings

Proposed operating model pending SIG approval:

- The SIG meets biweekly for 60 minutes during the initial operating period.
- The cadence should be reviewed after the first quarter of operation.
- Meetings should have a published agenda.
- Meeting records should be stored under `meetings/`.
- Minutes should capture agenda, attendees, discussion summary, decisions, action items, open questions, and references.

Open approval points:

- Whether the initial cadence should be biweekly or monthly.
- Whether special topic meetings should be separate from the regular SIG cadence.

## Agenda Preparation

Proposed operating model pending SIG approval:

- The chair or co-chair publishes a draft agenda at least five business days before the meeting.
- Contributors may propose additions or changes until two business days before the meeting.
- Agenda items should be framed as discussion, decision, or status topics.
- Topics requiring a decision should include a short written problem statement before the meeting when practical.

## Decision-Making

Proposed operating model pending SIG approval:

- The SIG should prefer rough consensus based on technical merit, portability, resource impact, and deployment practicality.
- Decisions may be made in meetings or asynchronously in Pull Requests when the context is fully documented.
- The recorded result becomes the authoritative SIG decision once it is written in meeting notes or in an approved Pull Request.
- If consensus cannot be reached in a reasonable time, the SIG may use a simple vote among active contributors participating in the decision.

Open approval points:

- Whether any decision classes require a stronger approval rule.
- How active contributors are counted for a fallback vote.

## Meeting Minutes And Action Tracking

Proposed operating model pending SIG approval:

- Meeting minutes should summarize decisions and reasoning rather than provide a transcript.
- Minutes should be published promptly after the meeting.
- Action items should include an owner, a target date when possible, and a status.
- Open questions should remain visible until resolved, deferred, or withdrawn.

## Technical Proposals

Proposed operating model pending SIG approval:

- Proposals belong under `proposals/`.
- A proposal should be used for topics that introduce shared guidance, reusable infrastructure, or cross-team architectural impact.
- Proposal content should follow the repository guidance for problem statement, motivation, proposed approach, alternatives, integration, security, resource impact, portability, and open questions.
- A proposal should not be described as accepted until that outcome is recorded by the SIG.

Open approval points:

- Which proposal categories require live meeting review.
- Whether proposal approval requires explicit maintainer sign-off in addition to recorded SIG consensus.

## Guidelines

Proposed operating model pending SIG approval:

- Agreed engineering guidance belongs under `guidelines/`.
- Guidelines should reflect accepted SIG decisions or stable implementation experience.
- Guidelines should be revised through Pull Requests that preserve traceability to discussions, proposals, or meeting decisions.

## Task Forces

Proposed operating model pending SIG approval:

- Focused initiatives may be organized under `task-forces/`.
- A task force should be created only when it has a named owner, a defined scope, an expected deliverable, and a review date.
- Task forces should report outcomes back to the SIG for review and decision.

Open approval points:

- Whether task-force owners must also be repository maintainers.
- How long an inactive task force remains active before it is closed or merged back into general SIG work.

## GitHub Workflow

Proposed operating model pending SIG approval:

- Use Discussions for exploratory topics, agenda input, and questions when available.
- Use Issues for action items, tracked work, and follow-up tasks.
- Use Pull Requests for governance updates, meeting records, guidelines, and proposals.
- Keep `main` as the canonical branch and use short-lived branches for changes.

Open approval points:

- Whether GitHub Discussions should be enabled for this repository.
- Whether specific labels or templates are needed for SIG actions, proposals, and task forces.

## Repository Changes

Proposed operating model pending SIG approval:

- Follow the contribution process in `CONTRIBUTING.md`.
- Keep repository content concise, technical, and linked to decisions where possible.
- Preserve traceability between discussions, decisions, proposals, and action items.
