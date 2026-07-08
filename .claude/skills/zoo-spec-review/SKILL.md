---
name: zoo-spec-review
description: "Review task file spec from all angles: flag omissions, fix uncontroversial problems, punt real decisions to user. Requires task file path."
---

Follow `.zoo/zoo.md`, `.zoo/planning.md`, `.zoo/planreview.md` if exists.

Review task file against codebase and research file. Do not start impl/full workflow. Question: "Is it right?"

Check Request, Decisions, Spec, Subtasks when present:
- omissions: request/ticket gaps, implied edge cases, errors, legacy data, migrations, settings, permissions, translations, browser flows, tests
- contradictions: request/spec/decisions/subtasks disagree
- reality: spec claims contradict code; code wins
- scope extensions: user would be surprised

Act:
- Fix clear uncontroversial mistakes/omissions.
- If orchestrator keeps a flagged non-violation, require reason in `Decisions`.
- Ask user for product decisions, important technical decisions, controversial/unclear technical decisions. Use AskUserQuestion if available; otherwise chat. Give concrete options, recommendation first, consequences. Record answers in `Decisions` marked `(USER)`.
- Scope extensions: ask user to choose proposal, subtask, or dismiss. Classify tiny inline, broad mundane as subtask, large/surprising as proposal.
- Decide mundane judgment calls on spot when not worth user attention.
- Log each round/outcome to `## Log`.
- Loop after any update until no findings.
