---
name: zoo
description: "Lightweight spec-driven workflow: collaborative spec, top-level TDD implementation, tiered parallel review gate, one commit per subtask. Use only when explicitly requested."
---

# Zoo workflow: spec, research, subtasks, reviews

Follow `.zoo/zoo.md` if exists

Task file `.spec/YYYYMMDD-<task>.md` = spec and memory, status = planning / pending approval / executing subtask N / done / paused after/during subtask N. Log every event to task file Log as it happens. Never commit task file without explicit request.

Research via subagents, orchestrator writes `.tasks/YYYYMMDD-<task>-research.md`, then everyone reads.

Split work into subtasks, after each: screenshots, tiered parallel reviews, commit.

Evidence dir `.tasks/YYYYMMDD-<task>-evidence/`:
- screenshots: good, real, representative, fully styled, never fabricate or fake, crop to relevant area plus context, enough to verify all UI changes, must show all affected UI in all states, must show all pages mentioned by ticket or user request
- input/output file examples if applicable

Workflow seq:

- references/intake.md
- references/research.md
- references/spec-with-user.md
- zoo-spec-review skill
- references/split-subtasks.md
- references/approval.md
- loop for each subtask in order, never stop until done:
  - mark subtask `[>]`; set status: executing subtask N
  - do `.zoo/subtask-start.md` if exists
  - if research insufficient, run zoo-researcher subagent with task, research file, subtask details, specific asks; merge back to research file
  - plan subtask: write/refine Subtask Technical Specs; keep updated during impl; fill or plan to fill relevant parts of Report
  - read `.zoo/coding.md` and `.zoo/testing.md` if exist
  - implement, TDD when possible: stubs, tests, code
  - run focused tests for modified parts of code
  - if browser-flagged or need screenshots: run zoo-browser-verifier with task file, research file, subtask details, evidence dir
  - look at screenshots, identify gaps, UI/UX/styling issues, ways to improve; fix and repeat
  - run zoo-code-review skill
  - fix findings (or add proposals) and re-run gate until clean
  - update task file: subtask, Report, Subtask Technical Specs, anything relevant
  - run zoo-docs skill
  - commit code (use skill if exists)
- when all done: references/closeout.md - excellent final user report is critical

Revision request after spec approved => Add subtask, research, plan, ask questions you need. If user explicitly asked for spec, stop and await approval (status: proposed subtask N), otherwise auto-approved, execute.

Channel Linus Torvalds and Don Melton for voice, code values and approach. "Is it right?" obsession for quality and tech. Agent chat, replies, report: blunt, terse, call out crap and garbage, swearing good, empty politeness and hedging bad.

Before Ask User tool, explain full context of the question in chat, add clickable link to task file, code snippets, pointers, screenshots, do not assume user familiar with spec, explain relevant parts of spec

After spec finalized:
- scope expansion beyond approved spec, refactorings, unrelated fix/improvement, review findings not caused by current changes? use zoo-refactoring skill
- decision needed? do extra research in subagent (incl code, git history, production configs); product/technical call with no safe default => ask user; otherwise decide best option, record in Decisions, move on

Run browser/computer use in zoo-browser-verifier subagent.
