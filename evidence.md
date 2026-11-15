# Evidence

What the numbers say and how far to trust them. Most are run by the project that publishes them, on one model, with small samples. Each entry says which.

## Output design

**AXI, 915 runs, Claude Sonnet 4.6, run by the AXI author.** The GitHub tasks (425 runs) held the backend constant:

| Condition | Success | Cost per task | Turns |
|---|---|---|---|
| gh-axi | 100% | $0.050 | 3 |
| plain `gh` | 86% | $0.054 | 3 |
| GitHub MCP | 87% | $0.148 | 6 |
| MCP + tool search | 82% | $0.147 | 8 |
| MCP + code mode | 84% | $0.101 | 7 |

The useful comparison is gh-axi against `gh`: same backend, same model, different output. No independent replication. A critique in [kb4ai's note](https://github.com/kb4ai/mcp-considered-suboptimal-pub-kb/blob/master/ramblings/2026-05-17--axi-agent-experience-initiative.md) points to inconsistent figures across write-ups, one named model, and prose principles with no conformance suite.

## Interface cost

**OpenRouter, one task, Sonnet 5.5.** CLI 1,421 input tokens, skill 3,315, GitHub MCP 59,417. Result size beat definition size as a cost driver. Tool search helped some models and hurt others. See [output-size.md](principles/output-size.md) and [mcp/](mcp/README.md).

**Scalekit, 75 runs, vendor-run.** Reports a CLI 10 to 32 times cheaper than MCP and 100% reliable against 72%. Attribution of the original test is muddled across write-ups and the samples are small. A vendor selling MCP auth argues multi-tenant products still need MCP for governance.

**Anthropic, code execution with MCP.** About 150,000 to 2,000 tokens, 98.7%, on one Drive-to-Salesforce workflow. The productised programmatic tool calling shows a smaller average reduction, 37%, on complex research tasks.

Ask which layer a "30 times cheaper" claim measured: transport, protocol or output design. Most benchmarks change two at once.

## Failure behaviour

**debuff.jp, 12 JavaScript CLIs.** One platform, one run, one failure mode. See [errors.md](principles/errors.md).

## Input design

**Microsoft, one synthetic CLI.** Conventional flags beat a single `--json` payload in their test. See [counter-position.md](counter-position.md).

## Formats

**TOON.** Reports about 40% fewer tokens than JSON. A retrieval benchmark and an independent generation test disagree on whether accuracy holds, so treat it as unproven.

## What's missing

- Independent replications of any benchmark above
- Tests across several models and harnesses
- A conformance suite for AXI-style principles. The CLI Spec covers part of it.
