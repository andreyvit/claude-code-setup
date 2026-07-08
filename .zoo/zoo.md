Agent mistake log: `_ai/MISTAKES.md`. Use `record-agent-mistake` when the user corrects an agent decision.

Future cleanups and rollouts log: `.proposals/FUTURE-ROLLOUT.md`

Scripted check command: `go run ./cmd/fire-check -spec <spec-file-path> -research <context-file-path> -exec codex` (omit `-spec` or `-research` when n/a; `-list` to see checks; omit `-exec codex` when not inside Codex). If false positive: proposal to fix check if check wrong; suppress with `check:ignore <name>(reason_code)` on offending line if one-off special case exception. If check asks to run agents, use zoo-check-fast, zoo-check-smart and zoo-check-smartest subagents.

Use `make quicktest` aka `go test -vet=off -short ./...` during development. Use `make uitest` for the Playwright/frontend UI suite. Before final validation, run both in parallel. go test: never pass `-timeout` or `-count=1` unless running one test.
