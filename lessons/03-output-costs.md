# Lesson 3 — Stop paying for ceremony
**⏱ 3 minutes · Tool: output shaping (optional) · zero-tool version included**

## Quick story

You ask your agent for a tiny change — "rename this variable." Thirty seconds of thinking, and back comes:

```text
Great! Let me take a look at that for you. 🎉

First, I'll explain my approach:

1. I will locate the variable in question.
2. I will rename it carefully.
3. I will verify everything still works.

Here's the file with your change (note how I've
reproduced your entire 40-line file, even though
only 1 line changed):

    ... 40 lines you already know ...

Let me know if you'd like me to explain anything,
or if you have any other questions! ✨
```

One line of actual work. The rest — the preamble, the plan recap, your own file echoed back, the cheery sign-off — is **ceremony**: words that cost output-token prices (several × input prices) and taught nobody anything.

## Decoder for this lesson

| Term | Plain English |
|---|---|
| **system prompt** | The standing instruction sheet the agent reads before every conversation — "you are a coding assistant, do X, never Y." You usually don't see it, but you pay for it every turn. |
| **thinking tokens** | Newer models can do silent scratch-work before answering ("let me consider…") — visible in pricing as extra output tokens. Useful for hard problems; overkill for renaming a variable. |
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

Paste this into your agent's instruction file (CLAUDE.md / `.cursorrules` / AGENTS.md — Lesson 4 shows exactly where):

```text
- Don't repeat file contents back to me; summarize changes as diffs.
- Skip preamble and closing summaries.
- Prefer editing existing code over rewriting files.
- Don't restate the task before solving it.
```

With those rules, the same rename comes back as:

```text
Renamed userName → currentUser in src/api.ts (line 42).
2 other references updated.
```

Same information, ~10% of the tokens, at output prices.

## So what do I actually *do*?

| If you… | Then… |
|---|---|
| just want the 2-minute version (most people) | paste the 4 rules above into your instruction file — done, no tools |
| already run `headroom wrap claude` and want more | `export HEADROOM_OUTPUT_SHAPER=1`, then `headroom proxy --port 8787` — it trims output on the model's side automatically |

✅ **Check yourself:** Why append "be terse" at the *end* of the system prompt instead of rewriting it?

<details><summary>Answer</summary>
Because of the loyalty discount (prompt cache): the AI company only discounts text that's identical to before. Changing the middle or start would void the discount on everything after it. Appending at the end leaves all the earlier text untouched, so you keep the discount <em>and</em> still steer behavior.
</details>

→ Next: [Lesson 4 — Stop explaining your project twice](04-memory.md)
