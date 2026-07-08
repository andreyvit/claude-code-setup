---
name: zoo-refactoring
description: Classify and route Zoo refactorings, local structural cleanups, and cross-cutting changes to proposals, separate subtasks, or current-task work. Use in Zoo workflows, planning, implementation, tests, and reviews when a needed/suggested change affects pre-existing code outside active task, creates broad mechanism, touches many call sites/files, changes subsystem behavior, introduces one-off local pattern for broader concern, or expands approved scope.
---

Follow `.zoo/zoo.md` if exists.

Use before cross-cutting work outside active task, or cleanup that makes touched code use a better-but-different pattern than similar code.

Principles:
- Keep current task simple. Use existing patterns when they fit; improve shared patterns when inadequate; invent abstractions only when existing options fail.
- Include changes only when requested, approved, natural, or technically blocking with one reasonable path.
- Proposal required for surprising/non-blocking/multi-approach expansion, or significant scope expansion needing approval: new jobs, persisted state/migrations, settings, public/API contracts, broad behavior, ops dashboards/recovery, etc.
- Prefer separate subtasks/commits for reviewable/testable broad work.
- Do not classify by file count alone. Small nearby edits usually current; broad mundane edits separate; local special fix for system-wide pattern proposal.
- Do not ignore required changes; route and explain.

Classify exactly one way:

## Proposal

Use for consequential changes needing human approval:
- new/material product behavior across subsystem
- jobs, storage, migrations, settings, admin/support surfaces, metrics, recovery/rollback, security, rate limiting, permission, privacy, deploy, scale, upload, rendering, streaming, API, rollout
- architecture/layering or future-code-writing changes
- broad security/data/compat/ops remediation
- local cleanup that solves system-wide framework/pattern problem only for current path

Action: write zoo-proposal; do not implement in current task; report proposal path/finding; record in `Refactorings` if spec exists. Proposal closes finding unless technically blocking. If blocking, mark task blocked pending approval.

## Separate Subtask/Commit

Use for broad ordinary work:
- shared function/interface signature across many call sites
- mechanical moves/helper extraction/package moves
- test helper cleanup/call-site migration
- low-consequence changes whose risk is diff pollution/review separation

Orchestrator: add/promote subtask; stash active work only if needed; complete/commit separately; record in `Refactorings`.

Delegated agent: report `Refactoring request` with classification, title, before/after active subtask, blocker/stash assessment, problem, scope/non-goals, done condition, outline, validation.

## Current Task

Use for small low-risk work: helper for current/nearby callers, small arg/adapter, names/structure in touched files. Threshold is review clarity: 3-5 simple call sites usually current; 20+ usually separate; ~10 depends.

Blocking = no reasonable subset can use existing mechanism. Serious security/design issue may block shipping, but not current code if larger fix can be proposed.

Ownership:
- Orchestrator owns workflow state/spec/subtasks/final routing/agent coordination. Consume classification directly; update state; do not write `Refactoring request` to self.
- Delegated agent owns no workflow state unless prompt says so. Do not edit spec, reorder subtasks, stash, switch tasks, commit, or implement separate cross-cutting work.

Output: classification, blocker assessment, required action, how current task continues.

For review findings, do not expand current task with significant cross-cutting work. Route proposal/subtask.
