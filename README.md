# Pinned! How interface states tame the Schottky barrier

Static teaching site for the graduate course *Introduction to Semiconductor*: how interface
states pin the Fermi level and weaken the dependence of the Schottky barrier height on the
chosen metal. Four sections, each with a live simulation, two scripted demos and a game.

Plain HTML, CSS and vanilla ES modules. No framework, no build step, no dependencies.
External resources (pinned, with SRI): KaTeX 0.16.11, qrcode-generator 1.4.4 (both from
jsDelivr) and Google Fonts (IBM Plex Sans / Mono).

## Run locally

```bash
python3 -m http.server
```

Then open <http://localhost:8000/>. ES modules need a server; opening `index.html` from disk
will not work.

## Test the physics engine

```bash
node --test
```

`tests/engine.test.mjs` checks every value listed in spec §5.7 (ideal preset, Si/GaAs preset
barriers, slopes, γ, limits, pivot invariance, charges, mission sets, toy contact model) plus
a few extra invariants (monotonic charge residual, no input mutation, C–V intercept).

## Deploy to GitHub Pages

1. Push this folder to a GitHub repository (the site lives at the repository root).
2. Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`.
3. Set `SITE_URL` in `js/config.js` to the Pages URL, e.g.
   `https://<user>.github.io/<repo>/`, so the QR code points to the public detailed-mode page.

All URLs are relative, so the site works under a sub-path.

## Configure

`js/config.js` holds everything you are expected to edit:

| Constant | Meaning |
|---|---|
| `TITLE` | Site title (used in the header, `<title>` and the hero) |
| `SITE_URL` | Public URL; the QR encodes `SITE_URL + '?mode=detailed'` (falls back to the current address) |
| `TOPIC29_URL`, `TOPIC30_URL` | Links to the neighbouring student sites (shown as TODO until set) |
| `AUTHORS` | Footer author line (shown as TODO until set) |
| `SECTION_TARGETS`, `TOTAL_TARGET` | Presenter-timer targets |

## Modes

* `?mode=present`: presentation mode for the live talk (one section per screen, no scrolling,
  fits 1280×720 to 1920×1080).
* `?mode=detailed`: detailed mode for phones and laptops (one scrolling article).
* Without the parameter the last choice (localStorage) is used; first visit = Detailed.
* The header toggle (or key `M`) switches modes.

### Presentation keys

| Key | Action |
|---|---|
| ← / → | previous / next section |
| A / B | Demo A / Demo B (C: contact animation in Section 1) |
| H | open the hook (game) |
| Space / R | pause-resume / replay the demo |
| Enter, 1–4 | hook: reveal or next round, pick an option |
| D | More drawer (concept, derivation, assumptions); Esc closes |
| F | fullscreen |
| T | presenter timer (section vs target, total vs 6:30) |
| Q | QR code to the detailed site |
| ? | key map |

## Layout

```
index.html
css/tokens.css  base.css  present.css  detailed.css
js/config.js          site constants
js/engine.js          pure physics, no DOM: the single source of truth for every number
js/facts.js           numbers quoted in the prose, computed with the engine
js/state.js           shared simulation state, presets, subscribe()
js/demo-player.js     scripted demos + caption bar
js/modes.js  qr.js  overlay.js  presenter.js (timer, key map)
js/layout-present.js  layout-detailed.js  layout-common.js  content-extras.js  main.js
js/plots/*.js         band diagram + ladder, charge comparison, MIGS, charge bars, budget,
                      φB–φm, heatmap (+contours), C–V, ρc–t, I–V (hand-written SVG)
js/sections/s1–s4.js  content, simulation, demos and hooks of each section
js/hooks/*.js         games (Sign it!, Predict and lock, Mission, Build a contact, Myth or fact)
js/ui/*.js            DOM helpers, controls, formatting, KaTeX helper, storage
data/materials.js     Si, GaAs, Table 9.1 metals, presets, slider ranges
data/course-data.js   values digitized from the course slides
tests/engine.test.mjs node --test
```

## Model limits

1D; uniform D_it with a single neutral level; interfacial layer as a uniform dielectric;
Boltzmann statistics (degenerate doping is flagged); no image-force lowering; no barrier
inhomogeneity; the Section 4 contact-resistance model is a qualitative toy.
