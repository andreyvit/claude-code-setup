# Zoo design rationale

For humans and AIs *modifying* this skill. Not workflow instructions — the executing agent must not load this file. Record the why behind every non-obvious rule here so changes don't silently break the reasoning.

## Why Zoo 3 Exists

- (USER) Zoo 3 replaces the old Zoo workflows rather than living beside them.
- Old Zoo Heavy runs ~22 cold subagents and writes ~20 Bureau reports per subtask. The dominant cost is context reconstruction: each subagent re-reads the spec, plan report, and all later reports. The reports exist mostly to feed the next cold agent — that is the file sprawl and the wall-clock.
- Old Zoo runs 16 broad agentic reviewers per subtask (8 plan uberreview + 8 code uberreview), each free to explore the codebase.
- Zoo 3 attacks both: one shared research file + narrow no-exploration reviewers + no step reports. Skipping specs and reviews was explicitly rejected — they trade machine time for human time in the wrong direction. The goal is keeping their value while deleting the reconstruction overhead.

## File layout

- Task file lives in `.spec/` (tracked): it is the collaboration artifact and the cold-resume point. `status:` frontmatter lets humans and tooling see task state at a glance.
- Research and evidence live in `.tasks/` (gitignored): machine-scale content with no review value in git history.
- The research file is the substitute for context forking, which Claude Code does not have (subagents start cold; SendMessage continuation is serial, so it can't power parallel fan-out). File + inline diff replace re-research.

## Task file is never auto-committed

- (USER) The workflow never commits the task file; the user commits it manually if and when they want.
- History: the first design auto-committed every task-file change as its own separate commit — never mixed with code — so code commits could be rebased/merged/moved without spec-file conflicts, with the intent to squash before push. That collided with "log every event" (several spec-only commits per subtask) and dragged in squash mechanics nobody needed. Never-auto-commit keeps the conflict-freedom and deletes the machinery.
- Consequence: the task file sits modified in the working tree for the task's whole life. Code commits must exclude it, and Log lines are written to it the moment events happen — there is no commit checkpoint to batch for.

## Log section

- One line per event, everything that happens. Replaces old Zoo's Execution memory and all step reports: it is the memory a cold session resumes from, and the record for debugging the workflow itself.

## Review gate tiers

- **Tier 0 (scripted)**: every rule migrated from prompt text into a grep/AST check is enforced for free, forever, with no reviewer needing reminding. Long-term destination for mechanically checkable AGENTS.md rules.
- **Tier 1 (narrow, diff inline)**: pasting the diff into each prompt means most checks need zero tool calls. Repeating a 30KB diff across 25 reviewers costs far less than 25 agents each doing 20 exploration calls. The ESCALATE verdict is the cost-safety valve: a cheap reviewer that suspects something deep doesn't guess — it names what to investigate and one follow-up agent gets full freedom.
- **Tier 2 (broad, 2–3 agents)**: the only agents able to catch cross-file regressions in *unchanged* code, which inline-diff reviewers structurally cannot see. Do not cut below two.
- **Re-run policy**: Tier 0 always (free); only failed Tier-1 check IDs (passed as args to a fresh gate run); Tier 2 only after substantial rework. Old Zoo's full re-review after every fix round was most of its tail latency.
- (USER) **Scope-extension policy**, ported from old Zoo's three-level routing: super tiny extensions inline; mass edits of existing code / refactorings / broad mundane work → separate subtask; large extensions (new jobs, persisted state/migrations, new settings, public/API contract changes, broad subsystem behavior changes, operational dashboards/recovery — anything outside direct ask) → proposal in `.proposals/`.

The asymmetry is deliberate: **spec review asks the user** to confirm routing (plan-time questions are cheap), while **code review classifies silently** via `scope: inline|subtask|proposal` finding tag (mid-execution interruptions are expensive). Only code-review escape hatch: `subtask`/`proposal` finding that blocks correctness, surfaced as blocker.

## Model doctrine

- Strong models where trust concentrates; cheap models where redundancy backstops.
- The research file is trusted downstream *without verification* — errors propagate into the implementation and every reviewer prompt, and nobody is positioned to catch them (catching them would mean redoing the research). This sets a floor on researcher model quality, not a mandate for the top tier.
- (USER) Researchers run on `smarter` (opus), not `smartest`: research is traversal and distillation of *existing* code, not novel design, so the second tier is judged sufficient. The trust-concentration argument means never go below `smarter`. If research-quality problems show up in practice (wrong pointers, misread patterns), bumping researchers to `smartest` is the first knob to try.
- Tier-1 checks can afford the `cheaper` tier because a missed finding is backstopped by sibling checks, Tier 2, and tests.

## Predefined agents

- Five agent definitions (`.claude/agents/zoo-researcher.md`, `zoo-check-fast.md`, `zoo-check-smart.md`, `zoo-check-smartest.md`, `zoo-reviewer.md`) instead of narrating roles in the skill: standing instructions move out of the skill and out of every per-call prompt, models are pinned in frontmatter, and role behavior stops varying between runs.
- Anti-drift rule: each role's instructions live ONLY in its agent file; SKILL.md names roles and never restates their rules. Old Zoo described each role in three places (agent file, `zoo-*` skill, orchestrator skill), and they drifted — that is the failure mode to avoid.
- Five, not nine: old Zoo's agents exist largely to relay workflow state through Bureau reports; these exist to deduplicate standing instructions and pin models. Thin or one-off roles don't get a definition — escalation investigation is zoo-reviewer with a pointed question.
- Browser verification uses zoo-browser-verifier plus zoo-browser-verification.
- (USER) Researchers return findings as output instead of writing files; orchestrator is sole research-file writer. This kills the parallel write-race (earlier design used per-researcher part-files merged with `cat`) and forces orchestrator to read research, useful for spec writing. Cost: research content passes through orchestrator context once.
- Agent definitions live in `.claude/agents/` but are not Claude-only in practice: they are plain readable instructions, so other harnesses use them via general subagents told to follow the role file. Check files remain the harness-neutral content.
- The skill has no Subagent rules section. Audit verdict: model doctrine is maintainer knowledge (this file + agent frontmatter — the harness applies pinned models, the orchestrator does nothing with them); per-check override mechanics live in zoo-code-review; no-broad-suites lives in the agent files of the agents that could violate it; old Zoo's no-recursive-spawn rule guards a hazard Zoo 3 lacks (agents get tiny role prompts, not orchestration skills). The single orchestrator-side duty that survived — prompts carry only specifics, never restate agent-file instructions — is one line in the skill intro.

## Just-in-time skill loading

- (USER) The review gate lives in its own zoo-code-review skill, invoked from subtask loop step 5 — the gate text moved, not rewritten. Reason: gate instructions re-enter context fresh at every gate run; in long multi-subtask sessions the main skill text (loaded at the start) is the first thing summarization degrades.
- The re-run policy moved into the gate skill (gate logic needed exactly when gate skill loads); loop step 6 defers to it.
- (USER) zoo-closeout (input-required) owns final validation, the `## Report` rewrite, `status: done`, zoo-rebase, and the final in-voice report. Deliberately a single paragraph for now; expected to grow substantially.
- (USER) zoo-subtask executes one subtask end to end (plan → TDD → validate → browser → gate → commit). Unlike zoo-code-review it is input-required, not standalone: the task file path and subtask number are mandatory, stop-and-ask if missing. Independent invocation buys cold-resume at `executing subtask N` without loading planning instructions, and fresh loop instructions on every iteration of a long run.
- Status ownership moved with the extraction: each zoo-subtask invocation sets `status: executing subtask <N>` at start and marks `[>]`/`[x]`, replacing the old approval-time set and end-of-loop bump in the main skill.
- Resist ballooning extracted skills: same text relocated for load timing, not an opportunity to add words.
- (USER) Runtime skill brevity is a requirement: every added sentence must change behavior, remove ambiguity, or earn deletion elsewhere.
- (USER) Subskills are independently invokable (e.g. zoo-code-review on any diff, no task file needed). Each carries a one-paragraph guard — you are not the orchestrator, execute only this step, never start the full Zoo workflow — which compresses old Zoo's whole "Modes" section. Because standalone runs don't load the main skill, the guard paragraph also carries a one-line voice note; that duplication is deliberate.

## Project independence

- (USER) Core Zoo skills carry no project specifics; `.zoo/` is the project customization folder. The skills state generic defaults and read `.zoo` files when they exist:
  - all Zoo skills: `.zoo/zoo.md` — general overrides: file locations, ticket tooling, the Tier-0 check command, `MISTAKES.md` path
  - spec writing (main skill): `.zoo/planning.md`
  - zoo-spec-review: `.zoo/planning.md` + `.zoo/planreview.md`
  - zoo-subtask: `.zoo/subtask-start.md`, `.zoo/coding.md`, `.zoo/testing.md`, `.zoo/browser.md`
  - zoo-closeout: `.zoo/testing.md` for validation; `.zoo/task-finish.md` takes priority over the skill (may skip/replace zoo-rebase); `.zoo/closeout.md` adds final report/chat rules; otherwise run zoo-rebase — the same closeout contract as Zoo.
- (USER) zoo-code-review reads `.zoo/codereview.md` for local review guidance. Durable mechanical checks still belong in `cmd/fire-check`; durable judgment checks can move to `.zoo/checks/`.
- (USER) Core Zoo skills say `MISTAKES.md` only; repo-specific path belongs in `.zoo/zoo.md`.

## Spec review

- (USER) zoo-spec-review runs after spec writing, before subtask split: analyze the task file from all angles, fix obvious uncontroversial problems directly, punt product decisions plus important or controversial/unclear technical decisions to the user, loop (update spec → re-review) until a clean pass.
- This is old Zoo's planner → plan_reviewer → plan-uberreview chain collapsed into one fix-or-punt pass; the key inversion is that real decisions go to the user instead of being review-round-tripped between agents.
- It reviews "Subtasks when included" — running before the split means subtask coverage isn't checkable yet; the split step's own sanity fan-out covers that. Re-running spec review later (it's input-required but independently invokable) reviews the full file including subtasks.
- (USER) Subtask splitting protects reviewability: split unrelated features and clean incremental steps unless task file records a good reason to keep them together. Avoid fake progress micro-subtasks; each subtask adds a meaningful functionality step.
- The rename zoo-review → zoo-code-review exists for naming symmetry with this skill.

## Collaborative spec

- Moves human steering from "wait 90 minutes, then request revisions" to upfront batched Q&A — same human effort, paid where it is cheap. The human replaces old Zoo's planner → plan_reviewer → plan-uberreview chain for product decisions; one narrow fan-out sanity-checks the mechanical rest of the plan, once per task.
- AskUserQuestion with concrete options, recommendation first, consequences spelled out.
- `What Happened` exists for bugs and investigations because product/technical specs are not enough to understand why the work matters. It forces the investigation narrative into the task file: bad behavior, mechanism, impact, regression status, and attribution/timing when knowable. It is explicitly skipped for tasks where that narrative would be noise.

## Voice

- Linus Torvalds in nasty cynical mood + Don Melton's "Is it right?" focus. Applies to chat and the final report only; specs, commits, code, docs, and subagent prompts stay dry.
- The "every insult must carry specifics" rule is load-bearing: it is what prevents the voice from degrading into substance-free theatrics.

## Harness notes

- (USER) Skills are dual-harness: the gate uses the Workflow tool when available (Claude Code — buys schema-validated verdicts with tool-layer retry and concurrency management) and plain parallel subagents otherwise (Codex), which must return the verdict JSON as their final message. Gate *content* (check files with frontmatter) is harness-neutral.
- The agent role files under `.claude/agents/zoo-*.md` are the cross-harness source of truth: harnesses without registered agent types launch general subagents that follow the role file first. AskUserQuestion degrades to plain chat questions where unavailable.
- (USER) Codex has registered functional copies: `.codex/agents/zoo-*.toml` with the same instruction bodies, plus the Zoo skills mirrored verbatim into `.codex/skills/`. Model-tier mapping in Codex terms: researcher `smarter` → `model_reasoning_effort = "high"`, check tiers mirror `cmd/fire-check` `-exec codex` (zoo-check-fast = `gpt-5.3-codex-spark`/`medium`, zoo-check-smart = `gpt-5.5`/`high`, zoo-check-smartest = `gpt-5.5`/`xhigh`), reviewer (session-strongest) → `"xhigh"`. When editing a Claude agent or skill, update the Codex copy in the same change.
- zoo-init initializes `.zoo/zoo.md` alongside the Zoo files (step 8 + Files list), so new repos get Zoo customization from the same init pass.
- Check files are deliberately harness-neutral: no `id` field (the filename is the ID), and `model:` uses abstract tiers `cheaper`/`smarter`/`smartest` instead of model names. Claude Code currently maps these to haiku/sonnet/opus; Codex maps them to tiered `zoo-check-*` agent files. Concrete model names belong in the skill or agent config (per harness), never in check files.
- Schema output means reviewer results arrive as validated objects — no report files, no parsing.
- Workflow resume caches the longest unchanged prefix of agent() calls, but passing failed check IDs as args to a fresh run is the cleaner re-run mechanism.

## Open items

- Seed `.zoo/checks/` by exploding `.zoo/review/*.md` (~25 narrow checks) and converting reusable `.zoo/codereview.md` rules into checks (fire-check for mechanical ones, `.zoo/checks/` for judgment ones).
- Grow `cmd/fire-check` (the Tier-0 checker: `RegisterCheck` registry, one self-contained file per check, `fire-check:ignore` line suppression) whenever a Tier-1 check proves mechanically expressible. Default mode checks files changed vs HEAD; `-entire-codebase` audits the whole repo — the first audit found ~164 legacy violations (x/exp imports, ExpectedStatusCode(303)), deliberately left for a separate cleanup task.
- If gate Workflow scripts drift between runs, pin a template into `references/`.
