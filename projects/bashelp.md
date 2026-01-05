# bashelp

Natural language to shell commands. Local-first, provider agnostic.

[GitHub](https://github.com/sqrew/bashelp) · [crates.io](https://crates.io/crates/bashelp)

---

## What it does

You describe what you want in plain English. bashelp gives you the shell command.

```
$ bashelp find all rust files modified this week

→ find . -name "*.rs" -mtime -7

[Enter to run, 'c' to copy, 'e' to edit, 'q' to quit]:
```

No more googling "how to tar again" or "find syntax flags". Just ask.

## Why it exists

Most AI shell tools require cloud API keys and send your commands to remote servers. bashelp is **local-first** — it works with [ollama](https://ollama.ai) out of the box. Your shell context never leaves your machine unless you choose a cloud provider.

## Features

- **Local-first** — works with ollama, no API key needed
- **12+ providers** — Claude, OpenAI, Gemini, Groq, Mistral, and more
- **No quotes needed** — `bashelp how do I list files` just works
- **Cross-platform** — Linux, macOS, Windows
- **Copy to clipboard** — press 'c' at the prompt
- **Explain mode** — understand commands you don't recognize

## Install

```bash
cargo install bashelp
ollama pull llama3
bashelp use llama3
bashelp find files larger than 100mb
```

## Tech

Rust. Single binary. Minimal dependencies. Built with clap, tokio, reqwest.

---

[← Back to projects](.)
