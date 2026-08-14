# Muse Code vs Codex in 2026: The Open-Source Roles Have Completely Swapped

On August 5, 2026, Meta shipped Muse Code. Closed source. No license. And as VentureBeat pointed out, neither the launch post nor Zuckerberg's thread so much as uses the word "open."

Meanwhile Codex CLI is Apache 2.0, written in Rust, sitting on roughly 85,000 GitHub stars with 365 outside contributors. OpenAI runs a \$1 million fund that hands open source maintainers up to \$25,000 in credits plus six months of Pro, for free, no strings.

And Meta's offer to that same group of maintainers? A discount, if you let them train on your code.

The roles didn't just shift. They inverted completely. 🙃

That's the frame for this comparison, and it turns out to matter more than any benchmark. Let's go!

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_1.png)

## Muse Code vs Codex Summary

| Category | Codex CLI | Muse Code | Winner |
| --- | --- | --- | --- |
| License | Apache 2.0, Rust, \~85K stars, 365 contributors | Proprietary, no license published | Codex |
| Cheapest tokens available | Luna at \$0.20/\$1.20 per Mtok | Contributor at \$0.10/\$0.20 per Mtok | Muse Code |
| Cheapest *without* a data trade | Luna, same \$0.20/\$1.20, no training rights | Standard at \$1.25/\$4.25 | Codex |
| Mid-tier rate | Terra at \$2/\$12 per Mtok | Standard at \$1.25/\$4.25 per Mtok | Muse Code |
| Free tier | Yes, Codex works on a \$0 ChatGPT account | None; payment method required even on contributor | Codex |
| Platform support | macOS, Linux, Windows | macOS and Linux; Windows via WSL only | Codex |
| Surfaces | CLI, web, IDE, macOS desktop, Slack, GitHub, Linear | Terminal. That's it. | Codex |
| Parallel fan-out | `max_threads` with configurable depth control | 2–16 children, one level, no nesting | Codex |
| Background agents | Spawned per task | 4 persistent observers that live for the session | Muse Code |
| Crash recovery | Session resume | Append-only event log, replay-exact | Muse Code |
| Config standard | `AGENTS.md` | `AGENTS.md` (falls back to `CLAUDE.md`) | Draw |
| Sandboxing | Seatbelt, Landlock/Bubblewrap, Windows restricted-token, Starlark exec policy | Seatbelt, bubblewrap, proxy-only network default | Codex |
| Ecosystem | 9,000+ MCP plugins, skills catalog, 150+ community tools | MCP and skills shipped day one, no ecosystem yet | Codex |

#### Summary

**Scorecard: Codex 10, Muse Code 4, one draw.**

This is a more lopsided result than [Muse Code vs Claude Code](https://composio.dev/content/muse-code-vs-claude-code), and for a different reason. Against Claude Code, Muse loses on capability but wins hard on price. Against Codex, **the price argument itself gets much weaker,** because on July 30, six days before Muse Code launched, OpenAI cut Luna by 80% to \$0.20/\$1.20.

That single move reframes the entire pitch. Meta's headline is a 12–21× discount, but that's measured against premium models. Measured against Luna, the contributor tier is roughly 2× cheaper on input and 6× on output and it costs you your codebase, while Luna costs you nothing.

Muse Code keeps two things Codex genuinely doesn't have. I'll get to those.

## What is Muse Code?

> Meta's first coding agent; terminal only, built around an event log, and priced to buy training data.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_2.png)

[Muse Code](https://developer.meta.com/ai/resources/blog/build-with-muse-code/) launched in beta on **August 5, 2026** out of Meta Superintelligence Labs, under Alexandr Wang. Install is one line, macOS and Linux only:

```bash
curl -fsSL https://dev.meta.ai/install.sh | bash
```

No npm, no Node, no Python, a \~97 MB binary. It runs on **Muse Spark 1.2**, a coding model with a 1M-token context window that Meta co-trained with the harness itself, on rejection-sampled harness trajectories.

It reads `AGENTS.md`, falls back to `CLAUDE.md`, scans `.claude/skills` and `.codex/skills`, and ships `muse skills import --from claude|codex`. Meta built migration paths off both competitors and pointed them straight at the incumbents.

## What is OpenAI Codex?

> OpenAI's coding agent, and the only major lab agent that's open source, OS-sandboxed, and multi-platform at once.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_3.png)

[Codex CLI](https://developers.openai.com/codex/cli) is Apache 2.0, written in Rust, installable through `npm i -g @openai/codex` or Homebrew. The current stable line is v0.147.0 as of August 7, 2026, with 550+ releases behind it.

But the CLI is only one face. Codex also runs as a managed cloud service: dispatch a task, it executes in an isolated cloud sandbox and hands back a pull request. You can trigger that from ChatGPT, Slack, the macOS desktop app, GitHub Code Review, or Linear.

Configuration runs on `AGENTS.md`  which is an **open standard under the Linux Foundation's Agentic AI Foundation**, read by Codex, Cursor, Copilot, Amp, Devin Desktop, and Gemini CLI. Codex also adopted the Agent Skills spec from Claude Code back in December 2025.

Recent releases have been substantial. v0.147.0 added `--approve-for-me` (approval prompts adjudicated by an automatic review pass), portable Agent Plugins searchable across local, personal, workspace and remote catalogs, and the opt-in MCP 2026-07-28 protocol with paginated discovery and non-blocking server startup. It also **removed** `codex exec --full-auto`, deprecated since v0.128, scripts still passing it now error out.

Related: [Claude Code vs Codex](https://composio.dev/content/claude-code-vs-openai-codex)

## 1. Philosophy: Two different approaches

Both of these companies want to own your terminal. They have opposite theories about how to get there.

**OpenAI is buying trust with source code and generosity.** Codex CLI is Apache-licensed, so you can read every line of the harness, fork it, and audit what it sends. `AGENTS.md` is a Linux Foundation standard, which means OpenAI deliberately chose a config format its competitors can read, your instructions survive a switch to Cursor or Copilot. And Codex for Open Source, launched March 7, 2026, gives qualifying maintainers up to \$25,000 in API credits plus six months of ChatGPT Pro, backed by a \$1 million fund, with no purchase commitment.

**Meta is buying data with a discount.** Muse Code is closed. There is no license page. The pitch to individual developers and open source maintainers is that access is nearly free if you let Meta train on your prompts and completions.

Both are self-interested. OpenAI's openness is strategic, own the standard, own the default. But the two offers land very differently on the same developer. One says *here's \$25,000, keep maintaining the thing everyone depends on*. The other says *your code is the payment*.

> The reversal has a date. Meta's Llama era effectively ended April 8, 2026, when MSL shipped the original Muse Spark as Meta's first proprietary model, cloud-only, no downloadable weights, no self-hosting. Wang said at the time that bigger models were coming with plans to open-source future versions. Four months later Muse Code shipped with no weights, no license, and no mention of the word.

There is one genuine counterweight: Meta open-weighted **Muse Glimmer**, a 30B agentic model distilled from Muse Spark, under Apache 2.0 on August 10 five days after Muse Code. And Muse Spark 1.2 is listed on OpenRouter, so you can test the model against your own repo without touching the harness or the contributor tier. Take that offer. It's the most useful thing this launch gives you.

## 2. Agent architecture: How Codex and Muse Code work

Both agents use the same basic process. They read your task, call a model, run tools, and use the results to continue. But they manage parallel work and session memory in different ways.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_4.png)

**Parallel agents.** Codex uses `max_threads`, and you can control the number of agents and their nesting depth. Muse Code starts 2–16 child agents based on your available CPU cores. Each child works at one level and cannot start more child agents.

Codex can also review changes across several repositories in one pass. You can use one session for work that spans multiple repositories or git remotes.

Muse Code keeps its agent structure to one level, so its event log stays clear and easy to review. This makes the process easier to inspect, but it limits complex task decomposition.

**Worktree isolation.** Muse Code gives each parallel agent an isolated git worktree by default, so the agents do not change your main working copy. Codex cloud runs each task in an isolated cloud sandbox and returns a pull request. Both systems protect your local work, but they use different methods.

**Background observers.** Muse Code runs four observer agents during your session. They handle memory recall, skill recall, goal tracking, and verification. It enables the first three by default. They stay active for the full session, so they can keep useful context as you work.

> Each observer makes separate model calls, so your total usage can be higher than the conversation shows. You can change these settings in the `runtime_capabilities` block.

**Event log.** Muse Code records each model call, tool run, approval, and edit before it runs the action. If the session stops, it can continue from the same point. You can use `muse replay` to review a session. You can also use `muse export --last --out session.json` to export it, or add `--redacted` before you share it.

This event log can help regulated teams because it provides a detailed record of the agent's actions. Codex can resume a session, while Muse Code also provides replay and export features.

**Ways to use each agent.** You can use Codex in the terminal, a cloud sandbox, an IDE, the desktop app, Slack, or GitHub review. These options use the same agent and the same `AGENTS.md` file. Muse Code currently runs in the terminal, so it works best for teams that already manage coding tasks there.

## 3. Pricing: Muse Code vs Codex

This is where the Muse pitch gets much harder to make than it does against Claude Code, and the reason is timing.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_5.png)

### Codex

Two completely separate billing channels:

**ChatGPT sign-in,** included on every plan, *including Free at \$0/mo* and Go at \$8/mo. Free and Go users get Terra; Plus, Pro, Business and Enterprise can choose Terra or Luna. Plus is \$20/mo, Pro is \$100 (5×) or \$200 (20×), Business is \$20/user/mo annual or \$25 monthly. Usage runs against a 5-hour rolling window plus weekly caps, shared across CLI, web and IDE, so a heavy CLI session competes with your web session for the same allowance.

**API key:** per-token, no window limits, no cloud features:

| Model | Input | Output |
| --- | --- | --- |
| GPT-5.6 Sol | \$5.00 | \$30.00 |
| GPT-5.6 Terra | \$2.00 | \$12.00 |
| GPT-5.6 Luna | \$0.20 | \$1.20 |

Cache reads keep a 90% discount; cache writes bill at 1.25× uncached input. Requests over 272K input tokens bill at 2× input and 1.5× output **for the entire request** worth knowing before you point it at a monorepo.

### Muse Code

No subscription exists. Metered only, two rates:

| Tier | Input | Cached | Output | Rate limit |
| --- | --- | --- | --- | --- |
| Standard | \$1.25 | \$0.15 | \$4.25 | 3,000 rpm / 4M tpm |
| Contributor | \$0.10 | \$0.002 | \$0.20 | 60 rpm / 2.1M tpm |

Meta also offers a zero‑data‑retention option for enterprise customers. You can request this configuration so your data is not used for model improvement. This option is not available for standard or contributor tier users.

### The comparison nobody is making

**Muse standard genuinely undercuts Terra.** \$1.25/\$4.25 against \$2/\$12 is 1.6× cheaper on input and 2.8× cheaper on output, with no data trade and no long-context premium; Muse bills flat regardless of how full the window is, while Codex doubles input past 272K. For long agentic runs on large repos, that's a real and underdiscussed advantage. This is the strongest honest pricing argument Meta has, and it's the one nobody made because it isn't the 20× headline.

**But the contributor tier's edge collapses against Luna.** On July 30, 2026, six days before Muse Code shipped; OpenAI cut Luna by 80% to \$0.20/\$1.20. Against that, contributor is 2× cheaper on input and 6× on output, and the difference is that Meta may train on everything you send.

> Run the actual numbers before you decide. On a workload of 3M input and 1M output tokens: Luna costs \$1.80 and Meta learns nothing about your codebase. Contributor costs \$0.50 and Meta gets the whole session. You are paying \$1.30 for your code's privacy. That may well be worth it for a hobby project. For client work it's not a pricing decision at all.

Two more asymmetries:

**Codex has a genuinely free tier.** A \$0 ChatGPT account gets you Codex with Terra. Muse Code has none, VentureBeat's install test hit a 402 paywall, meaning even the contributor tier requires a payment method on file, and the \$20 starter credit from the Muse Spark 1.1 launch is gone.

**Contributor throughput is the second price.** 60 requests per minute against 3,000 on standard, a 50-fold cut, applied per team, landing directly on the parallel-subagent architecture Meta is selling.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_6.png)

## 4. Models

Codex gives you three tiers with explicit routing: Luna for volume, Terra for everyday work, Sol for the hard problems, switchable per task, plus a `/fast` mode that trades 2× credits for 1.5× speed. All three share a 1.05M context window and 128K max output.

Codex also supports an OSS mode. You add `--oss` to the CLI command to run open‑source models locally through Ollama, LM Studio, or other local providers. This means you can use Codex with zero token cost if you run a local model. Muse Code has no equivalent option.

Muse Code gives you Muse Spark 1.2. That's it. A `--model` override exists and `--provider` accepts `meta` or `echo`, but there is no fallback if Spark 1.2 struggles on your stack, no mid-session escalation, no local option.

> On Meta's own launch chart, Muse Spark 1.2 posts 82.9% on Terminal-Bench 2.1 against Codex at 81.8%, and loses on DeepSWE 1.1 at 59.3% against 64.8%. But the Codex figure in both rows is **GPT-5.6 Terra, the mid-tier model;** Sol, OpenAI's flagship, does not appear on the chart at all. Developers flagged the substitution within an hour of the post going up. Treat a 1.1-point win over a competitor's second-best model accordingly.

## 5. Extensibility and ecosystem

Muse Code arrived unusually complete for week one, MCP over stdio and streamable HTTP, hooks, skills, sandboxing, worktree parallelism, all present at launch. Bundled skills are `/plan`, `/grilling`, `/grill-with-docs` and `/taste`.

> Detail that tells you how fresh this is: Meta's developer docs and Meta's launch post **disagree on the skill roster**. The docs list four and classify `/goal` as a background observer; the launch post counts `/goal` as a skill and names three. Six days old.

What Muse lacks is everything that accumulates. Codex ships 29 built-in slash commands, MCP as both client and server with `codex mcp add` and 9,000+ plugins, a skills catalog split into `.system`, `.curated` and `.experimental`, hooks in beta, Starlark-based exec policy for fine-grained permission rules, profiles you switch by name with `--profile`, and a community catalogue running past 150 tools, skills and subagents.

And because the CLI is Apache 2.0, the extension story has a floor Muse can't match: if something isn't supported, you can add it yourself and ship it.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_7.png)

## 6. Permissions and safety

Both ship sane defaults. This is closer than the rest of the comparison.

**Muse Code** defaults to `on-request` approval with OS-enforced sandboxing, Seatbelt on macOS, bubblewrap on Linux. Network defaults to **proxy-only**, so each new outbound host, port or protocol stops for review. Compound shell commands are reviewed stage by stage. A stricter `untrusted` mode escalates any unmatched stage.

That proxy-only network default is genuinely tight, and arguably tighter out of the box than what Codex ships.

**Codex** sandboxes on all three platforms; Seatbelt on macOS, Landlock/Bubblewrap on Linux, restricted-token on Windows and layers a Starlark exec policy on top for fine-grained rules. Profiles let you set `approval_policy = "untrusted"` and sandbox aggressively, then switch by name so you always know which configuration is active. v0.147.0 added `--approve-for-me`, which adjudicates approval prompts through an automatic review pass.

Two Muse gaps, both documented rather than hidden:

- **MCP tools are not sandboxed.** Approval applies; OS containment doesn't. Meta advises connecting only servers you trust.
- **`-yolo`** drops approval *and* sandbox and trusts the workspace. Meta explicitly warns against it on a fork or PR checkout, since `AGENTS.md`, rules and skills all become attacker-controlled content there.

Codex's equivalent lesson is instructive: `codex exec --full-auto` was deprecated in v0.128 and **removed** in v0.147.0. That's what a two-year-old agent's safety surface looks like after real incidents, the dangerous convenience flag gets taken away.

## What the open source argument actually looks like

The Muse Code launch itself generated less discussion than you'd expect a few hundred comments across Hacker News and Reddit in the first two days, with only a handful of first-hand results. (For the hands-on tests that do exist, including a competitor's system-prompt experiment and a 222-PR triage run, see our [Muse Code vs Claude Code](https://composio.dev/content/muse-code-vs-claude-code) breakdown.)

What got a real reaction was the open source question, and the week that followed is genuinely strange.

**Day one: developers caught the chart.** Meta benchmarked against GPT-5.6 Terra rather than Sol, and per Towards AI it was a substitution developers flagged within the hour. Sol never appears on the chart, and the chart never explains why.

That would be a minor gripe from most vendors. From Meta it's a pattern. With Llama 4 in April 2025, Meta submitted a specialised, unreleased variant tuned for human-preference battles to LM Arena, then had to admit the general release didn't perform the same way. LM Arena publicly said Meta's reading of its policy wasn't what they expected from a model provider. Nothing about the Terra substitution is comparable in severity, it's a chart choice, not a submission but it's the second time in sixteen months that the first question about a Meta benchmark has been which version is this actually.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_8.png)

**Day five: Meta shipped an open model anyway.** Muse Glimmer landed August 10 under Apache 2.0, weights on Hugging Face, 30B, runs on a single 24GB card. VentureBeat's headline read "Meta returns to open source." Zuckerberg published a long essay the same day, The Future Is for Everyone, saying Meta will resume releasing some open models.

And the engagement gap is the most telling number of the whole fortnight. **The Hacker News thread on Glimmer hit 555 points and 293 comments within hours** comfortably more than Muse Code's closed launch managed in two days. Meta's open release outdrew its closed one on the forum whose approval it most needs.

That distinction is exactly the axis this comparison sits on, and it cuts cleanly. **Codex CLI is open source in the strict sense** Apache 2.0 *source code*, 365 outside contributors, every line of the harness auditable. Muse Glimmer is open weights. Muse Code, the actual competitor to Codex, is neither. So even on Meta's best week for openness, the thing you'd be comparing against Codex is still a closed binary from a company asking to train on your repository.

**On Codex itself, the community verdict is settled and mostly favourable.** The recurring shape on HN is that Codex is the hands-off one dispatch and walk away, with its PR reviewer singled out repeatedly as best in class. The fair counter-criticism is that its reputation is inflated by who uses it: as one commenter put it, a greenfield, solo project is the ideal (only?) use case for running five agents in parallel, which describes the build-in-public crowd rather than enterprise work. Community size supports that scepticism, r/Codex runs roughly 1,200 weekly contributors against 4,200 on r/ClaudeCode. Codex is not the default. It's the strong second a lot of people run alongside.

Worth adding one independent data point, since almost everything else in this comparison is a vendor chart. Roboflow's Vision Evals scores 26 models against ground truth across six visual tasks, and there, Muse Spark 1.2 lands 5th at 80.4%, ahead of all three GPT-5.6 tiers: Sol at 76.9%, Terra at 72.4%, Luna at 71.5%. It does it at roughly a third of Sol's cost per sample.

This is vision, not coding, so it settles nothing about which agent writes better code. But it isn't irrelevant either, both agents accept image input, and a coding agent that reads a screenshot, a mockup, or an error dialog is doing exactly this task. It's also the clearest case so far of Meta beating OpenAI on a benchmark Meta didn't run.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/muse-code-vs-codex/image_9.png)

**One practical complaint worth budgeting for.** Roughly fifteen developers across Reddit and HN; Canada, UK, Australia, Thailand report the contributor tier returning `Model not found`, with US VPNs not helping. Meta's only on-record comment mentions expanded global access. No official page documents a restriction. User-reported, not confirmed.

## Which one should you pick?

**Pick Codex if:**

- You want to be able to read, fork, or audit the agent you're running
- You're on Windows, or your team is mixed
- You need surfaces beyond the terminal; cloud tasks, IDE, Slack, GitHub review, desktop
- You want model routing: Luna for volume, Terra for daily, Sol for the hard ones
- You want cheap tokens without a data trade, Luna at \$0.20/\$1.20 is the actual answer to the contributor tier
- You maintain an open source project, in which case apply to Codex for OSS and stop paying entirely
- You want an ecosystem that already exists rather than one that might

**Pick Muse Code if:**

- You need the event log. Replay-exact crash recovery and an exportable decision trace are real, and Codex doesn't have them
- You're running long, unattended jobs where a crash at hour five currently costs the whole run
- Your workload is shallow-and-broad; PR triage, log sweeps, categorisation, "read 200 things and tell me which matter"
- You want flat pricing with no long-context premium for multi-hour runs on big repos, where Muse standard genuinely beats Terra
- Your code is open source, a hobby project, or something you truly don't mind Meta training on

**Do the thing that costs you nothing:** Muse Spark 1.2 is on OpenRouter. Test the model against your own repo with an API key swap, no harness switch, no contributor tier, no data trade. Meta's launch chart is a vendor chart that quietly benchmarked against OpenAI's second-best model, checking it yourself is now a ten-minute job, and it's the most useful thing this release actually hands developers.

## Conclusion

Muse Code against Claude Code is a story about capability versus cost. Muse Code against Codex is a story about trust, and Meta picked the worst possible opponent for it.

Codex is open source, cross-platform, multi-surface, free on a \$0 account, and its parent company pays open source maintainers rather than asking them to pay in code. Then OpenAI cut Luna by 80% six days before Muse launched, which knocked most of the air out of the one argument Meta had. The contributor tier still wins on raw price — 2× and 6× — but It is harder to sell a small discount than a big discount. A 20× discount is easy to explain. A 2× discount with a data trade is not.

What Muse Code keeps is narrow and genuine. The event log is the best idea in the release, and nobody else has shipped it. Persistent observers are a real architectural bet. If you run long unattended jobs and need to prove afterwards what the agent did, that's worth something Codex can't currently offer at any price.

But it's one feature against an ecosystem, a license, three platforms, six surfaces, and a \$1 million fund pointed at exactly the developers Meta is trying to recruit. Meta is asking the open source community to fund its next model with their source code. OpenAI is writing them cheques.

The company named "Open" is winning the openness argument. Sit with that one for a second. ✌️
