# Token Efficiency 101
### Stop the leaks: where your AI coding agent's tokens actually go

**Level:** 100 (beginner — no math, no ML background needed)
**Time to complete:** ~20 minutes
**Prerequisites:** You've used an AI coding agent (Claude Code, Cursor, Codex, Aider...) at least once.

---

## Why this course exists

When you hit a usage limit mid-refactor, the instinct is: *upgrade the plan* or *switch to a cheaper model*. Both miss the point. On a typical agent session, **your prompts are a small fraction of the spend**. The real money goes to:

> Tokens = what you send + what the model thinks + what it writes back + everything you explain again tomorrow.

This course teaches you to see the four leaks around your prompt — and gives you one free, open-source fix for each.

## Course map

| # | Lesson | The leak it plugs | Tool | Time |
|---|--------|-------------------|------|------|
| 0 | [Where your tokens actually go](lessons/00-where-tokens-go.md) | (mental model) | — | 3 min |
| 1 | [Stop sending junk to the model](lessons/01-compress-inputs.md) | Bloated tool outputs, logs, JSON | [Headroom](https://github.com/headroomlabs-ai/headroom) | 4 min |
| 2 | [Stop reading the whole file](lessons/02-smart-context.md) | Full-file reads & grep dumps | Semantic navigation / RTK | 4 min |
| 3 | [Stop paying for ceremony](lessons/03-output-costs.md) | Verbose output & overthinking | Output shaping | 3 min |
| 4 | [Stop explaining your project twice](lessons/04-memory.md) | Lost context between sessions | Persistent memory (CLAUDE.md, claude-mem) | 3 min |
| 5 | [Final quiz + your 20-minute setup](lessons/05-quiz-and-setup.md) | Lock it in | — | 3 min |

## What you'll be able to do afterward

1. Explain, in one sentence each, the four places tokens leak in an agent session.
2. Install and run a compression proxy in front of your agent (`headroom wrap claude`) — and undo it.
3. Read a token-savings number and know whether it's real or marketing.
4. Set up persistent context so tomorrow's session doesn't start from zero.

## Honest caveats (read these)

- Compression works best on **repetitive payloads** — JSON, logs, search results. Plain-prose chat sessions see small gains. Don't expect miracles if you mostly converse.
- Always check a tool's **telemetry defaults** before installing. Example: Headroom's beacon sends ratios/model IDs (not code) but can be disabled with `HEADROOM_BEACON=off` or `DO_NOT_TRACK=1`.
- Claims like "95% fewer tokens" are almost always measured on **structured JSON workloads**, not general coding. The realistic coding-agent number is ~20–40%.

---

*Sources: [headroomlabs-ai/headroom (GitHub)](https://github.com/headroomlabs-ai/headroom) · [ZIRU, "4 Free Tools Ended My Claude Code Token Panic" (Level Up Coding)](https://levelup.gitconnected.com/4-free-tools-ended-my-claude-code-token-panic-no-plan-upgrade-needed-ad9f96a4ad08) · [dev.to: 9 Verified Tools to Stop Burning Claude Tokens](https://dev.to) · [Firecrawl: 12 Ways to Cut Token Consumption in Claude Code](https://www.firecrawl.dev/blog)*
