# CLAUDE.md — editmamei-ce

This is the **public face** of Editmamei — the GitHub-hosted docs, README, CHANGELOG, and issue tracker. This repo does **not** contain the npm package source; the source is private and lives in `Editmamei/` next door.

General dev-sec-ops spine lives in the parent [CLAUDE.md](../CLAUDE.md). This file is the repo-specific extension.

## Layout

- `README.md` — front-door narrative pitch + install + feature breakdown + editions table + privacy claim. Hand-edited, not generated.
- `CHANGELOG.md` — user-facing release notes.
- `docs/` — `installation.md`, `getting-started.md`, `faq.md`, `pro-features.md`. Linked from `editmamei.com` and from in-product error messages.
- `.github/ISSUE_TEMPLATE/` — `bug_report.md`, `feature_request.md`, `config.yml`.
- No source code lives here.

## Editorial constraints (these are NOT cosmetic)

- **Public visibility.** Everything here is read by users, competitors, and search engines. Do not paste private internal terminology, in-progress feature names, security details, or anything from the source repo's comments/docs that isn't already user-facing.
- **The Privacy section in `README.md` is a promise.** It claims "runs entirely on your machine, no cloud, no telemetry, no transmission of your content" with one carved-out exception (Pro license validation: `{key, version, os}` ~every 7 days). Any docs change that describes a new cloud-touching feature must update that section *and* the upstream `Editmamei/` code must match. Drift = user-trust hit.
- **The editions table in `README.md` is the canonical Pro/CE feature split.** It must match [docs/pro-features.md](docs/pro-features.md), [../editmamei-web/](../editmamei-web/)'s pricing page, and [../Editmamei/src/core/tool-tiers.ts](../Editmamei/src/core/tool-tiers.ts). If they diverge, the source of truth is `tool-tiers.ts` — fix the docs to match the code.
- **Issues are public.** The bug-report template tells users not to paste license keys or sensitive paths. Don't remove that warning, and if you reference real-world bug examples, scrub identifying detail.

## Commands

This repo has no build pipeline. Edits are markdown only.

- No `npm test` (no `package.json`).
- Manual: render the changed `.md` in a previewer to catch table/link/code-block rot.
- Link check (manual): walk every internal link in the diff; docs link heavily to `editmamei.com/*` and to each other.

## Full-process triggers (in addition to the workspace-root list)

The full nine-step process is mandatory when:

- Editing `README.md`'s Privacy or Editions sections.
- Editing `docs/pro-features.md` (cross-repo drift risk — must reconcile with [../Editmamei/src/core/tool-tiers.ts](../Editmamei/src/core/tool-tiers.ts) and [../editmamei-web/](../editmamei-web/)'s pricing page).
- Editing `docs/installation.md` (any install-step change must hold against the actual install behavior in `../Editmamei/`; ideally verified against a real Photoshop install on Windows *and* macOS).
- Editing `.github/ISSUE_TEMPLATE/` — these gate user bug-report quality.

## Verify checklist

- Render every changed `.md` in a previewer; tables and code blocks visually OK.
- Every internal link resolves (other files in this repo, or live URLs).
- No private/internal terminology slipped in.
- Privacy claim still holds if any feature description changed.
- Editions table matches `../Editmamei/src/core/tool-tiers.ts`.

## QA subagent prompts (customize from the root template)

1. **Privacy and public-surface review** — "Scan the diff for: (a) any new claim about features, behavior, or data handling that may not match the actual `../Editmamei/` source; (b) leakage of internal terminology, in-progress feature names, or non-public roadmap; (c) any modification to the Privacy section of `README.md` or the issue templates' user-warning text. Severity low/med/high. Under 200 words."

2. **Cross-repo consistency** — "Verify any edition/feature/pricing copy in the diff stays consistent with `../Editmamei/src/core/tool-tiers.ts` (canonical Pro/CE split), `../editmamei-web/`'s pricing page, and `docs/pro-features.md`. Flag every place a claim in this diff cannot be backed by the source repo's actual surface. Under 200 words."

3. **Doc quality** — "Check for: broken internal links, table rot, code blocks that won't render, dead `editmamei.com/*` URLs, stylistic drift from the rest of the doc set, instructions that contradict each other across files. Under 200 words."
