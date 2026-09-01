# DeepSeek Harness vs Claude Code in 2026: Open Runtime vs the Deepest Closed Harness

Claude Code is the most programmable coding agent anyone ships. Roughly 30 lifecycle hooks, skills, subagents, plugins, MCP, dynamic workflows that fan out across hundreds of parallel agents. Anthropic built a harness you can bend into almost any shape.

You just can't inspect or replace its core implementation.

On August 13, 2026, DeepSeek shipped the opposite trade. `deepseek-harness`, CLI name `dsh`, is MIT-licensed and structured so that the model adapter, the tool registry, the sandbox, the session log, the scheduler and the UI are all replaceable plugins. Including the agent loop. Its tagline is "Everything is a Plugin," and unusually for a tagline, it survives contact with the source.

Then it crossed 147k GitHub stars in four days and sits around 165k as of this writing, which is roughly the rate at which the internet decides something is a Claude Code killer regardless of what it actually is.

Most people compare coding agents by asking which model is underneath. That turns out to be only half the story. The harness decides what the model sees, which tools it gets, how failures are retried, what gets cached and what survives into the next turn. DeepSeek basically looked at that entire layer and said: why should any of this be fixed?

Let's go :)

## DeepSeek Harness vs Claude Code Summary

| Category | DeepSeek Harness (`dsh`) | Claude Code | Winner |
| --- | --- | --- | --- |
| **License** | Free, MIT licensed, full source | Proprietary, closed source | **DeepSeek Harness** |
| **Maturity** | Developer preview; breaking changes explicitly expected | Mature commercial product, \~\$2.5B annualised revenue | **Claude Code** |
| **Real tool use (our eval)** | 20/30 tasks passed | 19/30 tasks passed | **DeepSeek Harness** |
| **Context efficiency** | 88,562 avg tokens per task | 649,900 avg tokens per task | **DeepSeek Harness** |
| **Cost efficiency** | \$0.028 per shared success | \$0.074 per shared success, the highest of six | **DeepSeek Harness** |
| **Speed** | 283s per finished task | 263s per finished task, fastest of six | **Claude Code** |
| **Model support** | Any OpenAI-compatible endpoint; V4 Flash/Pro first-class | Anthropic models; third-party routing exists but is off the main path | **DeepSeek Harness** |
| **Extension surface** | Roughly 80 catalog plugins, almost all DeepSeek's own, days old | \~30 lifecycle hooks, skills, subagents, plugins, MCP, dynamic workflows, and a large community library | **Claude Code** |
| **Depth of access** | The loop, sandbox and session store are all replaceable | Rich hooks around a pipeline you cannot read or replace | **DeepSeek Harness** |
| **Observability** | Append-only event stream; everything model-visible is reconstructible | Managed transcripts and project memory | **DeepSeek Harness** |
| **Permissions & sandboxing** | Fail-closed ladder, `read-only` default; preview-stage | OS-level: Seatbelt on macOS, bubblewrap on Linux/WSL2; no native Windows yet | **Claude Code** |
| **Surfaces** | Local web UI, headless CLI, ACP/JSON-RPC, Python SDK | CLI, IDE, desktop, web, mobile, Chrome | **Claude Code** |
| **Ecosystem today** | Days old, roughly 80 documented plugins | Thousands of community skills, agents and plugins | **Claude Code** |

#### In Summary:

Don't add those rows up. They are not the same size: a distribution list is not worth what a sandbox is worth, and license decides procurement for some teams and nothing at all for others. What the table shows is a clean split along one line.

Claude Code wins the *product* questions. Maturity, speed, surfaces, ecosystem, and a sandbox with real deployments behind it. DSH wins the *ownership* questions. License, model freedom, inspectability and auditability.

Three rows sit outside that split, and they are the ones worth a second look. We put both harnesses on the same model and the same 30 tasks. The six-day-old developer preview passed one more of them than the mature product, spent a seventh of the context doing it, and came out at 3 cents per shared success against 7. Those are not licensing arguments or philosophy. That is DSH winning on Claude Code's own terms.

Everything else on DSH's side of the table is a head start Anthropic could erase, and everything else on Claude Code's side is a lead DeepSeek could close with time.

## What is DeepSeek Harness?

> An MIT-licensed agent *runtime* from DeepSeek AI where the model adapter, tools, storage, permissions, agent loop and UI are all replaceable plugins.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_1.png)

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) landed on August 13, 2026 under MIT, with npm serving `0.1.0-rc.8` as of this writing, at roughly 453,000 lines of TypeScript across about 219 workspace packages. One command gets you going:

```bash
npx @deepseek-ai/dsh web
```

That serves a local web UI on `127.0.0.1:3080`. Note what it doesn't do: it doesn't drop you in a terminal REPL. The primary entry point is a browser, which already tells you this isn't chasing Claude Code's form factor.

The architecture runs on [Cordis](https://github.com/cordiverse/cordis), a plugin and event-bus meta-framework whose design comes from a paper on *spatiotemporal composability*. DeepSeek source-vendored a fork, pinned at `4.0.0-rc.7` with 18 local patches, renamed into their own scope so the harness fully owns its framework layer.

Configuration lives in `cordis.yml` and composition works in layers: a profile lists bundles, each bundle is an npm package carrying a config patch, and patches apply in order to an empty root. There is no privileged core. The config catalog documents roughly 80 plugins covering models, tools, skills, sessions, sandboxes, storage, agent loops, scheduling and the UI, and the tool catalog lists 53 built-in tools.

Four presets ship out of the box. **Standard** is the full coding agent. **Code** exposes the same tools through a TypeScript SDK so the model can combine multi-step operations in one program. **Minimal** is two tools, persistent bash and `str_replace_editor`, and nothing else. **Creator** adds runtime inspection and in-memory plugin experiments.

Three things in the codebase are worth stealing regardless of whether you adopt `dsh`:

1. **"Model-visible equals logged" as a runtime invariant.** At every dispatch, an assertion checks that the outgoing request matches the projection derived from the append-only session log. Replay, fork, resume and the UI all derive from one event stream, so "what did the model actually see" is never a reconstruction exercise.
2. **Keyless transcript replay.** Recorded session logs are committed as fixtures and a deterministic mock model is derived from them. The same `.jsonl` is both replay input and expected output. No API keys in CI, no flaky LLM-as-judge.
3. **A fail-closed sandbox ladder.** `read-only`, then `workspace-write`, then `danger-full-access`. If a confined mode is requested and no backend is usable, it throws `SANDBOX_UNAVAILABLE` and refuses to run unconfined.

MCP support is client-only. And `packages/hooks/` ships bridges for Claude Code and Codex, which is the part everyone misread as an insult and is actually the whole thesis: DSH will happily run Claude Code as a sub-agent inside a workflow you designed.

## What is Claude Code?

> Anthropic's terminal-first coding agent, and the deepest programmable harness in the category.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_2.png)

[Claude Code](https://github.com/anthropics/claude-code) is a CLI that connects to Anthropic's models and now reaches well beyond the terminal: IDE extensions, a desktop app, web, mobile and Chrome. It is proprietary. You can observe its behaviour in enormous detail and you cannot read its implementation.

What you get instead is an extension surface nobody else matches. Four primitives stack:

- **Skills**: folder-based instruction packs, each a directory with a `SKILL.md` plus optional scripts and assets, loaded on demand when the task matches.
- **Hooks**: roughly 30 lifecycle events (`PreToolUse`, `PostToolUse`, `SessionStart`, `UserPromptSubmit` and so on) that can trigger shell commands, HTTP endpoints, MCP tools, prompt-based checks or agentic verifiers.
- **Subagents**: forked instances defined in `.claude/agents/`, each with its own context window, allowed tools and model.
- **MCP**: the same server ecosystem everyone else consumes.

On top sit plugins, plan mode, and dynamic workflows that orchestrate tens to hundreds of parallel subagents in one session. Memory is hierarchical through `CLAUDE.md` with `@path` imports.

Sandboxing arrived properly in 2026 and is OS-level: Seatbelt on macOS, bubblewrap on Linux and WSL2, with a network proxy for egress control, enabled via `/sandbox`. WSL1 is unsupported because bubblewrap needs user and mount namespaces. Native Windows is planned and not yet shipped, which is the one place Codex is currently ahead of it.

Related: [Claude Code vs OpenAI Codex](https://composio.dev/content/claude-code-vs-openai-codex)

## **1. Philosophy: Extending the boundaries vs replacing the core**

The cleanest way to see the difference is to ask what each project thinks you'll want to change.

Anthropic's answer is: behaviour at the boundaries. Hooks let you intercept what the agent is about to do and what it just did. Skills let you inject expertise. Subagents let you fork context. Every one of those is a way to shape the agent *from outside the loop*, and the surface is genuinely excellent, because Anthropic has spent roughly a year and a half learning which boundaries people actually reach for.

DeepSeek's answer is: potentially any of it. The model adapter is replaceable. The tool layer is replaceable. The session store is replaceable. The sandbox, scheduler, UI and even the agent loop are plugins. Profiles compose bundles and configuration patches through Cordis, so changing a runtime component does not require forking the harness itself.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_3.png)

That distinction matters when the thing you want to change sits below the extension surface. In DSH, a different execution backend, persistence layer or loop implementation can be mounted at the runtime seam. In Claude Code, the execution machinery and core loop remain fixed, so you extend around them rather than replacing them.

> The two projects are optimizing for different kinds of flexibility. Claude Code makes anticipated extension points exceptionally usable. DeepSeek exposes more of the scaffolding itself, including pieces most users will never need to touch.

Both are defensible. The question is whether "I need to change something the harness does not expose" is a real requirement for you or a hypothetical one.

## 2. Architecture: where the seams are

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_4.png)

Both systems run the same basic agent loop: gather context, construct the model request, call the model, execute tool calls, feed the results back, and repeat. The difference is which parts of that loop you are allowed to replace.

|  | DeepSeek Harness | Claude Code |
| --- | --- | --- |
| **License / source** | MIT, open-source runtime | Closed-source product; sandbox runtime is open source |
| **Language** | TypeScript (\~453K LOC / \~219 packages at time of review) | Main implementation not public |
| **Extension model** | Cordis plugins + profile/bundle/patch composition | Hooks, skills, subagents, agent teams, plugins, MCP |
| **Agent loop** | Plugin / replaceable | Core loop not exposed for replacement |
| **Session model** | Append-only event log; model-visible state derived from it | Local JSONL transcripts; `CLAUDE.md` + auto memory for persistent context |
| **Sandbox** | `read-only`, `workspace-write`, `danger-full-access` | Seatbelt on macOS; bubblewrap on Linux/WSL2; no native-Windows sandbox |
| **Model boundary** | Replaceable model/provider adapters | Claude models via Anthropic API, Bedrock, Vertex AI, Foundry, or gateways |
| **Entry points** | Web UI, CLI/headless, ACP/JSON-RPC, Python SDK | CLI, VS Code, JetBrains, desktop, web, mobile access |

Worth being precise about "extensibility," because calling this a simple win for either side misses the interesting part.

Claude Code's extension surface is more mature and easier to use for the things it already exposes. Hooks are documented and can react to a large set of lifecycle events; skills are straightforward to package; MCP has a broad ecosystem; subagents and agent teams are built into the product; and plugins provide a relatively approachable distribution mechanism. If the change you want is one Anthropic anticipated, Claude Code will usually get you there faster.

DSH exposes seams at a lower level. The model adapter, agent loop, tools, storage, permissions, scheduler and UI all participate in the same plugin architecture. That gives it a strictly broader *class* of things that can be replaced, but the tradeoff is maturity: the project is still a developer preview, the plugin ecosystem is young, and replacing runtime components is inherently more complicated than attaching a hook or installing a skill.

## 3. The benchmark: same model, different harness

We ran 30 hard agentic tool use tasks against real apps, six harnesses, every single one wired to the same model, DeepSeek V4 Pro 0813, through our [hosted MCP](https://composio.dev/mcp) router, with a 900-second ceiling per task.

The tasks hit Gmail, Google Calendar, Sheets, Drive, Airtable, Slack, GitHub, Linear, PostHog and PagerDuty. Real accounts, real state, graded on what the state looked like afterwards.

Same model, same tasks, same tools. The only thing that changes is the harness, so whatever gap shows up is the wrapper, not the model.

Here is how our two contenders did:

| Harness (DeepSeek V4 Pro, hard 30 set) | Tasks passed | Avg tokens per task | Avg tool calls | Cost per shared success |
| --- | --- | --- | --- | --- |
| Claude Code | 19/30 (63%) | 649,900 | 11.7 | \$0.074 |
| DeepSeek Harness 0.1.0-rc.7 | 20/30 (67%) | 88,562 | 14.2 | \$0.028 |

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_5.png)

**The preview won.** `dsh` passed one more task than Claude Code and spent a seventh of the context doing it. Cost per shared success, the paired metric across the 14 tasks every harness passed, came out at **3 cents against 7**.

Look at the tool call column, because it rules out the obvious explanation. Claude Code took *fewer* actions per task, 11.7 against 14.2, and still used 7.3 times the tokens. So it is not doing more work. It carries far more context into every single step.

One shared task shows it cleanly. On the PostHog aggregation case both harnesses passed all 8 checks, and Claude Code used 428,677 tokens against `dsh`'s 53,243. Same model, same answer, 8 times the context.

Claude Code does win the clock. It averaged 305.1s against 323.9s, and since both harnesses timed out exactly twice, that gap is real rather than an artifact. Strip the timeouts and it is 263s against 283s. Claude Code is the fastest harness of the six, and it is also the most expensive by a wide margin.

### The caching finding

On an earlier eight-harness run using V4 Flash, we priced each harness's token mix against the full uncached rate. Every other harness paid between 23% and 55% of sticker. **Claude Code paid 99%.**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_6.png)

Anthropic bills cache reads at 0.1x input, and Claude Code's context assembly is built around Anthropic's explicit `cache_control` breakpoints. Point it at someone else's model through a third-party gateway and that machinery stops paying off, while provider-agnostic harnesses keep their cache hits.

So the finding is narrow and useful. Claude Code's cost advantage is provider-coupled, and it does not travel. On Anthropic models it is presumably fine. Used as a harness for someone else's model, which is exactly what this benchmark does, the thing that makes it economical stays behind.

For the curious, Pi took the V4 Pro run at 21/30, Codex tied `dsh` at 20/30, OpenCode matched Claude Code at 19/30, and Hermes landed at 18/30.

One finding held across both runs. There are ten multi-app workflow tasks in the set, written the way a colleague would actually ask, with no schema and no success contract. Across every harness we have tested, on both models, they have produced **two passes out of eighty**. Nobody has solved that yet.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_7.png)

> NOTE: `dsh` ran against DeepSeek's native endpoint while the others went through OpenRouter, and it executed on August 18, five days after the other five arms. Claude Code hit two timeouts that left no usage record, so it is metered on 28 of 30 cases and only the paired cost metric compares cleanly. The 99% figure is a derived signal about cache behaviour on the Flash run, not a billing disclosure. Read the exact numbers with that in mind.

Check how each harness performed on the same benchmark with Kimi K3: [Best agent harnesses](https://composio.dev/content/best-ai-agent-harnesses)

## 4. Pricing

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_8.png)

### DeepSeek Harness

- The runtime is free. MIT, fork it, ship it. There is no hosted product and no subscription, because you run the loop yourself.
- You bring your own key. Credentials are referenced by environment variable name rather than pasted into config.
- V4 Flash moved to peak/off-peak billing on August 16, 2026: \$0.22 input / \$0.66 output per MTok off-peak, \$0.44 / \$1.32 peak. The flat \$0.14 / \$0.28 our eval priced against is now historical.

### Claude Code

- No free tier. Pro at \$20/month (\$17 billed annually) is the lowest tier that includes it. Max 5x from \$100, Max 20x at \$200, Team starts at \$20/seat/month when billed annually (\$25 monthly). Enterprise starts at \$20/seat/month plus usage billed at API rates, with annual contracts.
- Claude Code and the Claude chat apps draw from **one shared quota**. A heavy afternoon in the chat app directly reduces how much coding you can do that week. Limits stack a rolling five-hour window under a weekly cap.
- Anthropic publishes multipliers, not token counts, so "5x" is relative to Pro rather than a number you can budget against.
- On an API key: Sonnet 5 at \$2/\$10 per MTok through August 31, 2026, reverting to \$3/\$15 after. Opus 5 at \$5/\$25. Haiku 4.5 at \$1/\$5. Fable 5 at \$10/\$50. **Cache reads bill at 0.1x input**, which is the lever our eval found Claude Code failing to pull off-platform.

**The practical split:** DeepSeek Harness itself costs nothing; the bill is whatever model and infrastructure you put behind it. With V4 Flash, that can be as low as \$0.22/\$0.66 per MTok off-peak. Claude Code takes the opposite route: a \$20 Pro subscription can make heavy interactive coding remarkably cheap until you hit its shared usage limits, while API-scale workloads inherit Anthropic's substantially higher per-token rates.

| Model | Input | Output | Notes |
| --- | --- | --- | --- |
| **DeepSeek V4 Flash — off-peak** | **\$0.22** | **\$0.66** | Cheapest first-party DSH route |
| **DeepSeek V4 Flash — peak** | **\$0.44** | **\$1.32** | Peak: 01:00–04:00 & 06:00–10:00 UTC |
| **DeepSeek V4 Pro — off-peak** | **\$0.66** | **\$1.98** | Higher-capability DeepSeek model |
| **DeepSeek V4 Pro — peak** | **\$1.32** | **\$3.96** | Peak pricing |
| **Claude Haiku 4.5** | **\$1** | **\$5** | Cheapest current Claude API model |
| **Claude Sonnet 5** | **\$2** | **\$10** | Intro pricing through Aug. 31, 2026 |
| **Claude Sonnet 5 — from Sept. 1** | **\$3** | **\$15** | Standard pricing |
| **Claude Opus 5** | **\$5** | **\$25** | Higher-end coding/agent model |
| **Claude Fable 5** | **\$10** | **\$50** | Long-running agent model |

## 5. Models

The starkest column on the board.

`dsh` treats the model/provider layer as replaceable. Its first-party adapter ships `deepseek-v4-flash` and `deepseek-v4-pro`, both with a 1M-token context window. DSH exposes thinking controls through its adapter, while DeepSeek's native API distinguishes non-thinking from thinking mode and currently supports `high` and `max` reasoning effort; lower compatibility values map to `high`. The adapter's `maxTokens` setting is a configured cap rather than the model's ceiling, which DeepSeek documents at up to 384K output tokens. `deepseek-official` is the default route, but custom and OpenAI-compatible provider routes can be configured without replacing the agent loop.

The qualifier is support depth rather than architectural lock-in. DeepSeek explicitly says V4 received dedicated optimizations for agentic workloads, so its own models naturally have the most first-party-tested path through the stack. DSH remains model-agnostic at the architecture level; how well another provider behaves depends on the adapter and that model's tool-calling compatibility.

Claude Code takes the opposite shape. It is built around Claude models, which can be served through Anthropic directly, Bedrock, Vertex AI, Microsoft Foundry, or compatible gateways. Those options change where Claude is deployed and routed more than they turn Claude Code into a model-neutral harness. Prompt caching also lives in whichever infrastructure serves the model, so gateway or off-platform behaviour can materially affect cache economics, exactly the failure mode our eval encountered.

## 6. Extensibility

This is where the two projects are least comparable and most interesting.

Claude Code's **extensibility model is largely external and config-driven**. Hooks fire across roughly 30 lifecycle events and can invoke shell commands, HTTP endpoints, MCP tools, prompts or agentic verifiers; command hooks exchange structured JSON with Claude Code over stdin/stdout. MCP integrations can run as local stdio processes or remote HTTP services, with legacy SSE support still available. Skills are Markdown instruction packs whose descriptions are discoverable up front while their full contents load on demand. Plugins bundle skills, agents, hooks, MCP servers and other extension components into installable packages distributed through marketplaces and managed through `/plugin`.

**DSH exposes a lower-level extension surface, but its ecosystem is still extremely young.** Most of the runtime’s major services participate in the Cordis plugin graph: tools, sessions, subagents, scheduling, workflows, storage and UI components are composed through mounted packages and profile patches. That gives DSH a broader class of things that can be replaced, at the cost of substantially more runtime complexity.

The sharper distinction is not declarative versus imperative so much as **extension boundaries versus runtime composition**. Claude Code gives you well-defined places to attach behavior around a fixed core. DSH lets plugins participate inside the runtime’s service graph itself, carrying state and depending on other mounted services. That makes deeper replacement possible, but it also makes lifecycle, cleanup and failure handling harder.

**Low-friction extension points around a fixed core versus lower-level composition inside a replaceable runtime. Most users should prefer the former until they actually need the latter.**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_9.png)

## 7. Permissions and safety

Claude Code wins on maturity here, and the gap is narrower than it was a year ago because both now enforce below the application layer.

Claude Code's `/sandbox` uses Seatbelt on macOS and bubblewrap on Linux and WSL2, with a socat-based proxy for network egress control. Every child process inherits the restrictions, so an `npm install` runs its postinstall scripts inside the boundary too. The gap is Windows: WSL1 is unsupported and native Windows is planned rather than shipped.

DSH's ladder is structurally similar and fails closed. `sandbox-policy.mode` defaults to `read-only`, with `workspace-write` as an explicit opt-in, which is the single best decision in its config surface. That flip is the moment `dsh` stops being an inspector and starts being an editor, and making it deliberate is right. No usable sandbox backend means `SANDBOX_UNAVAILABLE` and a refusal to run, never a silent downgrade. Its Windows path is token-limited only, and the docs say so plainly: reads, network access and process visibility stay unrestricted.

Two things complicate DSH's auditability story, and they cut against the argument I've been making for it.

**The session log is an asset and a liability.** If "model-visible means logged" holds, the session log can contain sensitive prompts, file contents and tool outputs that reached the model. The property that makes the audit trail valuable is what makes that directory sensitive. Treat it as regulated data, add it to `.gitignore` before the first run rather than after, and give it a retention policy.

**Self-modification complicates runtime assurance.** The `cordis_*` tool family lets the agent author a plugin mid-session, mount it and tear it down. It is opt-in and approval-gated, and the docs are appropriately nervous about it. But on a long unattended run, the thing you reviewed at turn 10 is not necessarily the thing executing at turn 400, and the approval gate only helps while a human is watching. The log tells you what happened, which is a post-mortem, not a control. Keep dynamic Cordis off for unattended runs.

Claude Code has no equivalent hazard, because it has no equivalent capability. That is the trade in one sentence.

## What the internet thinks

DSH is too new for a mature community consensus, so the useful signal is not whether launch-day posts called it a Claude Code killer. It is what happens when developers try to put the architectural claims to work.

The [Hacker News thread](https://news.ycombinator.com/item?id=49285244) is the better read and unusually, one of the authors is in it. User `tianyicui` posted early to say it is an early developer preview, to expect rough edges and compatibility-breaking changes, and to ask for feedback. The account appears to belong to Tianyi Cui, the ex-Jane Street engineer DeepSeek reportedly hired in March 2026 specifically to lead harness development, which tells you this was a resourced bet rather than a side project bolted onto a model launch. Julian Goldie and Kasra Dash [ran both on the same prompt on the same afternoon](https://juliangoldie.com/deepseek-harness-vs-claude-code/), DSH on V4 Pro against Claude Code on Opus 5 at high reasoning. DSH finished the entire build in 11 minutes for roughly five cents. Claude Code passed the 30-minute mark and was still going when they moved on. Then they opened both homepages, and the Claude build was the only one that looked like a real business website: animation that tracked the mouse, and references to things they never typed, including a regional detail carried over from earlier work. Kasra scored Claude Code 9 out of 10, Julian gave DSH a 7 and was told he was being generous. Neither moved their daily work. Both moved specific tasks immediately. Their line is the best one-sentence verdict in the whole discourse: Claude Code takes anything a client will see, DSH takes volume, drafts and anything disposable.

🎥 [Watch on YouTube — Watch on YouTube](https://youtu.be/Gn5x9qHuCkQ?si=efgIZzUeb2s5CN89)

**The strongest corroboration came from a competitor.** Two days before the DSH launch, Elon Musk posted that Grok 4.6 works best with the Grok Build harness and that the experience is significantly worse without it, so evaluations should use Build. Read it again. The CEO of the company that shipped the model is telling you the model is not enough. That is the harness thesis stated by someone with every commercial reason to say the opposite.

**The most grounded skepticism came from people who tried to actually use it.** A developer writing at [AI Advances](https://ai.gopubby.com/should-you-switch-from-claude-code-to-deepseek-harness-54bc795f94fa) wanted one thing from `dsh`: drive the Qwen model already sitting on his SSD through Ollama. He found he could not, at least not without writing the provider entry himself. “Supporting OpenAI-compatible provider routes is not the same as ‘works with your local setup out of the box.’” is true and it is not the same as "works with your local setup out of the box." Separately, Magnus Hedemark [inspected the code before running it at all](https://magnus919.com/2026/08/deepseek-harness-what-i-found-before-i-let-it-run/) and shipped a wrapper script as a guardrail, handling the geopolitical question about as soberly as it can be handled: national origin is not evidence of wrongdoing, and software this close to your credentials and shell earns scrutiny regardless of who wrote it.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_10.png)

**One criticism aged in three days, which is its own data point.** On August 16 a stability review noted that the repo had received no public commits since launch, issues were disabled, there were no tags or releases, and npm was still serving the build it shipped with, and concluded the project had gone quiet. Reasonable at the time. Since then `0.1.0-rc.7` landed on August 17 and `0.1.0-rc.8` on August 19. Issues are still disabled and Discussions plus Discord remain the only official channels, so the governance criticism stands. The "abandoned already" reading does not.

The durable objection is plugin fatigue: plugin-heavy ecosystems fragment, accumulate abandoned packages and push maintenance onto users. "Everything is a plugin" is a bet that an ecosystem shows up. Right now there is DeepSeek, a documented catalog of roughly 80 plugins that are almost all DeepSeek's own, and a week of enthusiasm. Against Claude Code’s much larger established ecosystem of skills, subagents, hooks and plugins, DSH has a substantial adoption gap, and no amount of architectural elegance closes it this quarter.

One practical note if you run both: `dsh` reads `CLAUDE.md` and `AGENTS.md` together, and if the two hold identical content, which is common in repos that keep them in sync, the instruction set gets injected twice and the system prompt doubles.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/deepseek-harness-vs-claude-code/image_11.png)

## Which one should you pick?

**Pick Claude Code if:**

- You want to write code today, in a terminal, with the best-supported agent in the category
- You already pay for a Claude plan, which makes the harness free at the margin
- You want an ecosystem: skills, subagent packs and hook libraries that already exist
- You want it in more than one place, from CLI to Chrome
- You're running Anthropic models, where its cache economics actually work

**Pick DeepSeek Harness if:**

- You're building an agent product rather than using one
- You need to audit the implementation, not just observe the behaviour, because compliance asks
- You need real model portability, especially to local or self-hosted endpoints
- You want the replay-testing pattern, which you can port without adopting `dsh` at all
- You're running a cheap model at volume and the cost delta is the business case

**The honest recommendation:** Claude Code stays the daily driver for almost everyone reading this. Read DSH's source anyway. The three patterns worth stealing (logged-equals-visible, keyless replay, and fail-closed escalation as model UX) port to whatever you already run, and the second one would improve most agent test suites in the industry.
