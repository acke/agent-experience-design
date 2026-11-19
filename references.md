# References

Original sources, grouped by what they support. A source marked *not read in full* is one known only from an abstract or a secondary summary, so check it before quoting.

## Frameworks and standards

- Kun Chen, [AXI: Agent eXperience Interface](https://github.com/kunchenguid/axi) and [axi.md](https://axi.md/). Ten principles for agent-driven CLIs, with a 915-run benchmark. [Skill file](https://github.com/kunchenguid/axi/blob/main/.agents/skills/axi/SKILL.md).
- Ashraf Ali, [cli-best-practices](https://github.com/nerveband/cli-best-practices). 97-check audit, Agent DX Scale.
- [CLI Spec](https://clispec.dev/), [conformance](https://clispec.dev/#conformance).
- [clig.dev](https://clig.dev/), Command Line Interface Guidelines.
- [RFC 9457](https://www.rfc-editor.org/rfc/rfc9457), Problem Details for HTTP APIs.
- tumf, [Agentic CLI design: 7 principles](https://dev.to/tumf/agentic-cli-design-7-principles-for-designing-cli-as-a-protocol-for-ai-agents-2c10). *Not read in full.*
- [yogin16/better-cli](https://github.com/yogin16/better-cli). *Not read in full.*
- [CodyEngel/axi-axi](https://github.com/CodyEngel/axi-axi), a scaffolder and read-only checker for AXI.

## Measurements

- debuff.jp, [What agents need from CLI failures](https://debuff.jp/research/what-agents-need-from-cli-failures). 12 JavaScript CLIs.
- Microsoft, [Don't rewrite your CLI for agents](https://developer.microsoft.com/blog/dont-rewrite-your-cli-for-agents).
- OpenRouter, [MCP vs CLI vs skills](https://openrouter.ai/blog/insights/mcp-vs-cli-vs-skills/).
- Anthropic, [Code execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp).
- Scalekit, [MCP vs CLI](https://scalekit.com/blog/mcp-vs-cli-use). Vendor-run.
- AIMultiple, [Code execution with MCP](https://research.aimultiple.com/code-execution-with-mcp). *Not read in full.*
- kb4ai, [critique of AXI](https://github.com/kb4ai/mcp-considered-suboptimal-pub-kb/blob/master/ramblings/2026-05-17--axi-agent-experience-initiative.md).

## Formats

- [TOON](https://github.com/toon-format/toon) README.
- Matveev, [independent generation test](https://arxiv.org/abs/2603.03306), preprint. *Abstract only.*

## Design write-ups

- Expo, [Introducing Expo Agent CLI](https://expo.dev/blog/introducing-expo-agent-cli).
- InfoQ, [Cloudflare's `cf` CLI](https://www.infoq.com/news/2026/10/cloudflare-cf-cli/).
- David Cramer, [Sentry's `sentry mcp` bridge](https://x.com/zeeg/status/2120224571689484639).
- Naftiko, [Put MCP next to your API, not on top of it](https://naftiko.io/blog/put-mcp-next-to-your-api-not-on-top/).
- magiroux.com, [From use case to GraphQL operation](https://magiroux.com/posts/from-use-case-to-graphql-operation).
- Zuplo, [The API readiness gap for agents](https://zuplo.com/learning-center/api-readiness-gap-agent-callable-apis).
- API7, [Designing APIs for agentic CLIs](https://api7.ai/blog/designing-apis-for-agentic-clis).
- PostHog, [Are agents really killing UI?](https://x.com/posthog/status/2086896332862886283) and [38 things we've learned building AI products](https://newsletter.posthog.com/p/38-things-weve-learned-about-building).

## Review of agent-authored changes

- Addy Osmani, Morgan Westlee Lunt and Qing Wang, [How the Claude Code team does code review](https://claude.dev/blog/solving-code-review-at-anthropic/). Risk tiers, approval rules, revert rate as guardrail.
- Jovi De Croock, [Gittifacts](https://jovidecroock.com/blog/gittifacts/). Lanes from evidence, escalation rules, merge queue, per-change rollout. A design with a small trial.
- Sarah Deitke, [Teaching engineers, trusting AI](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/), QCon London. Duolingo's PR risk assessment and auto-approval.
- Chris Adams, Audris Mockus, Peter Rigby, Nachiappan Nagappan et al., [Automating low-risk code review at Meta](https://arxiv.org/abs/2605.30208), preprint. *Not read in full.*
- DX, [What are code reviews even for?](https://newsletter.getdx.com/p/what-are-code-reviews-even-for). Summary of RADAR results and the knowledge transfer tradeoff.
- Tyson Singer, [AI changed how Spotify builds](https://engineering.atspotify.com/2026/9/ai-changed-how-spotify-builds-what-we-learned-and-fixed-about-quality-at-higher-velocity). Change volume, automated maintenance, the dependency upgrade incident. InfoQ's [write-up](https://www.infoq.com/news/2026/10/spotify-ai-quality-controls/) gives the 2.5 million automated PR figure.
- Ryan Skidmore, [Cutting our inference bill by 68%](https://blog.cloudflare.com/obsessive-inference-optimization). Cost of an AI code reviewer. Vendor-run.
- Alberto Bacchelli and Christian Bird, [Expectations, outcomes, and challenges of modern code review](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ICSE202013-codereview.pdf), ICSE 2013.
- Purnang Borah, [Governed autonomous intelligence for mission-critical utility operations](https://infoq.com/articles/utility-governed-autonomous-intelligence-ai). Policy gate outside the model, action catalog, autonomy earned per action class. A design, no measured results.
- Simon Lynch and Aaron Evans, [HCP Terraform is the control plane for AI-driven infrastructure](https://www.hashicorp.com/en/blog/hcp-terraform-is-the-control-plane-for-ai-driven-infrastructure). "The agent can propose. It should not decide." Vendor write-up.
- Computer Sweden, [AI-agenter måste lära sig när det är nog](https://computersweden.se/article/4218645/ai-agenter-maste-lara-sig-nar-det-ar-nog.html), in Swedish. Allan Dabre (PwC) on confidence against authority.
