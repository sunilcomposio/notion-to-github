Moonshot AI, the same company that released Kimi K2, which rivalled Sonnet 4 as an open-source alternative, recently announced their new AI model, Kimi-K2.5, which they call a "Visual Agentic Intelligence" model and the **most powerful open-source model** to date. 🤯

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_1.png)

It ships with a **256K context window** and a pretty wild “agent swarm” idea, where the model can spin up to 100 sub-agents and coordinate up to 1,500 tool calls for wide, long tasks.

While Claude Opus 4.5 comes with $5/M input token and $25/M output token, Kimi-K2.5 is a fraction of that, at $0.45/M input token and $2.50/M output token. That's a wild thing, but open-source models are usually cheaper than proprietary ones.

To summarise the difference, Kimi-K2.5 is the open-source alternative with a bigger context window and a very agent-heavy design.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_2.png)

---

## TL;DR

If you want a short take on the results, here's how the two models did in two standard tests:

- **Kimi-K2.5:** Safest overall pick in my runs. It got to a working build faster, and the “fix loop” was much shorter. Test 2 (Auth + Composio + PostHog) was the main challenge, and Opus basically just shipped it. Still the most expensive and can be a bit slow as it loves to do web searches a bit too much.

- **Claude Opus 4.5:** Insanely good for an open model, and the price is honestly wild. Test 1 was solid after a small fix pass, but it did hit a Cesium base-layer rendering issue. Test 2 worked but felt a bit more fragile, like it needs a little more babysitting to get everything clean.

> 💡 If you want the safer pick for real project work, I’d still **go with Opus 4.5**. If cost and a bigger context window matter more and you’re okay doing some extra fixing and polishing, Kimi-2.5 is a great choice.

---

## Test Workflow

For the test, we will use the following CLI coding agents:

- **Claude Opus 4.5:** Claude Code with API Usage (Anthropic’s terminal-based agentic coding tool)

- **Kimi-K2.5:** Claude Code using Ollama Cloud with a Pro subscription (set up to run external models)

We will check the models on two different tasks:

### **Task 1:** Build TerraView (Google Earth-style globe viewer)

Each model is asked to build a Google Earth-inspired web app called TerraView using Next.js (App Router). The app must ship a working 3D globe viewer with smooth pan, rotate, zoom, and a small set of base layers (for example, OpenStreetMap imagery plus at least one alternate base layer).

### **Task 2:** Authentication + Live User Location System (Composio + PostHog)

> This was a fun test that lets you visualise your users on the globe. I built it to dogfood our newly updated [Posthog MCP integration](https://composio.dev/toolkits/posthog). Do check it out or other integrations to build your dream workflow.

Each model must add authentication to TerraView and integrate PostHog through Composio. Once users sign in, the app should capture their approximate location and display all active users on the globe as markers.

We’ll compare code quality, token usage, cost, and time to complete the build.

> 💡 **NOTE:** I will share the source code changes for in a `.patch` file. This way, you can easily view them on your local system by creating a brand new next.js app and applying the patch file using `git apply <path_file_name>`.

---

## Real-World Coding Comparison

### Test 1: Google Earth Simulation

Here's what I ask: I will start a brand new Next.js project, and they will begin from the same commit and follow the same prompt to build what's asked.

All the results here are evaluated from the **best of 3** responses from each model, so you can expect them to be very fair. We're not judging based on a model's unlucky response.

You can find the prompt I've used here: [Google Earth Prompt](https://gist.github.com/shricodev/e21f4def340db409d4d64ce93ea05a25)

If you look at the prompt, it's a lot to implement, but given how capable our models are, it shouldn't really be much of a problem. 😮‍💨

To summarise, the models are asked to build a polished Google Earth-style Next.js app with a smooth 3D globe, search, layers, pins, saved places, measuring tools, and production-ready code that runs locally out of the box.

### Kimi-K2.5

Kimi K2.5 got surprisingly far on the first draft. Most of the core features were in place without me having to babysit them too much.

That said, the first run was not perfect. It hit a nasty rendering issue when switching certain base layers (the Cesium error popup: `imageryProvider._resource` is undefined), which basically stopped the globe render. I had to do a follow-up fix pass to get the base layer implementation working.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_3.png)

Time-wise, it took **~29 minutes** to get the first draft out, then **+9 min 43 sec** to patch the issues. It surprisingly took a lot of time, considering this is a fast model. It takes a lot of time to reason. The fixes were not huge, but it's the same AI model problem (fixing one part breaks another working part of the code. 🤷‍♂️)

Here's the demo:

Token usage (from Claude Code’s model stats) looked like this:

- Token Usage (before the fix): 15k output

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_4.png)

- Token Usage (final): 15.9k output

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_5.png)

The fix pass added roughly **~900 tokens** in total.

Overall, after the fixes, the final build looked pretty solid.

- **Duration:** ~29 min (first draft) + ~9 min 43 sec (fix pass)

- **Code Changes:** 429 files changed (including asset files), 60,387 insertions(+), 100 deletions(-)

### Claude Opus 4.5

Opus 4.5 ended up looking very close to Kimi’s build, even down to the UI. It had the same overall layout, though I'm not sure how this happened.

Like Kimi, Opus did not achieve a fully working build on the first draft. It chose Resium for the globe integration, which immediately ran into a Next.js + React 18 style runtime crash.

```plain text
TypeError: can't access property "recentlyCreatedOwnerStacks", W is undefined
```

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_6.png)

The first draft took **~23 min**, and then it took about **~7 min** to remove Resium and replace it with Caesium directly. That immediately fixed the issues, and everything was running as expected.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_7.png)

Here's the demo:

You can find the code it generated here: [Claude Opus 4.5 Code](https://gist.github.com/shricodev/4f87d7434e75d1cc24f861b5c4915356)

For token usage, I unfortunately ran this on Claude Max, so I could not get the exact output token breakdown from the CLI. But Claude Code showed it used **53% of the session limit**, so it was definitely a big run.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_8.png)

- **Duration:** ~23 min (first draft) + ~9 min 43 sec (fix pass)

- **Code Changes:** 22 files changed, 3018 insertions(+), 45 deletions(-)

Overall, I slightly preferred Opus 4.5 here. It was a bit faster end to end, and the path to a working product was shorter. Kimi felt slower in comparison, which might just be Ollama Cloud latency, but either way, Opus got to the finish line with little friction. 🤷‍♂️

### Test 2: Authentication + User Location System

Here’s what I ask: I take the TerraView codebase from the final output of Test 1, reset it to a clean commit, and then both models start from that exact same state. They get the same prompt and the same constraints, and they implement the feature as a real production addition.

Just like Test 1, the results are evaluated from the best of 3 runs per model, so we are not judging anyone based on one unlucky response.

You can find the prompt here: Composio [Auth + PostHog + User Locations Prompt](https://gist.github.com/shricodev/51289b10a19ad0b91071b9c0b537801d)

To put it together, the models are asked to add authentication to TerraView, then integrate PostHog via Composio to track signed-in and registered users and their activity. The app should capture each user’s approximate location and then render all active users on the 3D globe as distinct markers. Clicking a marker should show basic user details like name and email.

### Kimi-K2.5

This one was a mess, honestly.

First, it tried to run a server-only package in the browser. That’s not an edge case; that’s just broken.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_9.png)

After I pushed it to fix that, authentication was the next failure. It initially went with NextAuth, but the implementation was so flawed I had to tell it to remove it and do something simpler.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_10.png)

Even after that, the “fixes” kept making things worse. I could get the UI back, but then the globe disappeared entirely, which is kind of the whole app. At that point, I stopped because it wasn’t converging; it was just breaking working stuff. The only thing the app is about. The Globe is gone.

Here's the demo:

- **Duration:** ~18 min (first draft) + 5m 2s (fix) + ~1m 3s (another fix that still broke stuff)

- **Token usage:** 24.3k output (first draft was ~15k, so +9.3k on top)

- **Code changes:** 21 files changed, 1881 insertions(+), 76 deletions(-)

### Claude Opus 4.5

Opus 4.5 absolutely nailed it.

I had basically zero hopes going in, honestly. This task is the kind where models usually fall apart. It has to understand the existing codebase, work around Composio, wire up auth properly, get PostHog tracking working, capture user location, render active users on the globe as markers, and keep the whole thing working end to end.

I mean, there's a lot to implement. Somehow it just… did it.

Here's the demo:

You can find the code it generated here: [Claude Opus 4.5 Code](https://gist.github.com/shricodev/91b1544958ffdbaa107e43ff8fd30e85)

The entire authentication flow works, events are stored, location gets pulled, and active users show up on the globe as distinct markers. Clicking a marker shows the basic user details like name and email exactly as asked.

On code quality, I honestly can’t nitpick much. It’s decent. Recent models don’t really write “bad code” like the old ones did anymore, and this is probably cleaner than what I’d have written myself in a fast build, so I’m not going to pretend I have some perfect standard here. 🤷‍♂️

Time-wise, it took a little over **40 minutes** (which I tracked manually). Most of that time is just the web search.

Token usage came out to **21.6k output tokens**.

If we map that to Opus 4.5 pricing at $25 per 1M output tokens, then 21.6k output tokens cost about **$0.54** for the output part alone.

- **Duration:** ~40+ minutes

- **Token Usage (output):** 21.6k

- **Estimated output cost:** ~$0.54

- **Code Changes:** 6 files changed, 654 insertions(+), 426 deletions(-)

---

## Final Thoughts

Kimi-K2.5 is genuinely a good model, especially with all its agentic capabilities and vision support. It’s roughly **8-9x cheaper** than Opus 4.5, the specs are crazy, and for an open model, it’s way more capable than I expected.

But after using it in this test on a real project, I still prefer Claude Opus 4.5 for actual software work.

Kimi gets far fast (usually, not all the time), but when something breaks, the fix loop can turn into that annoying back-and-forth where one patch breaks something else out of place. Opus just feels more consistent end-to-end.

So yeah, if you care mostly about cost and want a strong open model with a huge context window, Kimi-K2.5 is an easy pick. But if you care about getting working code with the fewest issues, I’d still bet on Opus 4.5.

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/kimi/image_11.png)

Kimi-K2.5 feels a lot better than open models like DeepSeek v1–3, GLM, and even Sonnet 4.5, but I wouldn’t say it’s better than, or even close to, Opus 4.5.

Let me know your thoughts in the comments. 🙌
