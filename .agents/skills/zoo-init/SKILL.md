---
name: zoo-init
description: Initialize repo-local .zoo customization files for Zoo from real repository research. Use when setting up Zoo customization for a repository.
---

Follow `.zoo/zoo.md` if exists.

Create concise repo-local Zoo guidance. No placeholders.

Codex only: before continuing, ensure global Codex config (`$CODEX_HOME/config.toml`, usually `~/.codex/config.toml`) contains `default_mode_request_user_input = true` under `[features]`. Required for `request_user_input` tool to work outside of planning mode. If added during running session and the tool stays unavailable, start a new turn/thread or ask user restart Codex. Bail without this tool available.

Research first: root agent instructions, docs dirs, layout, build/test/browser harness, commit/review/docs/proposal/release conventions. For `.zoo/codereview.md` and `.zoo/planreview.md`, mine high-level review questions from those sources; drop any that restate the built-ins in zoo-code-review / zoo-spec-review. For `.zoo/codereview.md` only, also drop questions already covered by the repo's scripted checks (same gate; use `-list` or equivalent if the command has one). Keep those in `.zoo/planreview.md` — scripted checks do not run at plan time.

After research, ask user with `request_user_input` / AskUserQuestion, give option for their existing value if different from recommended value:
- do they want to use terse messages style, recommend yes.
- spec (aka task) file path, recommend `.spec/YYYYMMDD-taskslug.md`
- should spec files be committed or gitignored, recommend commit
- evidence dir (screenshots and example files), recommend `.tasks/YYYYMMDD-taskslug-evidence/`
- should evidence be git tracked or gitignored, recommend ignore
- research path, recommend `.tasks/YYYYMMDD-taskslug-research.md`
- should research be git tracked or gitignored, recommend ignore
- proposals path, recommend `.proposals/YYYYMMDD-title.md`
- archived proposals path, recommend `archived/` under chosen proposals dir
- mistake log path, recommend `.ai-mistakes.md`, give option to have none

NB: when asking for paths, give full example (`.spec/YYYYMMDD-taskslug.md`), not prefix (`.spec/YYYYMMDD`).

If user agreed to terse style, load terse skill immediately, and add at the start of CLAUDE.md and AGENTS.md (whichever exist): `Load terse skill for voice in all messages incl between tool calls.`

Create `.zoo/`, create new files under it; merge new research if files already exist;
- `.zoo/zoo.md`: zoo dirs (specs, proposals, tasks), how to run tests and scripted checks, mistake log path, ticket tools
- `.zoo/planning.md`: pointers to planning docs if any (e.g. product, architecture info)
- `.zoo/planreview.md`: numbered high-level spec-review questions from repo research. Skip any that restate zoo-spec-review built-ins. Keep questions scripted checks would cover — those checks do not run at plan time. Questions only.
- `.zoo/testing.md`: pointers to test writing docs if any
- `.zoo/coding.md`: pointers to coding/style docs if any
- `.zoo/codereview.md`: numbered high-level code-review questions from repo research. Skip any that restate zoo-code-review built-ins or the repo's scripted checks. Questions only. Optional one-line "do not flag" notes.
- `.zoo/browser.md`: pointers to harness info, how to run app for browser testing, tunnel/callback rules
- `.zoo/docs.md`: pointers to doc destinations, instructions, validation, terminology, audience
- `.zoo/task-start.md`: leave empty
- `.zoo/subtask-start.md`: leave empty
- `.zoo/task-finish.md`: leave empty
- `.zoo/rebase.md`: leave empty
- `.zoo/push.md`: repo publish instructions (use git push, open PR, etc) if any
- `.zoo/proposals.md`: proposal folder/naming/archive/statuses/rules
- `.../MISTAKES.md`: initialize unless user refused

Update `.gitignore` to ignore/unignore according to user choices.

If any unknown files found under .zoo, list them and ask user if they want to delete.

Check if commit skill exists; if not, ask if they want to create it (recommended); if yes, run zoo-hr skill in subagent.
