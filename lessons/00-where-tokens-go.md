# Lesson 0 — Where your tokens actually go
**⏱ 3 minutes**

## The bill is not what you think

Run this thought experiment. Last time your agent burned through its limit during a refactor, how much of that was your typed instructions?

For most developers: surprisingly little. A session's token spend looks roughly like this:

```
Your prompts            ████░░░░░░░░░░░░  ~10-15%
Tool results it read    █████████░░░░░░░  ~35-45%   ← files, grep, logs, JSON
Model output            █████░░░░░░░░░░░  ~15-25%   ← often 3-5x pricier per token than input
Re-explained context    ████░░░░░░░░░░░░  ~15-20%   ← every new session
```

The article this course is based on puts it bluntly:

> "The real spend is the file your agent opened in full when it needed nine lines. The grep results it scanned and threw away. The 300-line class it wrote when 40 lines would have shipped. The Monday morning where you explain your project architecture from scratch, again."

## Why "write shorter prompts" doesn't fix it

Standard advice targets the smallest slice. Meanwhile:

- **Tool results are re-sent every turn.** A 5,000-token file read doesn't cost 5,000 tokens once — it can ride along in context for dozens of turns.
- **Output tokens cost multiples of input tokens** on frontier models. Verbosity is the most expensive habit.
- **Agents are amnesiacs.** No memory means you pay to re-transmit your architecture, conventions, and decisions every session.

## The mental model to carry through this course

Four leaks, four lessons:

1. **Junk in** → Lesson 1 (compress what flows to the model)
2. **Too much in** → Lesson 2 (read nine lines, not the whole file)
3. **Ceremony out** → Lesson 3 (trim the model's verbosity and overthinking)
4. **Repeat tomorrow** → Lesson 4 (remember between sessions)

✅ **Check yourself:** In one sentence — why is a single big file read more expensive than its token count suggests?

<details><summary>Answer</summary>
Because tool results stay in the conversation context and get re-sent to the model on every subsequent turn of the session.
</details>

→ Next: [Lesson 1 — Stop sending junk to the model](01-compress-inputs.md)
