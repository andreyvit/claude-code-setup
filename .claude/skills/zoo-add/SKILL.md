---
name: zoo-add
description: "Record user feedback for the active Zoo spec without interrupting current work. Use only when explicitly requested, especially when the user gives feedback to address later during Zoo Heavy, Zoo Lite, Zoo Zero, or another running Zoo workflow."
---

# Zoo Add

Read and follow `.zoo/zoo.md` if it exists.

Use this when the user explicitly asks to add feedback for later while a Zoo workflow continues.

Goal: preserve the feedback in the active workflow state without switching away from the current subtask.

## Behavior

- Top-level orchestrator: record the feedback, acknowledge it briefly, then continue the current subtask.
- Delegated agent: do not edit workflow state unless your assigned role already owns it. Include the feedback in your report or response for the orchestrator, then continue your assigned step.
- Do not start planning, review, implementation, or fixes for the new feedback immediately.
- Do not cancel, re-prompt, or replace in-flight subagents unless the feedback explicitly says their current work is harmful or obsolete.
- If the feedback cancels current work or warns about a destructive/security/data-risk issue, stop and treat it as an interrupt instead of deferring it.

## Where To Record

- Zoo Heavy or Zoo Lite: update the spec file as the single writer.
  - Add the feedback to `User Input` as a later user clarification.
  - Add or update a `(future)` subtask that addresses the feedback after the active `(next)` subtask.
  - If the feedback clearly belongs in an existing future subtask, append it there instead of creating a duplicate.
  - Leave the active `(next)` subtask and current plan untouched unless the feedback explicitly invalidates them.
- Zoo Zero or another workflow without a spec: add a native task-list item after the current item and mention the feedback in the next Bureau report or pause/status handoff.

## After The Current Subtask

- When the active subtask is completed and committed, route the recorded feedback before unrelated future work.
- Reopen planning or direct execution at that point according to the active Zoo workflow.
- Preserve the original request baseline. Do not rewrite deferred feedback as if it was part of the initial scope.
