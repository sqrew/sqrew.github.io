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
- "command volume up" → volume goes up
- "open terminal" → terminal opens (custom command)
- "command play" → music plays
- "command punctuation arrow" → types `=>`
- "command emoji fire" → types 🔥

No cloud. No account. No internet required. Whisper runs locally on your machine.

---

## Why It Exists

Accessibility tools should be:
- **Free** — disability often means limited income
- **Simple** — limited energy means no debugging Python
- **Reliable** — crashes lock you out of your computer
- **Private** — your voice stays on your machine

SS9K is one file. One binary. Zero runtime dependencies.

Someone in an iron lung could use this. Someone with severe RSI could use this. Someone with ALS could use this.

They shouldn't need $500 or a CS degree to talk to their computer.

---

## Features

**Dictation**
- Whisper-powered transcription (tiny → large models)
- GPU acceleration (Vulkan, CUDA, Metal)
- Works offline, completely local

**Leader Word**

SS9K uses a configurable leader word (default: `command`) to separate commands from dictation:
- Say "enter" → types the word "enter"
- Say "command enter" → presses the Enter key
- Say "command punctuation colon" → types `:`
- Say "command emoji smile" → types 😊

No reserved words. You can dictate anything. Change the leader in config:
```toml
leader = "voice"  # or "computer", "hey", whatever
```

**Voice Commands**

Say "command" + any of these:

| Category   | Commands                                                              |
|------------|-----------------------------------------------------------------------|
| Navigation | enter, tab, escape, backspace, space, up, down, left, right           |
|            | home, end, page up, page down                                         |
| Editing    | select all, copy, paste, cut, undo, redo, save, find                  |
|            | close tab, new tab                                                    |
| Media      | play, pause, next, previous, volume up, volume down, mute             |
| Utility    | help, config, repeat, repeat [N]                                      |

**Punctuation**

Say "command punctuation" (or "command punk") + symbol name:

- Basic: period, comma, question, exclamation, colon, semicolon
- Quotes: quote, single quote, backtick
- Brackets: open/close paren, bracket, brace, angle
- Programming: arrow (`=>`), thin arrow (`->`), double colon, equals equals, not equals, and and, or or

**Spell Mode**

Say "command spell" + NATO phonetic or letters:

| Input                              | Output  |
|------------------------------------|---------|
| command spell alpha bravo charlie  | abc     |
| command spell capital alpha bravo  | Ab      |
| command spell one two three        | 123     |
| command spell alpha at bravo dot com | a@b.com |

**Shift Mode (Selection)**

Say "command shift" + direction:

| Input                            | Effect                   |
|----------------------------------|--------------------------|
| command shift right              | Select 1 char right      |
| command shift right times five   | Select 5 chars right     |
| command shift word left          | Select word left         |
| command shift end                | Select to end of line    |

**Hold/Release (Gaming & Accessibility)**

| Input              | Effect                    |
|--------------------|---------------------------|
| command hold w     | Hold W key (run forward)  |
| command hold shift | Hold Shift modifier       |
| command release w  | Release W key             |
| command release all| Release all held keys     |

**Emoji**

Say "command emoji" + name: smile, thumbs up, fire, heart, crab, poop, and 80+ more.

**Repetition**

- "command backspace times five" → deletes 5 characters
- "command down times ten" → moves down 10 lines
- "command repeat" → repeats last command
- "command repeat three" → repeats 3 times

**Mishearing Tolerance**

Whisper mishears things. SS9K handles it:
- caret → also matches carrot, karet
- colon → also matches colin, cologne
- asterisk → also matches asterix, astrix

**Custom Commands**
```toml
[commands]
"open terminal" = "kitty"
"open browser" = "firefox"
"screenshot" = "flameshot gui"
"lock screen" = "loginctl lock-session"
```

Any voice phrase → any shell command. Custom commands work without the "command" leader.

**Alias System**
```toml
[aliases]
"taping" = "typing"
"come and" = "command"
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

**Linux:**
```bash
git clone https://github.com/sqrew/ss9k
cd ss9k
cargo build --release
./target/release/ss9k
```

**Windows:**

Requires Visual Studio Build Tools (C++ workload) and LLVM. Then build from x64 Native Tools Command Prompt:
```cmd
git clone https://github.com/sqrew/ss9k
cd ss9k
cargo build --release
```

**Pre-built binaries:** [GitHub Releases](https://github.com/sqrew/ss9k/releases)

**GPU acceleration:**
```bash
cargo build --release --features vulkan  # Linux/Windows (Intel/AMD)
cargo build --release --features cuda    # NVIDIA
cargo build --release --features metal   # macOS
```

---

## Config

```toml
# ~/.config/ss9k/config.toml

model = "small"           # tiny, base, small, medium, large
language = "en"           # ISO 639-1 code
threads = 4               # whisper inference threads
hotkey = "F12"            # any supported key
hotkey_mode = "hold"      # hold or toggle
toggle_timeout_secs = 0   # auto-stop in toggle mode (0 = disabled)
leader = "command"        # leader word for commands
quiet = false             # suppress verbose output

[commands]
"open terminal" = "kitty"

[aliases]
"come and" = "command"
```

Edit. Save. Changes apply instantly.

---

## Tech

- **Rust** — single binary, no runtime, no dependencies
- **whisper-rs** — local Whisper inference via whisper.cpp
- **rdev** — cross-platform global hotkeys
- **cpal** — cross-platform audio capture
- **enigo** — cross-platform keyboard simulation
- **arc-swap + notify** — lock-free hot-reload

No Electron. No Python. No frameworks. Just Rust.

---

## Status

- **Linux X11:** Working (daily driver)
- **Windows:** Working (tested, binaries available)
- **macOS:** Needs tester (CI disabled)
- **Wayland:** Not supported (security model blocks global hotkeys)

---

## Links

- **GitHub**: [sqrew/ss9k](https://github.com/sqrew/ss9k)
- **Releases**: [Download binaries](https://github.com/sqrew/ss9k/releases)
- **Issues**: [Report bugs, request features](https://github.com/sqrew/ss9k/issues)

---

*Built with Claude. Free forever.*

*Screech at your computer. It listens.* 🦀

[← Back to projects](.)
