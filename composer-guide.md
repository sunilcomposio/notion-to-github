The AI coding race is heating up again. After OpenAI, Anthropic, and Google, Cursor has stepped into the game with its new model, Composer 1, a coding-focused agent model that’s said to be 4x faster than other models with similar intelligence. 🤨

It’s said to output code at lightning speed, reason through large contexts, and even outperform models like GPT-5 and Claude Sonnet in engineering workflows.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/composer/image_1.png)

That’s a bold claim, so I decided to test it myself. In this post, we’ll see how Composer 1 performs when building an actual agent and to make things fair, I’ll put it head-to-head with Claude Sonnet 4.5, one of the most consistent coding models out there.

---

## TL;DR

If you just want the results, here’s a quick rundown of how both models performed in building a simple agent:

- **Composer 1** produced the most complete implementation and had the fastest output. It coded the entire agent in under 3 minutes, though it needed two small follow-ups.

- **Claude Sonnet 4.5** also got the job done, but sometimes used outdated API methods, even though I clearly provided the latest documentation for a Python package. It tends to rely more on its training data than the instructions you give it.

In terms of code quality and implementation, there’s not much difference. That said, Sonnet 4.5 burned almost twice the tokens, while Composer 1 delivered similar results in half the time (not 4x) with far fewer tokens. It’s efficient, fast, and feels like a strong pick for everyday coding.

---

## Brief on Cursor Composer

Since this model dropped only a few weeks ago, here’s a short refresher.

Composer 1 is the first agent-focused coding model from Cursor. They claim it’s about **4x faster** than similarly intelligent models.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/composer/image_2.png)

(By the way, what even is the **Cursor Bench Score**? Can't really trust all the metrics blindly. 🤷‍♂️)

It is said to be a mixture-of-experts (MoE) language model supporting long context generation and understanding. As mentioned earlier, it's built especially for building agents or general software engineering workflows in mind through Reinforcement Learning (RL).

They've positioned it as a frontier coding model, with top-tier intelligence that seems to surpass all its peer models with similar intelligence. It comes with a **250 tokens per second** output speed, which is roughly twice as fast as most coding models and about 4–5 times faster than some reasoning models.

If we talk about the pricing, it's priced the same as GPT-5, which is **$1.25** per million input and **$10 **per million output tokens, which is pretty affordable for what it promises.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/composer/image_3.png)

It is clearly aimed at replacing models like GPT-5 and Claude Sonnet 4.5 for software development. Although it is said that these models beat Composer 1 in pure coding intelligence, they run much slower.

So, we can say that Composer comes with a bit of an accuracy trade-off for a **lot** of speed gain, which could be a great option for some but not for everyone.

But all of these claims come from Cursor's own benchmark. It's up to you whether you decide to trust it or not. 🤷‍♂️

---

## Coding Comparison

Alright, enough talk. Let’s see how Composer 1 stacks up against Sonnet 4.5 in actual coding.

Since Composer is pitched as an “agentic” model, I wanted to see how well it could handle building an AI agent from scratch.

To build the agent, we will be using Composio's [Tool Router (Beta)](https://docs.composio.dev/docs/tool-router/quick-start), which automatically discovers, authenticates, and executes the right tool for any task without you having to manage authentication and wire everything per integration. 🔥

> 💡**Fun Fact:** Tool Router is what powers complex agentic products like [Rube](https://rube.app/).

We’ll compare code quality, token usage, cost, and time to complete the build.

### What's the plan?

I asked both models to build a small Python agent that takes a YouTube URL, finds the interesting parts of the video, and posts a Twitter thread on behalf of the user.

> **💁 Prompt:** "Create an AI Agent in Python when given a YouTube URL, The agent will find the interesting part of the video and post a tweeter thread on behalf of the user. For this, use the Composio's Tool Router: https://docs.composio.dev/docs/tool-router/quick-start. Note: Don’t use Composio’s YouTube Integration, build a custom tool on your own using the YouTube Transcript API (to make things a little harder)."

### Response from Composer 1

You can find the entire source code here: [Composer 1 AI Agent with Tool Router](https://gist.github.com/shricodev/aada89ce44f833a62fd41368d770b4c7)

Here's the output of the program:

Composer was really fast with the response. Took a little over 3 minutes to code the first response, it did run into a small issue with the implementation of the YouTube transcript function, and adding it as a custom tool to the agent.

It did also run into usage of some types and functions from the modules, but nothing much severe here.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/composer/image_4.png)

But after a few back and forth in two more prompts with a little help from my side, it was able to make all of it work.

In the overall, Composio part working with the Tool Router, it didn't really run into any problems.

Token usage was around **200K** tokens. For the timing, the first response took roughly 3 minutes, but on the follow-ups, it was negligible.

It was not asked, but it did quite a good job with the code quality and the overall user interface of this CLI chat.

> 💁 One thing I found a bit irritating is the number of comments it writes; there's a comment for every single line, which is insane! For many, it could be great, but this definitely feels a bit too much.

### Response from Sonnet 4.5

You can find the entire source code here: [Claude Sonnet 4.5 AI Agent with Tool Router](https://gist.github.com/shricodev/30b9218148df4bc35080336824f85b5e)

Here's the output of the program:

Sonnet kinda disappointed me here. The code quality was fine, yet it repeatedly used the old YouTube Transcript API methods (`get_transcript`, `list_transcripts`) even after being shown the newer version.

I eventually had to fix that part myself. And for some reason, whenever I asked for a small change, Sonnet rewrote half the working code, which felt unnecessary and ate up tokens like crazy.

Its total token usage was nearly double that of Composer’s, around **427K tokens**, and it took between about 10 minutes to finish the job.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/composer/image_5.png)

To be fair, its implementation of the Tool Router itself was solid but quite a bit slower and heavier overall.

### Summary

To summarize the results, there's not much difference I could find in the code quality or the implementation, both were able to read the Tool Router documentation and implement it well. But for some reason, it was getting tough to make Sonnet use the new API than what it's trained on.

Token usage and the time they took to implement are just **not comparable**. Claude used 427K tokens, which is **2.1 times** the tokens used by Composer 1, which is roughly 200K. In terms of time, there's a significant difference, and I also had to do many more follow-ups for this compared to Composer.

---

## Wrap Up!

This was a really quick test. You could ask it to add more features to the agent, and test both the model results, but I leave it to you. But even from this small run, Composer 1 stands out. ⚡

With more than half the time and far fewer tokens, it matched or even slightly outperformed Sonnet 4.5 in overall coding quality.

From my experience using it, I don’t think you’ll run into any major issues choosing Composer over Sonnet for everyday development. It’s fast, consistent, and honestly feels built for this exact kind of work.

Would love to hear if anyone else has benchmarked these models with cool real world projects. ✌️
