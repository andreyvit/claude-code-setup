<div align="center">

# Zoo 3

Reliable Codex & Claude workflows for complex projects.

_Specs • Tiered reviews • Browser use • Subtasks_

[![License: 0BSD](https://img.shields.io/badge/License-0BSD-blue.svg)](LICENSE)

</div>

Let agents review and fix their own shit before you see it.

1. Uses spec files so the task does not evaporate halfway through.
2. Co-writes spec with user input. User approves before implementation.
3. Splits work into explicit, reviewable subtasks and commits.
4. Runs tiered reviews: tests, scripted checks, broad review, visual review.
5. Verifies UI changes with screenshots.
6. Writes proposals for refactoring and out-of-scope issues found.
7. Presents a readable report at the end.
8. Follows pragmatic values of Linus Torvalds and Don Melton.

Zoo 3 is the lightweight successor to Zoo 2, targeting the smarter models of mid-2026.

See my posts for way more context on the idea:

* [Meet Zoo 2](https://tarantsov.com/meet-zoo-2/)
* [All Star Zoo](https://tarantsov.com/all-star-zoo/)

These skills are intended to be project-independent, so all customization is via `.zoo/*.md`, for example:

- shared Zoo paths and scripted check commands go into `.zoo/zoo.md`
- planning and spec-review instructions go into `.zoo/planning.md` and `.zoo/planreview.md`
- project-specific browser testing instructions go into `.zoo/browser.md`
- project-specific testing instructions go into `.zoo/testing.md`
- project-specific code-review risks go into `.zoo/codereview.md`
- and more; see [example .zoo files](.zoo/)

These will be generated for your project during the Zoo Init phase of installation.

Tip: if you're choosing between Codex and Claude for Zoo workflows...

- Codex + GPT 5.5 produces *much* better results than Claude Code + Opus 4.8
- Claude Code + Fable 5 might be on par with Codex + GPT 5.5
- Codex rate limits last *much* longer


## Quick start

### Installation

1. Run `./install.sh /path/to/your/project` from this repository.
2. Run Zoo Init skill from that project.
3. Review and customize the generated content under `.zoo/`.

I've published our real-world [`.zoo/*.md`](.zoo/) files, but you definitely should not just blindly copy them.

When installing, we add separate skill copies for Claude and for Codex. You can use symlinks if you prefer, but we avoid them to make handling Windows checkouts easier 🤮

### Running a task

1. Say `<task> with /zoo` or just `/zoo <task>`.
2. Answer questions, wait until spec is ready
3. Review spec, iterate if needed
4. Say `Approved`, or, better yet, `/goal Approved. Execute until done.` (Codex) or `/loop Approved. Execute until done.` (Claude)

### Tips for reviewing the work

When task ends, you will get a comprehensive report including screenshots of UI changes. See the spec file for more details; it is the best place to start your review.

Zoo 3 will produce small, separate commits for subtasks when practical. You probably want to squash those after review.


### Asking for revisions

To request a revision, run a Zoo skill again (`/zoo <revision request>`). It should recognize that it's a revision and continue working with the same spec and same task directory.

To add an unrelated revision while a task is running, use Zoo Add skill (`/zoo-add <revision request>`), which will record the request for the future without the agent dropping everything and switching to it.


## Advanced use

* Zoo Docs: invoke manually to beat some new knowledge into stupid machine brain
* Zoo HR: update skills and customize Zoo workflows (instructions under `.zoo`)
* Zoo Cleanup Finished Specs: archive completed `.spec/*.md` files and resolved proposal files without deleting them
* Zoo Rebase: `git pull --rebase`, resolve conflicts, retest; runs automatically at the end of each task
* Zoo Push: Zoo Rebase then `git push` if safe
* Zoo Ensure Safe Deploy: waste some extra tokens finding potential deployment problems
* Zoo Code Review: can invoke manually and specify the changes to review (“since v1.2.3”)
* Zoo Proposal: ask to write a proposal

### Proposals

When Zoo reviewers want to request a refactoring way outside the scope of the current task, they write up a proposal file under `.proposals/` for future consideration.

This is a way to keep security/modularity/etc reviews satisfied without blowing up the scope of simple tasks, but also without ignoring their findings.

Review these proposals occasionally, and see if you wanna execute them.


## Changelog

* Zoo 3 replaces Zoo Heavy/Lite/Zero with a single lighter workflow, introduces tiered reviews, drop bureau reports (for big token savings), and all steps share a single research file
* Zoo 2.3 adds Claude Code, proposals, final reports (Zoo Report skill invoked automatically when finishing tasks), Zoo Rebase, Zoo Push, and Zoo Ensure Safe Deploy skill (for manual invocation under `/goal` or `/loop`)
* Zoo 2.2 adds Uber Review to all Zoo flows.
* Zoo 2.1 refines Codex setup for GPT 5.5-xhigh, adds Zoo Lite and Zoo Zero workflows to reflect the preferred speed/accuracy balance of the smarter models, and is the first public release of Zoo 2.
* Zoo 2.0 is a reimagining of Zoo for Codex and the smarter GPT 5.4+ models. Introduces a spec file.
* Zoo 1.1 adds Bureau MCP for more consistent reporting.
* Zoo 1.0 is a Claude Code setup described in my blog post.


## License

Most of this is AI-generated and should be considered uncopywritable. But just in case, whatever is copywritable is © 2025-2026, Andrey Tarantsov, and is distributed under the [Zero Clause BSD](https://opensource.org/license/0bsd) license, which has no attribution requirements:

Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted.

THE SOFTWARE IS PROVIDED “AS IS” AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
