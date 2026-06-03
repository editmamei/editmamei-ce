# Changelog

All notable changes to Editmamei are documented here. This file mirrors the changelog from the private source repo, scoped to user-facing changes (internal process notes are stripped).

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

(Empty — next commit appends here.)

---

## [0.3.1] — 2026-06-03

PATCH bump for cross-repo changelog sync infrastructure. No user-visible
behavior changes; the public CE changelog and the landing-page changelog
cards are now auto-generated from this file.

---

## [0.3.0] — 2026-06-03

**Structured versioning starts here.** This release rolls up ~45 commits
of accumulated work since the v0.2.0 placeholder into a single coherent
baseline. Going forward, every commit that changes user-visible behavior
(or fixes a user-visible bug) bumps version + appends to this file.

The npm `editmamei` package still publishes the `0.0.0-placeholder` stub
reserving the name. The first user-installable release lands at v1.0.0
(no public `0.3.0` tarball — this is a private-source-repo milestone).

### Added

#### New tools

- `photoshop_overview` — orientation brief returning workflow contract,
  capabilities map, verification primitives, escape-hatch policy, known
  gaps. CE tier, read-only, no Photoshop call. Reference from
  `photoshop_ping` description hits at session boot.
- `photoshop_get_layer_bounds_diff` — verification primitive returning
  a one-word verdict ("aligned" / "shifted right" / "layer too small")
  plus numeric deltas for placement-accuracy checks.
- `photoshop_compare_regions` — per-channel histogram stats for two
  rectangles. Single-pixel sampling via 1×1 rect.
- `photoshop_get_histogram` (Pro) — full-image or per-channel histogram
  with bin counts + mean + stdev + median.

#### New tools landing as `'dev'` tier (excluded from CE + Pro shipped bundles)

- `photoshop_apply_shadows_highlights` — destructive S/H wrapped in the
  auto-duplicate-first pattern.
- `photoshop_apply_lens_blur` — realistic depth-of-field blur with iris
  shape + specular + noise + depth-source control.
- `photoshop_apply_smart_sharpen` — modern detail-aware sharpening with
  three blur-removal modes + per-tonal-region fade.
- `photoshop_apply_reduce_noise` — luminance + color noise + sharpen +
  JPEG-artifact removal with optional per-channel control.
- `photoshop_apply_high_pass` — edge-detail extraction for advanced
  sharpening + dodge-and-burn masking.
- `photoshop_apply_color_lookup` — destructive bake form of Color Lookup
  adjustment (non-destructive AdjL form known blocked at the PS
  scripting layer; see [docs/20260603-color-lookup-limitation.md](docs/20260603-color-lookup-limitation.md)).
- `photoshop_apply_equalize` — Image > Adjustments > Equalize.

#### New `add_adjustment_layer` types

- Black & White, Color Balance, Photo Filter, Vibrance, Channel Mixer,
  Selective Color, Gradient Map.
- Exposure, Color Lookup (3DLUT), Invert.
- Posterize, Threshold.

### Changed

- **Auto-duplicate-first pattern for destructive filters.** Every
  filter runs on a copy of the active layer by default. Original is
  preserved; result payload carries `target_was_copy` /
  `target_layer_name` / `original_layer_name`. Pass
  `apply_to_active_layer: true` to bake into the original.
- **Background-layer auto-promote** on `photoshop_move_layer`,
  `photoshop_rotate_layer`, `photoshop_scale_layer`. Returns
  `background_promoted: true` so callers see the side effect; no more
  duplicate-then-transform dance.
- **`photoshop_move_layer` three positioning modes** — relative
  `delta_*` (original), absolute `absolute_*` (target bounds top-left),
  or `center_on_*` (target bounds center). Exactly one pair per call;
  mixing modes returns a validation error.
- **`photoshop_get_preview` annotations.** Optional `annotations` array
  overlays rectangles (by explicit bounds OR by layer name), guides,
  points, or selection markers on the preview. 90s timeout when
  annotations are present. Defensive cleanup of stale `__mcp_preview__`
  duplicate documents on each call.
- **`photoshop_list_actions` text content** now enumerates every set
  name and action name verbatim, quoted, so the LLM can copy strings
  directly into `photoshop_play_action` (pre-fix the response was just
  a count summary; names lived only in `structuredContent`).
- **`photoshop_ping` discovery signals** — now returns `version`
  (absorbs the removed `photoshop_get_version`), `custom_action_sets`,
  `user_templates`, and `open_documents` for session-start orientation.
  Description nudges the LLM to call `photoshop_overview` on
  open-ended tasks.
- **`photoshop_execute_script` moved CE → Pro.** Escape hatch is now
  Pro-tier only.
- **Tool descriptions tightened** for `photoshop_create_layer_mask`
  (leads with the frame-opening use case + tells the LLM not to write
  `Mk Chnl` AM scripts), `photoshop_compare_regions` (three "Reach for
  this when" scenarios), `photoshop_get_histogram` (clipping detection +
  exposure verification + neutral-gray + op-confirmation scenarios).
- **Tier system extended.** `'dev'` and `'none'` tiers added alongside
  `'community'` / `'pro'`. New tools default to `'dev'` until live-PS
  verification; promoted in the same commit as the verification
  record. See [docs/20260603-tool-tier-process.md](docs/20260603-tool-tier-process.md).
- **9 tools demoted to `'dev'`** —
  `photoshop_apply_color_lookup`, `_apply_lens_blur`,
  `_select_color_range`, `_create_layer_mask`,
  `_apply_shadows_highlights`, `_apply_smart_sharpen`,
  `_apply_reduce_noise`, `_apply_high_pass`, `_apply_equalize`. None
  has a confirmed working live invocation post-rewrite; promotion path
  requires a captured session-log success.

### Fixed

- **`photoshop_get_preview` with annotations failed on PS 27.x** —
  `selection.stroke()` raised "no tool called stroke"; fill on empty
  overlay layers raised "command 'fill' not available." Replaced
  stroke with 4-edge filled rectangles. Flatten the duplicate before
  drawing so fills land on pixel content.
- **`sTID is not a function` runtime error in 5 filter snippets** —
  `applyShadowsHighlights`, `applyLensBlur`, `applySmartSharpen`,
  `applyReduceNoise`, `applyHighPass` used the `sTID(...)` helper
  without ever interpolating `${helperFunctions}`. A guard test now
  fails the build if any snippet calls `sTID` / `cTID` without bringing
  the helpers into scope.
- **`photoshop_add_adjustment_layer` type=color_lookup on PS 27.x** —
  legacy `lookupType` enum + `name` string + `dither` enum descriptor
  returned "General Photoshop error." Rewrote 3DLUT path to use
  `LUT3DFileName` + boolean `dither`. Abstract / device_link branches
  preserve legacy emission (unverified). Subsequent Bundle S iterations
  (S.1 / S.2 / S.3 / S.4) eventually documented the AdjL form as
  unsupportable in current PS scripting and introduced an experimental
  BAKE form behind the `'dev'` tier.
- **`photoshop_list_actions` returned empty names on PS 27.x** —
  `charIDToTypeID('Nm  ')` no longer dispatches to the descriptor's
  name field. New fallback chain: `stringIDToTypeID('name')` →
  `charIDToTypeID('Nm  ')` → key-scan for any STRINGTYPE matching
  `/name/i`. Unresolved names surface as `(unnamed *)`.
- **Shadows/Highlights + Reduce Noise event IDs were forum-lore-wrong on
  PS 27.x.** ScriptListener capture on macOS PS v27.7.0 confirmed event
  IDs changed (`shadowHighlight` → `adaptCorrect`, `reduceNoise` →
  `denoise`), descriptor structures restructured (flat keys → nested
  sub-descriptors), types corrected (integer → unitDouble percentUnit
  for amount/width), key names corrected (`midtoneContrast` → `center`,
  `sharpenDetails` → `sharpen`, `jpegArtifact` → `removeJPEGArtifact`).
  The "preserve Adobe's typo `Raduis`" comment in the codebase was
  load-bearing fiction — the actual key is `radius`. Removed.
- **Four AM-descriptor corrections from ScriptListener audit** — fixes
  to `selective_color`, `channel_mixer`, shadows/highlights root-key
  shape, and one other path that had drifted from current PS dispatch.
- **PS 27.x cross-platform regressions** — `levels` and `curves`
  `Mk-with-values` rejected with generic "program error" → rewrote as
  bare `Mk` + post-`Mk` `setd T=Lvls` / `T=Crv ` with verified
  descriptor shapes. `Hst2` (not legacy `Hsrt`) for master Hue/Sat
  entries. em-dash normalization in group-name lookup. macOS top-level
  `return` mangling in custom scripts.
- **Layer-mask descriptor** + `EACCES` fallback for `TempDir` after
  prior `sudo` sessions leaked `TMPDIR`.
- **Platform resilience hardening** — timeout cleanup; modal-state
  detection; richer "PS modal dialog open" diagnostic on the kill-on-
  overflow path.
- **Three ExtendScript P1s from the 2026-05-30 launch-readiness
  review.**

### Security

- **NDJSON session telemetry hardened** — string args >2048 chars
  truncated with `…[truncated:N→M]` marker (configurable via
  `SessionLog.maxArgStringLen`, default 2048, 0 disables); schema_version
  field; structured warning surfacing on degraded states. `LOG_SCRIPT_ON_ERROR=1`
  prints a one-shot WARN so the env oddity is visible.

---

## [0.2.0] — 2026-05

Placeholder release reserving the `editmamei` (unscoped) and
`@editmamei/placeholder` npm package names. Ships a "Coming Soon" stub
only. Not installable for end-user editing.

The full editing surface, Templates system, CE/Pro build pipeline, and
license activation flow land in v1.0.0.

---

[Unreleased]: https://github.com/editmamei/editmamei-ce/compare/v0.3.1...HEAD
[0.3.1]: https://github.com/editmamei/editmamei-ce/releases/tag/v0.3.1
[0.3.0]: https://github.com/editmamei/editmamei-ce/releases/tag/v0.3.0
[0.2.0]: https://github.com/editmamei/editmamei-ce/releases/tag/v0.2.0
