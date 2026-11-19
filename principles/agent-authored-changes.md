# Agent-authored changes

Agents write more changes than humans can read. Human review becomes a queue nobody clears or a rubber stamp. The fix is to route each change to the cheapest review that is still safe, decided by the change and its evidence, not by who wrote it.

## Lanes

Three lanes cover most changes. Lane 0 merges with no human reviewer, and the merge queue is the only writer of the main branch.

| Lane | Reviewers | Typical changes |
|---|---|---|
| 0 | Agent only | Docs, routine config, maintenance bumps, proven fixes |
| 1 | Agent plus one human, who can be the author | Additive API, ordinary product changes |
| 2 | Agent plus one or two humans | Breaking API, auth, migrations, new dependencies, sensitive paths |

### Lane 0: auto-merge

| Type | Condition | Source |
|---|---|---|
| Docs, copy, Markdown | No code paths touched | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/), [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |
| Internal-only features, additive changes behind a flag | Automated review and security checks green | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/) |
| Routine metadata or config | Listed in advance as pre-approved | [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |
| Automated maintenance and dependency upgrades | Safety checks pass, scheduled in the owning team's working hours, rollback ready | [Spotify](https://engineering.atspotify.com/2026/9/ai-changed-how-spotify-builds-what-we-learned-and-fixed-about-quality-at-higher-velocity) |
| Small bug fix | Red/green proof: a test fails on the base and passes on the head | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| Stale feature-flag or dead-code removal | Static analysis proves the code unreachable | No source. An inference from the rule that only proven, low-risk changes skip people |
| Config and infrastructure inside approved modules | Policy as code passes, blast radius scoped to one project | [HashiCorp](https://www.hashicorp.com/en/blog/hcp-terraform-is-the-control-plane-for-ai-driven-infrastructure), [InfoQ](https://infoq.com/articles/utility-governed-autonomous-intelligence-ai) |
| Reverts of merged changes | Opened automatically when errors rise | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| Formatting, lint autofix, regenerated code | Output of a deterministic tool | No source. An inference from the rule that only proven, low-risk changes skip people |

### Lane 1: one human

| Type | Condition | Source |
|---|---|---|
| Additive public API | API diff shows additions only | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| Moderate-size change | Small enough for the reviewer agent to assess in full | [Gittifacts](https://jovidecroock.com/blog/gittifacts/), [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |
| Ordinary product changes | Not on a sensitive path | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/) |
| Large but straightforward features | Not auto-approved | [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |
| Change with optional evidence missing | Required evidence present | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |

No source defines "moderate" as a number. Pick a size limit and tune it against the revert rate.

### Lane 2: full human review

| Type | Condition | Source |
|---|---|---|
| Breaking API changes | API diff shows removals or changes | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| Auth, permissions, migrations | Any change | [Gittifacts](https://jovidecroock.com/blog/gittifacts/), [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |
| New dependencies | Dependency diff adds a package | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| CI workflows and owner-listed sensitive paths | Always a human, two on the highest-risk paths | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/) |
| Fix without proof, scope drift | No red/green evidence, or the diff exceeds the plan | [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| Diff too large to assess | Exceeds the classifier's token limit, marked undetermined | [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |
| Author not on the trusted list | Any change | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/) |
| Repositories under audit requirements | Auto-approval disabled | [Duolingo](https://www.infoq.com/presentations/duolingo-ai-literacy-code-review/) |

## Classification rules

- **The change sets the lane, not the author.** A classifier reads the evidence: paths touched, diff size, public API diff, dependency diff, test proof, coverage on changed lines.
- **Unknown escalates.** A missing or failed signal can only raise the lane. A model's judgement can push a change up, never down. A reviewer agent that runs out of budget escalates.
- **An agent's claim is an input, not evidence.** The reviewer agent works without the author agent's summary.
- **An agent's approval never fills a human slot.** The person directing an agent counts as a reviewer. Approval covers one commit and resets on commits the system can't vouch for.
- **Open findings block the merge.** Fix each one, or resolve it with a reason.
- **Confidence is not authority.** A model can be sure an action is right and still lack permission to take it ([Computer Sweden](https://computersweden.se/article/4218645/ai-agenter-maste-lara-sig-nar-det-ar-nog.html), in Swedish). Keep the rules outside the model: a versioned catalog that maps each action class to a risk class and an approval requirement, enforced at one gateway ([InfoQ](https://infoq.com/articles/utility-governed-autonomous-intelligence-ai)). The agent proposes, a policy layer decides ([HashiCorp](https://www.hashicorp.com/en/blog/hcp-terraform-is-the-control-plane-for-ai-driven-infrastructure)).
- **Earn autonomy one class at a time.** Start read-only, widen one action class at a time, and make revoking as easy as granting ([InfoQ](https://infoq.com/articles/utility-governed-autonomous-intelligence-ai)).

## Guardrails

| Guardrail | Why | Source |
|---|---|---|
| Track revert and incident rate per lane | The only check that the lanes are right. Merges more than doubled while the reverted share fell by more than 40%. Auto-reviewed diffs revert at about a third of the rate of other diffs | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/), [Meta](https://arxiv.org/abs/2605.30208) |
| Make undo cheap | A feature flag per change, gradual rollout, automatic rollback on an error spike | [Gittifacts](https://jovidecroock.com/blog/gittifacts/), [Spotify](https://engineering.atspotify.com/2026/9/ai-changed-how-spotify-builds-what-we-learned-and-fixed-about-quality-at-higher-velocity) |
| Schedule automated changes when owners are working | An automated dependency upgrade passed every check and failed in production | [Spotify](https://engineering.atspotify.com/2026/9/ai-changed-how-spotify-builds-what-we-learned-and-fixed-about-quality-at-higher-velocity) |
| Let the merge queue re-run checks on main plus the change | Main only points at trees that passed | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/), [Gittifacts](https://jovidecroock.com/blog/gittifacts/) |
| Watch knowledge transfer | Only 14% of review comments concern defects. The rest spread knowledge and team awareness, which auto-merge removes. Meta's RADAR team flags knowledge transfer as the cost that grows with automation | [Bacchelli and Bird](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ICSE202013-codereview.pdf), [DX](https://newsletter.getdx.com/p/what-are-code-reviews-even-for) |
| Fix repeated findings at the source | A lint rule, skill or agent instructions file, so the reviewer stops flagging the same thing | [Anthropic](https://claude.dev/blog/solving-code-review-at-anthropic/) |

Measured results, and how far to trust them, are in [evidence.md](../evidence.md#review-of-agent-authored-changes).
