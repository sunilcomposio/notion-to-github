Three flagship AI coding models launched within weeks of each other. Claude Opus 4.5 on November 24. Gemini 3.0 Pro on November 18. GPT 5.1 Codex-Max on November 19. All three claim to be the best model for complex coding tasks and agentic workflows.

The benchmarks show they're neck-and-neck. I wanted to see what that means for actual development work. So I gave all three the same prompts for two hard problems in my observability platform: **statistical anomaly detection** and **distributed alert deduplication**. Same codebase, same requirements, same IDE setup.

Benchmarks show the gap between them is razor-thin. But synthetic leaderboards don’t always translate into real engineering outcomes. So instead of relying on charts and marketing collateral, I decided to find out how they perform on real development work.

**The Official Benchmarks **

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/claude/image_1.png)

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/claude/image_2.png)

### Pricing Comparison (Per 1M Tokens)

### Key Benchmark Scores:

- **SWE-bench Verifited:** Opus 4.5 leads at 80.9%, followed by GPT 5.1 Codex-Max at 77.9% and Gemini 3 Pro at 76.2%

- **Terminal-Bench 2.0:** Gemini 3 Pro tops at 54.2%, demonstrating exceptional tool use capabilities

- **MMMU-Pro (Visual Reasoning):** Gemini 3 Pro leads with superior multimodal understanding

- **WebDev Arena:** Gemini 3 Pro reaches 1487 Elo score for "vibe coding" capabilities

### **TL;DR:**

- **Opus 4.5(Claude):** Outstanding at strategy and design, but its solutions tend to be elaborate, slower to integrate, and prone to practical hiccups once they hit the metal.

- **GPT-5.1 Codex:** The most dependable for real-world development, integrates cleanly, handles edge cases, and produces code that holds up under load.

- **Gemini 3 Pro:** Lean, fast, and cost-efficient, strong for prototyping and greenfield builds, though its outputs need some hardening for production-grade resilience.

## How I Tested This

To really see what these models are made of, I threw all three at the **exact same prompts** and asked them to solve two problems that have bitten our production pipeline more than once: building a solid **anomaly detection path**, and hardening **alert deduplication** across multiple processors. These aren’t academic exercises, they’re the kind of tasks where clock drift, concurrency quirks, and partial crashes turn into 3 a.m. pages.

I ran everything in the same **Cursor environment** so no model got special treatment. From there, I wasn’t just watching token counts tick upward, I paid attention to how well each model understood the shape of the system, whether its code actually plugged into the real project, and ultimately whether the output felt like something I could trust in production and ultimately one key question:

**Is this something I would confidently deploy in a real system?**

**Tooling notes:**

- **Claude Code** still delivers the most refined user experience overall, offering structured reasoning traces, step-by-step visibility, and helpful inline feedback during development.

- **GPT Codex CLI (v0.59)** has leveled up significantly, now supporting streamed reasoning, stable session recovery, clearer accounting of cached tokens, and automatic context compaction, making long-running agent loops far more reliable.

- Gemini 3 Pro (via the Node SDK) delivered the fastest completions and lowest cost per task, though the model tends to reveal less of its internal reasoning compared to GPT-5.1 or Claude.

## Test 1: Statistical Anomaly Detection

The challenge: Build a system that learns baseline error rates, uses z-scores and moving averages, catches rate-of-change spikes, and handles 100k+ logs/minute with under 10ms latency.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/claude/image_3.png)

## Opus 4.5 Attempt

**Time:** 12m 11s | **Cost:** $1.28| **Diff:** +**2,981 lines** across 9 files

[https://github.com/VarshithKrishna14/Kompi/commit/aefae4e2a2e6e5befd8365d3f1c730ec564603be](https://github.com/VarshithKrishna14/Kompi/commit/aefae4e2a2e6e5befd8365d3f1c730ec564603be)

Claude delivered a huge implementation: a full statistical anomaly detector with rolling snapshots, Welford-based state tracking, spike detection, serialization logic, configuration structures, and extensive inline comments. On first read, it felt like production-ready engineering.

Then it hit the real system.

The first run immediately exposed a critical failure path. When the historical average approached zero, `calculateSpikeRatio()` produced astronomical values (e.g., `1e12` or worse), which were sent directly into `.toFixed(2)` without passing through the existing sanitization layer. The result: **hard runtime crashes from perfectly valid data**.

State restoration made things even worse. The `deserialize()` function reloaded snapshots but didn’t recompute the means, variance terms, or sample counts. After a restart, the spike detector and z-score logic were working off **statistically incompatible internal state**. No crash, no error, just silent corruption.

But the core design choice, using Welford for a rolling window, undermined the entire system. The output looked sophisticated, but the logic couldn’t behave correctly under real production workload patterns.

## GPT-5.1 Analysis 

Time: 6m | Cost: ~$0.24 | Diff: ~+577 lines for across 3 files modified

[https://github.com/VarshithKrishna14/Kompi/commit/2afad7e6e7940b6204a0ff0d1c51ea1b912a362e](https://github.com/VarshithKrishna14/Kompi/commit/2afad7e6e7940b6204a0ff0d1c51ea1b912a362e)

GPT-5.1 implemented a streaming statistical anomaly detector optimized for extremely high-throughput logging workloads (100k+ events/sec). Instead of heavy time-bucket structures or map-based rolling memory, this design uses a **single-pass O(1) update loop** with:

- **EWMA for online mean**

- **EWMA of squared error-rate for stable variance**

- **Rolling time window for short-term spikes**

- **Hard defenses against NaN, Infinity, invalid counts, and timestamp issues**

Everything executes synchronously with constant-time updates, making it viable in a log ingestion pipeline that runs inside a hot data path.

## **Gemini 3 Pro Attempt**

**Time:** ~5m 44s | **Estimated Cost:** ~$0.14 | **Diff:** +366 lines across 4 files

[https://github.com/VarshithKrishna14/Kompi/commit/1d711ecdec0786bc0265afe3b73e36bab0b6f553](https://github.com/VarshithKrishna14/Kompi/commit/1d711ecdec0786bc0265afe3b73e36bab0b6f553)

Gemini 3 Pro tackled the anomaly-detection problem with a **stream-optimized, low-latency architecture**. The implementation uses a **stateless EWMA model** (Exponentially Weighted Moving Average) rather than sliding windows, ensuring O(1) memory usage regardless of throughput. High-volume logs are aggregated in-memory and flushed to the detector at fixed intervals (e.g., 1s), ensuring the system easily handles 100,000+ logs/minute with negligible overhead.

**Edge cases are rigorously handled:**

- **Zero-Variance**: Explicitly guarded to prevent infinite Z-scores during flatline periods.

- **Math Safety**: Division operations use epsilon guarding (0.000001) to prevent zero-division errors.

- **Invalid Inputs**: NaN and Infinity are gracefully filtered out without crashing the pipeline.

**The test suite is comprehensive and deterministic:**

- Verifies **regime adaptation** (baseline shifting) using fast-adaptation alpha values.

- Tests **rate-of-change spikes** (e.g., 6x jumps) independently of Z-scores.

- Validates initialization convergence and mock database integration points.

## **Round 1 – Quick comparison**

## **Tool Router Integration**

*I*nstead of preloading giant toolkits into MCP and bloating every session with dozens of unused capabilities, the Tool Router acts as an **on-demand integration layer.**

*If you’re curious about the underlying system, the full technical breakdown is documented here:*[*https://composio.dev/blog/introducing-tool-router-(beta)*](https://composio.dev/blog/introducing-tool-router-(beta))

Before running our further tests and alerting workflows, I integrated through Tool Router. One OAuth handshake per user gives the MCP client access to Slack, Jira, PagerDuty, or any other connected tool, no manual wiring required. (For context, we first tried this setup in the Gemini version.)


**The benefits we see in practice:**

- **Seamless per-user integration:** a single router manages many apps, and each session only exposes the tools the user actually connected.

- **Instant, code-free updates:** newly connected services show up automatically, so agents can start using them immediately without redeploys or extra glue code.

- **Automated workflow routing:** alerts and tasks from our anomaly detection pipeline can be sent through Slack, Jira, PagerDuty, or any connected tool effortlessly.

In our processor pipeline, we integrated **Tool Router** to handle alerting dynamically across Slack, Jira, and PagerDuty. Instead of manually wiring each service, we use a unified `ComposioClient` that initializes the MCP client with a single configuration:

```python
const composioClient = new ComposioClient({
  apiKey: process.env.COMPOSIO_API_KEY!,
  userId: 'tracer-system',
  toolkits: ['slack', 'jira', 'pagerduty'],
});

const mcpClient = await composioClient.createMCPClient();
```


Once initialized, any alerting workflow can call agents directly. For example, our anomaly detection triggers the `log-anomaly-alert-agent`, which automatically decides which tools to notify.

## **Test 2: Distributed Alert Deduplication**

The challenge:  Implement distributed alert deduplication so multiple processors don’t fire duplicate alerts within a 5-second window, tolerating up to 3s clock skew and processor crash

### **Opus 4.5 Take**

**Time:** ~7 m 1 s (estimate) | **Cost:** ~$0.48 | **+715 lines across 4 files**

[https://github.com/VarshithKrishna14/Kompi/commit/0bb9ad721afe00f74f01f67409551a9ff0b256a0](https://github.com/VarshithKrishna14/Kompi/commit/0bb9ad721afe00f74f01f67409551a9ff0b256a0)

Opus 4.5 uses a **three-layer deduplication architecture**:

- **L1 cache** for fast in-memory rejection of recent alerts

- **L2 advisory-locks + explicit DB query** to coordinate across processors

- **L3 unique constraints on the alert table** to enforce deduplication at the database level

Clock skew is addressed by relying on the database’s `NOW()` timestamp rather than local processor clocks. The use of PostgreSQL advisory locks ensures that if a processor crashes while holding a lock, the lock is released automatically when the connection closes.

The test suite is well-sized (~493 lines) and covers cache hit/miss behavior, concurrent lock acquisition, clock skew edge cases, and simulated processor crashes.

**Where it falls short:**

- The L1 cache uses `Math.abs(ageMs)` to evaluate recency, but this fails to account for processor clock skew. While the L2 lock layer catches most cases, this failure in L1 means the fast path is unreliable under skewed clocks.

- The advisory lock key is derived only from `service:alertType` (no timestamp or recent history), which can cause excessive serialisation (different distinct alerts colliding unnecessarily).

- The unique constraint (L3) blocks *all* duplicate active alerts rather than only duplicates within the 5-second window; this may suppress legitimate, slightly delayed alerts beyond that timeframe.

Overall: **A sophisticated architecture with solid multi-tier deduplication strategy, ** but still a prototype rather than a fully hardened production module.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/claude/image_4.png)

## **GPT-5.1’s Take**

**Time:** ~4m | **Cost:** ~$0.27 | **+188 net lines across 2 files**

[https://github.com/VarshithKrishna14/Kompi/commit/406d262d9795d0a5e74582bb28a9215d609e4a4f](https://github.com/VarshithKrishna14/Kompi/commit/406d262d9795d0a5e74582bb28a9215d609e4a4f)

GPT-5.1 implemented distributed alert deduplication with a **clean, production-oriented architecture** built around a simple rule:

> Only one processor should emit an alert for the same anomaly within a 5-second window, even with multiple nodes, clock skew, or crashes.

### **Architecture**

GPT-5.1 introduced a new `alert-dedup.ts` module containing:

- **AlertDeduplicator interface**

- **DedupKeyValueStore abstraction**

- **KeyValueStoreAlertDeduplicator**

- **InMemoryDedupKeyValueStore**

## **Gemini 3 Pro’s Take**

**Time:** 4m 02s | **Cost:** ~$0.11 | **+103 lines across 2 files**

[https://github.com/VarshithKrishna14/Kompi/commit/4090823d34006500fed0e052c99af530076abef6](https://github.com/VarshithKrishna14/Kompi/commit/4090823d34006500fed0e052c99af530076abef6)

Gemini 3 Pro implemented distributed alert deduplication directly inside the main processing path, making it the only version so far that is **fully wired into the LogProcessor without additional scaffolding**.

### **Architecture**

Gemini introduced a centralized deduplication strategy based on PostgreSQL:

- **AlertDeduplicator interface** (`deduplicator.ts`)

- **PostgresDeduplicator**

- **InMemoryDeduplicator**

# **Round 2 Quick Compare**

## The Cost

Total spend across both tests:

- **Opus 4.5:** **$1.76**

- **GPT-5.1 Codex:** **$0.51** *(~71% cheaper than Opus)*

- **Gemini 3 Pro:** **$0.25** *(~86% cheaper than Opus)*

Opus was consistently the most expensive. The reason is straightforward: it generated **far more code**, ran longer chains of reasoning, and produced large comment blocks and support scaffolding that never landed in production. GPT-5.1 was far leaner,  it solved the same problem in less than half the tokens and with far less backtracking. Gemini came in even cheaper, largely because it produced compact implementations with fewer files changed.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/claude/image_5.png)

## **What I Actually Learned**

Across both challenges, three very different personalities showed up:

### **Opus 4.5**

Opus generated the most **ambitious, elaborate engineering** every time. Rolling statistics, advisory lock stacks, structured configuration, test coverage, comments, serialization logic, you could mistake the output for a whitepaper implementation.

But once plugged into a running system, hidden failure modes surfaced immediately:

- Stateful calculations that didn’t restore correctly

- Edge paths that threw runtime errors

- Logic that was architecturally elegant but not operationally safe

It **thinks at design-document scale**, but it requires another engineering pass to harden for production.

### **GPT-5.1 Codex**

GPT-5.1 was the opposite, smaller changes, far more focused, and always wired directly into the running codebase.

This model:

- Solved the problem in the fewest lines of code

- Handled error cases proactively

- Took real deployments into account (crashes, skew, dirty input)

- Integrated immediately into the live processor code

Not the most beautiful architecture, but consistently the most **deployable** and far cheaper to run.

### **Gemini 3 Pro**

Gemini landed in the middle, creative, compact, and technically solid. It implemented solutions that were:

- Fast

- Easy to reason about

- Minimal in moving parts

- Cheap in compute

It didn't produce architecture essays, but its solutions were easier to drop straight into a project, especially the dedup logic, which required almost no scaffolding. The downside is that some of its deeper edge cases had to be checked manually, its tests weren’t as exhaustive as the other two.

# **Why GPT-5.1 Stands Out**

For production engineering, GPT-5.1 hit the sweet spot:

- **Minimal rewrite required**

- **Zero critical bugs in either test**

- **Fast iteration**

- **One-pass integration**

It wasn’t the cheapest (Gemini was), but GPT-5.1 produced the most performable-ready code per dollar.

If you want a model that:

- Writes code that compiles,

- Runs on the first try,

- Handles operational edge cases,

- And fits into an existing codebase…

GPT-5.1 is the practical winner.

---

# **When to Use Opus 4.5**

Opus is the model to call when you need **deep architectural reasoning**:

- System design reviews

- Technical write-ups

- Planning modules or frameworks

- Long-term maintainability discussions

It produces more infrastructure than is needed, but that’s because it’s thinking **like a platform architect rather than a service engineer**. If you have the time to refine and integrate, Opus gives you the widest view of the problem space.

Just expect to:

- Wire things together manually

- Fix runtime behavior

- Trim unnecessary complexity

---

# **When to Use Gemini 3 Pro**

Gemini is the **fastest and cheapest path to working code**. It’s ideal when:

- You want to move quickly

- You’re okay filling in the deep testing yourself

- Simplicity matters more than formal architecture

Its solutions tend to be:

- Straightforward

- Operationally efficient

- Very easy to deploy

Just be ready to manually audit some boundary conditions, it doesn’t always anticipate the same number of failure modes as GPT-5.1.

---

# **Bottom Line**

Across these practical engineering scenarios, GPT-5.1 Codex stood out by producing solutions that were closest to “ready to deploy” with the least amount of intervention. Claude 4.5 consistently demonstrated the strongest architectural reasoning and long-horizon thinking, but its outputs usually required additional effort to integrate and stabilize. Gemini 3 Pro delivered fast, lightweight, and inexpensive solutions that worked well early but benefited from hardening when pushed into more demanding or distributed environments.

In other words:

- **Codex most often produced code that could be dropped straight into the system**,

- **Claude provided the deepest engineering thinking**, and

- **Gemini offered the quickest path to functional scaffolding at low cost**.

Complete code is available on GitHub if you want to examine the implementations. Fair warning: it's an evaluation harness I built for this test, not production code.

These results reflect only what I observed in these particular test cases, but they highlight the practical trade-offs engineers may see when using the models in day-to-day development.

`shouldEmit(fingerprint, dedupWindowMs)` → returns whether this processor is allowed to emit the alert.

Defines a shared atomic operation:

`setIfNotExistsWithTTL(key, ttlMs) `the backend (Redis, DynamoDB, etc.) ensures only one processor can create a key during the TTL window.

Production implementation that performs distributed coordination using the above atomic write.

Lightweight mock version used for local runs and unit tests.

Exposes `shouldAlert(alertKey, windowSeconds)` to decide whether a processor is allowed to emit an alert.

Uses an `INSERT ... ON CONFLICT` pattern to guarantee single-winner semantics.

The logic attempts to update the row only if `last_fired_at` is older than the dedup window, and returns whether the update succeeded.

A local fallback for environments without a database, matching the same interface for drop-in testing.
