---
name: zoo-rebase
description: Rebase completed changes onto upstream, resolve conflicts, rerun validation when incoming changes could affect tests, and decide push safety. Use during Zoo closeout and before Zoo Push.
---

Follow `.zoo/zoo.md`, `.zoo/rebase.md` if exists.

Use after trackable changes outside ignored task roots are committed and before final reporting/zoo-push. For final closeout, read `.zoo/task-finish.md` first; it may skip/replace zoo-rebase.

Ignored task roots (`.tasks/`, `_tasks/`, alternates) are workspace artifacts. Never stage, force-add, commit, or push them.

1. Verify trackable changes are committed; ignore ignored task roots and untracked dot/underscore dirs. Dirty => Bail with 'Unsafe - Work tree dirty'. Do not stash/edit/discard unrelated dirty work.
2. Require upstream. If missing, bail with `Unsafe - No upstream`.
3. Log branch, upstream, HEAD, upstream commit, git status in task file when present.
4. Run `git pull --rebase`.
5. Resolve conflicts; preserve incoming and task intent. If unclear, stop with `Unsafe - Cannot merge` and explain.
6. Rerun validation if incoming changes/conflict resolutions may affect tests, ie all except non-executable docs and instructions, changelogs, skills, etc. When uncertain, validate. All good? Return with 'OK, <no conflicts / conflicts resolved>, <no need to revalidate / validation passed>'.
7. If validation fails, identify cause. Broken incoming commits => do not fix, bail with 'Unsafe - Incoming commits broken'. Breakage caused by our changes or merge => output 'Unsafe - Extra fixes required' and route into active workflow as subtask.
