# Claude Opus 5 vs Fable 5: Coding and Tool Call Comparison

AI models are getting better at both writing code and using real tools, but those are two very different skills.

So for this comparison, I’m testing both.

First, the models have to build and extend a full browser-based city-builder game (not a real-world test), complete with traffic, disasters, fires, rain, and a real Composio-powered incident report. Then, in a separate tool-calling benchmark, they have to complete difficult workflows across live apps such as Gmail, Slack, Google Sheets, Salesforce, GitHub, and Linear.

Together, these benchmarks show whether a model can build something genuinely impressive and still handle real workflows without breaking the final state.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/gif_1.gif)

It matters, because writing great code and reliably operating real tools are not the same skill.

---

## TL;DR

This comparison has two separate benchmarks.

The first is a visual coding benchmark where Claude Fable 5 and Claude Opus 5 have to build and extend a browser-based SimCity-style game.

The coding benchmark is split into two tasks:

- Build the city.
- Add rain, fires, simulation consequences, and real Slack and Google Sheets integrations through Composio.

So far, Fable 5 delivered the strongest city-builder by a pretty large margin. Opus 5 simply couldn’t justify its pricing.

The second section is a real-account tool-calling benchmark using the Composio Golden Eval.

On that benchmark:

- Claude Fable 5 scored **21/23**
- Claude Opus 5 scored **20/23**
- Both models failed the refund-ledger scenario
- Fable 5 used fewer tokens, made fewer tool calls, and finished faster on average

---

## Coding Test Workflow

For the test, we will use the following CLI coding agents:

- **Claude Opus 5:** Claude Code (Anthropic’s terminal-based agentic coding tool)
- **Claude Fable 5:** Claude Code

We’ll compare both models on one SimCity-style coding test, split into two sections:

> 💁 I wouldn’t really call this a real-world coding test. It’s more of a visual game-building test to see how well these models can handle unusual system, game mechanics, and overall vibe.

### 1. Task 1: Build a SimCity-Style City Builder

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_1.png)

For the first part, each model has to build a nice browser-based city builder using the same starter repo and the same 3D assets.

And yeah, we’re using proper game assets here. Actual buildings, roads, cars, trees, and other city props. Not just a bunch of cubes pretending to be buildings.

So I’m expecting a little more from the visual side. And how the models put together the assets.

That way we’re judging how well can the models use the assets we are not judging based on their CSS skills.

### 2. Task 2: Add Disasters and Composio

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_2.png)

For the second task, the models must continue from the city they created in Task 1:

They need to add two disasters:

- Heavy Rain
- Building fires

They need to affect the actual city systems.

Heavy rain might slow traffic, block roads, reduce available power, affect the economy, and force cars to reroute.

A building fire should damage the selected building, reduce its economic output and so on. I think you get the idea.

The models also need to connect the city to real external tools using [Composio](https://composio.dev/).

Whenever a major incident happens, the game should:

1. Create an incident message in Slack.
2. Post meaningful live updates as the situation changes.
3. Update the Slack incident when it is resolved.
4. Log the final incident results in Google Sheets.

So the city handles the simulation, while Composio handles the incident reporting.

---

## Coding Comparison

---

## 1. Build a SimCity-Style City Builder

The task isn’t as simple as it sounds, trust me. Both models are being asked to build a full browser-based SimCity-style game.

If you’ve played the game, you know it’s pretty dang complex.

Even if we’re not able to rebuild this entire thing, we should get pretty close to it in terms of the look and feel.

There are roads, traffic, money, population, jobs, power, happiness, demand, and a bunch of systems that need to work together.

Opus 5 and Fable 5 are both supposed to be beast coding models, so the test has to be of their level.

We are also giving both models the exact same 3D assets, including buildings, roads, cars, and trees.

> 💁 If you’re interested in the assets used, they are from [Quaternius Downtown City MegaKit](https://quaternius.com/packs/downtowncitymegakit.html).

We’ll compare visual quality, gameplay feel, feature completion, bugs, performance, code quality, token usage, cost, time, and of course, the overall feel.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_3.png)

### Fable 5

Holy! Fable 5 pretty much nailed this one.

I expected it to produce a nice-looking Three.js city, maybe with a few buildings, some roads, and a bunch of statistics pretending to be a simulation. But what it built felt much closer to an actual small city-building game.

It had pretty much everything I asked for:

- a full starter city
- roads and connected intersections
- residential, commercial, and industrial zoning
- buildings that develop after placing zones
- parks, fire stations, and power plants
- population, jobs, happiness, and money
- power usage and capacity
- residential, commercial, and industrial demand
- moving traffic
- pause and speed controls
- day and night modes
- camera controls
- bulldozing
- space to expand the city

You could place roads, add new zones, watch buildings appear (in steps), see cars driving around following lanes, and notice the population, jobs, power, demand, and economy change as the city grew.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_4.png)

The visuals were probably the best part.

It inspected the asset catalogue, loaded the models, handled their scale and rotation, reused them across the city, and arranged everything into residential areas, industrial areas, a downtown section, parks, roads, and a waterfront.

It could have easily ended up looking like an asset-pack demo with random buildings placed everywhere. Instead, it looked like a properly planned miniature city.

The gameplay also felt way more polished than I expected.

Of course, it was not perfect.

The shadows had some issues, and there was also a funny glitch where parts of the land started looking like water when I zoomed in too much.

What I liked was how Fable tried to debug the shadow issue.

Instead of randomly changing lighting values and hoping something worked, it created a tiny temporary React Three Fibre scene just to test the shadows.

It first checked whether the directional light was the problem. Shadows worked in the small test scene, so it figured out that the issue was probably somewhere in the main scene setup or the lighting component.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_5.png)

That was honestly pretty cool to watch.

The amount of code it changed was also kinda insane.

It added **5,702 lines** and removed **313 lines**, so the full run touched more than **6,000 lines of code**.

It also spent a lot of time working on the project. The API time was a little over one hour.

It spent time inspecting files, running commands, building features, opening the app, finding issues, and fixing things.

Here is the game in action:

That was honestly pretty cool to watch.

The amount of code it changed was also kinda insane.

It added **5,702 lines** and removed **313 lines**, so the full run touched more than **6,000 lines of code**.

It also spent a lot of time working on the project. The API time was a little over one hour.

It inspected files, ran commands, built features, opened the app, found issues, and fixed things.

Here is the game in action:

🎥 [Watch the demo on YouTube](https://youtu.be/uAgPRxwctf0)

You can find the generated code here: [Fable 5: City Builder](https://github.com/shricodev/simcity-fable-gpt-kimi/tree/2706ab1be990a1aae6efbb30f448c9d354278d17)

- **Cost**: \$73.18
- **Duration**: 1h 3min 23sec API time, 2h 24min 26sec wall time
- **Code Changes**: +5,702 lines, -313 lines
- **Session Usage**: 30% used
- **Token Usage**:
	- **Claude Haiku 4.5**: 9.5k input, 36 output
	- **Claude Opus 4.8**: 28 input, 17.0k output
	- **Claude Fable 5**: 854 input, 248.9k output
	- **Cache read**: 45.35M
	- **Cache write**: 865.4k
- **Cost by model**:
	- **Claude Haiku 4.5**: \$0.0096
	- **Claude Opus 4.8**: \$1.47
	- **Claude Fable 5**: \$71.69

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_6.png)

> 💁 **NOTE:** This run cost **\$73.18**, so yeah, this was not cheap at all. Fable 5 alone used **\$71.69** of that. If you’re on API usage, you would be robbed.

One thing to note is that, for some coding tasks, Fable redirects the task to Opus 4.8, which explains the 17K token usage for Opus 4.8.

> The crazy number here is the **45 million cached tokens read**. That gives you an idea of how much context the agent kept going through while working on the project.

Honestly, this whole thing still feels kinda unreal to me.

If you showed me this back in 2020 and said an LLM would build the whole thing in an hour just six years later, I would’ve called it fake.

At this rate, give it a few more years, and it might be building Earth 2 or something. 🫩

---

### Opus 5

This is the one I was most curious about, and honestly, it did not land the way I expected.

By the time I was writing, it had not even been 24 hours since the model was released.

Opus 5 built the city, and the systems are there. But it did not use the assets anywhere near as well as it should have. Buildings feel placed rather than planned, and the UI has a lot of rough edges. Nothing that breaks the game, just a bunch of small things that make it feel rough.

> ℹ️ **To be blunt:** I think Kimi K3 did roughly much better here in terms of UI and overall look and feel. And Kimi is an open model running at a fraction of the price. You'll see it next.

Here's a quick demo:

🎥 [Watch the demo on YouTube](https://youtu.be/-6fv2LOooPc)

Then there's the cost. **\$53.40** for a single task. That is not a joke.

Imagine running one decent feature add in a production repo on API usage. You would get robbed. Opus 5 is marketed as the cheaper option next to Fable 5, but \$53.40 against Fable's \$73.18 is not the gap I was expecting, especially when the output is not even at 50% of what Fable 5 delivered.

It also wrote the most code of any model in this test, over 9,000 lines added.

You can find the code it generated here: [Opus 5: City Builder](https://github.com/shricodev/simcity-fable-gpt-kimi/tree/b2404a5d9a3abf567bc5e5ee632e813d94380f5d)

- **Cost:** \$53.40
- **Duration:** 1h 29min 45sec API time, 1h 49min 16sec wall time
- **Code Changes:** +9,053 lines, -356 lines
- **Token Usage:**
	- **Claude Haiku 4.5:** 9.5k input, 39 output
	- **Claude Opus 5:** 1.1k input, 422.4k output
	- **Cache read:** 68.1M
	- **Cache write:** 1.1M
- **Cost by model:**
	- **Claude Haiku 4.5:** \$0.0097
	- **Claude Opus 5:** \$53.39

> 💁 **NOTE:** 68 million cached tokens read and 422k output tokens. Opus 5 was clearly trying on this. The output just does not reflect the effort.

Disappointing given the expectations, but it is still a solid start. Let's see how it handles the disasters and Composio in Task 2.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_7.png)

---

## 2. Add Disasters and Composio

For the second part, the models have to extend the city with more interaction and logic.

Things like power failures, blocked roads, damaged buildings, fires, and changes to the city’s economy should all connect back to the systems built in the first task.

Whenever a major event happens inside the city, the game has to create an incident in Slack, post live updates as the situation changes, and log the final results in Google Sheets.

So we’re not just testing visuals here. We’re also testing whether the models can connect the game state to real external tools and make the whole flow feel natural.

### Claude Fable 5

This is peak coding. I genuinely don’t know how Fable 5 keeps pulling this stuff off.

I’ve been testing coding models since 2024, and this is easily the finest result I’ve seen so far. At this point, no other model I’ve tested comes close.

Just look at the output:

🎥 [Watch the demo on YouTube](https://youtu.be/wBhHGKGE1Gw)

The rain alone looks drop-dead realistic.

And the craziest part is that I did not provide it with any rain assets. There were no premade raindrop models, weather effects, or fancy shaders waiting inside the project.

Fable created the entire effect with Three.js.

The level of detail is honestly ridiculous.

But what impressed me even more was the Composio integration.

Fable searched the internet, read through the current Composio documentation, understood the SDK, figured out which actions to use, kept the API key on the server, and connected everything to the actual game state.

When an incident started, it created a real Slack message. As the disaster progressed, it posted live updates inside the same thread. When the incident ended, it updated the original message and posted a final summary.

It also logged the final incident result in Google Sheets, including the incident ID, disaster type, duration, affected buildings, blocked roads, economic loss, and final status.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_8.png)

All the pieces of functionality work. It just does.

Here’s the final demo:

🎥 [Watch the demo on YouTube](https://youtu.be/8LTcw92PI7A)

You can find the generated code here: [Fable 5: Disasters and Composio](https://github.com/shricodev/simcity-fable-gpt-kimi/tree/fable-5-simcity)

- **Cost:** \$45.28
- **Duration:** 47 minutes API time, 56 minutes 26 seconds wall time
- **Code Changes:** +4,195 lines, -236 lines
- **Session Usage:** 50%
- **Token Usage:**
	- **Claude Haiku 4.5:** 14.2k input, 36 output
	- **Claude Opus 4.8:** 62 input, 32.3k output
	- **Claude Fable 5:** 726 input, 193.6k output
	- **Cache Read:** 26.2M
	- **Cache Write:** 643.4k
- **Cost by model:**
	- **Claude Haiku 4.5:** \$0.0143
	- **Claude Opus 4.8:** \$2.80
	- **Claude Fable 5:** \$42.47

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_9.png)

> 💁 **NOTE:** Fable added another 4,195 lines of code and completed the whole run in under an hour of API time.

It understood an unfamiliar integration, connected it to an already complex game, built the disaster effects procedurally, and somehow still made everything look so good.

### Opus 5

Opus 5 did a much better job here than it did in Task 1.

Feature-wise, almost everything works. The disasters connect back to the city, the Composio flow does what it's supposed to, and the code itself is decent.

It also didn't write ton of useless comments or a pile of markdown files, which I really appreciate. A lot of models waste tokens on that.

So on the implementation side, I'm satisfied.

Here's a quick demo:

🎥 [Watch the demo on YouTube](https://youtu.be/Z3xCJwzQzcY)

The price is where this falls apart.

This task alone cost \$53.29, pushing the session total to **\$106.69 **(you could buy a GTA 6 Ultimate Edition in this price 😭). Fable 5 did the same task for **\$45.28**. So Opus 5 came out more expensive than the model it's supposed to sit below on price.

I don't know how or why, but it is freaking expensive. The cache read numbers are wild too, 89 million tokens on this task alone.

You can find the generated code here: [Opus 5: Disasters and Composio](https://github.com/shricodev/simcity-fable-gpt-kimi/tree/opus5-simcity)

- **Cost:** \$53.29 for this task, \$106.69 cumulative
- **Duration:** 43min 15sec API time, 1h 41min 2sec wall time
- **Code Changes:** 38 files changed, +5,311 lines, -43 lines
- **Token Usage (this task):**
	- **Claude Haiku 4.5:** 12.0k input, 1.1k output
	- **Claude Opus 5:** 800 input, 185.3k output
	- **Cache read:** 89.2M
	- **Cache write:** 400k
- **Cost by model (this task):**
	- **Claude Haiku 4.5:** \$0.0172
	- **Claude Opus 5:** \$53.28

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_10.png)

> 💁 **NOTE:** Full session for both tasks: \$106.69, 2h 13min API time, and 13,734 lines added. Fable 5 finished both tasks at \$118.46 with a lot better result overall. The gap is not what the pricing page suggests it. Rough value with the model so far.

---

## Tool-Calling Benchmark

The coding benchmark tests whether these models can build a complex visual application.

This next benchmark tests something different.

Here, the models have to operate real SaaS tools and leave the connected accounts in the exact expected state.

This is not about producing a convincing explanation or a nice-looking interface.

The workflow either finished correctly inside the real account, or it didn’t.

## The Two Models

### Claude Fable 5

Claude Fable 5 is Anthropic’s flagship model for expensive, long-running agent work.

It is aimed at ambitious coding projects, professional asynchronous workflows, enterprise tasks, and vision-heavy document work.

It supports a 1 million-token context window at standard pricing.

Its API pricing is:

- **Input:** \$10 per million tokens
- **Output:** \$50 per million tokens
- **Cached input:** \$1 per million tokens

### Claude Opus 5

Claude Opus 5 is the second model in this comparison.

In the tool-calling evaluation, it used more tokens, made more tool calls, and took longer on average than Fable 5.

Its API pricing is:

- **Input:** \$5 per million tokens
- **Output:** \$25 per million tokens
- **Cached input:** \$0.50 per million tokens

### Pricing Comparison

On base API pricing, Claude Opus 5 is the cheaper of the two.

Claude Fable 5 costs twice as much across input, cached input, and output tokens.

| Model | Input per 1M | Cached input per 1M | Output per 1M |
| --- | --- | --- | --- |
| Claude Opus 5 | \$5.00 | \$0.50 | \$25.00 |
| Claude Fable 5 | \$10.00 | \$1.00 | \$50.00 |

That is the brochure version.

I wanted the annoying version.

I wanted to point both models at real accounts and give them workflows designed to make polished demos fall apart.

Does Fable 5’s higher price actually translate into better results, or can Opus 5 get close enough at half the cost?

And when they fail, do they break on the same tasks, or in completely different ways?

---

## Tool Benchmark Workflow

The evaluation covered 23 common task scenarios.

The models had to handle complex multi-step workflows using business tools and APIs.

The judgment was simple:

**Did the workflow actually finish correctly inside the connected account?**

I expected both models to look strong at different points during the run.

What I cared about was where that stopped being enough.

---

## The Composio Golden Eval

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_11.png)

The Composio Golden Eval is a real-world tool-use benchmark.

An agent has to complete difficult, multi-step SaaS tasks against live accounts. After the agent finishes, a verifier reads the resulting account state through the API and checks what actually happened.

The agent is not graded based on how convincing its transcript sounds.

It is graded based on the final state of the real account.

The models interact with tools through the hosted Composio MCP router.

After each run, the verifier queries the connected account through the API and compares the resulting state against the expected assertions.

Writes are uniquely tagged and cleaned up afterward, which allows the benchmark to run against real accounts without permanently leaving test data behind.

The suite uses applications such as:

- Gmail
- Slack
- Google Sheets
- Google Calendar
- Google Drive
- Salesforce
- HubSpot
- GitHub
- Linear

---

## Tool-Calling Results

Both models were evaluated across the same 23 scenarios.

The final results were graded using real-account API readback verifiers.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_12.png)

### Overall Score

| Model | Passed Cases | Success Rate |
| --- | --- | --- |
| Claude Fable 5 | **21/23** | **91%** |
| Claude Opus 5 | **20/23** | **87%** |

The scoreboard is pretty close.

Fable 5 finished **21 out of 23** tasks.

Opus 5 finished **20 out of 23**.

Fable won by one scenario.

### Performance Metrics

| Metric | Fable 5 | Opus 5 | Winner |
| --- | --- | --- | --- |
| **Success Rate** | 91% | 87% | Fable |
| **Quality Score** | 91% | 87% | Fable |
| **Avg Runtime Tokens** | 537K | 698K | Fable |
| **Avg Tool Calls** | 7.6 | 9.4 | Fable |
| **Avg Agent Time** | 130.7s | 146.8s | Fable |
| **Verifier Rejections** | 2 | 3 | Fable |

Fable 5 used fewer tokens, made fewer tool calls, finished faster, and received fewer verifier rejections.

---

## Findings

### Fable 5 Failed Two Scenarios

Fable 5 failed:

1. **Refund Ledger** - 8/13 checks passed
2. **Gh Linear PR Issue State** - 6/7 checks passed

On the refund-ledger task, Fable incorrectly included two chargeback orders that should have been excluded.

It recorded 12 refunds instead of 10.

On the GitHub and Linear task, Fable swapped the GitHub issue identifiers for two adjacent records, #138 and #139.

It got the Linear states correct but mapped them to the wrong issue numbers.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/image_13.png)

### Opus 5 Failed Three Scenarios

Opus 5 failed:

1. **Gmail History Audit** - 5/6 checks passed
2. **Renewal Screen** - 5/11 checks passed
3. **Refund Ledger** - 8/13 checks passed

The report does not provide the specific failed assertion for the Gmail history audit.

The renewal-screen task had multiple check failures, but the detailed failure breakdown was not included.

On the refund-ledger task, Opus made the same core mistake as Fable.

It incorrectly included two chargeback orders that should have been excluded and recorded 12 refunds instead of 10.

### Refund Ledger Was the Universal Failure

Both models failed the refund-ledger scenario.

They both passed 8 of the 13 checks.

They both included two chargeback orders that should have been excluded.

They both recorded 12 refunds instead of 10.

That suggests the filtering logic in this task was genuinely difficult for both Claude models.

### The Key Difference

Fable failed the GitHub and Linear PR issue-state scenario that Opus passed.

Opus failed the Gmail history audit and renewal-screen scenarios that Fable passed.

Both failed the refund ledger.

Fable still finished one scenario ahead overall and did it with lower token usage, fewer tool calls, and a faster average runtime.

---

## Tool-Calling Verdict

Based on this run, Fable 5 is the stronger tool-calling model.

It achieved a better success rate while using fewer tokens, making fewer tool calls, and finishing faster on average.

Opus 5 remained competitive, but its extra token usage and longer runtime did not translate into a better score.

The universal refund-ledger failure is the bigger warning.

Both models confidently included records that should have been filtered out.

For that kind of production workflow, I would add:

- A verifier
- A controlled retry loop
- Idempotency protection
- Final-state validation
- Human review for high-impact actions

Before giving either model production authority, I would rerun similar account-backed workflows using my own tools, my own accounts, API-based final-state graders, and a retry path.

## Conclusion

So far, the two benchmarks are telling very different stories.

On the coding side, Fable 5 still delivered the strongest raw visuals.

Opus 5 is the odd one out. Task 1 was underwhelming, with weak asset usage and a rough UI. Task 2 was much better, with almost everything working and clean code underneath. But across both tasks it burned **\$106.69**, which is close enough to Fable 5's **\$118.46** that the price gap stops being a reason to pick it.

On the tool-calling side, Fable 5 also finished ahead.

It passed **21/23** scenarios, compared with **20/23** for Opus 5, while using fewer tokens, making fewer tool calls, and finishing faster on average.

The biggest warning is the refund-ledger task.

Both models failed it by including two chargeback orders that should have been excluded.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/claude-opus-5-vs-fable-5-coding-and-tool-call-comparison/gif_2.gif)

For those workflows, I would add a verifier, an idempotent retry loop, final-state checks, and human review for anything high-impact.

At this point, the picture is pretty clear: **Claude Fable 5 is leading both the visual coding test and the tool-calling benchmark, while Claude Opus 5 delivered a stronger second coding task but could not justify its cost or heavier tool-use footprint.**
