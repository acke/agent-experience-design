# Errors and retries

The most useful change you can make to a tool agents operate is to improve its failures. Success paths mostly work. Failures are where an agent either recovers or loops.

## What an agent must decide

After a failure the agent sees an exit code or status and some output. From that it picks one of four moves:

| Situation | Agent should |
|---|---|
| It worked | Continue |
| The thing you checked failed (tests, lint) | Read the report and fix the code |
| The command was wrong | Fix the flags or input and retry |
| The environment isn't set up | Fix it or ask a human, and don't retry |

If two of these produce the same signal, the agent guesses. `tsc --frobnicate` and a real type error both exit 1.

## Findings from 12 JavaScript CLIs

[debuff.jp tested](https://debuff.jp/research/what-agents-need-from-cli-failures) ESLint, webpack, npm, Jest, Biome, oxlint, Vitest, tsc, Vite, esbuild, Prettier and Rollup on one platform, one run, one failure mode. Treat it as a probe, not a survey.

- 10 of 12 reject an unknown option. Prettier and Rollup run anyway with a warning and exit 0.
- Three rejections are stack traces.
- Only ESLint and webpack give a usage error its own exit code.
- Seven have a JSON mode. Only npm emits JSON when the tool can't run at all.
- None says "retry may help" in a field an agent can act on.

## A contract

One workable shape, adapted from that study:

**Exit codes**

| Code | Meaning |
|---|---|
| 0 | Worked |
| 1 | The checked thing failed. Read the report |
| 2 | Usage error. Fix flags or input |
| 3 | Environment not ready. Don't retry, fix it |

**Error body**, one JSON document on stdout, including when the tool can't start:

```json
{
  "error": {
    "code": "image_not_found",
    "message": "No image tagged 'v9' in repo 'web'",
    "retryable": false,
    "hint": "Run `myctl images list --repo web` to see tags",
    "input": { "repo": "web", "tag": "v9" },
    "details": [{ "field": "tag", "problem": "unknown value" }]
  }
}
```

- Codes are stable across releases. Messages can change.
- `retryable` or `transient` is explicit. Don't leave it to inference.
- The hint is a command, not prose.
- The echo of the failing input lets the agent construct the fix without re-reading its own history.
- Logs go to stderr. Stdout carries exactly one document.
- Use the same codes in the API, the CLI and MCP tool errors, so one handler covers all three.

On the API side the standard shape is [RFC 9457 Problem Details](https://www.rfc-editor.org/rfc/rfc9457). The `type` URI doubles as a lookup key for docs. See [api/](../api/README.md).

## Retry is not free

Don't tell an agent to retry every 5xx. The server may have committed the change before the connection died. Separate "retry recommended" (rate limit, transient) from usage and auth errors, and give create-like operations an idempotency key, because agents double-submit. See [safe mutations](safe-mutations.md).

## Unknown flags

Reject them, exit non-zero, and name the closest valid one. A tool that warns and continues has decided on the agent's behalf that the flag didn't matter.
