# DepWatch — Build Plan

The plan is the tests. Written from the vision conversation, before any product code.

## Vision Summary

A developer runs `depwatch scan` and sees, in under 10 seconds, which dependencies have breaking changes and how risky they are. The output fits in an 80-column terminal. Urgent alerts go to Slack DM. GitHub issues get a comment with the risk assessment. No web UI, no accounts, no database. The tool reads the world, analyzes it, and tells you what matters. Precise, fast, confident.

## What We Would NOT Build

- Web UI. This is for developers who live in terminal and Slack.
- Database. State is in GitHub repos and npm registry. DepWatch reads, never stores.
- User accounts. Config is local. No server.
- Dashboard. Output is a table in the terminal.
- Real-time monitoring. Runs on demand or cron. Not a service.

## Test Suite

Tests installed: vitest (integration for all layers, CLI behavior, output formatting).

### CLI Tests (vitest — user experience)

```
// Core flow
test('scan command completes in under 10 seconds for a repo with 50 dependencies')
test('scan output is a table sorted by severity — most critical first')
test('table fits in 80 columns — no line wrapping, no horizontal scroll')
test('colors indicate severity — red for breaking, yellow for major, dim for safe')
test('when no breaking changes found, outputs a single line and exits with code 0')
test('when breaking changes found, exits with code 1 — CI can gate on this')

// Detail command
test('detail command for a specific dependency shows changelog excerpt, type signature changes, and affected imports')
test('detail output is structured — header, then changes, then recommendation')

// Config
test('config command adds and removes repos from .depwatch.json')
test('invalid config is rejected on read with a clear message, not a stack trace')
```

### Delivery Tests (vitest — cross-channel)

```
// Slack
test('urgent alert sends Slack DM with risk level, affected repos, and one-line summary')
test('Slack message uses the same terminology as CLI output — same risk levels, same dep names')

// GitHub
test('GitHub comment on issue includes risk assessment and recommended action')
test('comment is concise — under 500 characters for a single dependency update')

// Cross-channel
test('CLI, Slack, and GitHub all lead with the risk level, then the details')
test('same dependency update produces the same risk classification in all three channels')
```

### Integration Tests (vitest — enrichment/inference/interpretation)

```
// Enrichment
test('GitHub scanner reads package.json and lockfile from a repo')
test('npm registry client fetches version info, changelog, and type signatures')
test('rate limit management respects npm registry limits — no 429 errors on normal use')

// Inference
test('semver analyzer detects breaking changes from version diff')
test('type signature comparator flags removed or changed exports')
test('changelog parser extracts breaking change entries')

// Interpretation — the LLM call
test('risk assessment includes a severity score, plain-language explanation, and recommended action')
test('LLM output matches the schema — severity enum, explanation string, action string')
test('when changelog is empty, interpretation says so explicitly — no hallucinated changes')
```

## Build Order (derived from test dependencies)

1. Scaffold (CLI entry point, subcommands, config management)
2. Enrichment (GitHub scanner, npm client, rate limiting)
3. Inference (semver analyzer, type comparator, changelog parser)
4. Interpretation (LLM call with schema validation, prompt in dedicated file)
5. Delivery (CLI table formatter, Slack DM, GitHub commenter)
6. Subcommands wired together (scan, watch, detail, config)

## Key Design Decisions

- Message structure is designed even without a visual surface: lead with risk level, then affected repos, then details. Same structure in CLI, Slack, and GitHub.
- Exit codes are meaningful: 0 = no breaking changes, 1 = breaking found. CI depends on this.
- The LLM call has one prompt file with a graduation marker: `// graduate to template when patterns stabilize across 50+ summaries`.
- 80-char terminal width is a hard constraint. Tables adapt, not overflow.
- No colors on non-TTY output (piping, CI logs). Detect and fall back to plain text.
