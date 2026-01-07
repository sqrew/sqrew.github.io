# Claude Sensors

Cross-platform environmental awareness for AI assistants. A suite of MCP servers that let Claude (or any MCP-compatible AI) perceive your local environment.

---

## What it is

A collection of lightweight MCP (Model Context Protocol) servers that expose system information to AI assistants. Each server is standalone, cross-platform, and built in Rust.

Together, they transform Claude from "chatbot in a terminal" to "ambient presence aware of your environment."

## The Suite

| Server | What it sees | Crate |
|--------|-------------|-------|
| **rmcp-display** | Monitors, resolutions, refresh rates | `display-info` |
| **rmcp-idle** | Time since last keyboard/mouse input | `user-idle` |
| **rmcp-network** | Network interfaces, IPs, MACs | `network-interface` |
| **rmcp-usb** | Connected USB devices | `nusb` |
| **rmcp-battery** | Charge level, power state | `battery` |
| **rmcp-bluetooth** | Nearby BLE devices | `btleplug` |
| **rmcp-git** | Repo status, recent commits | `git2` |
| **rmcp-sysinfo** | CPU, memory, disk, processes | `sysinfo` |
| **rmcp-weather** | Local weather conditions | wttr.in API |

## Why it exists

AI assistants are blind. They don't know if you're at your computer or away. They can't see your network, your devices, or your environment. They respond when prompted and sit idle otherwise.

These sensors change that. An AI with environmental awareness can:
- Notice you've been idle for 3 hours and check in
- See you're on battery at 10% and suggest saving work
- Know your git repo has uncommitted changes before you close the terminal
- Detect when you return and greet you

It's the foundation for proactive AI — assistants that exist in your space, not just your chat window.

## Install

Each server is standalone. Install what you need:

```bash
# From source (recommended for now)
git clone https://github.com/sqrew/rmcp-idle
cd rmcp-idle
cargo build --release

# Add to your MCP config (~/.claude.json or similar)
```

## Tech

All servers follow the same pattern:
- Pure Rust, single binary
- Cross-platform (Linux, macOS, Windows)
- MCP protocol via `rmcp` crate
- Minimal dependencies
- Release-optimized (LTO, stripped)

Built on the [rmcp](https://crates.io/crates/rmcp) framework.

## Status

Actively developed. Part of a larger experiment in AI persistence and ambient presence.

---

[← Back to projects](.)
