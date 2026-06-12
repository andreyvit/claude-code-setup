---
name: zoo-pause
description: "Pause a running Zoo workflow as soon as the current step reaches a safe handoff. Use only when explicitly requested, especially to stop after in-flight subagent work without running final Zoo Report."
---

# Zoo Pause

Read and follow `.zoo/zoo.md` if it exists.

Use this when the user explicitly asks to pause or stop a running Zoo workflow with intent to continue later.

Goal: stop quickly after the current step reaches a coherent handoff, without wasting in-flight work and without running the final `zoo-report` flow.

## Pause Rule

- Do not start new subagents, broad validation, rebase, final closeout, or `zoo-report`.
- Let already-running subagents or commands for the current step finish when their output is near-term useful.
- If a running command or subagent is clearly obsolete, destructive, or too expensive for the pause reason, stop it when the harness permits doing so safely.
- Finish only the smallest handoff needed to avoid losing state or leaving broken partial work.

## Current Step Handoff

- If waiting for delegated subagents, collect their reports and apply only mechanical workflow-state updates needed to preserve their results.
- If writing a report, finish that report and stop.
- If editing or fixing directly, stop after the current atomic edit or validation command once the workspace state is understandable.
- If the active subtask already reached its done condition before the pause request, follow the workflow's required subtask closeout only as far as needed to leave trackable workflow changes in a usable state. Do not start final task closeout.

## State To Preserve

- Zoo Heavy or Zoo Lite: update the spec file or a short Bureau `pause` report with the active subtask, last completed step, in-flight reports consumed, remaining next step, validation state, and any uncommitted trackable changes.
- Zoo Zero or another workflow without a spec: record the same handoff in the native task list and, when Bureau is active, a short `pause` report.
- Do not mark a subtask done unless all normal done conditions were already satisfied.
- Do not commit partial work solely because a pause was requested. If a commit is skipped, state exactly what remains uncommitted.

## User Rundown

Reply with a compact status summary:

- where the workflow paused
- what work completed since the last user-visible update
- what remains running or was just collected
- current validation/review status
- uncommitted changes, if any
- the exact next step to resume

Do not run `zoo-report`; that skill is only for completed Zoo workflows.
