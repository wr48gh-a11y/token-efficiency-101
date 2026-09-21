# Lesson 1 — Stop sending junk to the model
**⏱ 4 minutes · Tool: [Headroom](https://github.com/headroomlabs-ai/headroom)**

## Plain English first

Every time your AI agent (Claude Code, Cursor…) does anything, it ships a giant bundle of text to the AI company's servers — including all the files it read and logs it collected. Most of that bundle is fat: repeated JSON, log lines, search results nobody needed.

**Headroom** is a free tool that shrinks that bundle **before it leaves your laptop**. Think of it as vacuum-sealing your luggage: same stuff, way smaller, and you can un-seal any bag when you actually need what's inside.

> 💡 **Analogy:** Headroom is a *mailroom* between your agent and the AI. Every package (file read, log, search result) passes through it. The mailroom flattens bulky boxes into slim envelopes, keeps the originals in a back room, and — this is the clever part — the AI can request the full original anytime it needs it. Nothing is destroyed, nothing leaves your machine un-shrunk.

## Three words you'll see, decoded

| Word | What it actually means |
|---|---|
| `proxy` | A middleman. Your agent sends its traffic to this little program on your laptop, and it forwards it to the AI — after shrinking it. |
| `pip install` | "Download and install this program." pip is the app store for Python tools. One command, done. |
| `headroom wrap claude` | "Start watching my Claude Code." It launches Claude Code as usual, but routed through the mailroom. `unwrap` puts it back to normal. Completely reversible. |

## Do it yourself — step by step

**Step 0 — open Terminal.** On a Mac: press `⌘ + Space`, type "Terminal", hit Enter. You'll see a window with a blinking cursor waiting for commands. That's it — you type a line, press Enter, the computer does the thing.

**Step 1 — install Headroom.** Try this first:

```bash
pip install "headroom-ai[all]"
```

**If Terminal says `command not found: pip`** — that's common and harmless. On many Macs the installer is named `pip3` instead. Try:

```bash
pip3 install "headroom-ai[all]"
```

**If `pip3` answers with a wall of text ending in `error: externally-managed-environment`** — also fine, and very common on Macs. Translation: your Mac's Python (from Homebrew) protects itself and refuses direct installs. It's telling you to give each app its own private sandbox. The tool for that is `pipx` — the proper app store for Python command-line programs. Run these three lines, one at a time:

```bash
brew install pipx
pipx ensurepath
pipx install "headroom-ai[all]"
```

Then **quit Terminal completely and reopen it** (so Terminal learns where the new `headroom` command lives), and continue with Step 2. No Homebrew either? Install Python itself from [python.org/downloads](https://www.python.org/downloads/), reopen Terminal, and go back to the first try.

*What you'll see when it works:* a stream of "Downloading… Installing…" lines for ~30 seconds, then your normal prompt returns. Nothing visual changed — the program now exists on your machine.

**Step 2 — turn it on for Claude Code:**

```bash
headroom wrap claude
```

*What you'll see:* a couple of startup lines from Headroom, then Claude Code opens exactly like always. Use it normally — work on a real task, let it read files, run searches. The shrinking happens invisibly in the background.

> 💻 **Using the Claude Code desktop app (or VS Code) instead of the terminal?** The desktop app reads the same settings as the CLI, so with the `wrap` terminal left open its traffic should flow through the mailroom too. The test: use the app for a real task, then run `headroom savings` in another terminal tab — if the number climbs, it's working. If it's stuck at zero, the app is bypassing the proxy; the CLI and VS Code (`headroom wrap vscode-claude`) are the officially supported routes. Either way: **keep the wrap terminal open** — closing it closes the mailroom (the agent still works, just uncompressed).

(Alternative: `headroom proxy --port 8787` runs just the mailroom, for any AI tool that can point at it.)

**Step 3 — check the damage report:**

```bash
headroom savings
```

*What you'll see:* a number of tokens (and roughly dollars) you didn't send. It starts near zero and grows as you work. Watching it climb is weirdly satisfying.

**Changed your mind?** One command and your setup is exactly as before:

```bash
headroom unwrap claude
```

## What the benchmarks say (for skeptics)

Measured with the real tokenizer ([reproducible in the repo](https://github.com/headroomlabs-ai/headroom)):

| Scenario | Before | After | Saved |
|---|---|---|---|
| Code search (100 results) | 17,199 | 13,597 | **21%** |
| SRE incident debugging | 55,957 | 24,340 | **57%** |
| Codebase exploration | 58,801 | 33,895 | **42%** |
| GitHub issue triage | 46,067 | 32,429 | **30%** |

Tagline: *"20% fewer tokens for coding agents, 60–95% fewer tokens for JSON, same answers."* (JSON = the bracket-and-quote data format programs use to talk to each other — extremely repetitive, so it shrinks brilliantly.) Note the honesty — coding agents get ~20%, not the headline 95%. Translation: if you normally hit your limit on Thursday, expect to hit it around Friday–Saturday. Not magic — but real.

## Know before you install

⚠️ **The tool "phones home" by default** — an automatic status ping (called *telemetry*) that reports usage stats like compression ratios and which model you used. It never includes your code, but you can turn it off with `HEADROOM_BEACON=off`, `DO_NOT_TRACK=1`, or `--offline`. Speed cost of the mailroom itself: ~0.2 ms per package — you'll never notice it.

✅ **Check yourself:** Why doesn't the compression proxy break the discount the AI company gives you for repeat conversation text?

<details><summary>Answer</summary>
The AI company gives a "loyalty discount" on any text identical to what you've already sent before (they keep it warm on their servers instead of re-charging full price). Headroom only shrinks the <em>new</em> stuff and leaves everything already sent byte-for-byte identical — so the discount still kicks in.
</details>

→ Next: [Lesson 2 — Stop reading the whole file](02-smart-context.md)
