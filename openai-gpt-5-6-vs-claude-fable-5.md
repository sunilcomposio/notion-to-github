# OpenAI GPT-5.6 vs Claude Fable 5

After a lot of cat-and-mouse, OpenAI finally decided to make GPT 5.6 Sol public, the answer to Claude Fable, along with smaller Terra and Luna versions.  And it seems it will be the last of the GPT 5 family, the next will definitely be GPT 6.

GPT 5.6 seems to have the same pre-training but definitely a much better post-training. This is different from Fable, which is a different base model. It’s crazy to think OpenAI was able to match Fable solely based on their post-training. OpenAI is legit flexing their post-training muscle at this point. It remains to be seen if they can catch up to Ant’s iteration speed.

On the pricing and efficiency front,

>

GPT‑5.6 is priced per 1M tokens across three model sizes: Sol is \$5 input / \$30 output; Terra is \$2.50 input / \$15 output; and Luna is \$1 input / \$6 output.

GPT‑5.6 also introduces more predictable prompt caching, including support for <span underline="true">[explicit cache breakpoints](https://developers.openai.com/api/docs/guides/prompt-caching#prompt-cache-breakpoints)</span>[⁠(opens in a new window)](https://developers.openai.com/api/docs/guides/prompt-caching#prompt-cache-breakpoints) and a 30-minute minimum cache life. For GPT‑5.6 and later models, cache writes are billed at 1.25x the model’s uncached input rate, while cache reads continue to receive the 90% cached-input discount.

Sol pricing is the same as GPT 5.5. On the other hand, Claude Fable 5 is priced at \$10 per 1 million input tokens and \$50 per 1 million output tokens. This is 2x higher input token cost and .6x higher output cost than GPT 5.6 Sol. Though token cost rarely tells us anything.

What stands out about GPT 5.6 is how token- and cost-efficient all the models are. On Agents’ Last Exam, all three models are beating Claude Fable 5.

> We trained GPT‑5.6 to get more useful work from every token. On <span underline="true">[**Agents’ Last Exam**](https://agents-last-exam.org/)</span>[⁠(opens in a new window)](https://agents-last-exam.org/), an evaluation of long-running professional workflows across 55 fields, GPT‑5.6 Sol sets a new high of 53.6, eclipsing Claude Fable 5 (adaptive reasoning) by 13.1 points. Even at medium reasoning, it beats Fable 5 by 11.4 points at roughly one-quarter the estimated cost. That efficiency extends to smaller models, which are essential to making intelligence more abundant and affordable: GPT‑5.6 Terra and GPT‑5.6 Luna outperform Fable 5 at around one-sixteenth the cost. On the <span underline="true">[**Artificial Analysis Intelligence Index**](https://artificialanalysis.ai/evaluations/artificial-analysis-intelligence-index)</span>[⁠(opens in a new window)](https://artificialanalysis.ai/evaluations/artificial-analysis-intelligence-index), a broad measure of intelligence spanning agentic work, coding, scientific reasoning, and general capabilities, GPT‑5.6 Sol with max reasoning comes within one point of Fable 5 while completing tasks in 61% less time at roughly half the estimated cost.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/openai-gpt-5-6-vs-claude-fable-5/image_1.png)

It’s rather interesting to see that even Terra and Luna are leaving Fable in the dust in cost and performance.

It’s the same thing on [CritPit](https://x.com/ArtificialAnlys/status/2075423964378366427), an advanced research-level Physics benchmark where OpenAI is crushing Fable and other competitors. Sol (max) is the current leader.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/openai-gpt-5-6-vs-claude-fable-5/image_2.png)

This graph from Artificial Analysis gives a much better view of intelligence per cost for all the GPT 5.6 variants.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/openai-gpt-5-6-vs-claude-fable-5/image_3.png)

The Artificial Analysis chart makes the trade-off clearer: Claude Fable 5 remains at the frontier of raw capability, but the GPT‑5.6 family offers a much stronger intelligence-per-dollar curve. Moving from Luna to Terra to Sol buys progressively more capability, while increasing effort from high to xhigh and max produces smaller gains at a rapidly rising cost. In practice, the best model depends on whether you are optimizing for the highest possible success rate or the most useful work per dollar.

- **Claude Fable 5 — best for maximum reliability:** Fable with fallback reaches an Intelligence Index score of roughly 60, but costs about **\$2.75 per task**, making it the most capable and the most expensive option shown. It is the safer choice for high-stakes workflows where a failed or incorrect action costs more than the model call itself.
- **GPT‑5.6 Sol — best overall balance:** Sol scales from medium through high, xhigh, and max, getting progressively closer to Fable as effort increases. Sol max reaches roughly 59—within about one point of Fable—at approximately **\$1.04 per task**, around **62% cheaper** than Fable with fallback. However, the final gains from xhigh to max are expensive; **Sol high is the best default** because it retains near-frontier capability without paying for sharply diminishing returns.
- **GPT‑5.6 Terra and Luna — best for cost-sensitive scale:** Terra and Luna give up some peak capability but sit deeper inside the chart’s attractive cost-performance region. Their high and max settings are better suited to routing, extraction, background research, and fleets of parallel subagents. Use **Terra** when a smaller model still needs dependable reasoning, and **Luna** when throughput and cost matter more than solving the hardest task on the first attempt.

On [DeepSwe](https://deepswe.datacurve.ai/), the Sol at max settings beats Claude Fable, while the Terra and Luna models have performed almost the same at a fraction of the cost. It seems we have some really good small models for long-running coding tasks.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/openai-gpt-5-6-vs-claude-fable-5/image_4.png)

GPT‑5.6 Sol comes with five effort settings: low, medium, high, xhigh, and max. If you’re wondering which one to use based completely on DeepSwe, here’s a little math

- **Sol xhigh:** 71% pass rate, \$4.70/task, 41k output tokens, 44 steps
- **Sol high:** 69% pass rate, \$3.47/task, 28k output tokens, 37 steps
- **Sol medium:** 61% pass rate, \$1.86/task, 18k output tokens, 31 steps

Moving from xhigh to high costs just **2 percentage points** of pass rate, but cuts cost by **26%**, output tokens by **32%**, and steps by **16%**. That is a very good trade-off, especially when you are running multiple subagents in parallel and those savings compound quickly.

Scale it to 100 tasks: high solves 69 tasks for \$347 and 3,700 steps. Xhigh solves 71 tasks for \$470 and 4,400 steps. In other words, you pay \$123 more and spend 700 extra steps for only 2 additional solved tasks.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/openai-gpt-5-6-vs-claude-fable-5/image_5.png)

In comparison, Claude Fable high also gets a 69% pass rate, but costs \$9.18/task vs \$3.47/task for GPT‑5.6 Sol high — roughly 2.65x more expensive for the same pass rate. It also uses 57k output tokens and 59 steps, compared with Sol high’s 28k output tokens and 37 steps. So for the same pass rate, Fable high costs \$5.71 more per task, uses 29k more output tokens, and takes 22 more steps.

For 100 tasks: Fable high solves 69 tasks for \$918, 5.7M output tokens, and 5,900 steps. Sol high solves the same 69 tasks for \$347, 2.8M output tokens, and 3,700 steps.

In other words, Fable costs \$571 more, generates 2.9M more output tokens, and takes 2,200 more steps without gaining any pass-rate advantage.

## GPT 5.5 Sol vs Claude Fable 5 in Composio Golden Eval

One reason for this benchmark is that there’s hardly any eval right now that measures how well a model (with its harness) performs in real-world applications.

For example, how reliable is Fable inside Claude Code at tracing a production checkout bug across GitHub, Linear, and Jira, identifying the canonical issue from cross-references, linking each duplicate exactly once, and avoiding unrelated tickets or unintended status changes?

These are the tasks people experience while coding every day. This is the reason for this benchmark.

It comprises 47 use cases across multiple applications and workflow categories, including software development, issue tracking, customer support, CRM, analytics, communication, and productivity.

Here’s how both the models fared,

| Metric | Claude Fable 5 in Claude Code | GPT‑5.6 Sol in Codex CLI (high) |
| --- | --- | --- |
| --- | ---: | ---: |
| Coverage | 47/47 | 47/47 |
| Scenarios passed | **47/47** | **45/47** |
| Success rate | **100%** | **95.7%** |
| Average runtime tokens | 276,957 | **223,779** |
| Average recorded tool calls | **5.1** | 6.5 |
| Average agent time | 84.4s | **79.6s** |

### Ten representative Golden Eval tasks

The following sample includes eight successful Sol runs and both of its failed runs. Token counts show total runtime tokens with the input/output split.

| Task | Model | Outcome | Checks | Runtime tokens | Tool calls | Agent time |
| --- | --- | --- | --- | --- | --- | --- |
| zendesk search hydrate sla | Fable 5 | ✅ passed | 9/9 | 342,901 | 6 | 131.0s |
|  | GPT-5.6 Sol high | ❌ failed (verifier rejected) | 8/9 | 307,935 | 9 | 161.4s |
| posthog cohort persons | Fable 5 | ✅ passed | 6/6 | 267,338 | 4 | 41.8s |
|  | GPT-5.6 Sol high | ❌ failed (verifier rejected) | 5/6 | 332,004 | 8 | 91.8s |
| jira transition lookup | Fable 5 | ✅ passed | 6/6 | 256,232 | 4 | 66.3s |
|  | GPT-5.6 Sol high | ✅ passed | 6/6 | 704,738 | 18 | 210.2s |
| gh code search secrets | Fable 5 | ✅ passed | 9/9 | 155,688 | 3 | 67.6s |
|  | GPT-5.6 Sol high | ✅ passed | 9/9 | 555,378 | 15 | 216.8s |
| crm identity dedup | Fable 5 | ✅ passed | 7/7 | 150,481 | 2 | 63.7s |
|  | GPT-5.6 Sol high | ✅ passed | 7/7 | 277,420 | 7 | 113.3s |
| renewal record alignment | Fable 5 | ✅ passed | 13/13 | 381,291 | 8 | 100.3s |
|  | GPT-5.6 Sol high | ✅ passed | 13/13 | 407,312 | 8 | 101.9s |
| sheets sql recon | Fable 5 | ✅ passed | 11/11 | 474,578 | 11 | 223.2s |
|  | GPT-5.6 Sol high | ✅ passed | 11/11 | 147,130 | 7 | 69.5s |
| slack thread harvest | Fable 5 | ✅ passed | 5/5 | 566,292 | 11 | 414.0s |
|  | GPT-5.6 Sol high | ✅ passed | 5/5 | 527,084 | 13 | 148.9s |
| salesforce soql pipeline | Fable 5 | ✅ passed | 7/7 | 110,436 | 2 | 28.4s |
|  | GPT-5.6 Sol high | ✅ passed | 7/7 | 38,087 | 2 | 26.4s |
| gcal freebusy batch | Fable 5 | ✅ passed | 10/10 | 182,620 | 3 | 54.3s |
|  | GPT-5.6 Sol high | ✅ passed | 10/10 | 73,201 | 3 | 38.1s |

Claude Fable 5 + Claude Code was overall the more reliable agent, completing all 47 scenarios successfully.

GPT‑5.6 Sol failed two scenarios, ending with a 95.7% success rate.

- In the Zendesk SLA task, Sol found the seeded tickets but included unrelated tickets in its final result. 
- In the PostHog cohort task, it correctly found the total cohort size but reported 186 people with email addresses, rather than the verified 181.

However, Sol used about **19% fewer runtime tokens** and completed tasks roughly **6% faster** on average. It also made around **27% more tool calls**, suggesting that it often took a more iterative route to reach an answer.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/openai-gpt-5-6-vs-claude-fable-5/image_6.png)

So, the trade-off is straightforward: Fable was more dependable, while Sol was faster and more token-efficient. For workflows where a single wrong action can be expensive, Fable’s reliability may justify the higher price. For high-volume workloads where occasional retries are acceptable, Sol’s efficiency could make it the better option.

<callout icon="ℹ️">

This is a model-plus-harness comparison, not a perfectly isolated model comparison. Fable ran inside Claude Code, while Sol ran inside Codex CLI with high reasoning effort.

</callout>

## GPT‑5.6 Sol’s efficiency may not translate into higher Codex limits

One of the unsaid benchmarks is how much you can use a model inside Codex or Claude Code without hitting the limit. But it seems not to be the case with GPT 5.6 Sol.

[ David Ondrej](https://x.com/DavidOndrej1/status/2075520327518470145) on Twitter (currently X)

>

the \$200/mo Codex plan used to be a steal

I never hit the limits, even running multiple /goal loops with GPT 5.5 xhigh

I could literally have 10+ agents running in parallel and would NOT hit the limit. ever.

but now, with GPT 5.6 Sol, i hit the limits instantly

even while running just 1-2 agents

i'm not happy with this at all...

> [Chris](https://x.com/ChrissGPT/status/2075359956677927302):  This model can’t work for two hours without needing me to upgrade to the \$200 plan.
>

[Youssef Al Toukhi](https://x.com/Youssofal_/status/2075643250518720699): The maths isn’t adding up.

5.6 Ultra isn’t even double the cost of 5.5 xhigh.

I used to run multiple 5.5xhigh /goal modes without ever running into limits. (Yes even taking into account the 2x rate limits ending).

Now on just two prompts of Sol ultra my plan is nuked.

In my opinion, the seemingly unlimited limit of Codex was its best feature; this is the reason I moved to Codex from Claude Code. If new GPTs are more token-efficient, they should offer a similar Codex limit. Even with Sol medium, the limits are getting exhausted faster than GPT 5.5 high.

## So, what shall be your pick in July 2026?

If I had to pick one default, it would be **GPT‑5.6 Sol high**.

Across the benchmarks, Sol high gives the best balance of performance, speed, and cost. It matches Claude Fable high on DeepSwe pass rate while costing far less, using fewer output tokens, and taking fewer steps. It also comes close to Fable in the Composio Golden Eval, while using fewer runtime tokens and finishing slightly faster on average.

But this does not mean Fable is obsolete. Claude Fable 5 still looks like the safer model-plus-harness combination when reliability matters most. In our Golden Eval, Fable completed all 47 tasks, while Sol missed two. If the workflow involves customer-facing updates, production issues, CRM changes, billing, or anything where a wrong action is expensive, I would still prefer Fable.

For everything else, especially high-volume coding agents, research agents, data extraction, and parallel subagent workflows, Sol high is the better default. The small reliability gap is easier to absorb when retries are cheap, and the cost/token savings compound fast.

So my recommendation is simple:

- Use **GPT‑5.6 Sol high** as the default for most agentic coding and tool-use workloads.
- Use **Claude Fable 5** when correctness matters more than speed or cost.
- Use **Sol xhigh/max** only for the hardest tasks where the extra pass-rate gain justifies the additional cost.
- Use **Terra/Luna** for cheaper background agents, routing, extraction, and parallel workflows.

Now, this definitely going to stir up the orange lab. I honestly don’t think Anthropic will take the Fable back out of market. Their hands are tied. It will be anhillation if they do that.
