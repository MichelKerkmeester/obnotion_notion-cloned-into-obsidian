---
title: "sk-code-obsidian: Note Database Plugin Evidence"
description: "The read-only evidence base a code workflow loads for the Note Database Obsidian plugin: its measured source conventions and design system."
trigger_phrases:
  - "local-obsidian"
  - "note database plugin evidence"
  - "obsidian plugin conventions"
version: 1.0.1.0
---

# local-obsidian: Note Database Plugin Evidence

The read-only evidence base a code workflow loads when it works on the **Note Database** Obsidian
plugin. It carries the plugin's real, measured source conventions: and honestly labels the ones a
later phase still has to adopt: so implementation, quality, review, debug, and verify work honor the
same single-stylesheet model, `.db-*` class grammar, and verification gate the plugin actually ships
under, instead of treating the repository as generic, unowned Node.js.

---

## 1. AT A GLANCE

| | |
| --- | --- |
| **Skill kind** | Standalone evidence skill (read-only domain evidence) |
| **Backend** | `evidence-base` |
| **Tools** | read-only: `Read, Bash, Grep, Glob`; mutates nothing |
| **Advisor** | routed directly; the local advisor returns `local-obsidian` on a matching prompt |
| **Detected surface** | `OBSIDIAN`: `manifest.json` (`minAppVersion`), `esbuild.config.mjs`, `from "obsidian"` imports, `.db-*` classes in `styles.css` |
| **Applies with** | a code workflow, e.g. `sk-code-quality` or `sk-code-review` |
| **Measured baseline** | `tsc --noEmit` clean · `build` clean · `vitest run` 386/49 files · `screenshots:verify` 180 entries · `lint` 115 known problems (not clean) |

---

## 2. OVERVIEW

### Why This Surface Exists

Before this packet, code work on the Note Database plugin detected as **UNKNOWN**: a generic
TypeScript/Node.js tree with no Webflow or `.skilled/` markers fell through surface
detection, so the hub could not auto-load the plugin's Obsidian API boundary, its single-stylesheet
grammar, its screenshot-fixture harness, or its real verification gate. This surface closes that gap:
OBSIDIAN now detects, and the plugin's design-system evidence loads with the workflow.

### What It Carries

- **The Obsidian API boundary**: the `Plugin`/`FileView`/`WorkspaceLeaf` surface `main.ts` consumes,
  and the `manifest.json` compatibility contract (`minAppVersion`, `isDesktopOnly: false`).
- **The single-stylesheet model**: `styles.css` as the one stylesheet (18,931 lines measured), the
  `.db-*` class grammar (1,196 distinct classes, 769 orphaned, 427 referenced by fixtures), and no
  component-scoped styles anywhere in the tree.
- **The screenshot-fixture harness contract**: hand-written fixture markup rendered against the real
  stylesheet in headless Chrome, not the live renderers; the freshness gate; and the stand-in files
  (`theme.css`, `runtime-vars.css`) for what Obsidian supplies at runtime.
- **The measured verification gate**: the exact commands and their current pass/fail counts, including
  the honest, non-zero lint baseline.
- **The target source conventions**: kebab-case naming, `MODULE:` banners, and paired folder docs,
  each labeled against the measured tree so a workflow never claims an unshipped convention is real.

### The Design-System Evidence Layer

This plugin ships no separate design-system document or live
catalog. The **evidence for this surface is the plugin source itself**: `styles.css` (the class
grammar), `src/views/screenshot-fixtures.test.ts` (the invented-class guard), and
`tools/screenshots/manifest.json` (the capture freshness record) are the sources of truth this packet's
`references/` distill rather than duplicate.

### The Obsidian Plugin Repository

The Note Database plugin is a **separate repository from this skills hub**: the **Obsidian Plugin
repo**. Its source (`src/`, `tools/`), its `specs/`, `manifest.json`, and `styles.css` all live there;
it is the source of truth. This skill is that repo's read-only convention mirror.
When a code workflow runs on the plugin, the Obsidian Plugin repo is the working tree, so every
`src/…`, `tools/…`, and `styles.css` path this packet cites resolves there: not against this hub. The
Obsidian Plugin repository additionally symlinks `.opencode`, `.claude`, `.codex`, `.cursor`, and
`.devin` at its root back to this hub, so detection resolves OBSIDIAN by a resolved-realpath check
rather than a literal `.opencode/` path match.

---

## 3. QUICK START

A workflow bundling this surface should, in order:

1. Read `references/obsidian-plugin-api.md` to confirm which Obsidian API a change touches and
   whether `manifest.json`'s `minAppVersion`/`isDesktopOnly` contract constrains it.
2. Read `references/db-class-naming.md` before adding, renaming, or citing any `.db-*` class: never
   invent one; `src/views/screenshot-fixtures.test.ts` fails any class absent from `styles.css` and
   `src/`.
3. Make the change, then run `references/verification.md`'s gate, `npx tsc --noEmit`,
   `npm run build`, `npx vitest run`, and `npm run screenshots:verify`: and open any changed PNG
   before claiming the change is done.

---

## 4. HOW IT WORKS

### How Routing Works

`local-obsidian` is a standalone skill. The local advisor scores a prompt against the trigger phrases
and intent signals in its `graph-metadata.json` and returns it directly. No other skill detects it,
bundles it or projects its routing map.

### Detection Markers

Detection is local and deterministic: `manifest.json` carrying `minAppVersion`, `esbuild.config.mjs`,
`from "obsidian"` imports, and `.db-*` classes in `styles.css`, resolved against the CWD and the
changed or target files. A resolved-realpath check catches the plugin repository's own paths, which
would otherwise read as generic Node.js.

---

## 5. INTEGRATION & NAVIGATION

### When This Skill Applies

Use it when the task works on the **Note Database** plugin's source tree, its `styles.css` class
grammar, its screenshot harness or its verification gate, and the phase needs the plugin's
design-system evidence. The workflow that applies it owns the edit; this skill supplies the evidence.

### Related Skills

- `../local-webflow`, `../sk-code/sk-code-skilled`: sibling read-only evidence skills.
- `../sk-code/sk-code-quality`, `../sk-code/sk-code-review`: workflows that apply this evidence.
- `../sk-code/shared/`: the implement → debug → verify doctrine this skill carries a copy of.
- `system-spec-kit` (spec folders + memory), `sk-git` (worktrees, commits, finish).

---

## 6. FAQ

**Does this surface edit the plugin?** No: it is read-only evidence. The bundled workflow mode makes
edits; this surface tells it the rules.

**Can I add a new `.db-*` class from here?** You add it in the plugin's `styles.css` (through the
workflow mode); this surface tells you the naming grammar and the fixture-coverage check a new class
must pass. Never cite or invent a class this surface cannot find in `styles.css` or `src/`.

**Why is a screenshot not proof?** `npm run screenshots:verify` only checks that a capture's sources
have not changed since it was taken: it does not check what the PNG shows, and the harness renders
hand-written fixture markup, not the live plugin. See `references/screenshot-harness.md`.

**Are the `MODULE:` banners and kebab-case names real yet?** No. §3b of `SKILL.md` states plainly that
these are target conventions a later phase adopts; the measured tree today is PascalCase-dominant with
no banners and no folder docs.

**Is it advisor-routable?** Yes. The local advisor returns `local-obsidian` when a prompt matches its
trigger phrases or intent signals.

---

## 7. VERIFICATION

- Skill shape: one skill-root `graph-metadata.json` with `skill_id: local-obsidian`, read-only
  `allowed-tools`, and no nested identity file below the root.
- Link audit: every relative link under this skill resolves.
- Routing: `node .skilled/bin/skill-advisor.cjs advisor_recommend --json '{"prompt":"obsidian plugin code"}' --format json` returns `local-obsidian` first.

---

## 8. RELATED DOCUMENTS

- `SKILL.md`: the skill contract, reference map, machine-readable routing, and standards.
- `references/`: the Obsidian API boundary, single-stylesheet ownership, `.db-*` grammar, screenshot
  harness, verification gate, and the target source conventions.
- `changelog/`: release notes for this packet.
- `../sk-code/`: the public hub, which no longer carries this skill's routing.
- The **Obsidian Plugin repo**: the plugin source, `manifest.json`, and `styles.css`. This surface is
  its read-only evidence mirror; see §2 "The Obsidian Plugin Repository".
- `specs/sk-code/007-sk-code-obsidian-surface/`: this surface's own spec packet, which lives beside the
  skill in this repository rather than in the plugin repo it mirrors.
