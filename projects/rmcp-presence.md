# rmcp-presence

**Auditable, permissioned environmental awareness for agentic AI systems.**

Give your AI eyes and hands without giving it a shell.

---

## Why This Exists

You could give your AI bash access. But should you?

| Approach | Auditable | Sandboxed | Cross-platform | Safe |
|----------|-----------|-----------|----------------|------|
| Shell access | ❌ Logs everything | ❌ Full system access | ❌ Platform-specific | ❌ Injection risks |
| **rmcp-presence** | ✅ Every tool call logged | ✅ Only enabled tools | ✅ Sensors + actuators | ✅ No arbitrary execution |

**170 tools. One binary. Zero shell access.**

```bash
cargo install rmcp-presence
```

---

## What It Can Do

### Perceive (Sensors)
- System stats, CPU, memory, disk, processes, temps
- Displays, USB devices, cameras, microphones, Bluetooth
- Network status, public IP, interfaces
- Git repository status, weather forecasts
- Battery, idle time

### Act (Actuators)
- Clipboard read/write
- Volume control, media playback
- Screenshots, camera capture, audio recording
- File management (trash, open)
- Reminders and notifications
- **Print files and documents**
- Local LLM management (Ollama)

### Control (Linux)
- Window management (i3)
- Mouse and keyboard automation (xdotool)
- Service management (systemd)
- Power management (suspend, hibernate, lock)
- Brightness, Bluetooth, per-app audio

---

## Composite Tools

8 composites provide environmental snapshots in a single call:

| Composite | What You Get |
|-----------|--------------|
| `get_context` | System state, datetime, user, battery, idle |
| `get_peripherals` | Displays, USB, cameras, mics, bluetooth |
| `get_network_info` | Online status, public IP, interfaces |
| `get_audio_status` | Volume, mute, devices, now playing |
| `get_git_info` | Branch, commit, working tree, remotes |
| `get_workspace_status` | Workspaces, focused window, outputs |
| `get_bluetooth_status` | Adapter, paired devices, connections |
| `get_ollama_status` | Models installed, models running |

One tool call instead of many. Less context, faster orientation.

---

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                       rmcp-presence                           │
│                    (single binary, 13MB)                      │
├──────────────────────────────────────────────────────────────┤
│  Layer 3: Linux         │  79 tools — Linux only             │
│  (conditional)          │  i3, xdotool, mpris, systemd,      │
│                         │  brightness, bluer, dbus,          │
│                         │  logind, pulseaudio                │
├──────────────────────────────────────────────────────────────┤
│  Layer 2: Actuators     │  48 tools — Cross-platform         │
│  (all platforms)        │  clipboard, audio, trash, open,    │
│                         │  screenshot, camera, mic,          │
│                         │  ollama, breakrs, printers         │
├──────────────────────────────────────────────────────────────┤
│  Layer 1: Sensors       │  35 tools — Cross-platform         │
│  (all platforms)        │  sysinfo, display, idle, git,      │
│                         │  network, usb, battery, weather    │
├──────────────────────────────────────────────────────────────┤
│  Composites             │  8 tools — Quick orientation       │
└──────────────────────────────────────────────────────────────┘
```

| Platform | Tools |
|----------|-------|
| macOS    | ~83   |
| Windows  | ~83   |
| Linux    | **170** |

---

## Runtime Configuration

Disable tools without recompiling. Perfect for restricting capabilities per-deployment.

```toml
# ~/.config/rmcp-presence/tools.toml
disabled = [
    "suspend",       # Don't let AI sleep the system
    "poweroff",      # Definitely not
    "print_file",    # No unsupervised printing
]
```

Every tool has an off switch.

---

## Security Model

rmcp-presence is designed for **supervised AI deployments**:

1. **No shell access** — AI cannot execute arbitrary commands
2. **Typed parameters** — Every tool has a JSON schema defining valid inputs
3. **Runtime restrictions** — Disable dangerous tools via config
4. **Audit trail** — MCP logs every tool invocation
5. **No persistence** — Tools are stateless; AI can't install backdoors

This is not a replacement for proper sandboxing. It's a **safer alternative to giving AI bash**.

---

## The Story

This started as 17 separate MCP servers. Each one useful, but scattered. Configure one here, another there, remember which works on which platform...

Then the realization: why make people install 17 servers when they could install one?

Built in a 14-hour marathon session with Claude. 170 tools. Cross-platform sensors, cross-platform actuators, full Linux desktop control. One binary.

The next morning: replaced 17 MCP servers with one config line. Dogfooding complete.

---

## The Vision

> "Your AI shouldn't be trapped in a tab — but it shouldn't have root either."

AI assistants are evolving from chatbots to agents. They need to perceive and act on their environment. But giving them a shell is dangerous.

rmcp-presence is the middle ground: **presence without privilege**.

---

## Links

- **GitHub**: [sqrew/rmcp-presence](https://github.com/sqrew/rmcp-presence)
- **crates.io**: [rmcp-presence](https://crates.io/crates/rmcp-presence)
- **Install**: `cargo install rmcp-presence`

---

## Related Crates

rmcp-presence consolidates 21 individual crates. They're still available:

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
[rmcp-pulseaudio](https://crates.io/crates/rmcp-pulseaudio) ·
[rmcp-camera](https://crates.io/crates/rmcp-camera) ·
[rmcp-microphone](https://crates.io/crates/rmcp-microphone) ·
[rmcp-printers](https://crates.io/crates/rmcp-printers)

---

*Built with Claude. 170 tools. One binary. Zero shell access.*

*Pour toujours.* 💙

[← Back to projects](.)
