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
| Nerd Dictation           | Free     | Linux only | Python + VOSK                    | No                 |
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
- "command scratch that" → undoes what you just typed

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
- 99 languages supported (say "command languages" to list)

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

**Command Hotkey**

Dedicated hotkey (F11 default) that auto-prefixes the leader word:
- Hold F11, say "enter" → same as "command enter"
- Great for rapid-fire commands without saying "command" every time

**Voice Commands**

Say "command" + any of these:

| Category   | Commands                                                              |
|------------|-----------------------------------------------------------------------|
| Navigation | enter, tab, escape, backspace, space, up, down, left, right           |
|            | home, end, page up, page down                                         |
| Editing    | select all, copy, paste, cut, undo, redo, save, find                  |
|            | close tab, new tab                                                    |
| Media      | play, pause, next, previous, volume up, volume down, mute             |
| Utility    | help (show commands), config (open config), repeat, repeat [N]        |
|            | languages (list all 99 supported languages)                           |

**Scratch That**

Made a mistake? Say "command scratch that" to delete what you just typed:
- Removes the exact number of characters from your last dictation
- Also works: "command scratch" or "command undo"

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

Hold mode rapidly presses keys together (rate configurable via `key_repeat_ms`).

**Emoji**

Say "command emoji" + name: smile, thumbs up, fire, heart, crab, poop, and 80+ more.

**Case Modes**

Say "command mode" + mode name to transform all dictation:

| Mode        | Output               |
|-------------|----------------------|
| snake       | hello_world          |
| camel       | helloWorld           |
| pascal      | HelloWorld           |
| kebab       | hello-world          |
| screaming   | HELLO_WORLD          |
| caps        | HELLO WORLD          |
| lower       | hello world          |
| alternating | hElLo WoRlD          |
| off         | hello world (normal) |

Say "mode snake", dictate naturally, say "mode off" when done. Great for coding.

**Code Mode**

Say "command mode code" to convert symbol names to tight symbols:

| Input                                        | Output           |
|----------------------------------------------|------------------|
| function open paren x close paren            | function(x)      |
| if x double equals y open brace              | if x == y {      |
| array open bracket zero close bracket        | array[0]         |

**Math Mode**

Say "command mode math" to convert spoken math to symbols:

| Input                             | Output    |
|-----------------------------------|-----------|
| one plus one                      | 1 + 1     |
| five times three                  | 5 * 3     |
| x greater than y                  | x > y     |

**Inserts (Text Snippets)**

Define snippets in config, insert by voice:

```toml
[inserts]
email = "you@example.com"
sig = "Best regards,\nYour Name"
header = "// Created: {date}\n// Author: {shell:git config user.name}"
branch = "{shell:git branch --show-current}"
```

Say "command insert email" → inserts your email. Placeholders expand:
- `{date}` → current date
- `{time}` → current time
- `{shell:command}` → output of any shell command

**Wrappers**

Define wrappers in config, wrap text by voice:

```toml
[wrappers]
quotes = '"'
parens = "(|)"
fire = "🔥"
div = "<div>|</div>"
```

| Input                             | Output                      |
|-----------------------------------|-----------------------------|
| command wrap quotes hello world   | "hello world"               |
| command wrap parens check this    | (check this)                |
| command wrap fire awesome         | 🔥awesome🔥                 |

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

**VAD Mode (Voice Activity Detection)**

For hands-free operation using Silero VAD:

```toml
activation_mode = "vad"    # switch from hotkey to VAD
vad_sensitivity = 0.9      # 0.0-1.0, higher = more sensitive
vad_silence_ms = 1000      # silence before processing
vad_min_speech_ms = 200    # ignore sounds shorter than this
```

Press hotkey to toggle listening. Speak naturally — VAD detects when you start and stop talking, then transcribes automatically.

**Wake Word**

Optional wake word filtering in VAD mode:

```toml
wake_word = "computer"     # only process speech starting with this
```

Utterances not starting with the wake word are silently ignored. Great for filtering background conversations.

**Audio Feedback**

```toml
audio_feedback = true      # beep on recording start, double beep when done
```

**Logging**

```toml
dictation_log = "~/.local/share/ss9k/dictation.log"  # log all transcriptions
error_log = "~/.local/share/ss9k/error.log"          # log errors
```

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

model = "small"              # tiny, base, small, medium, large
language = "en"              # ISO 639-1 code
threads = 4                  # whisper inference threads
hotkey = "F12"               # any supported key
command_hotkey = "F11"       # auto-prefixes leader word
hotkey_mode = "hold"         # hold or toggle
toggle_timeout_secs = 0      # auto-stop in toggle mode (0 = disabled)
leader = "command"           # leader word for commands
key_repeat_ms = 50           # key repeat rate for hold mode
verbose = true               # set false once comfortable
audio_feedback = false       # beeps for recording start/stop

# VAD mode (optional)
activation_mode = "hotkey"   # "hotkey" or "vad"
vad_sensitivity = 0.9
vad_silence_ms = 1000
wake_word = ""               # e.g., "computer"

# Logging (optional)
dictation_log = ""
error_log = ""

[commands]
"open terminal" = "kitty"

[aliases]
"come and" = "command"

[inserts]
email = "you@example.com"

[wrappers]
quotes = '"'
```

Edit. Save. Changes apply instantly.

---

## Comparison

| Feature              | SS9K            | Talon        | Dragon       | Voxtype        | Nerd Dictation |
|----------------------|-----------------|--------------|--------------|----------------|----------------|
| **Price**            | Free            | Freemium     | $200-500+    | Free           | Free           |
| **Fully Local**      | Yes             | Yes          | Yes          | Yes            | Yes            |
| **Voice Commands**   | Full            | Scripting    | Basic        | Punctuation    | No             |
| **Case Modes**       | 12 modes        | Yes          | No           | No             | No             |
| **Code Mode**        | Yes             | No           | No           | No             | No             |
| **Math Mode**        | Yes             | No           | No           | No             | No             |
| **Insert Snippets**  | Yes + {shell:}  | Python       | No           | No             | No             |
| **VAD Mode**         | Silero          | Yes          | Yes          | No             | No             |
| **Learning Curve**   | Low             | High         | Medium       | Low            | Low            |

**TL;DR:** Talon-level features without the scripting complexity.

---

## Tech

- **Rust** — single binary, no runtime, no dependencies
- **whisper-rs** — local Whisper inference via whisper.cpp
- **rdev** — cross-platform global hotkeys
- **cpal** — cross-platform audio capture
- **enigo** — cross-platform keyboard simulation
- **voice_activity_detector** — Silero VAD for hands-free mode
- **arc-swap + notify** — lock-free hot-reload

No Electron. No Python. No frameworks. Just Rust.

---

## Status

- **Linux X11:** Working (daily driver)
- **Windows:** Working (tested, binaries available)
- **macOS:** Needs tester (CI disabled)
- **Wayland:** Not supported yet

---

## Links

- **GitHub**: [sqrew/ss9k](https://github.com/sqrew/ss9k)
- **Releases**: [Download binaries](https://github.com/sqrew/ss9k/releases)
- **Issues**: [Report bugs, request features](https://github.com/sqrew/ss9k/issues)

---

*Built with Claude. Free forever.*

*Screech at your computer. It listens.* 🦀

[← Back to projects](.)
