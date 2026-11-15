# Safe mutations

Agents repeat themselves. They retry after timeouts, resume after context compaction, and run a step again because they can't tell whether it landed. A mutation has to survive that.

## Declare the retry behaviour

Every command or endpoint is one of:

| Kind | Repeat it and... |
|---|---|
| Read-only | Nothing changes |
| Idempotent | The end state is the same. The response says `"changed": false` when nothing changed |
| Neither | Needs an idempotency key, or a documented way to check whether it already happened |

Put the declaration in the machine-readable schema, not only in prose.

## Preview before committing

`--dry-run` on every destructive or non-idempotent command, and:

- Return a diff an agent can parse, not a sentence.
- Say whether the server validated the plan or the client only computed it. They are very different promises.

## A safe sequence

From [API7's write-up on agentic CLIs](https://api7.ai/blog/designing-apis-for-agentic-clis):

1. Read the current revision
2. Plan the change
3. Validate the inputs
4. Apply with a precondition (an ETag or revision check)
5. Observe the operation
6. Verify the resource

Steps 1 and 4 give you optimistic concurrency, so a stale agent can't overwrite newer work.

## Long-running work

Return a handle, not a hang. Give the agent a `--wait` flag with a timeout for the simple case, and a status call for the rest. MCP tasks do this natively. See [mcp/](../mcp/README.md).

## Confirmation without a terminal

If a command would ask "are you sure?" and there is no terminal, refuse with an error that names the bypass flag (`--yes`) and exits with the usage code. Never proceed silently, and never hang.
