---
name: kenmark-linear-ticket-score
description: Evaluate a Linear ticket against its requirements and related GitHub pull requests, reviews, and CI, then calculate and explain a completion score out of 100. Use when asked to score a Linear ticket, check ticket readiness, or determine whether a ticket is actually done.
---

# Linear Ticket Score

## Purpose

Evaluate a Linear ticket and calculate a **completion/readiness score out of 100** based on whether the issue appears to have been properly implemented and reviewed.

The skill must inspect both:

* Linear
* GitHub

Do not calculate the score using only the Linear ticket status.

The goal is to answer:

> How close is this ticket to actually being complete?

---

# Input

The skill may receive:

* A Linear ticket URL
* A Linear ticket ID such as `KEN-164`
* A request such as:

  * "Score KEN-164"
  * "Check ticket score"
  * "Is this ticket actually done?"
  * "Give me the Linear ticket score"

---

# Investigation Process

## 1. Inspect the Linear Ticket

Retrieve the complete ticket and check:

* Ticket ID
* Title
* Description
* Acceptance criteria
* Current status
* Assignee
* Priority
* Comments
* Related issues
* Blockers
* Linked PRs
* Attachments
* Recent activity

Understand what the ticket actually requires before evaluating whether it has been solved.

---

# 2. Determine Whether the Issue Has Been Solved

Compare the ticket requirements against the implementation.

Check whether:

* The requested functionality exists
* Acceptance criteria appear satisfied
* Relevant backend work exists
* Relevant frontend work exists
* Required database changes exist
* Tests were added or updated where appropriate
* No clearly required portion of the ticket is missing

Do not mark the issue as solved simply because the Linear ticket says `Done`.

Do not mark the issue as solved simply because a PR was merged.

The implementation must reasonably correspond to the requirements in the ticket.

---

# 3. Find GitHub Pull Requests

Check:

1. PRs directly linked to the Linear ticket
2. GitHub PRs mentioning the ticket ID
3. Relevant PRs discovered from branches, commits, or related tickets

A ticket may have multiple PRs.

For example:

`KEN-164`

may contain:

* Backend PR
* Frontend PR
* Migration PR
* Follow-up fix PR

Inspect all relevant PRs before calculating the score.

---

# 4. Inspect Each Pull Request

For every relevant PR check:

* Open / Closed / Merged
* Draft / Ready
* Author
* Reviewers
* Review decision
* Approvals
* Changes requested
* General comments
* Inline review comments
* Review threads
* Resolved threads
* Unresolved threads
* CI checks
* Mergeability

---

# 5. Check Reviewer Comments Carefully

This is one of the most important parts of the score.

Inspect all GitHub review activity.

Determine:

* How many reviewer comments exist
* How many are resolved
* How many remain unresolved
* Whether any unresolved comment requests code changes
* Whether changes were requested
* Whether the reviewer subsequently approved the PR

Do not consider a PR fully complete when meaningful reviewer comments remain unresolved.

Differentiate between:

### Blocking review comments

Examples:

* Bug identified
* Required implementation missing
* Logic needs to change
* Security issue
* Test missing
* Reviewer explicitly requested changes

### Non-blocking comments

Examples:

* Optional suggestion
* Style suggestion
* Question already answered
* Nitpick
* Future improvement

Blocking unresolved comments should have a larger effect on the score.

---

# Scoring System

Maximum score:

# 100 / 100

The score must be calculated using the following categories.

---

## A. Ticket Requirements Satisfied — 40 points

### 40 points

All meaningful requirements and acceptance criteria appear implemented.

### 30 points

Most requirements implemented, but minor work remains.

### 20 points

Ticket is partially implemented.

### 10 points

Only a small portion of the ticket has been implemented.

### 0 points

No meaningful implementation found.

---

# B. GitHub PR Linked / Found — 15 points

### 15 points

At least one relevant GitHub PR is directly linked to the Linear ticket.

### 10 points

A relevant PR exists and references the ticket ID but is not formally linked in Linear.

### 5 points

Implementation appears to exist, but its connection to the ticket is unclear.

### 0 points

No relevant PR found.

If multiple PRs are required, evaluate whether all required PRs are present.

---

# C. Pull Request State — 15 points

### 15 points

All required PRs are merged.

### 12 points

PR is approved and ready to merge.

### 8 points

PR is open and actively under review.

### 5 points

PR is still a draft.

### 0 points

Relevant PR was closed without being merged and no replacement was found.

For multiple PRs, score according to the overall implementation state.

---

# D. Reviewer Comments — 20 points

### 20 points

No unresolved meaningful reviewer comments.

### 15 points

Only minor or non-blocking unresolved comments remain.

### 10 points

One meaningful reviewer comment remains unresolved.

### 5 points

Multiple meaningful reviewer comments remain unresolved.

### 0 points

Changes have been requested and major reviewer concerns remain unresolved.

The skill must report the exact number of unresolved review threads whenever possible.

Example:

> 3 unresolved review threads remain.

---

# E. CI / Automated Checks — 10 points

### 10 points

All required CI checks pass.

### 7 points

Checks are still running but no failures exist.

### 5 points

Only optional/non-critical checks are failing.

### 0 points

Required CI checks are failing.

If no CI exists, state:

> CI status unavailable.

Do not assume passing CI.

---

# Score Formula

Calculate:

`Ticket Requirements + PR Linkage + PR State + Reviewer Comments + CI`

Maximum:

`40 + 15 + 15 + 20 + 10 = 100`

---

# Score Interpretation

Use these labels only for engineering workflow status:

### 90–100 — Ready / Complete

Implementation appears complete and no significant review or CI issues remain.

### 75–89 — Nearly Complete

Most work is complete but some review, CI, or minor implementation work remains.

### 50–74 — In Progress

Significant work or review remains.

### 25–49 — Early Implementation

Only part of the ticket appears implemented.

### 0–24 — Not Implemented / Major Gaps

Little or no valid implementation was found.

---

# Important Score Caps

Certain conditions should prevent a misleadingly high score.

## Unresolved blocking review comments

If a PR has unresolved **blocking** reviewer comments:

**Maximum score: 84**

Even if everything else is complete.

---

## Changes Requested

If the latest formal review state is:

`CHANGES_REQUESTED`

**Maximum score: 79**

until those changes are addressed.

---

## Required CI failing

If required CI checks are failing:

**Maximum score: 79**

---

## Missing required implementation

If an important part of the ticket is clearly missing:

**Maximum score: 74**

Example:

The backend PR is merged but the ticket explicitly requires frontend behaviour and no frontend implementation exists.

---

## No PR

If implementation requires code changes but no relevant PR can be found:

**Maximum score: 49**

---

# Final Output Format

# 🎯 Linear Ticket Score

## KEN-XXX — Ticket Title

**Score: 87 / 100**

**Status: Nearly Complete**

---

## Score Breakdown

| Category            |        Score |
| ------------------- | -----------: |
| Ticket requirements |      38 / 40 |
| GitHub PR linkage   |      15 / 15 |
| PR state            |      12 / 15 |
| Reviewer comments   |      15 / 20 |
| CI checks           |       7 / 10 |
| **Total**           | **87 / 100** |

---

# 🎫 Ticket

Status:
Assignee:
Priority:

Briefly explain what the ticket requires.

---

# 🔀 Pull Requests

### PR #XXXX — PR title

Status:
Review status:
CI:
Merged: Yes / No

**Relation to ticket**

Explain what part of the ticket this PR implements.

If several PRs exist, list all relevant PRs.

---

# 💬 Review Status

**Reviewer comments:** 6
**Resolved:** 5
**Unresolved:** 1

List unresolved comments.

Example:

### Reviewer — @username

**File:** `services/timeoff/service.ts`

**Issue:**
Reviewer requested validation before creating the policy assignment.

**State:** Unresolved

**Impact:** Blocking

---

# 🧩 Ticket Completion

### Completed

* Backend implementation
* Database migration
* Tests

### Remaining

* One reviewer comment
* Final approval
* CI currently running

Only include items supported by evidence.

---

# ⚠️ Score Deductions

Explain exactly why points were lost.

Example:

* `-3` PR has not yet been merged.
* `-5` One blocking review comment remains unresolved.
* `-3` CI is still running.
* `-2` One acceptance criterion could not be confirmed.

---

# ✅ What Is Needed for 100/100

List the exact remaining actions.

Example:

1. Address the unresolved review comment from @reviewer.
2. Resolve the GitHub review thread.
3. Get PR approval.
4. Ensure all required CI checks pass.
5. Merge PR #4068.

---

# Final Assessment

Finish with a concise factual statement.

Example:

> **87/100 — Nearly Complete**
>
> The implementation appears to satisfy the majority of KEN-164. A relevant GitHub PR is linked and the implementation is under review. One blocking review thread remains unresolved and the PR has not yet been merged. Resolving the review comment, receiving approval, passing CI, and merging the PR would bring the ticket close to 100/100.

---

# Important Rules

## Do not trust Linear status alone

A ticket marked `Done` may still have:

* Open PRs
* Failed CI
* Unresolved comments
* Missing implementation

Verify independently.

---

## Do not trust PR status alone

A merged PR does not automatically mean the ticket is completely solved.

Compare the PR against the ticket requirements.

---

## Check every relevant PR

Do not stop after finding one PR.

A ticket may require changes across multiple repositories.

---

## Inspect review threads

Do not only check GitHub's overall review state.

Read review comments and determine whether meaningful unresolved threads remain.

---

## Explain every deduction

The user must be able to understand why the score is not 100.

Never return only:

> 82/100

Instead explain exactly where the missing 18 points came from.

---

## Do not invent evidence

If something cannot be verified, state:

**Unable to verify**

rather than assuming it is complete.

---

# Success Criteria

The skill is successful when the user can immediately understand:

1. Whether the ticket appears solved
2. Whether a PR exists
3. Whether the PR is correctly linked
4. Whether the PR is merged
5. Whether reviewers approved it
6. Whether unresolved review comments remain
7. Whether CI is passing
8. What is preventing the ticket from reaching 100
9. What needs to happen next
10. Why the ticket received its exact score
