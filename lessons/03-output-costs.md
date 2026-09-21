# Lesson 3 — Stop paying for ceremony
**⏱ 3 minutes · Tool: output shaping (e.g., Headroom's output shaper)**

You pay for input once per turn — but **output tokens typically cost several times more** than input on frontier models. And a lot of output is pure ceremony:

- The *"Great, let me now..."* preamble
- Your own file printed straight back at you as a "summary"
- Deep extended thinking spent on reading one config file

## Decoder for this lesson

| Term | Plain English |
|---|---|
| **system prompt** | The standing instruction sheet the agent reads before every conversation — "you are a coding assistant, do X, never Y." You usually don't see it, but you pay for it every turn. |
| **thinking tokens** | Newer models can do silent scratch-work before answering ("let me consider…") — visible in pricing as extra output tokens. Useful for hard problems; overkill for reading a config file. |
| **prompt cache** | The AI company's loyalty discount: text identical to what you sent before is re-charged at a steep discount. Anything that changes your earlier text *voids the discount* for it. |

## What output shaping does

Headroom's output shaper (free, off by default) does two things:

1. **Verbosity steering** — appends a short "be terse" note to the *end* of the system prompt. End-placement matters: the earlier text stays untouched, so the prompt-cache loyalty discount still applies.
2. **Effort routing** — dials thinking down when the model is merely resuming after a tool result. New questions and real errors keep full effort.

```bash
export HEADROOM_OUTPUT_SHAPER=1   # read live — no restart needed
headroom proxy --port 8787
```

Measured honestly: the project holds out 10% of conversations without shaping, as a comparison group — like a drug trial's placebo group — so the ~**31% output savings** figure is measured against a real control, not estimated (margin of error roughly ±4%).

## The zero-tool version: habits

If you don't want another proxy, put this in your agent's instruction file (CLAUDE.md / `.cursorrules` / AGENTS.md — see Lesson 4):

```text
- Don't repeat file contents back to me; summarize changes as diffs.
- Skip preamble and closing summaries.
- Prefer editing existing code over rewriting files.
- Don't restate the task before solving it.
```

This alone trims a meaningful slice of output — and the model follows it surprisingly well.

✅ **Check yourself:** Why append "be terse" at the *end* of the system prompt instead of rewriting it?

<details><summary>Answer</summary>
Because of the loyalty discount (prompt cache): the AI company only discounts text that's identical to before. Changing the middle or start would void the discount on everything after it. Appending at the end leaves all the earlier text untouched, so you keep the discount <em>and</em> still steer behavior.
</details>

→ Next: [Lesson 4 — Stop explaining your project twice](04-memory.md)
