---
name: zoo-push
description: Manually publish completed Zoo workflow changes by reading repo push instructions, running Zoo Rebase first, and pushing or following the repo's prescribed PR/trunk workflow only when Zoo Rebase reports that the repo is safe to push. Use only when the user explicitly asks to push, publish, open a PR, or run Zoo Push.
---

# Zoo Push

Read and follow `.zoo/zoo.md` if it exists.
Read and follow `.zoo/push.md` if it exists.

Zoo Push is manual only. Do not invoke it from ordinary Zoo closeout unless the user explicitly asks to push or publish.

## Workflow

1. Read `.zoo/push.md` if it exists and treat it as the repo-specific publishing contract. If it is empty or missing, default to pushing the current branch to its upstream with `git push`.
2. Run `zoo-rebase` first.
3. Read the `rebase` Bureau report and current git status. Use the report's `Safe to push` decision as the push gate.
4. If Zoo Rebase says `Safe to push: no` or `Safe to push: unknown`, do not push yet. Follow the Zoo Rebase routing:
   - report incoming broken commits with diagnostics and proposed fix approach without fixing them
   - route this workflow's breakage back into Heavy/Lite as a fix subtask, or into Zoo Zero outside Heavy/Lite
   - treat an unresolved validation or test failure that the workflow cannot fix as the normal stop condition after rebase
5. If Zoo Rebase says `Safe to push: yes`, publish according to `.zoo/push.md`; if it is empty or missing, run `git push`.
6. If publication fails because the remote moved, such as a non-fast-forward push rejection, run `zoo-rebase` again and retry publication. Repeat until publication succeeds or Zoo Rebase reports that the repo is not safe to push.
7. Write a Bureau report with suffix `push` recording:
   - rebase report used and whether it was safe to push
   - any remote-moved, non-fast-forward, or repeated rebase/push attempts
   - push or PR/trunk command followed
   - remote branch, PR URL, or other publication target when available
   - final git status
   - `Push result: pushed|opened PR|not pushed|failed`

## Guardrails

- Push only when the user explicitly invoked Zoo Push or otherwise explicitly asked to publish.
- Push only when the latest Zoo Rebase report says `Safe to push: yes`.
- Do not block push merely because the rebase had conflicts. A rebase with resolved conflicts and passing required validation is safe to push.
- Do not push unresolved conflicts, failing validation that the workflow could not fix, or uncommitted trackable Zoo workflow changes outside ignored task roots.
- If a push fails because the branch is behind the remote, rebase again and retry instead of stopping at the failed push.
- If a task root such as `.tasks/`, `_tasks/`, or a repo-specific alternate is ignored by git, never stage, force-add, commit, or push files under it to satisfy push safety checks.
- Do not alter unrelated pre-existing dirty work.
