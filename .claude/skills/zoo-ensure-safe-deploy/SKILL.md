---
name: zoo-ensure-safe-deploy
description: "Manual deployment-safety validation for unpushed commits or user-specified changes. Use only when explicitly requested, especially in Codex /goal or Claude /loop, to hunt production rollout failures, legacy-data and migration hazards, mixed-version issues, and other real deployment risks before declaring safe or unsafe."
---

Follow `.zoo/zoo.md`, `.zoo/coding.md`, `.zoo/testing.md`, `.zoo/codereview.md`, `.zoo/browser.md` if exists/relevant.

Top-level only. Default scope: unpushed commits on current branch; use user-specified range/PR/files/ticket when given. Exclude unrelated dirty work unless asked; report if it affects validation.

Goal: prove safe to deploy or prove unsafe. No "looks good", "low risk", or "probably safe" conclusion.

Stop only when:
- scoped changes are proven safe, with affected deployment surfaces validated and relevant checks passing
- unsafe behavior is proven with failing test, repro, query, browser evidence, or concrete code/data evidence
- blocked by missing access/credentials, destructive approval, ambiguous scope/base, or user stop

Method:
1. Scope: record branch, upstream, git status, base, commits, files, diff stats, migrations, config, jobs, API/UI, infra, data-shape changes.
2. Inventory risks: API, UI, data, DB/migrations, auth, billing, jobs, integrations, config/flags, time, concurrency, cache, rollout, rollback, mixed versions. Trace entry points through code/templates/jobs/migrations/config/tests. Compare before/after when needed.
3. Hunt production failures: old records, weird tenants, pending jobs, webhooks/retries, old payloads, rollback/partial migration, stale caches/sessions/clients, flags, prod-only hooks, rate limits. Search persisted names, enums, API fields, config props, routes, job kinds, payloads, cache keys, templates, analytics/log consumers.
4. Inspect prod safely: read-only, smallest useful counts/samples/invariants, no secrets or sensitive records. Verify production config/data when assumptions depend on it.
5. Validate targeted risks first; broader suites only as blast radius requires. Add/tighten tests or repros for real invariants. Use browser/integration evidence for visible changes. Rerun after fixes/repros.
6. Use zoo-code-review or reviewers when useful; focus prompts on deployment failures, prod data/config, legacy data, rollout/rollback, migration safety.
7. Report scope, guarantee (`safe to deploy` only if proven; else `unsafe` or `not proven safe`), evidence, problems, residual risk.
