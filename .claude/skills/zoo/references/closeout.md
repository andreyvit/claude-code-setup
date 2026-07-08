Close out task whose subtasks are all done. Required input: task file path (`.spec/YYYYMMDD-<task>.md`). If missing, stop and ask; do not guess. Execute only closeout: do not start subtasks or full workflow.

Follow `.zoo/zoo.md`, `.zoo/testing.md`, `.zoo/task-finish.md` if exist

Final checks:

- Run full repo validation if any changes since last done (normally none)
- Ensure full set of screenshots for all states of the UI (some data, lots of data, empty/default, disabled, styled, all relevant states) and all screens relevant to user request, so that user can comprehensively review UI changes. View screenshots to ensure suitability.
- If task had an associated ticket and we have access to reading tickets, re-fetch and review the original ticket again. Make sure covered entirely (or to the extent explicitly requested by user). Flag any discrepancies and omissions in Zoo task file Report:
    1. If user explicitly asked to omit or specified explicit scope, discrepancies/omissions belong in report.
    2. If user didn't specify an explicit scope or omissions, and you found a forgotten part of the scope, restart Zoo workflow to address anything missed.
    3. If everything is fully covered, add note for confident reporting later.

Closeout:

- Follow `.zoo/task-finish.md` if exists.
- Review references/task-file-template.md for any Report guidance/subsections dropped from task file, and add it back
- Rewrite task file Report into coherent final report per embedded guidance
- Set `status: done`; never commit task file.
- Unless overridden, run zoo-rebase and follow routing if not clean.

Display final report in chat:

- start with a clickable link to task file
- clearly give task status, and display a list of subtasks with their statuses
- link other relevant refs (files)
- include everything from Report in task file
- show screenshots inline
- show all subsections from Report
