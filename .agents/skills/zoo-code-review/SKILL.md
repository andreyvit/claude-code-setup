---
name: zoo-code-review
description: "Tiered code review gate: tests, scripted checks, narrow parallel checks, broad review. Invokable standalone on any diff or inside Zoo workflow."
---

Follow `.zoo/zoo.md`, `.zoo/codereview.md` if exists.

Review change. If no task/research file, run gate anyway.

If the task file has `## False positive or rejected review findings`, skip those findings. Pass that section into reviewer and check prompts. Do not re-raise them. Do not record a task-local rejection in source.

Assemble the broad review question list: built-ins below, then `.zoo/codereview.md` if it exists (skip any that restate a built-in). Renumber continuously. Put the full numbered list in the zoo-reviewer prompt. Ask for `OK` or a specific suggestion per question. Generic concerns are useless. Default `OK`.

Built-in questions:

1. Is there a way to simplify this code while still meeting requirements?
2. Is there a large chunk of code, function, class, module, or package that is not obviously correct at a glance in isolation? If so, is there a way to rewrite it much more clearly?
3. Is there a business rule that is not implemented in one place, and is instead an emergent property of multiple spread-out parts? If so, is there a way to concentrate it?
4. Is there premature pessimization in code that might process many items, where a clearly cheaper algorithm or much lower memory, fewer allocations, or less boxing/copying would not be much harder to implement and understand?
5. Is a new helper, abstraction, type, enum, or interface duplicating an existing one that could be modified or extended within reason?
6. Are any of these changes unsafe to deploy in a way not acknowledged or accepted by the user?
7. Carefully review all modified code. Are there any other significant improvements you can suggest?

Run in parallel:
- prescribed tests, if not already done
- broad review in zoo-reviewer with that question list in the prompt
- scripted review; if it asks for agents, use zoo-check-fast, zoo-check-smart, zoo-check-smartest
- visual review in zoo-reviewer when UI/screenshots matter: look/feel (spacing, hierarchy, alignment, consistency), target-audience clarity (labels, terms, when to use controls), a11y/light/dark/platform norms, all states/interactions/test plans/harness data, broken controls/flows. Inspect existing screenshots, find gaps, redo/add evidence for all relevant UI states and example files.

Quality bar: active-subtask result passes AND right; other findings are explicitly routed.

Route every finding through Zoo skill `references/change-or-suggestion.md`: broad review, scripted review, visual review. Never implement or dismiss routed findings merely because review found them. If a finding is a false positive or rejected for this task, add it to `False positive or rejected review findings`. Never ask user during the gate.
