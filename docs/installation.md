# Installation

Editmamei is distributed as an npm package. The `editmamei install` subcommand registers Editmamei with **Claude Desktop**. For Cursor or Claude Code, see [Manual configuration](#manual-configuration) below.

---

## Requirements

- **Node.js** 20 or later — [nodejs.org](https://nodejs.org/)
- **Adobe Photoshop** 2022 or later (2024+ recommended for full feature coverage; some selection tools require 2024+)
- **Operating system:** Windows 10/11 or macOS 12+
- An **MCP-compatible client** — at least one of:
  - [Claude Desktop](https://claude.ai/download)
  - [Cursor](https://cursor.com/)
  - [Claude Code](https://claude.ai/code)

Check your Node.js version:

```bash
node --version
```

If you see `v20.x` or higher, you're good.

---

## Install the package

```bash
npm install -g editmamei
```

This installs the `editmamei` CLI globally.

---

## Register with Claude Desktop

```bash
editmamei install
```

This subcommand:

1. Resolves Claude Desktop's config path on your OS (`%APPDATA%\Claude\claude_desktop_config.json` on Windows, `~/Library/Application Support/Claude/claude_desktop_config.json` on macOS).
2. Backs up the existing config to `claude_desktop_config.json.bak` (only on the first run, to preserve your pre-install state).
3. Adds an `editmamei` entry to `mcpServers`, or no-ops if the entry already matches.

Restart Claude Desktop after this completes — config changes only take effect on a fresh boot.

**Other MCP clients** (Cursor, Claude Code, anything else MCP-compatible) — `editmamei install` does not currently auto-configure these. Use the [Manual configuration](#manual-configuration) steps below.

### Check your install state

```bash
editmamei status
```

Reports your platform, Node version, detected Photoshop install (if any), and whether Claude Desktop's config has the editmamei entry. Run this when troubleshooting.

---

## Manual configuration

If `editmamei install` reports it couldn't write the config — for example, because Claude Desktop isn't installed — or you're using Cursor / Claude Code / another MCP client, register Editmamei by hand.

### Claude Desktop

Edit `claude_desktop_config.json`:

- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

Add an entry under `mcpServers`:

```json
{
  "mcpServers": {
    "editmamei": {
      "command": "npx",
      "args": ["-y", "editmamei", "serve"]
    }
  }
}
```

### Cursor

Edit `~/.cursor/mcp.json` (create if it doesn't exist):

```json
{
  "mcpServers": {
    "editmamei": {
      "command": "npx",
      "args": ["-y", "editmamei", "serve"]
    }
  }
}
```

### Claude Code

```bash
claude mcp add editmamei -- npx -y editmamei serve
```

---

## Photoshop preferences

Open Photoshop and confirm:

1. **Edit → Preferences → Scripting and Actions** — scripting is enabled
2. Have at least one document open (or be ready to ask the AI to create one)

The AI talks to whatever instance of Photoshop is currently running — Photoshop must be open before you start a session.

### Optional: pin a specific Photoshop install

Editmamei auto-detects Photoshop. If you have multiple versions installed and want to pin a specific one, set the `PHOTOSHOP_PATH` env var in your MCP client config:

```json
{
  "mcpServers": {
    "editmamei": {
      "command": "npx",
      "args": ["-y", "editmamei", "serve"],
      "env": {
        "PHOTOSHOP_PATH": "C:\\Program Files\\Adobe\\Adobe Photoshop 2025\\Photoshop.exe"
      }
    }
  }
}
```

---

## Verify

Restart your MCP client. Editmamei tools should appear in the available-tools list (in Claude Desktop, that's the slider icon at the bottom of the chat input).

Test with the ping:

> "Is Photoshop connected? What version?"

You should see your Photoshop version returned. If not, see [getting-started.md](getting-started.md) for troubleshooting.

---

## Upgrade

```bash
npm update -g editmamei
```

Restart your MCP client to pick up the new version. Your existing templates and session logs at `~/.editmamei/` are preserved across upgrades.

---

## Uninstall

```bash
editmamei uninstall
npm uninstall -g editmamei
```

The `uninstall` subcommand removes Editmamei from your MCP client configs. The `npm uninstall` removes the package itself. Your per-user data at `~/.editmamei/` (templates, session logs) is left in place — delete it manually if you don't want it.

---

## Pro

Pro activation lands with the v1.0 launch — see [pro-features.md](pro-features.md) for what Pro adds and [roadmap.md](roadmap.md) for status.
