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

## Review of agent-authored changes

All self-reported by the organisation running the process, on its own repositories. None has an independent replication. See [agent-authored-changes.md](principles/agent-authored-changes.md).

**Meta, RADAR, arXiv preprint.** Over 535,000 diffs reviewed and over 331,000 landed. RADAR-reviewed diffs show a revert rate of about one third, and a production incident rate of about one fiftieth, of other diffs. Low-risk diffs land after deterministic validation. Known from the abstract only. The abstract's time-to-close figure doesn't add up as a percentage, so it is left out here.

**Anthropic, Claude Code repositories.** From June to September, merged PRs more than doubled and the reverted share fell by more than 40%. The share of internal PRs with substantive review comments rose from 16% to 54%. Changes landing per day in the monorepo went from about 2,500 to over 30,000 between May and September. The revert figure is the guardrail. Incident numbers are not given.

**Duolingo, conference talk.** About 300 engineers and about 200 PRs a day. Auto-approved PRs grew from about 0% to about 10% in roughly six months, and median time to merge fell from about 18 to about 12 hours. One or two auto-approved PRs a day are reverted. The speaker knows of no incident caused by one. Spoken estimates, not a study.

**Spotify, engineering blog.** Merged changes went from about 8,100 to 17,000 a month, year over year. The vast majority of automated maintenance PRs merge automatically after safety checks. No share is given. One automated dependency upgrade passed every check and failed in production. Incidents reviewed so far found no material direct contribution from AI-authored code.

**Gittifacts, one author, own repository.** Six agent-opened changes landed through the queue. Evidence took 13 to 17 minutes per change, and pre-existing test failures pushed every change into full review. A design with a small trial, not a production result.

**Cloudflare, AI code reviewer cost.** Average cost per review fell from $2.43 to $0.77 and median latency from 4m 09s to 1m 32s. About two thirds of jobs are re-reviews, so skipping unchanged reviewers saves real money.

**Bacchelli and Bird, Microsoft, 2013.** Of the review comments they classified, 78 (14%) concerned defects. Code improvements were the largest category at 29%. Predates agent-written code.

## What's missing

- Independent replications of any benchmark above
- Tests across several models and harnesses
- A conformance suite for AXI-style principles. The CLI Spec covers part of it.
