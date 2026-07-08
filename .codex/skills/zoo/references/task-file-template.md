---
status: planning
---

## Request

<verbatim-ish user ask + ticket content; update whenever the user steers>

## What Happened

<clearly explain WHAT undesirable behavior is happening, WHY/HOW it is happening, impact, is it a regression or was it always broken, WHO and WHY and WHEN introduced a change that broke it and/or affected it>

## Spec

### Product Changes

<detailed contract of observable behaviors: top-to-bottom behavior, grouped by area, most visible first>

### How Its Gonna Work

<very high level overview of our technical approach>

### Persisted Data Changes

<all changes to on-disk/in-database data>

### Model changes

<all model changes not covered under Persisted Data Changes>

### UI Changes

<all changes to UI grouped by area and screen, with exact texts and labels>

### Dependency Changes

<list of all changes outside of this repository, and dependencies added/upgraded, if any>

### High Level Technical Spec

<high level spec of how internals change, grouped by package, most extensive changes first, no need for >

## Subtasks

<a detailed split into meaningful, self-contained, small subtasks, each ending in a compact reviewable commit; split unrelated features and clean incremental steps unless Decisions records a good reason; avoid micro-subtasks with no meaningful functionality step; subtask executing marked with [>] on start>

<example>
1. [x] Do this and that

    Do this in corethis and implement thatutil.That. <The instructions specific to this subtask, as you're going to use to guide implementation.>

2. [>] Change these and those to those and these

3. [ ] Frubbernate all of those
</example>


## Report

<this will be the final report; fill in as you go, and fully reconsider and rewrite after ending>

### Status

<overall task status>

<list of subtasks with their statuses>

### How it works

<Detailed dev-facing explanation. Go top-down: concept changes, data flows, code flows, edge cases, domain model, synthetic objects/abstractions, extended existing abstractions, and rationale. Make it skimmable, from high-level to lower-level details. Include recorded decisions when relevant. Highlight controversial, counter-intuitive, or unusual choices. Keep small changes simple; use subsections for larger changes.>

### Bonus scope

- <highlight anything done that wasn't part of user request, but was implemented additionally>

### Surprises

- <list everything that might be surprising about our implementation to someone who only knows the initial user request>

### Risky changes

- <list of the most risky modifications made that could cause deployment issues, performance slowdowns, break specific configurations, or otherwise highest-risk areas, to guide user's reviewing efforts>

### Dependency changes

- <list of all changes outside of this repository, and dependencies added/upgraded, if any>

### Screenshots

<inline a strong set of screenshots showing any new or modified UIs in all relevant states, cropped to focus on relevant areas>

### Commits

- <list of all commits made>


<example>
1. Frubernation happens during redemption, right after generating the discount code.

2. The result of a successful frubernation is stored in logical coupon details, next to physical coupons.

3. Frubernation can fail. A failure **does NOT cause entire redemption to fail** because code cancellation is too late. Instead, schedule retry job for 5 more frubernation attempts. This unusual flow was the only reasonable failure handling; existing flows could not be rearranged around it.

4. Frubernation status holds our internal ID, provider ID, status, and the error returned by provider if any.

5. Frubernation results are returned by the provider asynchronously as a webhook, which is automatically created during SetupShop phase and handled via normal integration webhook routing.
</example>

<...lower level details, like package layout, which internal API hooks integration uses, ...>~

### Deployment process and risks

<explain how this should be deployed safely, what risks exist -- code, migrations, effect on existing users, legacy configurations, preexisting data; explain what you did to mitigate those risks, and what still needs to be done to ensure safety>

### Remaining work

<what was not done, proposals written for the future, what cleanup steps should be taken later and where they've been recorded, and similar>

### Quick guide to publish in the ticket

<write a guide to user-visible product-level changes, to post in the ticket targeting ticket opener; include exact screens and labels/names; add QA checklist and deployment/configuration/rollout checklist; do not go code-level, focus on user-visible and product-level changes, but do consider the entire rollout and configuration story>


## Decisions

<itemized list; product first, then technical; mark user calls with (USER)>


## Subtask Technical Specs

### 1. Do this and that

<detailed itemized spec of internals after implementation; update during implementation>

### 2. Change these and those to those and these

<detailed itemized spec of target internals, planned when starting a subtask>

### 3. Frubbernate all of those

<rough itemized spec of future internal changes; detailed planning waits until subtask start, but outline exists now>

## Log

<log everything that happens, one line per event, newest last, written as it happens: research done, spec approved, subtask started/done (commit hash + gate stats), review round failed and why, validation failed, escalation investigated, user steering, decision changed, blocker hit and resolved, mention details>

<example>
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
</example>
