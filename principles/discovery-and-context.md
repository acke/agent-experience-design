# Discovery and context

An agent has to know a tool exists, how to call it, and what to do after each call. Models know `git` and `curl` from training. They don't know your tool.

## Self-description

- **A `schema` command that works offline.** It lists commands, arguments, output shapes and error codes, with no auth, config or network. An agent can pick an operation from metadata without reading `--help` repeatedly, a failure mode people report as "stuck in a `--help` loop". Test it in CI. The [CLI Spec](https://clispec.dev/) defines one.
- **Concise per-subcommand help.** Short, with an example.
- **No arguments shows live state, not help.** `git status` is the model. An agent that runs the bare command learns something real.

## Next steps

End each response with the command to run next. [Expo's agent CLI](https://expo.dev/blog/introducing-expo-agent-cli) does this on every call, and it's the cheapest way to steer an agent. It also fits errors: the hint field is the next command.

## Getting the tool into the context

Two routes, in order of preference:

1. **Ambient context.** An opt-in setup command installs a session hook that tells the agent the tool exists and what state it's in. Cheapest at runtime, most intrusive to set up.
2. **An on-demand skill.** One line in the agent's list, the full instructions loaded when needed. In OpenRouter's measurements a skill cost 3,315 input tokens against 1,421 for a bare CLI.

Make installation explicit and ask first. Don't write to a user's agent config as a side effect of `npm install`.

## One contract, several surfaces

Generate the CLI, the API client and the MCP tools from the same spec. Cloudflare's [`cf`](https://www.infoq.com/news/2026/10/cloudflare-cf-cli/) is generated from the OpenAPI schemas that already drive its docs and SDKs, which gave it more than 3,000 operations against about 280 in the hand-built tool it replaces, with consistent verbs. Two hand-written contracts drift. See [api/](../api/README.md).

## Names

A new name can escape stale priors. Cloudflare released `cf` instead of changing `wrangler` partly so models wouldn't apply what they learned about the old tool to a different one.

## Feedback from agents

If your tool surprises agents, you want to know. An opt-in feedback channel, shown to the user for approval before anything is sent, lets agents report where the tool got in their way. Scrub before sending, and default to off.
