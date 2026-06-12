---
status: planning
---

## Request

<verbatim-ish user ask + ticket content; update whenever the user steers>

## Decisions

<itemized list; product first, then technical; mark user calls with (USER)>

## Product Spec

<detailed contract of observable behaviors: top-to-bottom behavior, grouped by area, most visible first>

## High Level Technical Spec

<high level spec of how internals change to support product changes, grouped by package, most extensive changes first>

## Subtasks

<a detailed split into small subtasks that each result in a self-contained reviewable commit; subtask executing marked with [>] on start>

1. [x] Do this and that

    Do this in corethis and implement thatutil.That. <The instructions specific to this subtask, as you're going to use to guide implementation.>

2. [>] Change these and those to those and these

3. [ ] Frubbernate all of those


## Report

<this will be the final report; fill in as you go, and fully reconsider and rewrite after ending>

### High-level overview of completed result

<Introduce what you have done, and how it works -- which parts of user request were implemented, and how>

### Bonus scope

- <highlight anything done that wasn't part of user request, but was implemented additionally>

### Dependency changes

- <list of all changes outside of this repository, and dependencies added/upgraded, if any>

### Screenshots

<a very good set of screenshots showing any new or modified UIs in all relevant states, cropped to focus on the relevant areas>

### Remaining work

<what was not done, proposals written for the future, etc>

### Commits

- <list of all commits made>

### How it works

<Write a detailed explanation for the developer USER how the implementation works, so that user understands nuts and bolts inside. This must go top-down, and explain conceptual changes, data flows, code flows, edge cases, domain model, synthetic objects and abstractions invented, and which existing abstractions have been extended and why. This must be skimmable, easy to read, starting with very high level and going to interim and lower level details. Where appropriate and if recorded, this includes the decisions made and their rationale. Highlight and call out anything that's controversial, counter-intuitive or untypical in our codebase. Keep simple for a small change. Feel free to group this into subsections for larger changes.>

1. Frubernation happens during redemption, right after generating the discount code.

2. The result of a successful frubernation is stored in logical coupon details, next to physical coupons.

3. Frubernation can fail. A failure **does NOT cause entire redemption to fail**, because it's too late to cancel the code at that time. Instead, we schedule a retry job that will make 5 more attempts to frubernate. This introduces a weird flow that's unlike anything in the app, but was the only way to handle failures reasonably. We found no ways to rearrange the existing flows around it.

4. Frubernation status holds our internal ID, provider ID, status, and the error returned by provider if any.

5. Frubernation results are returned by the provider asynchronously as a webhook, which is automatically created during SetupShop phase and handled via normal integration webhook routing.

<...lower level details, like package layout, which internal API hooks integration uses, ...>

## Subtask Technical Specs

### 1. Do this and that

<detailed itemized spec of how internals look like after implementation; this is updated as implementation actually happens>

### 2. Change these and those to those and these

<detailed itemized spec of how internals should look like, planned out in detail when starting a subtask>

### 3. Frubbernate all of those

<rough itemized spec of how internals change for future tasks; detailed planning can wait until we start that subtask, but we should have outline of changes beforehand>

## Log

<log everything that happens, one line per event, oldest first, newest last, written as it happens: research done, spec approved, subtask started/done (commit hash + gate stats), review round failed and why, validation failed, escalation investigated, user steering, decision changed, blocker hit and resolved, mention details>

- User request recorded
- Research done: .tasks/YYYYMMDD-<task>-research.md
- Subtask 1 “Do this and that” planning done
- Subtask 1 “Do this and that” coding done
- Subtask 1 “Do this and that” browser testing done: .tasks/YYYYMMDD-<task>-evidence/001-that.png
- Subtask 1 “Do this and that” checks failed: foo, fubar
- Subtask 1 “Do this and that” fixes done
- Subtask 1 “Do this and that” checks passed
- Subtask 1 “Do this and that” reviewer flagged issue: <issue description>
- Subtask 1 “Do this and that” fixes done
- Subtask 1 “Do this and that” checks passed
- Subtask 1 “Do this and that” reviewer passed
- Subtask 1 “Do this and that” committed: <commit>
