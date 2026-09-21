# Lesson 3 — Stop paying for ceremony
**⏱ 3 minutes · Tool: output shaping (e.g., Headroom's output shaper)**

## Output tokens are the expensive ones

You pay for input once (well, per turn — see Lesson 0) — but **output tokens typically cost several times more than input tokens** on frontier models. And a lot of output is ceremony:

- The *"Great, let me now..."* preamble
- Your own file printed straight back at you as a "summary"
- Deep extended thinking spent on reading one config file

## What output shaping does

Headroom's output shaper (free, off by default) does two things:

1. **Verbosity steering** — appends a short "be terse" note to the *end* of the system prompt. End-placement matters: your prompt cache still hits because the cached prefix is unchanged.
2. **Effort routing** — dials thinking effort down when the model is merely resuming after a tool result. New questions and real errors keep full effort.

```bash
export HEADROOM_OUTPUT_SHAPER=1   # read live on every request — no restart needed
headroom proxy --port 8787
```

Measured honestly: the project holds out 10% of conversations as an unshaped control (`HEADROOM_OUTPUT_HOLDOUT=0.1`), so the ~**31% output savings** figure in their dashboard is measured, not estimated (95% CI: 27.7–35.7%).

## The zero-tool version: habits

If you don't want another proxy, put this in your agent's instructions file (CLAUDE.md / `.cursorrules` / AGENTS.md):

> - Don't repeat file contents back to me; summarize changes as diffs.
> - Skip preamble and closing summaries.
> - Prefer editing existing code over rewriting files.
> - Don't restate the task before solving it.

This alone trims a meaningful slice of output — and the model follows it surprisingly well.

✅ **Check yourself:** Why append "be terse" at the *end* of the system prompt instead of rewriting it?

<details><summary>Answer</summary>
Providers cache the prompt prefix. Appending at the end leaves the cached prefix byte-identical, so you keep the prompt-cache discount while still steering behavior.
</details>

→ Next: [Lesson 4 — Stop explaining your project twice](04-memory.md)
