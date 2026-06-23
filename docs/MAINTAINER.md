# Maintainer Guide — SCANERATOR / Guide de maintenance — SCANERATOR / Guía de mantenimiento — SCANERATOR

> This file is the architectural memory of the project. Update it whenever architecture, integrations, or gotchas change. / Ce fichier est la mémoire architecturale du projet. Mettez-le à jour à chaque changement d'architecture, d'intégration ou de contrainte. / Este archivo es la memoria arquitectónica del proyecto. Actualícelo cuando cambien la arquitectura, las integraciones o las restricciones.

---

## Stack

- **Frontend**: Vanilla HTML + JavaScript (no framework, no build step)
- **Backend**: None — fully static single-file app
- **Data layer**: None — no persistence; all state is in-memory per session
- **Key third-party libs**:
  - [JsBarcode 3.11.6](https://github.com/lindell/JsBarcode) — Code 128 generation (SVG), loaded from cdnjs CDN
  - [bwip-js 4.5.1](https://github.com/metafloor/bwip-js) — QR Code + Data Matrix generation (Canvas), loaded from cdnjs CDN

Both libraries are loaded from CDN on first use and cached by the browser — the app works offline afterwards.

## Repository layout

```
scanerator/
├── index.html        — The entire application (HTML + CSS + JS, single file)
├── README.md         — User-facing documentation (trilingual FR/EN/ES)
├── CONTRIBUTING.md   — Contributor guide (trilingual FR/EN/ES)
└── docs/
    ├── WORKFLOW.md   — Session process, branching, versioning
    ├── MAINTAINER.md — This file
    ├── BACKLOG.md    — Pending work items
    ├── DONE.md       — Completed items archive
    └── Bugs.md       — Confirmed bugs
```

## Branch strategy

| Branch | Purpose |
| --- | --- |
| `main` | Stable, smoke-tested releases — auto-deployed to GitHub Pages |
| `feature/*` | Feature branches — merge to `main` after smoke test |

See `WORKFLOW.md` for the full process.

## Dev setup

No prerequisites. No install step.

```bash
# Open directly in browser (Windows)
start index.html
```

First load requires an internet connection (CDN libs). Subsequent loads work offline.

## Architecture

### Single-file structure

The entire app is one IIFE in `index.html`. There is no module system, no bundler, no transpiler. Structure inside the IIFE:

1. `i18n` object — all translatable strings for `fr`, `en`, and `es`
2. `setLang` / `applyTranslations` — language switching
3. DOM element references
4. Scenario tab logic (Libre / Réception / Picking)
5. Scenario builders (`buildReceptionText`, `buildPickingText`)
6. Format radio + dimension controls
7. Charset toggles + mode pills (Aléatoire / Séquentiel)
8. `generate()` — parses textarea, renders preview + print area
9. Event listeners for all buttons

### i18n (trilingual: FR / EN / ES)

All user-visible strings live in `i18n.fr`, `i18n.en`, and `i18n.es` inside the script. The `data-i18n="key"` attribute on HTML elements is used by `applyTranslations()` to update `innerHTML`. Input placeholders are updated separately (no `data-i18n` on inputs — they're set explicitly in `applyTranslations`).

**Rule:** every new user-visible string must have `fr`, `en`, and `es` entries. The corresponding HTML element must carry `data-i18n="theKey"`.

### Barcode rendering

- **Code 128**: rendered as SVG via `JsBarcode(svgEl, value, opts)`
- **QR Code / Data Matrix**: rendered to a `<canvas>` via `bwipjs.toCanvas(canvas, opts)`, then converted to `<img>` for print
- Preview uses smaller dimensions (`small=true`); print uses full dimensions

### Family override system

Each family in the preview has a ⚙ gear button that opens an override panel. Overrides are stored in `familyOpts[familyName]` as an object with nullable keys. `getOptsForFamily(name)` merges global opts with per-family overrides. Overrides are lost on page refresh (no persistence).

### Random / Sequential generation

The random generator panel has two mode pills: **Aléatoire / Random / Aleatorio** (default) and **Séquentiel / Sequential / Secuencial**.

- **Random mode**: generates `count` codes of `len` random chars drawn from the selected charset, with optional prefix and suffix
- **Sequential mode**: generates `count` codes of `len` random chars + zero-padded counter (`seqDigits` wide), starting at `start`. Total code = `prefix + random(len) + counter(seqDigits) + suffix`

The mode pills use the `.charset-toggle` CSS class but carry `data-mode="randmode"` to be excluded from the generic charset-toggle click handler. See Known technical constraints in `WORKFLOW.md`.

## Commands / scripts reference

| Action | How |
|--------|-----|
| Run app | Open `index.html` in any modern browser |
| Deploy | `git push origin main` — GitHub Pages rebuilds automatically |
| Bump version | Edit both `subtitle:` strings in `i18n.fr`, `i18n.en`, and `i18n.es` in `index.html` |

## Smoke test checklist

Minimum manual pass before merging any feature branch:

1. Open `index.html` in Chrome or Edge
2. Default sample codes render in the preview on load
3. **Random mode**: select a family, set count=5, click 🎲 → 5 codes appear in the textarea and preview
4. **Sequential mode**: click Séquentiel pill, set start=1, seqDigits=3, count=3 → codes end with `001`, `002`, `003`; random chars appear before counter
5. **Prefix + Suffix**: set prefix `SKU-` and suffix `-A` → codes wrapped correctly
6. **Barcode formats**: switch between Code 128, QR Code, Data Matrix — all render
7. **Per-family override**: click ⚙ on a family, change format → that family renders in new format only
8. **Print**: click 🖨 Imprimer A4 → print dialog opens with barcodes laid out
9. **Language toggle**: switch FR → EN → ES → all labels update correctly
10. **Responsive**: narrow the window — middle column scrolls, no overflow
11. **Reception / Picking scenarios**: apply each → codes injected into textarea, preview renders
