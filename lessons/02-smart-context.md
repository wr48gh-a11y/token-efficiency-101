# Lesson 2 — Stop reading the whole file
**⏱ 4 minutes · Tools: semantic navigation, RTK, context packs**

Your agent needed nine lines. It read 900. Then it ran a search that returned 100 matches — and every one rode in context for the next twenty turns (Lesson 0's forgetful consultant, re-reading the report every meeting).

## Decoder for this lesson

| Term | Plain English |
|---|---|
| `grep` | The built-in terminal search command. It scans files for text and prints *every* matching line — can easily dump hundreds of lines. |
| `git diff` | "Show me what changed." Prints only the edited lines, plus a few lines of surrounding context. |
| **symbol** | The name of one function, class, or variable — like `parseConfig`. Code is organized into these named building blocks. |
| **MCP server** | A plug-in that adds new skills to your agent (MCP is a standard plug socket for AI tools — like app extensions). The Serena plug-in teaches your agent to look up symbols and line ranges instead of swallowing whole files. |

## Fix 1 — navigate, don't ingest

**Semantic navigation tools** (e.g., the Serena MCP server — which Headroom's `wrap` installs automatically) give the agent symbol-level operations: "find `parseConfig`", "show lines 40–60", "list methods of this class" — instead of whole-file dumps. One instruction line changes behavior:

```text
"When exploring, prefer reading specific line ranges
 and symbol definitions over whole files."
```

## Fix 2 — compress command output at the source

Agents spend a lot of time running terminal commands and reading the results. **RTK** intercepts the noisiest ones — `git diff`, `grep`, `ls` (list files), `tree` (show folder structure), `log` — and prints compact versions instead. Reported: **20–40% fewer input tokens** when the session involves lots of commands.

## Fix 3 — pack context deliberately

For "brief the agent on this repo" moments, tools like **repomix** or **codesight** produce a structured summary of the codebase — the map, minus the dead weight and boilerplate — instead of concatenating every file into one monster document. Combined with the above, reports of **60–70% total bill reduction**.

## Rule of thumb

| The agent asks for... | Cheap answer | Expensive answer |
|---|---|---|
| "Where is X defined?" | symbol lookup / line range | read whole file |
| "What changed?" | `git diff` (compressed) | paste the module |
| "How is this repo organized?" | pre-built context pack | agent free-range exploration |

> 💡 **See the leak yourself:** run a normal session, then ask your agent *"List the tool calls you made this session with approximate token counts."* Most people find one or two file reads and a search dump responsible for the majority of the session's context. That's your target.

✅ **Check yourself:** Your agent searches for a function name and gets 100 results. Roughly how many times does the model pay for those results?

<details><summary>Answer</summary>
Once per subsequent turn in the session — the results persist in context (the model's only working memory) until compacted or dropped. Ten more turns ≈ ten times the cost.
</details>

→ Next: [Lesson 3 — Stop paying for ceremony](03-output-costs.md)
