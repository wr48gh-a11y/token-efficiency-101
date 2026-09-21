# Lesson 0 — Where your tokens actually go
**⏱ 3 minutes**

## What you need for this course

A laptop, and any AI coding agent (Claude Code, Cursor, Codex) used at least once. That's it. Lesson 1 will teach you the few technical words you need — terminal, install, proxy — from zero. If you can type a sentence and press Enter, you can finish this course.

## Four words, before anything else

| Word | Plain English |
|---|---|
| **token** | The unit the AI reads and writes in — roughly ¾ of a word. "Understanding" ≈ 2 tokens. You pay *per token*, for text going in *and* coming out. Think of them as tiny metered water drops. |
| **context** | Everything the model can "see" right now: your messages, its answers, files it read, command output. It's the model's only working memory. |
| **agent** | An AI that doesn't just chat — it acts. It reads your files, runs searches, edits code, then decides what to do next. |
| **session** | One continuous work conversation with the agent. Close the laptop or start fresh → new session, empty memory. |

## The thought experiment

Last time your agent burned through its limit mid-refactor, how much of that was your typed instructions?

For most developers: **surprisingly little.** Which is exactly why "write shorter prompts" advice never moves the needle.

## Why a file read costs more than it looks

Imagine the model is a brilliant consultant with **terrible memory** — they forget everything the moment you finish each sentence. So every time you speak, you must **recite the entire conversation so far**, word for word, including anything they asked to look at.

Hand them a 50-page report in meeting #1? In meeting #2, #3, and #20, you're reading that same report aloud again at the start. That's what "a file read rides in context" means — and the agent usually needed **nine lines** of those fifty pages.

## The three ways you overpay

- **Tool results are re-sent every turn.** A 5,000-token file read doesn't cost 5,000 once — it rides in context for dozens of turns.
- **Output tokens cost multiples of input** on frontier models. Verbosity is the most expensive habit.
- **Agents are amnesiacs.** No memory = you pay to re-transmit your architecture, conventions, and decisions every single session.

> 💡 **The quote that frames this course:** *"The real spend is the file your agent opened in full when it needed nine lines. The grep results it scanned and threw away. The 300-line class it wrote when 40 lines would have shipped. The Monday morning where you explain your project architecture from scratch, again."*

## Four leaks, four lessons

1. **Junk in** → Lesson 1 (compress what flows to the model)
2. **Too much in** → Lesson 2 (read nine lines, not the whole file)
3. **Ceremony out** → Lesson 3 (trim the model's verbosity and overthinking)
4. **Repeat tomorrow** → Lesson 4 (remember between sessions)

✅ **Check yourself:** Why is a single big file read more expensive than its token count suggests?

<details><summary>Answer</summary>
Because tool results stay in the conversation context (the model's only working memory) and get re-sent to the model on every subsequent turn of the session — like re-reading the report aloud at every meeting.
</details>

→ Next: [Lesson 1 — Stop sending junk to the model](01-compress-inputs.md)
