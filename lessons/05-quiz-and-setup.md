# Lesson 5 — Final quiz + your 20-minute setup
**⏱ 3 minutes**

## Quiz (5 questions)

**1.** Rank these by typical share of a session's token spend (highest first): (a) your typed prompts, (b) tool results kept in context, (c) model output.

**2.** Headroom's headline claim is "60–95% fewer tokens." For a normal coding-agent session, what number should you actually expect?

**3.** Why is verbose *output* often more expensive per token than input?

**4.** Your teammate installs a token-compression proxy. What's the first thing to check before trusting it on a work machine?

**5.** Your CLAUDE.md has grown to 4,000 tokens of accumulated notes. What's the problem, and the fix?

<details><summary><b>Answers</b></summary>

1. **(b) tool results > (c) model output > (a) your prompts.** Tool results dominate (~40%), then output (~20%), then your prompts (~12%) — which is why "write shorter prompts" advice misses the point.
2. **~20%** for general coding-agent workloads (up to ~40% on exploration-heavy sessions). The 60–95% figures apply to repetitive JSON/structured outputs.
3. Frontier models price output tokens at a multiple of input tokens — so preamble, echo-back, and unnecessary thinking are the most expensive words in the session.
4. **Telemetry / what data leaves your machine — and how to turn it off** (e.g., `HEADROOM_BEACON=off` / `DO_NOT_TRACK=1`). Prefer local-only tools for sensitive code.
5. It's re-sent in every turn of every session — constant overhead (Lesson 0's re-read report, except you wrote it). Trim to essentials, move history into dated decision logs, use path-scoped rules so context loads only when relevant.
</details>

**Scoring:** 5/5 — you're ready to run a lean setup. 3–4 — skim the lessons you missed. <3 — re-read Lesson 0; the mental model carries everything else.

---

## Your 20-minute setup checklist

Do these in order — total time ≈ 20 minutes:

| ⏱ | Do | Effect |
|---|---|---|
| 3 min | Write a minimal `CLAUDE.md` / `AGENTS.md` (project, conventions, decisions) | Kills the re-explanation leak |
| 2 min | Add the anti-ceremony rules (no echo-back, no preamble, prefer diffs) | Trims expensive output |
| 2 min | Add "prefer line ranges / symbol lookups over whole files" | Shrinks exploration cost |
| 5 min | `pip install "headroom-ai[all]"` && `headroom wrap claude` (+ `HEADROOM_BEACON=off` if needed) | Compresses everything else |
| 3 min | Run one real task through the wrapped agent; check `headroom savings` | Baseline number |
| 5 min | Revisit in a few days — is the savings number climbing? Tune from there | Loop closed 🎉 |

## Where to go next (200-level ideas)

- **Measure before optimizing:** `ccusage` (a free tool) reads the usage diary Claude Code already keeps on your laptop and turns it into a bill — spend per session, per day, per model. See where the money goes before changing anything.
- **Model routing:** use the big model for design, a small one for mechanical edits.
- **Read the source:** the [Headroom repo](https://github.com/headroomlabs-ai/headroom) documents its compressor pipeline (CacheAligner → ContentRouter → compressor → CCR) — a great case study in what actually compresses well.
