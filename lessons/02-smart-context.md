# Lesson 2 — Stop reading the whole file
**⏱ 4 minutes · Tools: semantic navigation (Serena MCP), RTK, structured context packers**

## The leak

Your agent needed nine lines. It read 900. Then it ran a grep that returned 100 matches — and every one of them rode in context for the next twenty turns.

Two fixes, applied at different layers:

### Fix 1: Let the agent navigate instead of ingest

**Semantic navigation tools** (e.g., the Serena MCP server — which Headroom's `wrap` installs automatically) give the agent *symbol-level* operations: "find the function `parseConfig`", "show lines 40–60 of this file", "list methods of this class" — instead of whole-file dumps.

In your agent instructions, one line changes behavior dramatically:

> "When exploring, prefer reading specific line ranges and symbol definitions over whole files."

### Fix 2: Compress command output at the source

**RTK** (a CLI output compressor) intercepts the noisy outputs agents generate most — `git diff`, `grep`, `ls`, `tree`, `log` — and emits compact versions. Reported effect: **20–40% fewer input tokens** from command-heavy sessions.

### Fix 3: Pack context deliberately

For "brief the agent on this repo" moments, tools like **repomix** or **codesight** produce structured, tree-shaken summaries of a codebase instead of concatenating everything. Reports of **60–70% total bill reduction** when combined with the above habits.

## Rule of thumb

| The agent asks for... | Cheap answer | Expensive answer |
|---|---|---|
| "Where is X defined?" | symbol lookup / line range | read whole file |
| "What changed?" | `git diff` (compressed) | paste the module |
| "How is this repo organized?" | pre-built context pack | agent free-range exploration |

## How to see the leak yourself

Run a normal session, then ask your agent: *"List the tool calls you made this session with approximate token counts."* Most people find one or two file reads and a grep dump responsible for the majority of the session's context. That's your target.

✅ **Check yourself:** Your agent greps for a function name and gets 100 results. Roughly how many times does the model pay for those results?

<details><summary>Answer</summary>
Once per subsequent turn in the session — the results persist in context until compacted or dropped. Ten more turns ≈ ten times the cost.
</details>

→ Next: [Lesson 3 — Stop paying for ceremony](03-output-costs.md)
