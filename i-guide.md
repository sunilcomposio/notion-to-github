Three new AI coding models dropped in the past two months. Claude Sonnet 4.5 with extended thinking on September 29. GPT-5 Codex with unified reasoning on September 23. Kimi K2 Thinking with 1T parameters on November 6-7. All three claim to handle complex coding tasks better than anything before them.

The benchmarks say they're close. I wanted to see what that means for actual development work. So I gave all three the same prompts for two hard problems in my observability platform: statistical anomaly detection and distributed alert deduplication. Same codebase, same requirements, same IDE setup.

Full code's on [github.com/rohittcodes/tracer](http://github.com/rohittcodes/tracer) if you want to dig in. Fair warning: it's an evaluation harness I built for this, not a polished product. Expect rough edges.

## TL;DR

**Test 1 - Advanced Anomaly Detection:** GPT-5 Codex was the only one that shipped working code. Claude and Kimi both had critical bugs that would crash in production.

**Test 2 - Distributed Alert Deduplication:** Codex won again with actual integration. Claude had solid architecture, but didn't wire it up. Kimi had clever ideas but a broken duplicate-detection logic.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/i/image_1.png)

**The kicker:** Codex cost me $0.95 total vs Claude's $1.68. That's 43% cheaper for code that actually works.

## The Official Benchmarks (For What They're Worth)

**Pricing:**

- Claude: $3/M input, $15/M output

- GPT-5: $1.25/M input, $10/M output

- Kimi: $0.60/M input, $2.50/M output

## How I Tested This

I gave all three models identical prompts for two hard problems in an observability platform: statistical anomaly detection and distributed alert deduplication. Not toy problems, the kind of stuff that needs deep reasoning about edge cases and system architecture.

I set up everything in Cursor IDE, and tracked token usage, time, code quality, and whether it actually integrated with the existing codebase. That last part turned out to matter way more than I expected.

**Quick note on the tooling:** Codex CLI has gotten way better since I last used it. Streams reasoning, resumes sessions reliably, and shows you cached token usage. Claude Code is still the most polished, with inline critiques, replayable steps, and clean thinking traces. Kimi CLI feels early. No easy way to see the model's reasoning, context fills up faster, and cost tracking is basically non-existent (just a dashboard number). Made iteration painful.

## Test 1: Statistical Anomaly Detection

The challenge: Build a system that learns baseline error rates, uses z-scores and moving averages, catches rate-of-change spikes, and handles 100k+ logs/minute with under 10ms latency.

### Claude's Attempt

Time: 11m 23s | Cost: $1.20 | +3,178 lines across 7 files

[Commit 05dbf00](https://github.com/rohittcodes/tracer/commit/05dbf00f67639c202f72dfbd46cbfd7aa0e8cc31)

Claude went all-in. Statistical detector with z-score, EWMA, and rate-of-change checks. Extensive docs. Synthetic benchmarks. It looked impressive.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/i/image_2.png)

Then I actually ran it.

The `calculateRateOfChange()` function returns `Infinity` when the previous window is zero, and the alert formatter calls `toFixed()` on it. Instant `RangeError` crash. The baseline isn't actually rolling; the circular buffer drops old samples, but `RunningStats` keeps everything, so it can't adapt to regime changes. Unit tests use `Math.random()`, making the whole suite non-deterministic. Oh, and none of this is wired into the actual processor pipeline.

Cool prototype. Completely broken for production.

### GPT-5 Codex's Attempt

Tokens: 86,714 input (+ 1.5M cached) / 40,805 output (29,056 reasoning)

Time: 18m | Cost: $0.35 | +157 net lines across 4 files

[Commit 878f313](https://github.com/rohittcodes/tracer/commit/878f31336665cdfb7e99e1934841d975b20d7c13)

Codex actually integrated it. Modified the existing `AnomalyDetector` class, wired it into `index.ts`. It runs in production immediately.

The edge case handling is solid, checks for `Number.POSITIVE_INFINITY` and uses a descriptive string instead of crashing on `toFixed()`. The baseline is truly rolling with circular buffers and incremental statistics (sum, sum-of-squares) that update in O(1). Time buckets align on wall-clock boundaries for predictability. Tests are deterministic with controlled bucket emissions.

There are trade-offs. The bucket approach is simpler but slightly less flexible than circular buffers. It extended the existing class instead of creating a new one, which couples statistical detection to threshold logic. Documentation is minimal compared to Claude's novel-length bundle.

But here's the thing: this code ships. Right now. As-is.

### Kimi's Attempt

Time: ~20m | Cost: ~$0.25 (estimated) | +2,800 lines

[Commit ed72f3f](https://github.com/rohittcodes/tracer/commit/ed72f3f2a7e61c2c0e42878072c383928c9268fd)

Kimi tried to support both streaming logs and batch metrics. Added MAD and EMA-based detection. Ambitious.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/i/image_3.png)

The fundamentals are broken, though. It updates the baseline before checking the new value, making the z-score effectively zero. Real anomalies never fire. There's a TypeScript compilation error: `DEFAULT_METRIC_WINDOW_SECONDS` used before declaration. Rate-of-change divided by `previousValue` without checking for zero, same `Infinity` crash as Claude. Tests reuse the same log object in tight loops, never seeing realistic patterns. Nothing's integrated.

This doesn't even compile.

### Round 1 Quick Compare

Codex pulled ahead because it was the only one that shipped working, integrated code.

## Tool Router Integration

> I wanted to dogfood Tool router which is in beta and basically allows you to add any Composio apps and it can load tools from appropriate toolkits only when needed based on task context. Reducing you MCP context bloat by a mile. You can read here.

Before kicking off Test 2, I integrated everything through our tool router MCP that we ship with Tracer. Quick refresher on why I bother with it: Tool Router exposes all of a user's connected apps as ready-to-call tools for any agent. One OAuth handshake per user, and the AI SDK gets a unified surface instead of me hand-wiring Slack, Jira, PagerDuty, and whatever comes next.

What that buys me in practice:

- **Unified access with per-user auth:** one router for 500+ apps, and each session only sees the integrations that the user actually connected.

- **No redeploys, SDK-native:** new connections show up instantly with proper params/schemas, so agents can call them without glue code.

(Also: this is the exact service backing Rube MCP on the backend.) The helper that spins it up lives in `packages/ai/src/composio-client.ts`:

```typescript
export class ComposioClient {
  constructor(config: ToolRouterConfig) {
    this.apiKey = config.apiKey;
    this.userId = config.userId || 'tracer-system';
    this.toolkits = config.toolkits || ['slack', 'gmail'];

    this.composio = new Composio({
      apiKey: this.apiKey,
      provider: new OpenAIAgentsProvider(),
    }) as any;
  }

  async createMCPClient() {
    const session = await this.getSession();

    return await experimental_createMCPClient({
      transport: {
        type: 'http',
        url: session.mcpUrl,
        headers: session.sessionId
          ? { 'X-Session-Id': session.sessionId }
          : undefined,
      },
    });
  }
}
```

With that in place, any LLM can drop the same Slack/Jira/PagerDuty hooks without me juggling tokens. Swap the toolkit list and any agent, or even an internal automation, get the same stabilized catalog.

## Test 2: Distributed Alert Deduplication

The challenge: Fix race conditions when multiple processors detect the same anomaly. Handle ≤3s clock skew and processor crashes. Prevent duplicate alerts when processors fire within 5 seconds of each other.

### Claude's Take

Time: 7m 1s | Cost: $0.48 | +1,439 lines across 4 files

[Commit a7cac5c](https://github.com/rohittcodes/tracer/commit/a7cac5c8c034097c32815fc8645f59c0bf67bc8f)

Claude designed a three-layer architecture: L1 cache, L2 advisory locks + DB query, L3 unique constraints. Handles clock skew with database `NOW()` instead of processor timestamps. PostgreSQL advisory locks auto-release on connection close, handling crashes gracefully. The test suite is 493 lines covering cache hits, lock contention, clock skew, and crashes.

Same problem as test 1: not integrated into `apps/processor/src/index.ts`. The L1 cache uses `Math.abs(ageMs)`, which doesn't account for clock skew (though L2 catches it). Advisory lock key is `service:alertType` without a timestamp, causing unnecessary serialization. The unique constraint blocks *all* duplicate active alerts, not just within the 5-second window.

Great architecture. Still just a prototype.

### GPT-5's Take

Tokens: 44,563 input (+ 1.99M cached) / 39,792 output (30,464 reasoning)

Time: ~20m | Cost: $0.60 | +166 net lines across 6 files

[Commit 6d9cf3b](https://github.com/rohittcodes/tracer/commit/6d9cf3b297e3e9957adae7d0d5f597e5534fc7c9)

Codex integrated it. Modified the existing `processAlert` function and wired in deduplication. Uses a reservation-based approach with a dedicated `alert_dedupe` table with expiration, simpler than advisory locks, and easier to reason about. Transaction-based coordination with `FOR UPDATE` locks for serialization. Handles clock skew with database `NOW()`. Crashes are handled through a transaction rollback that clears reservations automatically.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/i/image_4.png)

There's a minor race condition in the `ON CONFLICT` clause where both processors can pass the `WHERE` check before either commits. No background cleanup for expired `alert_dedupe` entries (though stale entries get cleaned up on each insert). The dedupe key includes `projectId`, treating the same service+type across projects as different; it might be intentional, but worth noting.

Production-ready except for that small ON CONFLICT fix.

### Kimi's Take

Time: ~20m | Cost: ~$0.25 (estimated) | +185 net lines across 7 files

[Commit 31aa8f5](https://github.com/rohittcodes/tracer/commit/31aa8f5465495bdb601a775ce095cb1200e548ef)

Kimi actually integrated this one. Modified `processAlert` and wired in deduplication. Uses discrete 5-second time buckets, simpler than a reservation table. Atomic upsert with database-native `ON CONFLICT DO UPDATE` to handle races. Implements exponential backoff retry logic.

Critical bugs, though. Duplicate detection compares `createdAt` timestamps, which are identical for simultaneous inserts, and returns the wrong `isDuplicate` flag. The retry logic calculates a new bucket but never uses it, passes the same timestamp, and hits the same conflict again. The severity update SQL is unnecessarily complex.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/i/image_5.png)

Good approach, broken execution.

### Round 2 Quick Compare

Codex won again with cleaner integration and fewer showstopper bugs.

## The Money

**Total across both tests:**

- Claude: $1.68

- GPT-5 Codex: $0.95 (43% cheaper)

- Kimi: ~$0.51 (estimated from aggregate)

Codex is cheaper despite using more tokens. Claude's extended thinking and higher output costs ($15/M vs $10/M) kill you. Codex's cached reads (1.5M+ tokens) bring costs way down. Kimi's CLI only shows aggregate project spend, so I had to estimate per-test costs.

## What I Actually Learned

Codex won both tests by shipping production-ready code with the fewest critical bugs. Claude made better architectures, Kimi had clever ideas, but Codex was the only one consistently delivering working code.

**Why Codex won:**

- Actually integrates code instead of creating parallel prototypes

- Catches edge cases everyone else misses (that `Infinity.toFixed()` bug bit both Claude and Kimi)

- Both implementations are production-ready

- 43% cheaper than Claude

**The downsides:**

- Less comprehensive documentation than Claude

- Minor ON CONFLICT race in test 2

- Takes longer (18-20m vs Claude's 7-11m), but worth it for code that works

### When to Use Claude Sonnet 4.5

Best for architecture design and documentation. The thinking is genuinely excellent; the three-layer defense in test 2 shows real distributed systems understanding. Documentation is thorough (7 files for test 1). Fast execution at 7-11 minutes. The extended thinking mode with self-reflection produces well-reasoned solutions.

But it doesn't integrate anything. You get prototypes that need serious wiring. Critical bugs in both tests. More expensive at $1.68. Over-engineered (3,178 lines vs Codex's 157 net).

**Use it when:** You want a thoughtful architecture review or documentation pass, and you're okay spending time wiring it in and fixing bugs.

### When to Use Kimi K2 Thinking

Best for creative solutions and alternative approaches. Time buckets in test 2, MAD/EMA attempts in test 1 show creative thinking. Actually integrates code like Codex. Good test coverage. Probably the cheapest (though CLI doesn't expose usage).

But there are critical bugs in core logic everywhere. Broken duplicate detection and retry in test 2. Baseline update order issues in test 1. CLI limitations (no cost visibility, context fills fast). Fundamental logic errors prevent the code from working.

**Use it when:** You want creative ideation and can afford to refactor the output. Budget extra time to harden everything.

## Bottom Line

I'm shipping production work with GPT-5 Codex. It delivers integrated code that handles edge cases, costs 43% less than Claude, and needs minimal polish. Claude's my go-to for architecture reviews or documentation, even though I know I'll spend time wiring it in and chasing bugs. Kimi's the wild card, creative and cheap, but the logic bugs mean I budget serious refactoring time.

The real insight? All three models generate impressive-looking code. But only Codex consistently ships. Claude designs better but doesn't integrate. Kimi has clever ideas but introduces showstoppers. For real-world development where you need working code fast, Codex is the practical choice.
