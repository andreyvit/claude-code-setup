---
name: zoo-proposal
description: Write durable proposal files for significant cross-cutting work needing human approval before implementation. Use when user asks for a proposal, or when Zoo Refactoring, planning, implementation, or review classifies a finding as proposal-worthy.
---

Follow `.zoo/zoo.md`, `.zoo/proposals.md` if exists.

Write proposal. Do not implement.

Filename: `.proposals/YYYYMMDD-slug.md` unless overridden. Copy `references/proposal-template.md`; replace placeholders.

Quality bar: approvable/rejectable/revisable; explicit scope, non-goals, validation, risks, open questions; no small cleanup/current-task work; no silent design decisions.

If in Zoo task, add:
- `Report > Remaining work`: `- Proposal: <path> - <finding>`
- `Log`: proposal path and current task continues/blocks

Report proposal path, status, finding handled, current-task disposition. Proposal closes finding unless technically blocking.
