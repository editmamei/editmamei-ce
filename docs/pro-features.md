# Pro features

Editmamei ships in two editions from a single npm package. The same `editmamei` install gives you Community by default; activating a Pro license unlocks the Pro tool surface on the next MCP-client restart.

This page describes the line between Community and Pro at v1.0. The split may evolve over time — see [editmamei.com/pricing](https://editmamei.com/pricing) for the current state.

---

## Activating Pro

```bash
editmamei license activate <your-license-key>
```

Restart your MCP client. The Pro tools appear in the available-tools list.

To check current license status:

```bash
editmamei license status
```

To deactivate (e.g. when moving to a new machine):

```bash
editmamei license deactivate
```

Purchase a Pro license at [editmamei.com/pricing](https://editmamei.com/pricing).

---

## Community Edition — what's included free

The Community edition covers the core editing surface most photographers need day to day:

- **Documents** — create, open, save, export, close, crop, resize
- **Layers** — create, duplicate, delete, rename, reorder, group, merge, flatten
- **Layer properties** — opacity, blend mode, visibility, locking
- **Basic adjustments** — Brightness/Contrast, Hue/Saturation, Auto Levels, Auto Contrast as direct bakes
- **Filters** — Gaussian Blur, Motion Blur, Sharpen, Add Noise
- **Selections** — Rectangle, Select All, Deselect, Invert, Feather
- **Layer masks** — create from selection, apply, delete
- **History** — undo, redo, get history states, jump to state
- **Actions** — list, play recorded Photoshop Actions
- **Preview & inspection** — downscaled preview JPEG, per-channel histogram, document info, layer tree
- **Text** — create text layers, set font / size / color / alignment
- **Image placement** — place files, fit to document
- **Escape hatch** — `photoshop_execute_script` for arbitrary ExtendScript when no specific tool fits

This is enough to drive a full landscape or product editing workflow in conversation with your AI.

---

## Pro Edition — what's added

Pro extends the surface for professional non-destructive workflows, batch editing, and reproducible aesthetic recipes.

### Templates system

The Templates system is the headline Pro feature. A template is a reproducible aesthetic recipe — capture the current edit as a named bundle, then apply it later to new images.

- `photoshop_template_create_evidence` — gathers session evidence (tool calls, history states, metadata snapshot) and renders before/after previews
- `photoshop_template_save` — saves the template bundle to `~/.editmamei/templates/<slug>/`
- `photoshop_template_apply` — applies a saved template, the AI reads its own previous reasoning to recreate the look
- `photoshop_template_list` / `photoshop_template_delete`

Templates are how editing decisions become repeatable rather than one-shots. The AI uses its own captured reasoning to drive the existing pipeline tools on a new image, self-judging against the template's exit criteria.

### Non-destructive adjustment layers

Curves, Levels, Hue/Saturation, Brightness/Contrast as **adjustment layers** rather than direct bakes — editable, maskable, removable. Selection-aware: an active selection at call time becomes the new adjustment's mask automatically. Clip-to-below for targeted edits.

### Smart selection surface

- `photoshop_select_subject` — Sensei-backed subject isolation
- `photoshop_select_sky` — Sensei-backed sky selection
- `photoshop_select_color_range` — selection by color similarity
- `photoshop_magic_wand_select` — contiguous-pixel selection by tolerance
- `photoshop_refine_selection_edge` — edge refinement with smoothing, contrast, shift

Every selection returns a rich feedback bundle — area coverage, edge complexity, partial-vs-full pixel counts — so the AI can verify a selection actually grabbed what was intended before committing.

### Layer styles

Drop shadow, stroke, outer glow, inner shadow as full layer styles — settings exposed, editable after creation.

### Advanced transforms

Free-transform with skew / perspective / distort. Selection-modify primitives (expand / contract / smooth / border).

### Coming in Pro post-v1.0

The Pro roadmap (see [editmamei.com/roadmap](https://editmamei.com/roadmap)):

- Smart Objects with editable contents
- Smart Filters
- Channels and alpha-channel storage
- Vector paths and vector masks
- Complete adjustment-layer type catalog (Color Balance, Vibrance, Photo Filter, Selective Color, Channel Mixer, Gradient Map)

When this lands, Editmamei Pro becomes a complete AI-driven non-destructive editor.

---

## Pricing

Current tiers at [editmamei.com/pricing](https://editmamei.com/pricing). Pro is available as a subscription or a one-time lifetime license.

Pro subscribers get:

- Priority email support at `support@editmamei.com`
- Early access to new Pro tools as they ship
- A vote in roadmap prioritization through the Pro community Discord (planned)
