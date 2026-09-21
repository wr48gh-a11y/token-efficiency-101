# Lesson 4 — Stop explaining your project twice
**⏱ 3 minutes · Tools: CLAUDE.md / AGENTS.md, claude-mem, headroom memory**

Every fresh session, your agent knows nothing. So you (or it) re-derive your architecture, conventions, and naming rules — and **you pay for that re-explanation every session**, in time and tokens.

## Tier 1: The instruction file (free, 5 minutes, do this today)

Every major agent reads a project instructions file at startup — `CLAUDE.md`, `AGENTS.md`, `.cursorrules`. Think of it as a **sticky note on the door**: the agent automatically reads it before every session, so anything written there is something you never have to explain again. Keep it short and factual:

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

**Why short matters:** this file is re-sent in *every* session's context. A 2,000-token CLAUDE.md costs you 2,000 tokens × every turn × every session — Lesson 0's re-read report, except you wrote it yourself. Trim it like a landing page.

(In the example, "ADR-007" just means "decision record #7" — a note file where teams document why they chose option A over B.)

You can also make rules **path-scoped** — meaning they only load when the agent touches a specific folder, e.g. "the /api rules apply only when editing /api" — so you pay for each rule only when it's actually relevant.

## Tier 2: Automatic memory

If you want memory that maintains itself:

- **claude-mem** and similar plug-ins automatically compress and persist each session, then feed relevant prior context back on demand — the agent remembers last Tuesday without you re-briefing it.
- **Headroom** includes *cross-agent* memory — one shared memory used by all your tools (Claude, Codex, Gemini, Grok), with automatic removal of duplicates. It also has `headroom learn`, which reviews sessions where things went wrong and writes the corrections into your instruction file for you.

## The test

Start a new session and ask: *"What is this project and what are its conventions?"*

- If the answer is right **without you typing anything** → your memory tier is working.
- If it hallucinates or says "I don't have context" → go back to Tier 1.

✅ **Check yourself:** Why is a huge CLAUDE.md self-defeating?

<details><summary>Answer</summary>
It's re-sent as context in every session and every turn, so it becomes exactly the kind of always-present token bloat this course is about — the re-read report, except you wrote it. Keep it minimal and path-scoped.
</details>

→ Next: [Lesson 5 — Final quiz + your 20-minute setup](05-quiz-and-setup.md)
