# Installation

Editmamei is distributed as an npm package. The `editmamei install` subcommand handles MCP client registration for Claude Desktop, Cursor, and Claude Code automatically.

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

## Register with your MCP client

```bash
editmamei install
```

This subcommand:

1. Detects which MCP clients are installed on your system
2. Writes the appropriate config entry for each
3. Backs up any existing client config before modifying it

If you have multiple clients (e.g. Claude Desktop *and* Cursor), all of them get configured in one pass.

Restart your MCP client(s) after this completes — config changes only take effect on a fresh boot.

---

## Manual configuration (if `editmamei install` can't reach your client)

If `editmamei install` reports it couldn't write the config — for example, because you use a portable install of your MCP client — you can register Editmamei by hand.

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

## Activate Pro

If you have a Pro license:

```bash
editmamei license activate <your-license-key>
```

The Pro tool surface becomes available the next time your MCP client restarts. See [pro-features.md](pro-features.md) for what's included.
