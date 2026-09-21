# Lesson 4 — Stop explaining your project twice
**⏱ 3 minutes · Tools: CLAUDE.md / AGENTS.md, claude-mem, headroom memory**

## The leak

Every fresh session, your agent knows nothing. So you (or it) re-derive your architecture, your conventions, your naming rules — and **you pay for that re-explanation every single session**, in both your time and tokens.

## Tier 1: The instruction file (free, 5 minutes, do this today)

Every major agent reads a project instructions file at startup — `CLAUDE.md`, `AGENTS.md`, `.cursorrules`. This is your cheapest memory. Keep it short and factual:

```markdown
# Project
Monorepo: FastAPI backend in /api, React in /web, shared types in /packages/shared.

# Conventions
- Python: ruff, type hints required, no classes for pure functions.
- Commits: Conventional Commits.
- Never edit /packages/generated.

# Decisions
- 2026-09: chose Postgres over Mongo for transactional integrity (see ADR-007).
```

**Why short matters:** this file is re-sent in *every* session's context. A 2,000-token CLAUDE.md costs you 2,000 tokens × every turn × every session. Trim it like a landing page. Path-scoped rules (rules that only load when touching certain directories) cut this further — Firecrawl's benchmarks attribute a large share of total savings to instruction-file hygiene alone.

## Tier 2: Automatic memory

If you want memory that maintains itself:

- **claude-mem** and similar plugins automatically compress and persist what happened in each session, then feed relevant prior context back on demand.
- **Headroom** includes cross-agent memory — a shared store across Claude, Codex, Gemini, and Grok with auto-dedup, plus `headroom learn`, which mines failed sessions and writes corrections to your instruction file automatically.

## The test

Start a new session and ask: *"What is this project and what are its conventions?"*

- If the answer is right **without you typing anything** → your memory tier is working.
- If it hallucinates or says "I don't have context" → go back to Tier 1.

✅ **Check yourself:** Why is a huge CLAUDE.md self-defeating?

<details><summary>Answer</summary>
It's re-sent as context in every session and every turn, so it becomes exactly the kind of always-present token bloat this course is about. Keep it minimal and path-scoped.
</details>

→ Next: [Lesson 5 — Final quiz + your 20-minute setup](05-quiz-and-setup.md)
