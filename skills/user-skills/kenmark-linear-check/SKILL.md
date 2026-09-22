---
name: kenmark-linear-check
version: 1.0.0
category: issues
scope: universal
phase: verify
description: "Reconstruct the complete state of a Linear ticket by checking requirements, activity, related work, pull requests, reviews, CI, blockers, and remaining actions."
triggers:
  - kenmark-linear-check
  - check a Linear ticket
  - investigate a Linear ticket
  - analyze this Linear issue
  - what remains on this ticket
  - reconstruct ticket context
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - Web
  - AskUserQuestion
risk: read-only
disable-model-invocation: false
---

# Kenmark Linear Check

Investigate a Linear ticket deeply enough to explain what it requires, what has
already been done, what remains, and what the user should do next. Produce an
evidence-backed report rather than a restatement of the ticket description.

## Inputs

Accept any of the following:

- A Linear issue URL
- A Linear issue identifier
- A request to inspect the user's assigned ticket when Linear context makes the
  ticket identifiable

If no ticket can be identified, ask for the URL or identifier. Do not guess.

## Investigation workflow

### 1. Read the complete Linear issue

Record the issue identifier, title, description, status, priority, assignee,
creator, team, project, initiative, cycle, labels, dates, due date,
attachments, external links, and linked documents.

Read the full description and separate:

- Explicit requirements
- Acceptance criteria
- Constraints and edge cases
- Requirements inferred from surrounding context
- Points that need clarification

Consider frontend, backend, API, database, migration, permissions,
authentication, security, testing, compatibility, notifications, jobs,
integrations, analytics, audit logs, and documentation only when the ticket or
linked evidence supports them. Mark unsupported possibilities as potential
considerations, not requirements.

### 2. Read activity and comments

Inspect comments and history when available. Capture decisions, scope changes,
assignment or priority changes, status changes, added or removed links, and
newly created sub-issues. Attribute important decisions to the person and date
when available.

### 3. Follow Linear relationships

Inspect the parent issue, child and sub-issues, related issues, blockers,
blocking issues, and duplicates. Explain why each relationship matters and
whether the related work is complete, active, or unresolved. If a duplicate
exists, inspect the canonical issue.

### 4. Find all relevant pull requests

Search both links from Linear and GitHub references to the issue identifier.
Include open, merged, closed, superseded, prerequisite, and follow-up PRs.
For each relevant PR, record:

- Repository, number, title, URL, author, dates, source branch, and target branch
- Draft/ready state and open/closed/merged state
- Review decision, requested reviewers, approvals, requested changes, and
  unresolved comments
- Mergeability and check/CI state
- Changed files and the application layers affected

Do not treat a linked PR as proof that the ticket is complete. Classify its
relationship as full implementation, partial implementation, prerequisite,
follow-up, abandoned attempt, superseded attempt, tests-only, backend-only,
frontend-only, database/migration, infrastructure, or regression fix.

### 5. Compare requirements with implementation

Inspect the implementation of relevant PRs when accessible. Map each explicit
ticket requirement to the files, feature, or PR that addresses it. Identify
missing, partial, or unverified requirements. Review tests, migrations, API
changes, UI changes, business rules, and compatibility implications.

### 6. Reconcile the current state

Determine the factual state using both Linear and GitHub. Use precise states
such as no implementation found, started, partially implemented, complete but
under review, merged with follow-up work, blocked, waiting for review, changes
requested, or waiting for CI. Call out inconsistencies such as a completed
Linear status with an open PR or an in-progress Linear status with merged code.

## Evidence and uncertainty

Prefer direct links and concrete facts. Distinguish:

- **Confirmed:** directly stated or observable
- **Likely:** supported by implementation or surrounding evidence
- **Needs clarification:** ambiguous or unsupported

Never invent issue identifiers, PRs, reviewers, CI results, requirements, or
next actions. If access is unavailable, state exactly what could not be
verified.

## Report format

Use this structure unless the user requests another format:

```markdown
## Ticket

**[ISSUE-ID] — Title**

Status:
Priority:
Assignee:
Project:
Cycle:

### What this ticket is asking for

Concise explanation of the actual requirement.

## Requirements

### Backend
- ...

### Frontend
- ...

### Database
- ...

### Tests
- ...

### Other
- ...

## Related Linear Work

For each meaningful parent, child, related, blocking, blocked-by, or duplicate
issue, explain the relationship and its current state.

## Pull Requests

For each relevant PR, summarize metadata, relation to the ticket,
implementation, reviews, unresolved comments, CI, and merge state.

## Requirement → Implementation Mapping

| Ticket requirement | Implementation | PR | State |
| --- | --- | --- | --- |
| ... | ... | ... | Done / Partial / Missing / Unverified |

## Blockers

List actual blockers, or state **No active blockers found.**

## Outstanding Items

Separate confirmed remaining work from possible or clarification-dependent work.

## Activity Timeline

Include only meaningful dated events.

## Current State

Give a concise factual conclusion.

## Next Actions

List concrete actions supported by the evidence. Do not add filler actions.
```

The report must answer, when the available data allows it:

1. What is being built and why?
2. What related work and blockers exist?
3. What implementation and PRs exist?
4. What is the status of reviews and CI?
5. Which requirements are complete, partial, missing, or unverified?
6. What should happen next?
