# CLI

Rules that only make sense for a command-line tool. The [shared principles](../principles/agent-as-reader.md) apply as well.

## Input

1. **Never prompt.** Every input works as a flag, environment variable, file or stdin. A command that would prompt, with no terminal and no `--yes`, fails with an error that names the bypass flag.
2. **Reject unknown flags** with a usage exit code, and suggest the closest valid one.
3. **Keep secrets out of argv.** Read credentials from environment variables, stdin, a keychain or a saved profile. `--token abc123` ends up in process lists and shell history.
4. **Consistent verbs and flags** across commands. An agent that learns `list` and `get` on one resource will try them on the next.

## Output

5. **JSON when piped,** and on request through an explicit `--format` or `--output` flag. Data on stdout. Progress and diagnostics on stderr. No color codes when stdout isn't a terminal.
6. **The same shape on failure as on success,** including when the tool can't run at all. One JSON document, not a stack trace.
7. **Meaningful exit codes.** 0 worked, 1 the checked thing failed, 2 usage error, 3 environment not ready. See [errors](../principles/errors.md).

## Discovery

8. **`mytool schema` works offline.** See [discovery](../principles/discovery-and-context.md).
9. **No arguments shows live state,** not help.
10. **`--dry-run`** on destructive commands, returning a machine-readable diff. See [safe mutations](../principles/safe-mutations.md).

## Lifecycle

11. **Self-update that agents can check.** `update check` reports availability and exits non-zero if a newer version exists, without downloading.
12. **Ship a short skill.** A `SKILL.md` that teaches workflows and guardrails, backed by the schema, beats long docs.

## Keep it a normal CLI

Stay a good Unix citizen: a readable human mode, composable output, conventional flags. [Microsoft measured](../counter-position.md) a conventional CLI against a single `--json` payload and found the conventional one held up.

## Checking

- [cli-best-practices](https://github.com/nerveband/cli-best-practices) has a 97-check audit across 17 categories. Each check is verified by running a command.
- [CLI Spec conformance](https://clispec.dev/#conformance) tests `schema` output and runtime behaviour against a published standard.
- A quick read: score seven axes from 0 to 3, the Agent DX Scale from the same repo, adapted from Justin Poehnelt.

## Examples

- [gh-axi](https://github.com/kunchenguid/axi) against plain `gh`: same backend, 100% against 86% task success at about equal cost in the project's own benchmark.
- Cloudflare's [`cf`](https://www.infoq.com/news/2026/10/cloudflare-cf-cli/): JSON by default, schema-generated, typed config, a new name.
- Sentry's `sentry mcp`: [one install, CLI plus an MCP server sharing credentials](../mcp/README.md).
