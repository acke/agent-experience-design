# Output size

Result size is a bigger lever than definition size. That's the main finding of [OpenRouter's measurements](https://openrouter.ai/blog/insights/mcp-vs-cli-vs-skills/), and it holds for CLIs and APIs as much as for MCP.

## Numbers

Same task (latest release of `cli/cli`), Sonnet 5.5, all runs correct. Input tokens:

| Interface | Tokens |
|---|---|
| CLI | 1,421 |
| Skill | 3,315 |
| GitHub MCP, 46 default tools | 59,417 |

The 46 tool definitions cost 19,436 tokens. One release object cost about 20,400, more than all the definitions together. A `fields` argument on `list_issues` cut one response from 8,336 to 714 characters. Unbounded, one model asked for 100 issues and got 281,963 characters.

Single task, single vendor's harness. The shape of the result matters more than the exact figures.

## Rules

1. **Small by default.** Three or four fields per list item, enough to pick one. [AXI](https://github.com/kunchenguid/axi) uses the same rule.
2. **Offer the rest explicitly.** `--full`, `--fields a,b,c`, or `fields=` on the API.
3. **Truncate with a size hint.** "Showing 2,000 of 48,311 characters. Use `--full` or `--offset 2000`." Say when a result is partial in the output itself (`truncated: true`, `next_cursor`, `total`).
4. **Paginate anything that can grow.** Pick a sane default page size. Don't default to "all".
5. **Cap before the model sees it.** If the client can limit a payload, it should. A 4,000-character cap worked in the OpenRouter test only because the interesting fields sat near the top of the object, so put the useful fields first.
6. **Pre-compute aggregates.** Counts and statuses in the response replace the follow-up call.
7. **Say empty explicitly.** "0 results".

## Formats

JSON is fine. [TOON](https://github.com/toon-format/toon) reports about 40% fewer tokens than JSON, but that figure is project-reported and independent tests are mixed. Fix field selection and pagination first, since they save more and cost nothing in compatibility.

Tables are for eyes, JSON is for parsers. Cloudflare's `cf` defaults to JSON for that reason. Whether to default to JSON or switch on a pipe is a judgement call. Switching on a pipe plus an explicit `--format` flag keeps tables for humans.
