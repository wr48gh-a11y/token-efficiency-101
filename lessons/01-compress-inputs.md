# Lesson 1 — Stop sending junk to the model
**⏱ 4 minutes · Tool: [Headroom](https://github.com/headroomlabs-ai/headroom)**

## The idea

Headroom is a free, open-source proxy that sits **between your agent and the model** and compresses what flows through: tool outputs, logs, file reads, RAG chunks, even conversation history. Compression runs **locally on your machine** — nothing is shipped to a third party.

The clever part: it's **reversible compression**. Originals are cached locally, and the model can pull the full text back when it genuinely needs it. Compressed view first, full fidelity on demand.

## What the benchmarks actually say

Measured with the real tokenizer ([reproducible in the repo](https://github.com/headroomlabs-ai/headroom)):

| Scenario | Before | After | Saved |
|---|---|---|---|
| Code search (100 results) | 17,199 | 13,597 | **21%** |
| SRE incident debugging | 55,957 | 24,340 | **57%** |
| Codebase exploration | 58,801 | 33,895 | **42%** |
| GitHub issue triage | 46,067 | 32,429 | **30%** |

Tagline: *"20% fewer tokens for coding agents, 60–95% fewer tokens for JSON, same answers."* Note the honesty — coding agents get ~20%, not the headline 95%.

## Hands-on (2 minutes)

```bash
pip install "headroom-ai[all]"

# Option A: wrap your agent (starts proxy + configures Claude Code)
headroom wrap claude

# Option B: run as a plain proxy and point any OpenAI-compatible client at it
headroom proxy --port 8787
```

Then run your agent through the wrapper instead of plain `claude`. After a few days, `headroom savings` shows your running total. Undo anytime with:

```bash
headroom unwrap claude
```

## Know before you install

- **Telemetry:** an anonymous beacon is ON by default (sends compression ratios + model IDs, never code). Turn it off: `HEADROOM_BEACON=off`, or `DO_NOT_TRACK=1`, or run with `--offline`.
- **Expectation setting:** compression scales with payload repetitiveness. Repeated JSON and log lines get crushed; plain prose barely moves.
- **Latency is negligible:** ~0.2 ms on a 10K-token JSON payload — far below network time.
- **Bonus — output trimming** (off by default):
  ```bash
  export HEADROOM_OUTPUT_SHAPER=1
  ```
  (Details in Lesson 3.)

✅ **Check yourself:** Why does a compression proxy not break your provider's prompt caching?

<details><summary>Answer</summary>
It compresses only newly added bytes, leaving the earlier prefix untouched — so the provider's KV-cache prefix still matches and the cache discount still applies.
</details>

→ Next: [Lesson 2 — Stop reading the whole file](02-smart-context.md)
