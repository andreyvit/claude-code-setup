---
name: zoo-code-review
description: "Tiered code review gate: tests, scripted checks, narrow parallel checks, broad review. Invokable standalone on any diff or inside Zoo workflow."
---

Follow `.zoo/zoo.md`, `.zoo/codereview.md` if exists.

Review change. If no task/research file, run gate anyway.

Run in parallel:
- prescribed tests, if not already done
- general review in zoo-reviewer
- scripted review; if it asks for agents, use zoo-check-fast, zoo-check-smart, zoo-check-smartest
- visual review in zoo-reviewer when UI/screenshots matter: look/feel (spacing, hierarchy, alignment, consistency), target-audience clarity (labels, terms, when to use controls), a11y/light/dark/platform norms, all states/interactions/test plans/harness data, broken controls/flows. Inspect existing screenshots, find gaps, redo/add evidence for all relevant UI states and example files.

Quality bar: passes AND right.

Stay in scope: problems caused by reviewed changes or task scope. Unsafe/major unrelated issue => zoo-proposal; minor unrelated issue => ignore.

Merge findings, sort critical first, document ignored findings in task file. Never ask user.
