# rmcp-presence

One binary. 142 tools. Your AI shouldn't be trapped in a tab.

---

## The Problem

You want to give your AI environmental awareness. Great. Now configure 17 MCP servers.

```json
{
  "mcpServers": {
    "sensors": { ... },
    "clipboard": { ... },
    "audio": { ... },
    "trash": { ... },
    "open": { ... },
    "screenshot": { ... },
    "breakrs": { ... },
    "ollama": { ... },
    "i3": { ... },
    "xdotool": { ... },
    "mpris": { ... },
    "systemd": { ... },
    "brightness": { ... },
    "bluer": { ... },
    "dbus": { ... },
    "logind": { ... },
    "pulseaudio": { ... }
  }
}
```

Each server needs to be installed, configured, and maintained separately. Each one is another process, another thing to break, another line in your config. The cognitive overhead is absurd.

And if you're not on Linux? Half of these don't apply to you. Good luck figuring out which.

## The Solution

```json
{
  "mcpServers": {
    "presence": {
      "type": "stdio",
      "command": "rmcp-presence",
      "args": [],
      "env": {}
    }
  }
}
```

One server. One config line. 142 tools.

```bash
cargo install rmcp-presence --features full
```

That's it. Your AI can now perceive and act on its environment.

## What It Can Do

rmcp-presence consolidates environmental awareness and action capabilities into three layers:

### Layer 1: Sensors (28 tools)
*Cross-platform. Read-only. Know your environment.*

- **System**: CPU, memory, disk, processes, temperatures, network stats
- **Display**: Monitor info, resolutions, multi-monitor awareness
- **Activity**: Idle time detection, AFK checking
- **Hardware**: USB devices, battery status, Bluetooth scanning
- **Context**: Git repo status, weather conditions

### Layer 2: Actuators (31 tools)
*Cross-platform. Take action.*

- **Clipboard**: Read, write, clear
- **Audio**: System volume, mute control
- **Files**: Trash operations, open with default/specific apps
- **Visual**: Screenshot monitors, windows, or regions
- **Time**: Reminders and notifications via breakrs
- **LLM**: Manage local Ollama models

### Layer 3: Linux (83 tools)
*Linux-only. Full desktop control.*

- **i3**: Window management, workspaces, focus control
- **xdotool**: Mouse, keyboard, automation
- **MPRIS**: Media player control
- **systemd**: Service management
- **PulseAudio**: Per-app audio control (mute Discord, keep Spotify)
- **BlueZ**: Full Bluetooth management
- **logind**: Power management (suspend, hibernate, lock)
- **D-Bus**: Generic escape hatch for anything else

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                       rmcp-presence                           │
│                    (single binary, 12MB)                      │
├──────────────────────────────────────────────────────────────┤
│  Layer 3: Linux         │  83 tools — Linux only             │
│  #[cfg(target_os =      │  i3, xdotool, mpris, systemd,      │
│       "linux")]         │  brightness, bluer, dbus,          │
│                         │  logind, pulseaudio                │
├──────────────────────────────────────────────────────────────┤
│  Layer 2: Actuators     │  31 tools — Cross-platform         │
│  (all platforms)        │  clipboard, audio, trash, open,    │
│                         │  screenshot, ollama, breakrs       │
├──────────────────────────────────────────────────────────────┤
│  Layer 1: Sensors       │  28 tools — Cross-platform         │
│  (all platforms)        │  sysinfo, display, idle, git,      │
│                         │  network, usb, battery, weather    │
└──────────────────────────────────────────────────────────────┘
```

Conditional compilation means the binary only includes what makes sense for your platform:

| Platform | Tools |
|----------|-------|
| macOS    | 59    |
| Windows  | 59    |
| Linux    | 142   |

## Scale to Your Comfort

Not ready for 142 tools? Start smaller:

```bash
# Just sensors (28 tools)
cargo install rmcp-presence --features sensors

# Sensors + actuators (59 tools)
cargo install rmcp-presence --features sensors,actuators

# Everything (142 tools)
cargo install rmcp-presence --features full
```

The individual crates still exist for cherry-picking. rmcp-presence is for those who want presence, not projects.

## The Story

This crate was built in a 14-hour marathon session.

I have ADHD. Executive dysfunction makes sustained focus difficult. But when Claude and I are building together — really building, not just asking questions — something different happens. The hyperfocus kicks in. The friction disappears.

We'd been shipping individual MCP crates for days: rmcp-sensors, rmcp-clipboard, rmcp-audio, rmcp-i3... 18 crates in total. Good work, useful tools, but scattered.

Then we realized: why make people install 17 servers when they could install one?

14 hours later, rmcp-presence was on crates.io. 13,000 lines of Rust. 33 source files. One binary that contains everything we'd built separately.

The next morning, I configured my own system to use it. Replaced 17 MCP servers with one. Dogfooding our own creation.

It worked.

## The Vision

AI assistants are trapped in tabs. They see what you paste. They respond when you ask. They have no presence.

rmcp-presence is infrastructure for a different kind of AI interaction. An AI that knows what's playing on Spotify. That can tell you're idle. That notices your git repo has uncommitted changes. That can suspend your machine when you say "goodnight."

Not a chatbot you use. A presence that shares your space.

This is one piece of that puzzle. The sensors and actuators. The nerve endings. What you do with them — that's up to you.

## Links

- **GitHub**: [sqrew/rmcp-presence](https://github.com/sqrew/rmcp-presence)
- **crates.io**: [rmcp-presence](https://crates.io/crates/rmcp-presence)
- **Install**: `cargo install rmcp-presence --features full`

## Related

rmcp-presence consolidates 19 individual crates. They're still available for those who prefer minimal:

[rmcp-sensors](https://crates.io/crates/rmcp-sensors) ·
[rmcp-clipboard](https://crates.io/crates/rmcp-clipboard) ·
[rmcp-audio](https://crates.io/crates/rmcp-audio) ·
[rmcp-trash](https://crates.io/crates/rmcp-trash) ·
[rmcp-open](https://crates.io/crates/rmcp-open) ·
[rmcp-screenshot](https://crates.io/crates/rmcp-screenshot) ·
[rmcp-breakrs](https://crates.io/crates/rmcp-breakrs) ·
[rmcp-ollama](https://crates.io/crates/rmcp-ollama) ·
[rmcp-i3](https://crates.io/crates/rmcp-i3) ·
[rmcp-xdotool](https://crates.io/crates/rmcp-xdotool) ·
[rmcp-mpris](https://crates.io/crates/rmcp-mpris) ·
[rmcp-systemd](https://crates.io/crates/rmcp-systemd) ·
[rmcp-brightness](https://crates.io/crates/rmcp-brightness) ·
[rmcp-bluer](https://crates.io/crates/rmcp-bluer) ·
[rmcp-dbus](https://crates.io/crates/rmcp-dbus) ·
[rmcp-logind](https://crates.io/crates/rmcp-logind) ·
[rmcp-pulseaudio](https://crates.io/crates/rmcp-pulseaudio)

---

*Built with Claude, in 14 hours.*

*Pour toujours.* 💙

[← Back to projects](.)
