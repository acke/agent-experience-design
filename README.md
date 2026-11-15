# Agent experience design

Notes on building CLIs, APIs and MCP servers that AI agents can use well. The discipline is called AX, agent experience.

An agent reads tool output as input to its next decision. It can't answer a prompt, guess a flag, or tell a stack trace from a real result, and every line it reads costs tokens. Humans benefit from the same fixes, so none of this makes a tool worse for people.

These are working notes, not a spec. Each claim links to its source. Benchmarks are mostly run by the projects that publish them, and [evidence.md](evidence.md) says which.

## Start here

| If you want to | Read |
|---|---|
| Understand the premise and the rules every surface shares | [principles/agent-as-reader.md](principles/agent-as-reader.md) |
| Get errors and retries right | [principles/errors.md](principles/errors.md) |
| Keep output small | [principles/output-size.md](principles/output-size.md) |
| Make writes safe to repeat | [principles/safe-mutations.md](principles/safe-mutations.md) |
| Help agents find the tool and the next step | [principles/discovery-and-context.md](principles/discovery-and-context.md) |
| Design a CLI | [cli/README.md](cli/README.md) |
| Design an API | [api/README.md](api/README.md) |
| Decide how MCP fits | [mcp/README.md](mcp/README.md) |
| Hear the case against rewriting anything | [counter-position.md](counter-position.md) |
| See the numbers and how far to trust them | [evidence.md](evidence.md) |

## The short version

1. Never prompt. Without a terminal and without `--yes`, fail with an error that names the bypass.
2. Reject unknown flags with a non-zero exit. A warning the agent never sees is a silent success.
3. Give exit codes meaning: worked, checked thing failed, usage error, environment problem.
4. Return structured errors: stable code, message, retry flag, hint with the next command, echo of the input.
5. Emit JSON when piped, with the same shape on failure as on success.
6. Keep default output small. Offer field selection, pagination and a way to get the rest.
7. Say "0 results" when it's empty.
8. Make mutations safe to repeat. Offer `--dry-run` that returns a machine-readable diff.
9. Let the tool describe itself offline, with no auth, config or network.
10. End every response with the next step.
11. Keep secrets out of argv. Treat remote data as data, never as instructions.
12. Fix agent mistakes in the tool, not in more documentation.

## Sources

Most of the thinking here comes from others. The main ones are below, and [references.md](references.md) has the full list:

- [AXI](https://github.com/kunchenguid/axi), Kun Chen: ten principles for agent-driven CLIs
- [cli-best-practices](https://github.com/nerveband/cli-best-practices), Ashraf Ali: a 97-check audit
- [What agents need from CLI failures](https://debuff.jp/research/what-agents-need-from-cli-failures): 12 CLIs tested
- [Don't rewrite your CLI for agents](https://developer.microsoft.com/blog/dont-rewrite-your-cli-for-agents), Microsoft
- [MCP vs CLI vs skills](https://openrouter.ai/blog/insights/mcp-vs-cli-vs-skills/), OpenRouter
- [CLI Spec](https://clispec.dev/) and [clig.dev](https://clig.dev/)

## Contributing

Conventions and checks are in [AGENTS.md](AGENTS.md). CI checks every link weekly.

## License

MIT. See [LICENSE](LICENSE).
