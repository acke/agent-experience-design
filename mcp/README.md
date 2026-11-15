# MCP

A stub. MCP is the third surface, and the cost and architecture questions are where it overlaps with the CLI and the API. The [shared principles](../principles/agent-as-reader.md) apply as well.

## Cost

- **Definitions cost every request.** 46 GitHub MCP tools cost 19,436 tokens on Sonnet 5.5. [Anthropic's own example](https://www.anthropic.com/engineering/code-execution-with-mcp) is 58 tools across five servers at about 55,000 tokens before the conversation starts.
- **Results cost more.** One release object was about 20,400 tokens. Field selection and default page sizes matter more than trimming definitions. See [output size](../principles/output-size.md).
- **Scope the toolset.** Expose only the tools the task needs (`--tools=`, `--toolsets=`).
- **Tool search helps, unevenly.** In OpenRouter's test it cut Sonnet's whole-task cost from 59k to 30k tokens and made Haiku 5.5 worse, from 99k to 155k, through retry loops on wrong-typed arguments. Their docs advise keeping three to five tools loaded.
- **Code execution** is [Anthropic's answer](https://www.anthropic.com/engineering/code-execution-with-mcp): expose servers as code modules, load definitions on demand, filter data before it returns. One workflow dropped from about 150,000 to about 2,000 tokens. It needs a sandbox.

## Architecture

- **Next to the API, not on top.** See [api/](../api/README.md).
- **Both, with MCP steering to the CLI.** [Sentry ships one install](https://x.com/zeeg/status/2120224571689484639) that gives a CLI and `sentry mcp`, an MCP server in stdio mode sharing the CLI's credentials. The CLI has a filesystem, so an agent can pull raw logs and grep them. MCP is native to the agent, so it gets discovery and steering without being told the tool exists. The server knows it was launched from the CLI and can point the agent back to it for more detail. Sentry calls the shared-auth shortcut a stopgap.
- **Same error codes everywhere.** An agent should handle a CLI failure, an API failure and an MCP tool error with one handler. See [errors](../principles/errors.md).

## Protocol features worth using

- **Elicitation:** the server asks the user something mid-call. The human-in-the-loop hook.
- **Tasks:** a long-running call returns a handle, with status, cancel and result later.
- **Server `instructions`:** harnesses support them unevenly, so don't depend on them alone.

## Caveats

Harnesses differ in deferred tool loading and in how much of the spec they implement. Test against the clients your users have. Every number above comes from a vendor or a single test, so see [evidence.md](../evidence.md).
