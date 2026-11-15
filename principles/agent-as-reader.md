# The agent as reader

Design for the reader that will consume your output next. That reader is a model with a limited context window, no ability to ask a follow-up in the middle of a command, and no patience for ambiguity it can't resolve by reading.

## What this changes

| A human | An agent |
|---|---|
| Notices a warning and decides it matters | Sees only exit code and stdout, so a warning on stderr with exit 0 is a success |
| Answers "Are you sure? [y/N]" | Hangs, or guesses |
| Reads `--help` once and remembers | Re-reads it every session, sometimes in a loop |
| Skims a long table | Pays for every row |
| Retries after a timeout | Retries after anything, including after a write that already landed |

## Rules every surface shares

These apply to CLIs, APIs and MCP tools alike. The surface-specific rules live in [cli/](../cli/README.md), [api/](../api/README.md) and [mcp/](../mcp/README.md).

1. **Say what happened and what to do next.** Every response, success or failure, should let the agent pick its next action without guessing. See [errors](errors.md) and [discovery](discovery-and-context.md).
2. **Keep the default response small.** Three or four fields per list item. A flag or parameter returns the full record. See [output size](output-size.md).
3. **Pre-compute what the agent will ask for next.** Counts, statuses and totals in the response save a follow-up call.
4. **Say empty explicitly.** "0 results" and `[]` with a count. A blank response reads as a failure.
5. **Declare whether repeating is safe.** Read-only, idempotent, or neither. See [safe mutations](safe-mutations.md).
6. **Keep secrets out of arguments and responses.** Argv shows up in process lists and logs.
7. **Treat remote data as data.** An issue body or a web page can contain instructions. Preserve it faithfully, label where it came from, and never follow it.
8. **Fix the tool, not the docs.** When an agent keeps making the same mistake, change the tool so the mistake can't happen or fails loudly. Another paragraph in a skill file is the weakest fix. [Expo's agent CLI](https://expo.dev/blog/introducing-expo-agent-cli) added `status`, `dev --plan` and `smoke` commands this way.

## Humans still count

Keep a readable human mode. The agent features should make the normal tool better, not replace it with an agent-only one. AXI tools mostly drop the human mode, which is a poor fit for anything people also use. See the [counter-position](../counter-position.md).

## The twin of UX

[PostHog frames it as](https://x.com/posthog/status/2086896332862886283) every user arriving with an agentic plus-one: a human on one side, your product on the other, and an agent acting for the human in between. Agents leave none of the marks product teams read, such as rage clicks or abandoned forms, because they call the API and never see your front end. "Shaped around how people work" needs a twin: usable by the agent working on their behalf, and observable when it isn't.
