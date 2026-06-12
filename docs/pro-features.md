# Pro features

Editmamei ships in two editions from a single npm package. The same `editmamei` install gives you Community by default; activating a Pro license unlocks the Pro tool surface on the next AI-client restart.

Both editions follow the same rule: AI orchestration, not generation. Photoshop edits with its own real tools; Pro just gives the AI a deeper toolkit to orchestrate.

This page describes the line between Community and Pro as it stands today. The split may evolve over time — see [editmamei.com/pricing](https://editmamei.com/pricing) for the current state.

---

## Activating Pro

Pro activation ships with the v1.0 launch — the license CLI and purchase flow are part of that release. See [roadmap.md](roadmap.md) for current status. Purchasing and pricing will be at [editmamei.com/pricing](https://editmamei.com/pricing) once Pro is available.

---

## Community Edition — what's included free

The Community edition covers the core editing surface most photographers need day to day:

- **Documents** — create, open, save layered PSDs, export JPEG/PNG, close, crop, resize
- **Layers** — create, duplicate, delete, rename, reorder, group, merge, flatten, stamp visible
- **Layer properties** — opacity, blend mode, visibility, locking, rasterize
- **Non-destructive adjustments** — Curves, Levels, Hue/Saturation, Brightness/Contrast as adjustment layers (an active selection becomes the new layer's mask automatically)
- **Filters & tonal tools** — Gaussian Blur, Motion Blur, Sharpen, Smart Sharpen, Reduce Noise, High Pass, Add Noise, Shadows/Highlights, Equalize — destructive ops run on an auto-duplicated layer by default so the original is preserved
- **Selections** — Magic Wand, Rectangle, Select All, Deselect, Invert, Feather — every selection returns rich feedback (area, edge complexity, pixel counts) plus a selection preview
- **Layer masks** — create from selection, apply, delete
- **Layer styles** — drop shadow, stroke, outer glow
- **Templates** — list, apply, verify, and recall saved templates from `~/.editmamei/templates/`. Templates are bundles that pair a markdown recipe with before/after previews and the captured tool-call evidence; CE applies and verifies them, Pro creates them. (Templates are saved per-machine — a fresh install starts with an empty library until you author one with Pro or copy a bundle in.)
- **History** — undo, redo, inspect history states
- **Visual verification** — downscaled preview JPEGs returned inline, layer-bounds diffs, region comparison, and 256-bin per-channel histograms with mean / stdev / median
- **Document insight** — camera metadata (make, model, lens, ISO, focal length, GPS), ACR develop settings, full layer tree as JSON, capability overview
- **Text** — create text layers, set font / size / color / alignment, update content
- **Image placement** — place image files into the document

This is enough to drive a full landscape or product editing workflow in conversation with your AI.

---

## Pro Edition — what's added

Pro extends the surface for professional non-destructive workflows, batch editing, retouch power tools, and reproducible aesthetic recipes.

### Templates system — authoring side

A template is a reproducible aesthetic recipe — capture the current edit as a named bundle, then apply it later to new images. The authoring side is Pro; applying, verifying, and recalling templates is Community.

- `photoshop_template_create_evidence` — gathers session evidence (tool calls, history states, metadata snapshot) and renders before/after previews
- `photoshop_template_save` — saves the template bundle to `~/.editmamei/templates/<slug>/`, optionally with a machine-checkable style signature
- `photoshop_template_delete` — removes a saved template

CE users get `photoshop_template_apply`, `photoshop_template_list`, `photoshop_template_verify`, and `photoshop_template_recall` (above, under Community); they can apply and verify any template at `~/.editmamei/templates/<slug>/` but cannot create, save, or delete templates. Pro adds the authoring tools so editing decisions become repeatable rather than one-shots — the AI uses its own captured reasoning to drive the existing pipeline tools on a new image, self-judging against the template's exit criteria.

### Sensei-backed selections

- `photoshop_select_subject` — Sensei-backed subject isolation
- `photoshop_select_sky` — Sensei-backed sky selection

Every selection returns a rich feedback bundle — area coverage, edge complexity, partial-vs-full pixel counts — so the AI can verify a selection actually grabbed what was intended before committing. The non-Sensei selection tools (Magic Wand, rectangle, feather) are in Community.

### Content-aware retouch

- `photoshop_apply_content_aware_fill` — fill a selection from its surroundings
- `photoshop_apply_patch` — Patch-tool-style repair from a sampled region
- `photoshop_apply_content_aware_move` — move a selected object and heal the source

Photoshop's premium retouch moves, driven against a selection the AI can verify first.

### Layer transforms

- `photoshop_move_layer`, `photoshop_scale_layer`, `photoshop_rotate_layer`, `photoshop_fit_layer_to_document`

Free transforms on the active layer (background layers are auto-promoted instead of erroring).

### Actions and scripting

- `photoshop_list_actions` / `photoshop_play_action` — enumerate and play your recorded Photoshop Actions
- `photoshop_execute_script` — the escape hatch: arbitrary ExtendScript when no specific tool fits

### Coming in Pro post-v1.0

Roadmap items live in [`roadmap.md`](roadmap.md). Highlights:

- Smart Objects with editable contents and Smart Object lifecycle tools
- Smart Filters
- Channels and alpha-channel storage
- Vector paths and vector masks
- Complete adjustment-layer type catalog (Color Balance, Vibrance, Photo Filter, Selective Color, Channel Mixer, Gradient Map)
- `photoshop_refine_selection_edge`
- Advanced transforms (skew / perspective / distort, selection-modify primitives)

When this lands, Editmamei Pro becomes a complete AI-driven non-destructive editor.

---

## Pricing

Pricing and purchase options will be published at [editmamei.com/pricing](https://editmamei.com/pricing) when Pro launches.
