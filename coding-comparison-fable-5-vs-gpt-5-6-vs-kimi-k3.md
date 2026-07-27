# Coding Comparison: Claude Fable 5 vs GPT-5.6 Sol vs Kimi K3

## Coding Comparison

---

## 1. Build a SimCity-Style City Builder

The task isn’t as simple as it sounds, trust me. All three models are being asked to build a full browser-based SimCity-style game.

If you’ve played the game, you know it’s pretty dang complex.

Even if we’re not able to rebuild this entire thing, we should get pretty close to it in terms of the look and feel.

There are roads, traffic, money, population, jobs, power, happiness, demand, and a bunch of systems that need to work together.

Fable 5, Kimi K3, and GPT-5.6 Sol are all supposed to be beast coding models, so the test has to be of their level.

We are also giving all three models the exact same 3D assets, including buildings, roads, cars, and trees.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_1.png)

> 💁 If you’re interested in the assets used, they are from [Quaternius Downtown City MegaKit](https://quaternius.com/packs/downtowncitymegakit.html).

We’ll compare visual quality, gameplay feel, feature completion, bugs, performance, code quality, token usage, cost, time, and of course, the overall feel.

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

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_2.png)

The visuals were probably the best part.

It inspected the asset catalog, loaded the models, handled their scale and rotation, reused them across the city, and arranged everything into residential areas, industrial areas, a downtown section, parks, roads, and a waterfront.

It could have easily ended up looking like an asset-pack demo with random buildings placed everywhere. Instead, it looked like a properly planned miniature city.

The gameplay also felt way more polished than I expected.

Of course, it was not perfect.

The shadows had some issues, and there was also a funny glitch where parts of the land started looking like water when I zoomed in too much.

What I liked was how Fable tried to debug the shadow issue.

Instead of randomly changing lighting values and hoping something worked, it created a tiny temporary React Three Fiber scene just to test the shadows.

It first checked whether the directional light was the problem. Shadows worked in the small test scene, so it figured out that the issue was probably somewhere in the main scene setup or the lighting component.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_3.png)

That was honestly pretty cool to watch.

The amount of code it changed was also kinda insane.

It added **5,702 lines** and removed **313 lines**, so the full run touched more than **6,000 lines of code**.

It also spent a lot of time working on the project. The API time was a little over one hour.

It spent time inspecting files, running commands, building features, opening the app, finding issues, and fixing things.

Here is the game in action:

https://youtu.be/uAgPRxwctf0

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

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_4.png)

> 💁 **NOTE:** This run cost **\$73.18**, so yeah, this was not cheap at all. Fable 5 alone used **\$71.69** of that. If you’re on API usage, you would be robbed.

The crazy number here is the **45 million cached tokens read**. That gives you an idea of how much context the agent kept going through while working on the project.

Honestly, this whole thing still feels kinda unreal to me.

If you showed me this back in 2020 and said an LLM would build the whole thing in an hour just six years later, I would’ve called it fake.

At this rate, give it a few more years and it might be building Earth 2 or something. 🫩

### GPT-5.6 Sol

Okay, so Sol did… decent. But man, there were just too many issues along the way.

The first attempt technically gave me a build. But when I opened it with `npm run dev`, my CPU usage shot above 90%, and my laptop straight up crashed. Twice. Before I could even record the demo.

Here’s what that first approach looked like:

https://youtu.be/HNaVGiLJpU8

> ℹ️ Notice, the CPU usage at the top of the screen.

After a bunch of back-and-forth fixing, it finally got me a working build.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_5.png)

But to be fair, this isn’t anywhere close to the Fable 5 output. There are just too many bugs in the game implementation itself. It’s not polished, the cars are literally jumping back and forth, and there’s no proper simulation running underneath.

That was a little disappointing, especially since I expected more from the new GPT-5.6 Sol lineup.

One thing I’ll give it credit for: it was fast. Sol shipped its first build in a fraction of the time Fable took. But speed doesn’t mean much when the implementation doesn’t come close.

- **Duration**: \~22 minutes
- **Code Changes**: \~2,400+ lines, 166 deletions, 12 files changed
- **Context Used**: 37% (104k/258k)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_6.png)

Here’s the final demo after all the fixes:

🎥 [Watch the demo on YouTube](https://youtu.be/Mb9o1sUQDNo)

As you can clearly see, this isn’t close.

> 💁 **NOTE**: This test mainly focuses on visual quality. So even if the underlying implementation is strong, a result that does not look visually right cannot be considered the best overall.

### Kimi K3

Okay, this one seriously surprised me.

Kimi K3 delivered the best result I’ve seen from an open model in any of these coding tests so far. For **\$15.02**, the implementation is kinda ridiculous.

The shadows, cars, traffic, and overall look and feel are much better than GPT-5.6 Sol. More importantly, the whole thing feels rock solid, prettier, and actually functional.

Here’s a quick video demo (see the shadow effect):

🎥 [Watch the demo on YouTube](https://youtu.be/NC-HwTqgKSY)

Considering the price, this might be even more impressive than Fable 5. It was a lot slower, taking around **1.5 hours**, but the final result absolutely justified it.

You can find the generated code here: [Kimi K3: City Builder](https://app.notion.com/p/composio/CODE_LINK_PLACEHOLDER)

- **Cost**: \$15.02
- **Duration**: \~1.5 hours
- **Context Used**: 26%
- **Token Usage**: 275,607 tokens

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_7.png)

> 💁 **NOTE:** This run used Kimi K3 through OpenCode with OpenRouter, so the exact timing and behavior may have been affected by the OpenRouter workflow.

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

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_8.png)

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

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_9.png)

> 💁 **NOTE:** Fable added another 4,195 lines of code and completed the whole run in under an hour of API time.

It understood an unfamiliar integration, connected it to an already complex game, built the disaster effects procedurally, and somehow still made everything look so good.

### GPT-5.6 Sol

GPT-5.6 Sol struggled with this one.

The rain and fire effects technically worked, but neither looked particularly convincing in the actual game. That could come down to personal preference, but compared with the Fable 5 result, the visual difference was pretty clear.

The bigger problem was the Composio integration.

Sol managed to write much of the integration logic correctly. It understood the general Composio flow, created the server-side pieces, and implemented the Slack and Google Sheets actions.

But it could not properly connect those pieces to the live game state.

So while the individual pieces were there, Sol was not able to put everything together into a working end-to-end experience.

Here’s the demo:

🎥 [Watch the demo on YouTube](https://youtu.be/JvO1iJ3Z6dU)

You can find the generated code here: [GPT-5.6 Sol: Disaster and Composio](https://github.com/shricodev/simcity-fable-gpt-kimi/tree/gpt5.6-sol-simcity)

- **Context Used:** 197k of 258k
- **Context Remaining:** 25%
- **Weekly Usage Remaining:** 81%

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_10.png)

### Kimi K3

Kimi K3 handled the disaster and Composio implementation better than GPT-5.6 Sol.

It connected the game state to Composio, kept the incident updates tied to the simulation, and somehow the whole thing actually works.

The only obvious gap was the rain. There is a bit of a thunder effect, but no proper visible rainfall. That is still a miss, but visuals are not the main thing in this part. The disaster logic and the Composio flow work, which matters more here.

At this price, it somehow feels even more impressive than Fable 5. Kimi K3 has now overshadowed GPT-5.6 Sol in both coding tests, which is honestly a little scary for an open model.

Here’s a quick demo:

🎥 [Watch the demo on YouTube](https://youtu.be/o0rYRxu9kZg)

You can find the generated code here: [Kimi K3: Disasters and Composio](https://app.notion.com/p/composio/CODE_LINK_PLACEHOLDER)

- **Cost**: \$10.90 for this task, \$25.92 cumulative
- **Duration**: \~1 hour
- **Token Usage**: 117,845 tokens for this task, 393,452 cumulative

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/coding-comparison-fable-5-vs-gpt-5-6-vs-kimi-k3/image_11.png)

---

##
