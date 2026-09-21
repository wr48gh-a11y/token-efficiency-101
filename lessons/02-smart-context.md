# Lesson 2 — Stop reading the whole file
**⏱ 4 minutes · Tools: semantic navigation (Serena — automatic), RTK (optional)**

## One story, then everything makes sense

You type into your agent:

```text
"Where do we check if a user's session has expired?"
```

A reasonable question. But watch what the agent has to do to answer it — it can't just "know" your code, it has to **read your files and paste them into its working memory** (context, from Lesson 0):

- It searches for "session" → **214 matching lines** pasted into memory
- It guesses the answer lives in `auth.py` → pastes **all 612 lines** of the file. The answer is on lines 305–309. **Five lines.**
- It's not sure, so it opens two more files → **+900 lines**

To answer one question, it pasted **~1,700 lines of code into memory** — and per Lesson 0, all of it gets re-sent with every future message in the session.

That's the leak this lesson plugs. And here's the good news up front: **all three fixes below happen automatically** once you've done Lesson 1's `headroom wrap claude`. Your job in this lesson is mostly to *recognize* the leak.

## Fix 1 — teach the agent to use the index, not the whole book

**Semantic navigation** sounds fancy. It just means: instead of "read the entire book to find one paragraph," the agent learns to **use the index** — look up the exact function by name, or read exactly lines 305–309.

| | Without it (the leak) | With it |
|---|---|---|
| Agent's move | paste all 612 lines of auth.py | paste lines 305–309 (5 lines) |
| What it costs | ~5,000 tokens, re-sent every turn | ~50 tokens |
| Answer quality | same | same |

The tool that enables this is a **plug-in called Serena** — and Headroom's `wrap` from Lesson 1 installs it for you automatically. No action needed.

If you skip Headroom entirely, you can still nudge behavior by pasting this line into your project's instruction file (Lesson 4 explains those):

```text
"When exploring, prefer reading specific line ranges
 and symbol definitions over whole files."
```

## Fix 2 — shrink the command receipts

Agents constantly run terminal commands and paste the full output into memory — like taping every receipt into a scrapbook, then carrying the scrapbook everywhere. **RTK** is a tool that prints a *compact summary* instead of the raw dump: same facts, fraction of the space.

| Command the agent ran | Raw output | With RTK |
|---|---|---|
| "show me what changed" (`git diff`) | 520 lines | ~80 lines, same info |
| "search for 'session'" (`grep`) | 214 lines | top matches only |

Reported effect: **20–40% fewer input tokens** when the session involves lots of commands. RTK is the same story as Lesson 1 — a smaller mailroom — and optional. Headroom already shrinks much of this for you.

## Fix 3 — send a briefing memo, not the whole library

Sometimes you *want* to give the agent the big picture ("here's how this project is organized"). Tools like **repomix** or **codesight** build a clean summary of your codebase — the map, without the boilerplate — so you hand the agent a 2-page memo instead of dumping every file into the conversation. Only relevant if you regularly start sessions that way; otherwise skip it.

## So what do I actually *do*?

| If you… | Then… |
|---|---|
| did Lesson 1's `headroom wrap claude` | ✅ nothing — Fix 1 came bundled, Fixes 2–3 are largely covered |
| skipped Lesson 1 | paste the "prefer line ranges" line above into your instruction file |
| want to *see* the leak with your own eyes | ask your agent: *"list your tool calls this session and roughly how many tokens each returned"* — expect one or two file reads to dominate |

✅ **Check yourself:** Your agent searches for a name and gets 100 matching lines. How many times does the model pay for those lines?

<details><summary>Answer</summary>
Every remaining turn of the session — they're pasted into memory and re-sent with each message. If you keep working for 20 more turns, that's ~20× the original cost. This is why one lazy search can eat an afternoon of usage limit.
</details>

→ Next: [Lesson 3 — Stop paying for ceremony](03-output-costs.md)
