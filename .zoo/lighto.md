# Lighto project configuration

- Tickets live in Linear; read with `linearis issues read DEV-1234`, download images with `linearis embeds download <url> --output <path>`; see `_ai/linear.md`.
- Tier-0 scripted check command: `go run ./cmd/fire-check` (no args = all checks over files changed vs HEAD; check names as args to run a subset; `-list` to enumerate). Suppress a deliberate violation with a `fire-check:ignore` comment on the offending line.
