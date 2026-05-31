# Changelog

All notable changes to Editmamei are documented here. This file mirrors the changelog from the private source repo.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

Pre-launch work toward v1.0. The npm package `editmamei` currently publishes the v0.2.0 placeholder reserving the name; the first user-installable release will be v1.0.0.

Changes landed in the private source repo since the placeholder release:

- **CLI** — `editmamei install / uninstall / status / help` subcommands. `install` registers the MCP server with Claude Desktop (other clients via manual config). `status` reports platform, Node version, detected Photoshop install, and Claude Desktop config state.
- **Cross-platform reliability (PS 27.x)** — descriptor-shape fixes for the levels and curves `Mk-with-values` paths, the create-from-selection mask descriptor on pixel layers, the `Hst2`-vs-`Hsrt` master Hue/Saturation key, em-dash normalization in group-name lookup, and macOS-specific top-level `return` mangling in custom scripts. The session log now surfaces structured error context (PS version, platform, script head) on failure.
- **Install-time hardening** — `TempDir.create` now falls back to a user-owned cache dir on `EACCES`/`EPERM` from the system temp dir (most often after a prior `sudo` session leaked `TMPDIR`). One-shot WARN logged so the env oddity is visible.
- **Pre-release audit (2026-05-30)** — 15-track FAANG-style launch-readiness review captured in the private source repo. Outcome and remediation plan inform the v1.0 scope.

None of the above is on npm yet — the placeholder is still what `npm install -g editmamei` resolves. The v1.0.0 release will bundle these changes into the first user-installable build.

---

## [0.2.0] — 2026-05

Placeholder release reserving the `editmamei` and `@editmamei/placeholder` npm package names. Not installable for end-user editing; ships only a stub that prints a "Coming Soon" message and points to [editmamei.com](https://editmamei.com).

The full editing surface, Templates system, CE/Pro build pipeline, and license activation flow land in v1.0.0.

---

[Unreleased]: https://github.com/editmamei/editmamei-ce/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/editmamei/editmamei-ce/releases/tag/v0.2.0
