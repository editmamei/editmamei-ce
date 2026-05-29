# Editmamei

**The AI-Superpowered Photoshop Automation Bridge.**

*(Pronounced like* edamame*. Yes, the snack.)*

[![npm version](https://img.shields.io/npm/v/editmamei.svg)](https://www.npmjs.com/package/editmamei)
[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-red.svg)](https://editmamei.com/license)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS-lightgrey.svg)]()

Editmamei is a Model Context Protocol (MCP) server that lets your AI assistant — Claude Desktop, Cursor, Claude Code, and any other MCP-compatible client — drive Adobe Photoshop directly. Open documents, build non-destructive layer stacks, apply adjustments, run smart selections, save reusable editing recipes, and export final deliverables, all through natural-language conversation with your AI.

This repository is the **public face** of Editmamei. It hosts the user-facing documentation, the issue tracker, and the changelog. The source for the npm package itself is private; what you install from npm is the same compiled artifact described in these docs.

- **Website:** [editmamei.com](https://editmamei.com)
- **Install:** see [docs/installation.md](docs/installation.md)
- **Pro features:** see [docs/pro-features.md](docs/pro-features.md)
- **Report a bug:** [open an issue](https://github.com/editmamei/editmamei-ce/issues/new/choose)

---

## Quick install

```bash
npm install -g editmamei
editmamei install
```

Then restart your MCP client (Claude Desktop, Cursor, Claude Code) and ask it to ping Photoshop:

> "Is Photoshop connected?"

The AI calls `photoshop_ping` and you'll see your Photoshop version returned. Full setup walkthrough in [docs/getting-started.md](docs/getting-started.md).

---

## Requirements

- **Node.js** 20 or later
- **Adobe Photoshop** 2022 or later (2024+ recommended)
- **Operating system:** Windows 10/11 or macOS 12+
- An **MCP-compatible AI client** — Claude Desktop, Cursor, Claude Code, or any other MCP host

---

## What it does

Editmamei exposes ~80 Photoshop operations as MCP tools — each with structured input/output, schema validation, and context awareness. Your AI calls them as primitives in service of whatever you actually want to do. The result is Photoshop that responds to *"make the sky more dramatic but keep the foreground natural"* instead of *Layer → New Adjustment Layer → Curves → drag the curve up at the highlight end.*

- **Documents** — open, save, export, close; full format coverage (PSD, JPEG, PNG, TIFF, DNG, HEIC, raw)
- **Layers** — create, duplicate, delete, rename, reorder, group, merge, flatten; opacity, blend mode, visibility, locking
- **Smart selections** — Select Subject, Select Sky, Color Range, Magic Wand, plus rectangle/feather; rich selection feedback
- **Non-destructive adjustments** — Curves, Levels, Hue/Saturation, Brightness/Contrast as adjustment layers
- **Filters** — Gaussian Blur, Motion Blur, Sharpen, Add Noise; layer styles (drop shadow, stroke, outer glow)
- **Templates** — save the current edit as a reproducible recipe; apply later to new images
- **Visual verification** — downscaled preview JPEGs and per-channel histograms returned inline
- **History & Actions** — undo, redo, jump to state; play recorded Photoshop Actions

Full feature breakdown at [editmamei.com](https://editmamei.com).

---

## Editions

| | Community | Pro |
|---|---|---|
| Core editing surface (documents, layers, basic adjustments, filters, selections, masks) | ✅ | ✅ |
| Templates system | | ✅ |
| Full non-destructive workflow surface | | ✅ |
| Smart Object lifecycle tools | | ✅ |
| Expanded adjustment-layer types | | ✅ |
| Advanced selection refinement | | ✅ |
| Channels, paths, vector masks | | ✅ |
| Priority support | | ✅ |

Detailed comparison and pricing at [editmamei.com/pricing](https://editmamei.com/pricing). See [docs/pro-features.md](docs/pro-features.md) for the full Pro tool list.

---

## Privacy

Editmamei runs entirely on your machine. There is no cloud, no telemetry, no transmission of your content. Photos, documents, templates, and metadata stay local. Photoshop does the work; Editmamei translates between your AI's intent and Photoshop's scripting API.

The one exception is Pro license validation, which sends a license key + Editmamei version + OS to the license server roughly once per 7 days. It contains no document, image, file path, or template data. Full privacy policy at [editmamei.com/privacy](https://editmamei.com/privacy).

---

## Issues & support

**Bug reports and feature requests** belong in [this repo's issue tracker](https://github.com/editmamei/editmamei-ce/issues). Before opening one, please read the templates — they tell you what to include for a fast triage.

**Important:** issues are public. **Do not paste your license key, full file paths from sensitive projects, or screenshots of unfinished client work.** The bug report template tells you what's safe to share.

For account, billing, or license issues, email [support@editmamei.com](mailto:support@editmamei.com) (Pro subscribers) — those don't belong in a public issue.

For security disclosures, see [editmamei.com/security](https://editmamei.com/security).

---

## Documentation

- [Installation](docs/installation.md) — setup for Windows and macOS, all supported MCP clients
- [Getting started](docs/getting-started.md) — first session, ping test, first real edit
- [Pro features](docs/pro-features.md) — what's in Pro that's not in CE, with pricing link
- [FAQ](docs/faq.md) — common questions

Full reference docs live at [editmamei.com/docs](https://editmamei.com/docs).

---

*Pairs well with: a layered PSD, a willing AI, and a small bowl of edamame.*
