---
name: zoo-cleanup-finished-specs
description: Archive completed Zoo spec files and resolved proposal files. Use when Codex is asked to clean up finished `.spec` work, move completed specs into an `archived/` subfolder, or move fully implemented/rejected proposals into an archive, while honoring repo-local Zoo path overrides.
---

# Zoo Cleanup Finished Specs

Read and follow `.zoo/zoo.md` if it exists. Also read `.zoo/proposals.md` if it exists.

Use this skill to move finished Zoo specs and resolved proposals out of the active folders without deleting them.

## Locate Folders

1. Start with defaults:
   - specs: `.spec/`
   - archived specs: `.spec/archived/`
   - proposals: `.proposals/`
   - archived proposals: `.proposals/archived/`
2. Override those defaults with explicit repo-local instructions from `.zoo/zoo.md`.
3. For proposals, also honor `.zoo/proposals.md` when it defines incoming or archived proposal paths.
4. Create missing archive folders with `mkdir -p`.
5. Ignore files already inside archive folders.

## Classify Specs

Archive a spec only when all of these are true:

- It is a spec document in the configured active spec folder, usually `*.md`.
- It has actually been executed, shown by completion evidence such as commits, final planner/closeout text, final validation, or completed Zoo reports referenced in the spec.
- Its `## Subtasks` section has no active or incomplete items:
  - no `(next)`
  - no `(future)`
  - no `(planned)`
  - no `(blocked)`
  - no `Plan: TBD`
  - every subtask status present is `(done)`

Do not archive specs that are only draft plans, have no subtask section, or still contain deferred future work. If a spec looks complete but has ambiguous text, list it as a candidate and leave it in place.

## Classify Proposals

Archive a proposal only when it is in the configured active proposal folder and one of these is true:

- frontmatter contains `status: "done"` or `status: done`
- frontmatter contains `status: "rejected"` or `status: rejected`

Do not infer that a proposal is fully implemented from code alone unless the proposal file already records that disposition. If a proposal has prose saying it is implemented or rejected but still has `status: "proposed"` or no status, leave it in place and report it as needing a status update or human decision.

## Move Files

1. Build two explicit lists before moving:
   - specs to archive
   - proposals to archive
2. If no files qualify, report that and stop.
3. Refuse to overwrite existing files in archive folders. If a destination exists, stop and report the collision.
4. Use `git mv` for tracked files. Use ordinary `mv` only for untracked files.
5. Preserve filenames exactly.
6. Do not move task reports under `.tasks/` or `_tasks/`; those are workflow state, not active spec/proposal files.

## Validate

After moving files:

- Run `git status --short`.
- Run a simple listing of the active and archived spec/proposal folders to confirm only the intended files moved.
- Do not run code tests for archive-only moves unless the cleanup also edits executable files.

## Report

Report:

- path rules used and which `.zoo` files supplied them
- specs archived
- proposals archived
- ambiguous or skipped candidates and why they stayed active
- collisions or blockers
- validation commands run
