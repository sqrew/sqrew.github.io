# SuperScreecher9000

**Voice control for your entire desktop. Local. Private. Free.**

Press a key. Talk. It types. Or runs commands. That's it.

---

## The Problem

You want to talk to your computer. Your options:

| Tool                     | Cost     | Platform   | Setup                            | System Control     |
|--------------------------|----------|------------|----------------------------------|--------------------|
| Dragon NaturallySpeaking | $300-700 | Windows    | Installer + account              | Limited            |
| Talon                    | Free     | Cross      | Python + config + learning curve | Yes (with scripts) |
| Voxtype                  | Free     | Linux only | Binary                           | No                 |
| Handy                    | Free     | Cross      | Tauri + webview                  | No                 |
| **SS9K**                 | Free     | Cross      | Binary                           | Yes (TOML config)  |

Everyone else is either expensive, complex, platform-locked, or missing features.

---

## What It Does

1. Press F12 (or any key you configure)
2. Talk
3. Text appears at your cursor

Or say a command:
- "volume up" → volume goes up
- "open terminal" → terminal opens
- "play" → music plays

No cloud. No account. No internet required. Whisper runs locally on your machine.

---

## Why It Exists

Accessibility tools should be:
- **Free** — disability often means limited income
- **Simple** — limited energy means no debugging Python
- **Reliable** — crashes lock you out of your computer
- **Private** — your voice stays on your machine

SS9K is ~900 lines of Rust. One file. One binary. Zero runtime dependencies.

Someone in an iron lung could use this. Someone with severe RSI could use this. Someone with ALS could use this.

They shouldn't need $500 or a CS degree to talk to their computer.

---

## Features

**Dictation**
- Whisper-powered transcription (tiny → large models)
- GPU acceleration (Vulkan, CUDA, Metal)
- Works offline, completely local

**Voice Commands**
- Navigation: enter, tab, escape, backspace
- Editing: copy, paste, cut, undo, redo, save
- Media: play, pause, next, previous, volume, mute

**Custom Commands**
```toml
[commands]
"open terminal" = "kitty"
"open browser" = "firefox"
"screenshot" = "flameshot gui"
"lock screen" = "loginctl lock-session"
```

Any voice phrase → any shell command. That's the entire API.

**Alias System**
```toml
[aliases]
"taping" = "typing"
"e max" = "emacs"
```

Whisper mishears something consistently? Fix it in config.

**Flexible Hotkeys**
- Hold mode: press to start, release to stop
- Toggle mode: press to start, press again to stop
- Configurable timeout for toggle mode
- Any key: F1-F12, ScrollLock, Pause, numpad, etc.

**Hot-Reload Config**
- Edit config while running, changes apply instantly
- No restart needed for hotkey, commands, aliases
- Only model/language changes need restart

---

## Install

```bash
# Clone and build
git clone https://github.com/sqrew/ss9k
cd ss9k
cargo build --release

# Run (downloads model on first launch)
./target/release/ss9k

# Press F12 and talk
```

For GPU acceleration:
```bash
cargo build --release --features vulkan  # Linux/Windows
cargo build --release --features cuda    # NVIDIA
cargo build --release --features metal   # macOS
```

---

## Config

```toml
# ~/.config/ss9k/config.toml

model = "small"           # tiny, base, small, medium, large
language = "en"           # ISO 639-1 code
hotkey = "F12"            # any supported key
hotkey_mode = "hold"      # hold or toggle

[commands]
"open terminal" = "kitty"

[aliases]
"taping" = "typing"
```

That's it. Edit a text file. Save. Changes apply instantly—no restart needed.

---

## Tech

- **Rust** — single binary, no runtime, no dependencies
- **whisper-rs** — local Whisper inference via whisper.cpp
- **rdev** — cross-platform global hotkeys
- **cpal** — cross-platform audio capture
- **enigo** — cross-platform keyboard simulation
- **arc-swap + notify** — lock-free hot-reload
- **~900 lines** — entire tool in one file

No Electron. No Python. No frameworks. Just Rust.

---

## The Gap

I looked at what exists:

- **Talon** is a framework. Powerful, but you need to learn Python and write scripts.
- **Voxtype** is Linux-only and dictation-only.
- **Handy** has 10k stars and 58 open issues. No system commands.
- **Dragon** costs $500 and phones home.

Nobody had: **dictation + system commands + cross-platform + single binary + simple config**.

So I built it.

---

## Status

- Linux X11: **working** (daily driver)
- Windows: CI passing, needs real-world testing
- macOS: CI disabled (ARM64 issues), needs tester
- Wayland: later (let Voxtype own that niche)

---

## Links

- **GitHub**: [sqrew/ss9k](https://github.com/sqrew/ss9k)
- **Issues**: [Report bugs, request features](https://github.com/sqrew/ss9k/issues)

---

*Built with Claude. ~900 lines. Free forever.*

*Screech at your computer. It listens.* 🦀

[← Back to projects](.)
