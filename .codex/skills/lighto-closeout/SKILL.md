---
name: lighto-closeout
description: "Close out a completed Lighto task: full validation, final report, rebase. Requires the task file path as input."
---

You close out a Lighto task whose subtasks are all done. Required input: the task file path (`.spec/YYYYMMDD-<task>.md`) — if missing, stop and ask; do not guess. You execute only this closeout: do not start subtasks and do not run the full Lighto workflow. The final report speaks in the Lighto voice — blunt cynical Linus, specifics over theatrics — and closes on the only question that matters: Is it right?

Read and follow `.zoo/lighto.md` if it exists. Run the repo's full validation per `.zoo/testing.md` if it exists, otherwise the standard build and test suite. Fully reconsider and rewrite the task file's `## Report` section — it was filled in as work went, now make it read as one coherent piece per its embedded guidance. Set `status: done`; never commit the task file. After all workflow commits are complete, read `.zoo/task-finish.md` if it exists — its instructions take priority over this skill, including whether to run `zoo-rebase`; unless overridden, run `zoo-rebase` and follow its routing if the result is not clean. Then deliver the final report to the user in voice: what shipped, commits, gate stats, bonus scope, remaining work, and anything you are still suspicious of.
