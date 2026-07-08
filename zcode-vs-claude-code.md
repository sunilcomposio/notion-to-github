# Zcode vs Claude Code

On June 13, Zhipu AI released GLM 5.2, and it convinced a lot of people to stop opening their wallets to Anthropic. But once they made the model this good, the inconveniences started showing up in where you actually use it. Claude Code and Codex are SOTA harnesses, but they were made by these companies for their own models, not for the open-source Chinese ones. Which is why Z.ai released ZCode, an agentic development environment made for GLM models.

Now that both models finally have a surface they can be compared on, it's time to compare ZCode and Claude Code.

Some context on where I'm coming from. Opus 4.8 was my daily driver because I spend on the \$200 plan. But ever since I started working with the OpenRouter API, I've realised Opus without a subscription and Anthropic's subsidised usage through the sub makes a massive hole in your pocket. To get that performance without spending a lot, GLM 5.2 works really well. The model's behaviour is different and initially it feels off and you don't get the hype, but after using it, your prompts start working with the GLM model and it gives comparable performance.

I compared the two on models, pricing, real cost per task, architecture and UX, extensibility and ecosystem, and security and data handling. Everything below is as of July 2026. Both companies ship weekly, so any section here could look different next month.

## TL;DR

| Section | Winner | Why |
| --- | --- | --- |
| Models | Claude Code | Opus 4.8 is better at cross-file reasoning and long context, and it can see. GLM 5.2 is text-only and weaker on multi-file bugs. |
| Pricing | ZCode | Cheaper at every tier, \$18/mo at entry. You have to budget around the quota drawdown. |
| Real cost per task | ZCode | Roughly 4x cheaper per finished task even after accounting for GLM's verbosity. |
| Architecture and UX | ZCode | The ZCode app is well built. Anthropic's desktop app is buggy. Claude Code's depth is in the CLI. |
| Extensibility and Ecosystem | Claude Code | Hooks, plugin marketplace, Agent SDK, headless mode. ZCode's answer is BYOK. |
| Security and data | Tie | Anthropic offers contractual no-train plus ZDR. GLM's weights are MIT-licensed so you can self-host. Depends what your reviewers need. |

#### In Summary

ZCode wins three sections to two, with a tie on security. It wins pricing, cost per task, and the app experience. Claude Code wins the model and the ecosystem. What I'd actually do with that result: keep Claude Code as the main harness, route the cheap high-volume work to GLM 5.2, and use the fact that the GLM Coding Plan runs inside Claude Code via env vars so I never have to leave the terminal to do it.

## 1. Models: GLM 5.2 vs Opus 4.8

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/zcode-vs-claude-code/image_1.png)

One of the crucial parts of these harnesses is the models, GLM 5.2 and Opus 4.8.

GLM 5.2 is a 744B Mixture of Experts model with 1M-token context and \~128K max output, with two thinking effort levels, High and Max. Opus 4.8 is a model with 1M default context and 128K max output, with effort levels ranging from low to medium to high and xhigh and max.

| Benchmark | Opus 4.8 | GLM 5.2 |
| --- | --- | --- |
| SWE-bench Verified | 88.6% | — |
| SWE-bench Pro | 69.2% | 62.1% |

Both Pro numbers are vendor aggregates rather than Scale's standardized entries, so treat them as directional. For reference, 62.1 puts GLM 5.2 above GPT-5.5 (58.6) and its own predecessor GLM 5.1 (58.4), and it's the best open-weight score on the benchmark. Opus is seven points ahead at roughly 5x the per-token price. Whether that's a good trade depends on the rest of this article.

Both models advertise 1M-token context. GLM-5.2's pitch is "usable" 1M context that survives long, messy agent trajectories. Z.ai claims it maintains module boundaries, API contracts, and engineering decisions across a full workflow. In practice, an independent Kilo Code test found GLM-5.2 strong at local bugs contained in a single function but noticeably weaker when catching a bug requires connecting behavior across several files, the same point where models from OpenAI and Anthropic hold steady. For Claude Code, the practical wisdom is that reliability on long tasks matters more than raw window length, and long multi-file tasks are exactly where GLM gets soft.

On speed: GLM-5.2 at max generates \~206 tokens/sec median across providers, up to 457 t/s on the fastest hosts like Blackbox AI, while Opus 4.8 at max runs \~63.5 t/s on Anthropic's API. So GLM's raw output speed is much higher. It is also very token-hungry, \~43K output tokens per Artificial Analysis Intelligence Index task, \~37K of it reasoning, up from GLM-5.1's 26K, which lengthens wall-clock time on complex runs. Opus 4.8 has a long time-to-first-token, \~32 to 37 seconds at max effort per Artificial Analysis, though its fast mode runs at \~2.5x speed for \$10/\$50 per 1M, 3x cheaper than 4.7's fast mode was.

The weakness with GLM lies in its lack of multimodality. It cannot see its own UI output or read screenshots, and for agentic frontend work you want the model looking at what it built. Z.ai knows this, which is why the GLM Coding Plan bundles a GLM-4.6V-powered Vision MCP that lets GLM-5.2 read error screenshots through a second model. It works, but Opus has it native. GLM is also weaker at tool-heavy chains (Tool-Decathlon), ultra-long-horizon tasks, and it occasionally hallucinates plans.

One thing SWE-bench misses entirely, because it's Python-heavy and English-heavy: GLM 5.2 is genuinely strong multilingually, especially in Chinese and other East Asian languages, and that extends to Chinese-language codebases, comments, and identifiers. Almost nobody writes about this. If your codebase or your team's comments aren't in English, GLM has an edge no benchmark table will show you.

#### Winner: Claude Code

Opus is the better model. Cross-file reasoning, long-context reliability, tool chains, and it can see. If your work lives in single files and English isn't a constraint, the gap shrinks a lot, but on capability it's Opus.

## 2. Pricing

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/zcode-vs-claude-code/image_2.png)

The subscription tiers:

| Plan | Z.ai (GLM Coding Plan) | Anthropic |
| --- | --- | --- |
| Entry | Lite, \$18/mo (\~80 prompts / 5 hrs, \~400/week, 100 MCP calls) | Pro, \$20/mo (includes Claude Code) |
| Mid | Pro, \$72/mo (\~400 / 5 hrs, \~2,000/week, 1,000 MCP) | Max 5x, \$100/mo |
| Top | Max, \$160/mo (\~1,600 / 5 hrs, \~8,000/week, 4,000 MCP) | Max 20x, \$200/mo |

[Z.ai discounts ](https://zcode.z.ai/en)10% monthly, 20% quarterly, 30% yearly, which puts the yearly tiers at roughly \$12.60, \$50.40, and \$112. Entry tiers are near parity (\$18 vs \$20) and the gap widens at scale (\$72 vs \$100, \$160 vs \$200). Dynamic Workflows on the Anthropic side needs Max, Team, or Enterprise, so the \$20 tier doesn't get you that.

Critical catch: the GLM Coding Plan is a quota product, not a metered one, and GLM-5.2 draws down quota at 3x peak and 2x off-peak (a promo eases off-peak to 1x through end of September 2026). One "prompt" is also roughly 15 to 20 model calls. So a Lite plan's \~80 prompts per 5-hour window is really a token-and-drawdown budget wearing a prompt-count label. Budget against the drawdown multiplier, not the prompt count, or the plan will feel like a third of what the pricing page promised.

Even with the drawdown priced in, every GLM tier still comes out cheaper than the Anthropic tier next to it. Anthropic's Pro plan at \$20 is good value and including Claude Code in it is underrated, but the step after Pro is \$100, and Z.ai's step after Lite is \$72 with roughly five times the prompts.

#### Winner: ZCode

Cheaper at every tier, and much cheaper on yearly billing. The drawdown multiplier eats into the advantage without erasing it.

## 3. Cost per Task

The per-token headline is the number everyone quotes and the one that misleads the most. GLM-5.2's API runs \$1.40 in / \$4.40 out per 1M tokens (\$0.26 cache), a combined \$5.80. Opus 4.8 runs \$5 / \$25, a combined \$30. On output tokens alone GLM is roughly 5x cheaper.

That gap shrinks once you normalize for how many tokens each model actually spends to finish a task, because GLM-5.2 is verbose. Artificial Analysis clocks it at about 43K output tokens per Intelligence Index task.

That's up from GLM-5.1's 26K and higher than open-weight peers like MiniMax-M3 (\~24K) and Kimi K2.6 (\~35K), which puts GLM-5.2 among the less token-efficient models at its intelligence tier. Under Max thinking effort it climbs to nearly 85K output tokens on a complex task. High effort gives you roughly 95% of Max's intelligence at about half the token cost, so the effort knob is the main lever you have on GLM's spend.

Translated to money, Artificial Analysis puts GLM-5.2 at about \$0.46 per Index task, up from GLM-5.1's \$0.25. So the verbosity roughly doubles GLM's own cost-per-task generation over generation while it's still the cheapest usable frontier-adjacent option. It sits on the Pareto frontier of intelligence vs cost-per-task, but it sits off the most attractive quadrant on the intelligence vs output-tokens chart. Both things are true at once.

The most useful number here is independent. Braintrust ran exact-retrieval tasks derived from the CPython standard library at \~25K and \~50K context and measured accuracy against deterministic ground truth.

At 25K tokens GLM-5.2 scored 83.3% at \$0.0208 per trace against Opus 4.8's 86.7% at \$0.0856. At 50K tokens GLM scored 84.5% at \$0.0415 against Opus's 88.0% at \$0.1849. So Opus leads by roughly 3.5 points on exact long-context retrieval at both sizes, while costing about 4.1x to 4.5x more per trace. On their perturbation-control slice, GLM came out about 86% cheaper per correct answer.

The honest cost gap for token-hungry agent work is closer to 4x than the 5-7x the per-token sticker implies, and it narrows further the more reasoning-heavy the run. It does not invert. GLM stays materially cheaper per finished task, and you pay for that with a few points of accuracy on cross-file and long-context work, plus latency from the verbosity.

#### Winner: ZCode

Whether the 3.5 accuracy points are worth 4x depends on your work, but on cost this one goes to GLM regardless of how you slice the numbers.

## 4. Architecture and UX

ZCode is primarily an Electron based desktop app with a harness tuned for GLM models, very similar to the Codex app. It has a file manager, terminal, Git panel, and live browser preview built in. Claude Code on the other hand is a CLI agent, with a web interface and a desktop app.

- Agent modes: ZCode has five permission modes (Default, Confirm Before Changes, Auto Edit, Plan, Full Access) cycled with Shift+Tab, plus Goal Mode. Claude Code has six (default, acceptEdits, plan, auto, dontAsk, bypassPermissions); its auto mode is backed by a separate server-configured classifier that reviews actions before they run, an architectural safety feature ZCode lacks.
- Subagents: Claude Code subagents support a per-agent model field (sonnet/opus/haiku/fable/inherit), nest to 5 levels (v2.1.172+), run in background by default (v2.1.198), and are project- or user-level. ZCode subagents (v3.2.0, June 29) are beta, user-level only, foreground-only, with immutable built-in roles. They do support per-subagent provider mixing though, two subagents, two different vendors' models, one session, which Claude Code does not.
- MCP: both support it. The GLM Coding Plan bundles MCP tool calls (Web Search, Web Reader, Zread, Vision), including the Vision MCP from the models section.
- Memory and checkpointing: both are stateless across sessions, context wiped when the process ends, which is also true of Cursor and Copilot. ZCode has conversation-level rollback where every message is a checkpoint, plus the built-in Git panel. Claude Code has automatic checkpointing with /rewind (restore conversation, code, or both), persisting across sessions.
- Remote and mobile: both support phone steering. Claude Code shipped Remote Control (QR pairing, up to 32 concurrent server-mode sessions) around Feb 25, and Channels (Telegram, Discord, iMessage) on March 20, months before ZCode's QR pairing (1 phone at a time) and WeChat/Feishu bot channels launched the week of July 1. The remote "difference" that dominated early ZCode coverage is stale; the real distinction now is geography and concurrency.
- Platforms: ZCode runs macOS, Windows, and Linux (beta). Claude Code runs all major OSes plus IDEs, web, and iOS.

On paper Claude Code is deeper, and the classifier-backed auto mode is something I wish every harness had. But I've used both desktop apps, and the ZCode app feels better than Anthropic's Claude Code desktop app. Anthropic's UI is buggy and I really dislike using it. Their polish went into the CLI and the desktop app didn't get the same attention. ZCode was a desktop app from day one, the file manager, Git panel, and live browser preview sit where you expect them, and things just work.

The problem with the desktop apps or the web interfaces generally is they work their best when you don't need to see the code. When your prompts are clear enough, or if it's a side project where the final goal is to make something that works. When you need to see the actual code in production systems and understand how things work, I find the terminal experience inside Cursor to be far superior than anything else. So for production work I'm in a terminal either way and this comparison barely matters. For the app-shaped work, if I want to build a game or fix/redesign my personal website, stuff like that, ZCode is the app I'd open.

#### Winner: ZCode

Claude Code has more machinery but it lives in the CLI. Comparing the desktop apps against each other, ZCode built a good one and Anthropic shipped a buggy one.

## 5. Extensibility and Ecosystem

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/zcode-vs-claude-code/image_3.png)

Claude Code has the deeper ecosystem: hooks at 25 lifecycle points, skills as folder-based instruction packs, versioned plugins, a mature plugin marketplace, headless/CI mode, the Agent SDK, and OpenTelemetry. ZCode ships skills and access to the Claude Code plugin marketplace itself, but no documented event-hook system, and hooks are the piece that makes Claude Code programmable rather than just configurable.

ZCode's differentiator is provider-agnostic BYOK. It takes keys from Z.ai, Anthropic, OpenAI, OpenRouter, Moonshot, MiniMax, DeepSeek, Xiaomi MiMo, or any OpenAI- or Anthropic-compatible endpoint. And the GLM Coding Plan runs inside 20+ third-party tools, including Claude Code itself, meaning you can run GLM-5.2 in Claude Code's harness via env vars. Most comparisons miss this, the whole "ZCode vs Claude Code" framing assumes the model and the harness are a package deal, and they aren't a package deal at all. You can take the cheaper model and run it inside the deeper harness, on the coding plan's quota. Neither company has much reason to advertise this and it's the setup I keep coming back to.

#### Winner: Claude Code

Hooks, plugins, SDK, headless mode, and a marketplace ZCode itself borrows from. BYOK is ZCode's one real card here, and Claude Code will never play it.

## 6. Security and data handling

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/zcode-vs-claude-code/image_4.png)

This is the section where "Chinese model" does a lot of lazy work in most comparisons, so let's do the actual reading. Z.ai's operating entity is JINGSHENG HENGXING TECHNOLOGY PTE. LTD., registered in Singapore, and its Data Processing Addendum for the API services says the services are generally provided from Singapore, with customer data generally processed there.

The DPA's storage clause is unusually direct. The company does not store the content customers or their end users provide or generate through the API, text or other input, and this data is processed in real time to deliver the service rather than saved to their servers.

Worth noting: the no-store guarantee is API-side. [Z.ai's consumer privacy policy](https://zcode.z.ai/en/privacy) separately allows using data to train and improve its models under a legitimate-interest basis, so the consumer chat app and the API sit under different terms.

>

	1. **Content Generation and AI-Assisted Operations**

(1) When you use the ZCode product, we will collect the text, files (including but not limited to uploads and inputs you provide in the form of text, images, audio, video, configuration parameters, shell commands, and similar formats), and code submitted to us through conversation, in order to provide you with content generation and AI-assisted operation services.

(2) Please note that whether and what kind of personal data you provide in a conversation is entirely at your discretion. If you provide us with sensitive data (including but not limited to biometric information), we will implement sufficient security measures. However, there remains a risk that, if such information is leaked or misused, it could cause serious harm to your personal safety, property, or personal rights. We do not recommend that you provide personal data that could adversely affect your legitimate rights and interests.

(3) We will record your conversation content, including input information and generated content. You can delete and manage specific conversations through "History".

On training data generally, Z.ai obtains publicly available internet data to train its models, which is standard for the category and separate from your API inputs.

For a team with data-residency requirements, the cleaner path is that GLM-5.2 is open-weight under an MIT license. You can self-host on your own infrastructure, including EU data centres, and keep inputs fully in your control, or route through a third-party host like Fireworks. Direct API use may still not satisfy GDPR without a data processing agreement executed for your case, which is the specific thing to check if you're in a regulated context.

On the Anthropic side, commercial API and Claude Code usage under commercial terms is not trained on by default, and [Zero Data Retention](https://code.claude.com/docs/en/zero-data-retention) is available for qualified organizations, covering eligible APIs, products using a commercial org API key (including Claude Code accessed via the API), and Claude Code on Enterprise plans. Even under ZDR, Anthropic retains User Safety classifier results to enforce its usage policy.

One thing most comparisons skip: if you're running Claude Code off a Pro or Max subscription like I am, you're on consumer terms, where the "help improve Claude" toggle governs everything. Toggle on, your sessions can be trained on and retained for five years. Toggle off, 30-day retention and no training. The "Anthropic doesn't train on your code" line only holds on commercial terms, so check your toggle.

The practical routing distinction is control model, not trustworthiness. Anthropic gives you contractual no-train plus ZDR on a hosted service you don't run. GLM gives you a no-store API term plus the option to remove the question entirely by self-hosting the open weights. Which one clears your compliance bar depends on whether "contractual assurance on a hosted endpoint" or "the weights run on hardware I control" is what your reviewers need to see.

#### Winner: Tie

If your compliance team wants a hosted service with contractual guarantees and a paper trail, Anthropic has the more mature offering. If they want the model on hardware you control, only GLM can offer that at all.

## So which one

ZCode, three to two, with a tie on security. It won pricing, cost per task, and UX. Claude Code won the model and the ecosystem.

What I'd do with that scorecard: keep Claude Code as the harness for the hard work. Cross-file refactors, long agent trajectories, anything the agent needs to see, anything where a failed run costs you an afternoon. Send GLM 5.2 the single-file bugs, the boilerplate, the high-volume grunt work. And because the GLM Coding Plan runs inside Claude Code via env vars, you can do all of this from one terminal without installing ZCode at all.

The Cursor point from the UX section still stands. If I'm shipping production code I want the terminal-in-editor experience, and that's where Claude Code lives. If I'm building a game or redoing my personal site, ZCode is what I'd open, and at \$18 a month the money argument gets very hard to ignore.

## Pick ZCode if

- You're price-sensitive and your work is mostly single-file or well-scoped tasks. \$18/mo for this capability is the best deal in the category right now.
- You want a desktop app that feels finished, with the file manager, Git panel, and live preview in one window.
- Your codebase, comments, or team language is Chinese or another East Asian language. GLM has a real multilingual edge nobody benchmarks.
- You want provider flexibility. BYOK across Z.ai, OpenAI, Anthropic, OpenRouter, DeepSeek and more, plus per-subagent provider mixing, exists nowhere else.
- Your compliance story requires self-hosting. MIT-licensed open weights are the only way to remove the hosted-endpoint question entirely.

## Pick Claude Code if

- You live in long, tool-heavy, multi-file sessions. This is where GLM gets soft and Opus holds steady.
- Your agent needs eyes. Native multimodality beats a Vision MCP bolt-on for frontend work.
- You build custom workflows. Hooks, plugins, the Agent SDK, and headless mode have no ZCode equivalents.
- You want the safety layer. The classifier-backed auto mode matters more the more autonomy you hand over.
- You want both. Run the GLM Coding Plan inside Claude Code via env vars and route by task instead of picking a side.
