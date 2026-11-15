# The counter-position

Microsoft [tested the advice](https://developer.microsoft.com/blog/dont-rewrite-your-cli-for-agents) that a CLI should replace flags with one `--json` payload because agents think in structured data. They used a deliberately awkward synthetic CLI: two services, three levels of nesting, arrays, mixed value types, cross-references, over 30 values. Conventional flags held up.

The reasoning: agents have seen millions of conventional invocations in training, and a bespoke input format adds failure modes of its own. Don't build an agent-only surface when the existing one works.

## Takeaways

- Apply the cheap rules first. Reject unknown flags, give exit codes meaning, emit structured errors, never prompt. None needs a redesign.
- Think twice before format-level changes such as a new output encoding, an agent-only mode or a rewrite. The evidence for those is thinner.
- The same argument runs against a second representation. WebMCP for documentation and "put MCP next to the API" both say not to maintain two versions of one interface.

## Where it stops

It tested one thing, how input is shaped. It says nothing against structured errors, exit codes, small default output or safe retries, which are about what comes back. The gh-axi against `gh` result, where output design moved success from 86% to 100% on an unchanged backend, is the strongest case that output matters. It's also project-run. See [evidence.md](evidence.md).
