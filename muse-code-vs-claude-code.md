# Muse Code vs Claude Code in 2026: Meta's Cheapest Weapon vs the Default Everyone Uses

Meta shipped its first coding agent last week, and the funniest thing about the launch is the benchmark chart.

Meta published three of them. Terminal-Bench 2.1, DeepSWE 1.1, and their own internal coding bench. Claude Code sits at the top of all three. Meta put that chart in its own launch post, on its own website, on launch day.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_1.png)

That is not an accident, and it is not humility. It is the pitch. Meta is not claiming Muse Code is smarter than Claude Code. Meta is claiming Muse Code is 12 to 21 times cheaper, as long as you let them train on your code.

So this is a different kind of comparison than the usual one. Not "which agent is better." More like: what is a codebase worth, and is Meta offering enough for yours?

Let's see!

## Muse Code vs Claude Code Summary

| Category | Claude Code | Muse Code | Winner |
| --- | --- | --- | --- |
| Vendor benchmarks (Meta's own charts) | Leads all three: 86.7% TB 2.1, 65.0% DeepSWE | 82.9% TB 2.1, 59.3% DeepSWE | Claude Code |
| Standard token price | Sonnet 5 at \$2/\$10 per Mtok (intro, ends Aug 31) | \$1.25/\$4.25 per Mtok, flat, no long-context step | Muse Code |
| Cheap tier | None. no data-for-discount trade offered | \$0.10/\$0.20 per Mtok on the contributor tier | Muse Code |
| What the cheap tier costs you | N/A | Meta may train on your prompts and completions | Claude Code |
| Predictable billing | Pro at \$17–20/mo bundles the harness | Pay-per-token only, no subscription exists | Claude Code |
| Parallel fan-out | 20 concurrent subagents, nesting 3 deep | 2–16 children (cores minus two), one level only | Claude Code |
| Background agents | Spawned per task | 4 persistent observers that live for the session | Muse Code |
| Crash recovery | Checkpoints and rewind | Append-only event log, replay-exact resume | Muse Code |
| Surfaces | Terminal, VS Code, JetBrains, desktop, web, mobile, Channels, Routines | Terminal. That's it. | Claude Code |
| Platform support | macOS, Linux, Windows | macOS and Linux; Windows only via WSL | Claude Code |
| Model choice | Claude models only | Muse Spark 1.2 only | Draw |
| Extensibility | Plugins, marketplaces, MCP, skills, 14 hook events | MCP, skills, hooks; all shipped day one, no ecosystem yet | Claude Code |
| Openness | Closed | Closed, but weights for the model are promised "soon" | Draw |
| Long, unattended runs | Strong, but no replay log | Built for it , 24h/1,000+ tool call demo | Muse Code |

#### In Summary:

**Scorecard: Claude Code 7, Muse Code 4, two draws.**

Claude Code wins on almost everything that shows up in a feature table, and Meta's own numbers agree with that. Muse Code wins on the two things a feature table is bad at measuring: what a task costs, and what happens when a six-hour run dies at hour five.

And here's the twist. The scorecard is lopsided, but the most interesting result anyone has published on Muse Code so far is a 222-pull-request audit that finished in under five minutes and cost ten cents. Nobody switched their daily driver. Several people added a second one.

I'll get to why that matters.

## What is Muse Code?

> Meta's first coding agent; terminal only, built around an event log, and priced to buy training data.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_2.png)

[Muse Code](https://developer.meta.com/ai/resources/blog/build-with-muse-code/) launched in beta on **August 5, 2026**, announced by Mark Zuckerberg on X and shipped by Meta Superintelligence Labs under Alexandr Wang. It is the first coding-specific product out of MSL.

Install is one line, macOS and Linux only:

```bash
curl -fsSL https://dev.meta.ai/install.sh | bash
```

No npm, no Node, no Python, just a \~97 MB binary and a browser auth flow. Windows users get WSL or nothing.

It runs on **Muse Spark 1.2**, a coding-focused model with a 1M-token context window that Meta co-trained with the harness itself. That is the actual architectural claim: the model was trained inside the runtime it ships in, on rejection-sampled harness trajectories, with specific recipe work for goals, compaction, and subagents.

Zuckerberg's own framing of what it does: planning changes, writing code, validating the results, across large repositories.

## What is Claude Code

> Anthropic's batteries-included coding agent, and still the thing every new agent gets measured against.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_3.png)

[Claude Code](https://code.claude.com/docs/en/overview) went to research preview in February 2025 and became the fastest-scaling developer product anyone has measured. As of the v2.1.224 line in August 2026 it ships subagents nesting three layers deep, Agent Teams, plan mode, MCP as client and server, Agent Skills, plugins with official and community marketplaces, hooks on 14 lifecycle events, checkpoints with rewind, and a permission system with five modes.

It also has the widest reach of any agent in the category: terminal, VS Code and Cursor extensions, a JetBrains plugin, desktop apps, web, Remote Control for steering a local session from your phone, Channels for bridging Telegram/Discord/iMessage into a session, and Routines for cloud-hosted recurring tasks that keep running when your laptop is shut.

Muse Code has a terminal.

Related: [Claude Code vs Codex](https://composio.dev/content/claude-code-vs-openai-codex) · [Pi Agent vs Claude Code](https://composio.dev/content/pi-agent-vs-claude-code)

## 1. Philosophy: Muse Code vs Claude Code

Claude Code's bet is vertical integration. Anthropic trains the model and the harness together, so scaffolding today becomes model behaviour tomorrow which is why Boris Cherny's team keeps deleting harness code every time a new model ships.

Meta made the same bet, then added a second one on top: **recoverability over capability**.

Read the launch material closely and Meta never argues it wins. Its own charts say Claude Code wins. What Meta argues is that a long agentic run should survive a crash, that every action should be inspectable after the fact, and that the whole thing should cost approximately nothing.

That is a genuinely different thesis. Claude Code optimises for what the agent can do. Muse Code optimises for what happens when the agent has been doing it for nineteen hours and the process dies.

And then there is the third bet, which is not really about engineering at all. Meta needs training data; the kind you cannot scrape. Scraped GitHub gives you the finished commit. A coding agent in a working developer's terminal gives you the prompt, the plan, the failed attempt, the correction, and the test run that finally passed. That trajectory is what teaches a model to *act* rather than autocomplete, and nobody has it at scale.

>  Meta's Q2 2026 numbers explain the urgency better than any product post. Revenue beat at \$60.8B, earnings missed badly at \$6.18 against \$7.14 consensus, and free cash flow collapsed to \$784M from \$8.55B a year earlier; with capex guidance raised to \$130–145B. The contributor tier is not a pricing decision. It's a procurement strategy.

## 2. Agent Architecture: Claude Code vs Muse Code

Both run the same loop. Read task, call model, execute tools, feed results back, repeat. The differences are in how each one splits work up and what it remembers.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_4.png)

**Fan-out.** Muse Code spawns children at roughly your core count minus two, clamped between 2 and 16; so it scales with your laptop, not with a config value. Critically, **a child cannot spawn its own children**. One level, full stop. Claude Code's docs on August 7, 2026 describe 20 concurrent subagents nesting three layers below the main conversation, tunable via `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`, with `ultracode` sessions exempt from the cap entirely.

Muse's one-level rule is deliberate, not a limitation they forgot to lift. It keeps the event log linear and the failure modes tractable. You give up recursive decomposition to get an audit trail you can actually read.

**Worktree isolation.** When a job is big enough, per Zuckerberg, it fans out to separate sub-agents working in parallel in isolated worktrees. Standard git primitive, but making it the default fan-out mechanism means your working copy never gets touched by a parallel agent. Claude Code has a `--worktree` flag; Muse builds the whole parallelism model on it.

**Background observers — the actually novel bit.** Alongside your session, Muse Code runs four persistent observer agents: memory recall, skill recall, goal tracking, and verification. The first three are on by default. They stay alive for the whole session instead of respawning per task, so context accumulates rather than resetting.

> Each observer makes its own model calls. A Muse Code session bills for more than the conversation you can see. If your token spend looks strange, toggle them in the `runtime_capabilities` block of your settings before you file a bug.

**The event log.** Every model call, tool run, approval, and edit is appended to a local log *before it executes*. It's a write-ahead log applied to agent actions. Meta describes the result as replay-exact and restart-safe, crash mid-task and the agent resumes precisely where it stopped, no re-prompting. `muse replay` walks a session step by step; `muse export --last --out session.json` dumps the whole thing offline, with `--redacted` if you're sharing it.

For compliance-heavy teams this is the single most interesting thing in the release. A replayable trace of every decision is rare in this category.

**The CLI itself** is tidy and boringly familiar if you've used Claude Code:

```bash
muse init                    # writes AGENTS.md
muse                         # interactive TUI
muse exec --json "fix tests" # headless, JSONL events, CI-ready
muse resume --last
muse skills import --from claude
```

That last command is worth pausing on. Muse Code reads `AGENTS.md`, falls back to `CLAUDE.md`, and scans repo-local `.claude/skills` and `.codex/skills`. Meta built a one-command migration path off its two biggest competitors and pointed it directly at Anthropic's private format.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_5.png)

## 3. Pricing: Claude Code vs Muse Code

This is the section that actually decides the comparison, so let's be precise. These are different shapes of pricing.

| **Model** | **Input (\$/1M)** | **Output (\$/1M)** | **Total (\$/1M)** | **Source** |
| --- | --- | --- | --- | --- |
| **Muse Spark 1.2 Contributor** | **\$0.10** | **\$0.20** | **\$0.30** | [**Meta**](https://dev.meta.ai/docs/pricing-rate-limits) |
| **Muse Spark 1.1 / 1.2** | **\$1.25** | **\$4.25** | **\$5.50** | [**Meta**](https://dev.meta.ai/docs/pricing-rate-limits) |
| Claude Opus 5 | \$5.00 | \$25.00 | \$30.00 | [Anthropic](https://platform.claude.com/docs/en/about-claude/pricing) |
| Claude Fable 5 / Claude Mythos 5 | \$10.00 | \$50.00 | \$60.00 | [Anthropic](https://platform.claude.com/docs/en/about-claude/models/overview) |

### Claude Code

- **Pro**: \$20/month, or \$17/month billed annually (\$200 up front). Bundles Claude Code, Cowork, Design, and Science.
- **Max**: from \$100/month for 5× Pro usage; a 20× tier exists at \$200.
- **API** (if you're not on a plan): Sonnet 5 at \$2/\$10 per Mtok on the introductory rate; **reverting to \$3/\$15 on August 31, 2026**. Opus 5 at \$5/\$25. Fable 5 at \$10/\$50.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_6.png)

### Muse Code

There is no subscription. At all. It is metered API usage, two rates:

| Tier | Input | Cached input | Output | Rate limit |
| --- | --- | --- | --- | --- |
| Standard | \$1.25 | \$0.15 | \$4.25 | 3,000 rpm / 4M tpm |
| Contributor | \$0.10 | \$0.002 | \$0.20 | 60 rpm / 2.1M tpm |

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_7.png)

The contributor tier is 12.5× cheaper on input and 21× cheaper on output. The difference is that on contributor, Meta may use your prompts and completions to train future models.

Three things almost nobody is reporting correctly:

**1. The standard tier is the underrated story.** At \$1.25/\$4.25 with no data rights and no throughput penalty, it undercuts Opus 5 by roughly 4× on input and 6× on output, and it's a flat rate with *no long-context premium*, so cost scales linearly with tokens regardless of how full the window is. For long agentic runs that's unusually easy to forecast. A 5.9× discount is a boring headline next to a 21× one, so it went unreported.

**2. The contributor tier's real second price is throughput.** 60 requests per minute against 3,000. That's a 50-fold cut, applied per team rather than per API key, and it lands directly on the parallel-subagent architecture Meta is selling.

**3. It is not free, and it is one config string.** VentureBeat's install test hit a 402 paywall: even on contributor you need a payment method on file before the agent will do anything, and the \$20 starter credit from the Muse Spark 1.1 launch is gone. Meanwhile the tier is selected by a **model id**, not a signed agreement; meaning any developer on your team can flip it in a config file with no SSO gate, no admin console, and no policy layer described anywhere in the launch material.

> For agency and client work this is not a pricing decision, it's a contracts decision. Most NDAs written before 2024 restrict disclosure to "third parties" and say nothing about training corpora. Silence in an old NDA is not permission. And an agentic session isn't a chat message; Muse Code is built to read across large repos, so the blast radius of "we train on your prompts" scales with how much of the codebase the agent ingests.

Also budget for the add-on: web search grounding bills \$2.50 per 1,000 queries on top of tokens, and those four background observers each make their own model calls.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_8.png)

## 4. Models: one lab, one model, either way

This one's a genuine draw, and it's the least discussed similarity between them.

Claude Code runs Claude. Opus, Sonnet, Haiku, switchable with `/model`, with effort levels from `low` through `xhigh` plus the session-only `max` and `ultracode` settings. It's tuned end to end because Anthropic owns the whole stack.

Muse Code runs Muse Spark 1.2. That's it. `--model` exists as an override and `--provider` accepts `meta` or `echo`, but there is no fallback model if Spark 1.2 struggles on your stack, no mid-session switching, no local option.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_9.png)

The difference is what happens next: Meta open-weighted **Muse Glimmer**, a 30B agentic model distilled from Muse Spark, under Apache 2.0 on August 10, and both Zuckerberg and Wang have said open weights for Muse Spark 1.2 itself are coming "soon." Anthropic has said nothing of the kind and won't.

## 5. Extensibility

Muse Code arrived unusually complete for a week-one product. MCP over both stdio and streamable HTTP, hooks, skills, sandboxing, and worktree parallelism are all present on day one. That is not typical.

The bundled skills are `/plan` (turns a task into an approval-gated, decision-complete plan), `/grilling` (stress-tests a plan until it holds), `/grill-with-docs` (same, but records decisions into project docs), and `/taste` (a design-quality gate for frontend work).

> Small detail worth noting because it tells you how fresh this is: Meta's developer docs and Meta's launch post **disagree on the skill roster**. The docs list four and classify `/goal` as a background observer; the launch post counts `/goal` as a skill and names three. Six days old.

What Muse Code lacks is time. No Windows build. No IDE extension. No subscription tier. No third-party plugin ecosystem, no marketplace, no community catalogue.

Claude Code's extensibility model is external and config-driven, and it has had a year to compound: hooks fire on 14 lifecycle events and talk JSON over stdin, MCP servers run as separate processes, Skills are markdown instruction packs with lazy loading, and plugins bundle all of it into installable packages with official and community marketplaces behind `/plugin`.

Related: [Best Claude Code Plugins](https://composio.dev/content/top-claude-code-plugins) · [Top 10 CLI Tools to Level-Up Claude Code](https://composio.dev/content/top-10-cli-tools-for-claude-code)

## 6. Permissions and safety

Both of these ship sane defaults, which makes this section shorter than it would have been a year ago.

**Muse Code** defaults to `on-request` approval with an OS-enforced sandbox; Seatbelt on macOS, a bubblewrap helper on Linux. Network policy defaults to **proxy-only**, meaning each new outbound host, port, or protocol stops for review; `restricted` (no network) and `enabled` (full egress) are the alternatives. Compound shell commands are reviewed stage by stage, so the first unapprovable stage blocks the whole line. A stricter `untrusted` mode escalates any unmatched shell stage.

That proxy-only network default is arguably tighter out of the box than what most agents ship with.

**Claude Code** is deny-by-default with five permission modes, allow/deny rules, and OS-level sandboxing for Bash. It has had real CVEs; and they got found, patched, and written up, which is what happens when millions of people use something.

Two Muse gaps, both documented rather than hidden:

- **MCP tools are not sandboxed.** Approval still applies, but OS-level containment does not. Meta's docs advise connecting only servers you trust.
- **`-yolo`** drops approval *and* the sandbox and trusts the workspace. Meta explicitly warns against it on a fork or PR checkout, because `AGENTS.md`, rules, and skills all become attacker-controlled content in that situation.

Both are the right disclosures to make. They also mean you have no excuse for tripping over them.

## **The first week of reactions**

Honestly? Less than you'd expect. Two days after launch, total discussion across Hacker News and Reddit amounted to a few hundred comments, and only a handful of people had posted a first-hand result from actually running the CLI.

One distinction most coverage misses: **most of the early enthusiasm is about the model, not the agent.** People running Muse Spark 1.2 through OpenCode saying it's fast is a different claim from people saying Muse Code is good. Keep them separate.

The most substantial hands-on review so far is [Theo's 44-minute stress test](https://www.youtube.com/watch?v=-Gj0-EIyx6g), and it's worth watching in full because it swings hard in both directions.

🎥 [Watch the demo — Watch on YouTube](https://youtu.be/-Gj0-EIyx6g?si=cGk2j9zthQjp3O14)

**The good.** He pointed it at his own repo and asked it to audit every open pull request; classify each as mergeable, needs work, close, or superseded, rank by priority, attach confidence scores, fan out sub-agents. It indexed and reviewed all **222 PRs in under five minutes**. Cost on the contributor tier: **ten cents**. Roughly \$2 on standard. The output had clickable links per PR, confidence scores, and clean-versus-dirty merge indicators; cleaner, in his judgment, than what his usual tools produce. He plans to run it as a daily job.

Also good: about 191 tokens/second, the first terminal voice-to-text he rates as actually good, seven background sub-agents running concurrently without throttling, and a weirdly distinctive visual taste in generated frontends; squircles, tilted panels, shadows that doesn't feel distilled from the usual sources.

**The bad.** Asked to add Muse as a provider inside his own harness, the model mapped the task onto an entirely unrelated product and confidently ran the whole investigation on that assumption. When GPT-5.6 scored both plans blind, Muse's came out at 4.8/10 against Fable's 7/10. Generated code that didn't run. Rate limits fired constantly *on the paid per-token tier*, which is the complaint that stings most given the architecture is built on parallelism.

His summary line is the one to quote: the model knows how to act like a modern smart model without knowing what the modern smart models know.

And his bottom line, which I think is right: he'd rather wait two or three times longer for something he can merge than roll a fast model five times and clean up the mess. **Triage tool, not a merge tool.**

**On the contributor tier**, X split cleanly into two camps. One side reads it as the most honest deal in the industry; explicit opt-in, stated terms, real discount, better than the vendors who train on you quietly. The other side is just not giving Zuckerberg their source code at any price. Both reactions are entirely reasonable and neither is going to convince the other.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-claude-code/image_10.png)

**The loudest practical complaint isn't philosophical, it's access.** Roughly fifteen developers across Reddit and HN; Canada, UK, Australia, Thailand, report that the contributor tier just returns `Model not found` for them, and US VPNs generally don't get around it. Meta's only on-record statement mentions "expanded global access." No official page documents a regional restriction. Treat it as widely user-reported rather than confirmed, but budget for it before you plan around contributor pricing.

**On open source**, asked directly on X whether Muse Code would be open sourced, Zuckerberg's full answer was I'll have more to share on that soon. Theo's version of the same complaint was blunter: Meta is an open-source company at heart, ship the source. React and Llama are the precedent everyone is pointing at.

## Which one should you pick?

**Pick Claude Code if:**

- You want the thing that works at full power today, with a year of ecosystem behind it
- You need surfaces beyond the terminal; IDE, desktop, web, phone, or cloud-hosted recurring tasks
- You're deploying to a team and need permissions, SSO, spend caps, and audit trails that exist
- You need wide parallel decomposition, 20 concurrent subagents nesting three deep, not 16 one level deep
- You're on Windows
- Your usage fits inside a \$20 Pro plan, in which case the flat rate is a genuine discount and not a marketing artifact

**Pick Muse Code if:**

- The work is shallow-and-broad rather than deep: PR triage, log digging, categorisation, report generation, "read 200 things and tell me which ones matter"
- You're running long, unattended jobs where a crash at hour five currently costs you the whole run
- You need the audit trail; the event log is a compliance artifact by construction, not an afterthought
- Your code is open source, a hobby project, or otherwise something you genuinely don't mind Meta training on
- You want the standard tier's flat, no-long-context-premium pricing for forecastable multi-hour runs, this one applies even if you'd never touch contributor

**Do what the evidence actually supports:** run Claude Code as the daily driver and keep Muse Code around as the cheap bulk-processing pass. The ten-cent PR audit is not a gimmick, it's a genuinely new price point for a class of work most people currently don't do at all because it costs too much. Just don't merge what it writes without reading it, and decide the tier at the policy layer rather than letting each developer pick a model id.

## Conclusion

Meta shipped a launch chart where it loses on every row, and then priced the product like the chart doesn't matter. That is either very confident or very cynical, and I've gone back and forth on which.

Here's where I've landed. Muse Code is not competing with Claude Code for your afternoon. It's competing for the work you're not doing right now; the 222-PR audits, the log sweeps, the "categorise all of this" jobs that are obviously useful and obviously not worth \$20 of Opus tokens. At ten cents, that math changes completely, and a whole category of tasks moves from "not worth it" to "run it nightly."

Whether Meta ever closes the capability gap is a separate question, and the contributor tier is the bet that it can, every session on that tier is a labelled trajectory feeding the next model. The 11-point jump on the Artificial Analysis index in four months suggests the flywheel is at least spinning.

But for now: Claude Code is what you open when you have a bug and you want it fixed. Muse Code is what you point at 222 pull requests on a Tuesday night. Both of those are real jobs. Only one of them was worth doing last week.
