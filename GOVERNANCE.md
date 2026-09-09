# Governance

Status: draft proposed operating model. Nothing in this document should be treated as an adopted SIG rule until it is approved and recorded by the SIG.

## Purpose

The RDK-B Rust SIG coordinates discussion, proposals, and engineering guidance for using Rust within RDK-B.

Proposed operating model pending SIG approval:

- The SIG is a technical coordination and recommendation body.
- The SIG is not intended to be a mandatory approval gate for every Rust-related implementation.
- The SIG should improve consistency, traceability, and reuse across Rust-related work in RDK-B.

Open approval points:

- Whether the SIG should operate as an advisory forum, a recommendation body, or a formal approval gate for Rust-related work in RDK-B.

## Responsibilities

Proposed operating model pending SIG approval:

- Review and refine SIG goals, scope, and priorities.
- Discuss Rust-related technical proposals and integration patterns.
- Publish engineering guidance and recommended reusable approaches.
- Track meeting decisions, action items, and task-force outcomes.
- Coordinate with related architecture, platform, and security work where needed.

Open approval points:

- Which responsibilities should be included in the SIG's initial operating model.

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

Proposed operating model pending SIG approval:

- The SIG meets biweekly for 60 minutes during the initial operating period.
- The cadence should be reviewed after the first quarter of operation.
- Meetings should have a published agenda.
- Meeting participants must state or display their affiliation and GitHub ID during SIG calls using the format `[affiliation] @github-id` or `[affiliation] name`.
- Meeting records should be stored under `meetings/`.
- Minutes should capture agenda, attendees, discussion summary, decisions, action items, open questions, and references.

Open approval points:

- Whether the initial cadence should be biweekly or monthly.
- Whether special topic meetings should be separate from the regular SIG cadence.
- Whether meeting participants must state or display their affiliation and GitHub ID during SIG calls using the format `[affiliation] @github-id` or `[affiliation] name`.

## Agenda Preparation

Proposed operating model pending SIG approval:

- The chair or co-chair publishes a draft agenda at least five business days before the meeting.
- Contributors may propose additions or changes until two business days before the meeting.
- Agenda items should be framed as discussion, decision, or status topics.
- Topics requiring a decision should include a short written problem statement before the meeting when practical.

Open approval points:

- Whether draft agendas should be published at least five business days before each meeting.
- Whether contributors may propose agenda changes until two business days before each meeting.

## Decision-Making

Proposed operating model pending SIG approval:

- The SIG should prefer rough consensus based on technical merit, portability, resource impact, and deployment practicality.
- Decisions may be made in meetings or asynchronously in Pull Requests when the context is fully documented.
- The recorded result becomes the authoritative SIG decision once it is written in meeting notes or in an approved Pull Request.
- Members of the `rust-sig-maintainers` GitHub group may approve Pull Requests based on documented SIG consensus.
- If consensus cannot be reached in a reasonable time, the SIG may use a simple vote among active contributors participating in the decision.

Open approval points:

- Whether rough consensus should be the default decision-making method.
- Whether decisions may be made asynchronously in Pull Requests when the context is fully documented.
- Whether members of the `rust-sig-maintainers` GitHub group may approve Pull Requests based on documented SIG consensus.
- Whether any decision classes require a stronger approval rule.
- How active contributors are counted for a fallback vote.

## Meeting Minutes And Action Tracking

Proposed operating model pending SIG approval:

- Meeting minutes should summarize decisions and reasoning rather than provide a transcript.
- Minutes should be published promptly after the meeting.
- Action items should include an owner, a target date when possible, and a status.
- Open questions should remain visible until resolved, deferred, or withdrawn.

Open approval points:

- Whether meeting minutes should be concise summaries stored in `meetings/`.
- Whether action items should be recorded with an owner, target date when possible, and status.

## Technical Proposals

Proposed operating model pending SIG approval:

- Proposals belong under `proposals/`.
- A proposal should be used for topics that introduce shared guidance, reusable infrastructure, or cross-team architectural impact.
- Proposal content should follow the repository guidance for problem statement, motivation, proposed approach, alternatives, integration, security, resource impact, portability, and open questions.
- A proposal should not be described as accepted until that outcome is recorded by the SIG.

Open approval points:

- Whether proposals should be used for topics that introduce shared guidance, reusable infrastructure, or cross-team architectural impact.
- Which proposal categories require live meeting review.
- Whether proposal approval requires explicit maintainer sign-off in addition to recorded SIG consensus.

## Guidelines

Proposed operating model pending SIG approval:

- Agreed engineering guidance belongs under `guidelines/`.
- Guidelines should reflect accepted SIG decisions or stable implementation experience.
- Guidelines should be revised through Pull Requests that preserve traceability to discussions, proposals, or meeting decisions.

Open approval points:

- Whether to approve `guidelines/ci.md` as the full CI guideline for Rust component repositories, including the documented Rust tooling coverage.

## Task Forces

Proposed operating model pending SIG approval:

- Focused initiatives may be organized under `task-forces/`.
- A task force may be triggered by a SIG proposal or by work already initiated in one or more components of the RDK-B stack.
- A task force may be used to document resource needs and request reserved capacity from RDKM or any other contributor.
- A task force should be created only when it has a named owner, a defined scope, an expected deliverable, and a review date.
- Task-force work should be managed through a GitHub Project created ad hoc for the SIG task-force group.
- Task forces should report outcomes back to the SIG for review and decision.

Open approval points:

- Whether task forces should be created only with a named owner, defined scope, expected deliverable, and review date.
- Whether task forces may be triggered by a SIG proposal or by work already initiated in one or more components of the RDK-B stack.
- Whether task forces may be used to document resource needs and request reserved capacity from RDKM or any other contributor.
- Whether task-force work should be managed through a GitHub Project created ad hoc for the SIG task-force group.
- Whether task-force owners must also be repository maintainers.
- How long an inactive task force remains active before it is closed or merged back into general SIG work.

## Communication And GitHub Workflow

Proposed operating model pending SIG approval:

- Use mail or Slack for exploratory topics and agenda input.
- Use Issues for action items, tracked work, and follow-up tasks.
- Use Pull Requests for governance updates, meeting records, guidelines, and proposals.
- Keep `main` as the canonical branch and use short-lived branches for changes.

Open approval points:

- Whether to use mail or Slack for exploratory topics and agenda input, Issues for action items, and Pull Requests for document changes and proposal approval.
- Whether GitHub Discussions should be enabled for this repository.
- Whether specific labels or templates are needed for SIG actions, proposals, and task forces.

## Repository Changes

Proposed operating model pending SIG approval:

- Follow the contribution process in `CONTRIBUTING.md`.
- Keep repository content concise, technical, and linked to decisions where possible.
- Preserve traceability between discussions, decisions, proposals, and action items.
