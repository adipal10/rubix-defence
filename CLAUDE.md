# CLAUDE.md — Rubix Defence portfolio site

Context for Claude Code working on this repo.

## What this is

A single-page marketing/portfolio site for **Rubix Defence** — a defence-tech
studio showcase. One hero animation (a canvas drone over a green/black
"futuristic" banner) followed by a data-driven catalogue of projects, each
expandable into its applications and use-cases.

Remote: `https://github.com/adipal10/rubix-defence.git`

## Layout

```
index.html      Entire site — markup, CSS, and vanilla JS in one file (~53 KB)
assets/         The 7 project images as real .jpg files
demo.html       Standalone demo page
README.md
.gitignore
CLAUDE.md       This file
```

There is **no build step and no framework** — plain HTML/CSS/JS. Open
`index.html` in a browser (or any static server) to view. No npm install, no
bundler.

> History note: `index.html` used to be a 1.6 MB file with all images inlined as
> base64. It was restructured so images live in `assets/` and the HTML stays
> small and editable. Keep it that way — don't re-inline images.

## Data model (all inside the `<script>` in index.html)

The page renders itself from a few JS structures. To change content, edit these
— you rarely need to touch markup.

- **`PROJECTS`** — array of project objects. Fields:
  `id, cat, code, name, dom, status, img, icon, blurb, long`
  - `id` — slug, also the key into APPS and USES (`APPS[p.id]`, `USES[p.id]`)
  - `img` — UPPERCASE key into the `IMAGES` map (e.g. `'WARSIM'`); may be omitted
  - `icon` — key into the `ICONS` map (`target, eye, pin, graph, box, swarm, radar`)
  - `cat` — category/filter tag; must match a section's `data-cat`
    (`sim, sa, comms, sov, uav, mfg`); `status` — e.g. `'Operational'`
  - `gallery` (optional) — array of image paths shown as a "Product sheet"
    image grid on the detail page (used by `fieldlink` for its two infographics).
  - Current ids: `warsim, arty, drishti, geovision, osint, fieldlink, rasad,
    nirikshak, swarm, surv, logi, drop, print3d` (13 products)
  - **FieldLink** is the Communications product (section 03, `cat:'comms'`):
    a LoRa mesh comms system, written throughout as a shipping product, not a
    proposal. Its hero/card photo is a crop of the client's own infographic
    banner; the full infographics live in `gallery`. Sections after it are
    numbered 04–08.

- **`IMAGES`** — map of image key → path, e.g. `WARSIM:"assets/warsim.jpg"`.
  Looked up as `IMAGES[p.img]`. **All 12 projects now have a photo** in
  `assets/`. Keys: `NIRIKSHAK, ARTY, DROP, SURV, SWARM, LOGI, WARSIM` (product
  screenshots) plus `DRISHTI, GEOVISION, OSINT, RASAD, PRINT3D`
  (free-licence Unsplash stock — swap for your own product shots when you have
  them, keeping the same filename).

- **`ICONS`** — map of icon name → inline SVG string.

- **`APPS`** — map `id → [bullet strings]` (the "applications" list per project).

- **`USES`** — map `id → [[title, description], ...]` (the use-case cards).

- **`FLOW`** — map `id → [[step title, step description], ...]` (4 steps each).
  Rendered by `flowHTML(id)` as the animated **"How it works" pipeline**
  infographic on each detail page. To change a project's process, edit its
  `FLOW` entry.

## Visuals / infographics

- **Photos** are the card thumbnail and the detail-page hero banner.
- **`animScene(kind)`** builds the SMIL-animated "System concept — live"
  diagram in the detail sidebar (radar sweep, data flows, ping rings, swarm
  drift, print build, `mesh` relay-hop network for FieldLink, etc.). `kind`
  comes from the `SC` map (`id → scene kind`).
  SMIL runs with no JS, so it works offline. (`sceneSVG` is the older static
  version, kept as a fallback.)
- **`flowHTML(id)`** renders the horizontal numbered step pipeline with a CSS
  "data flowing" accent bar (`.flow` / `.flowbar` / `.fstep` in the `<style>`).
- The hero drone is Three.js (CDN, r128). Counters, reveals and the pipeline
  all honour `prefers-reduced-motion`.

## Theme tokens

Colors/fonts are CSS custom properties on `:root` in the `<style>` block. Change
these to re-skin the whole site:

`--bg --panel --panel-2 --line --line-2 --fg --mut --dim --sig --steel`
(`--sig` is the signal/accent green), plus layout/type tokens `--maxw --f-disp
--f-body --f-mono`.

## Common edits

- **Add a project:** append an object to `PROJECTS` with a unique `id`; add an
  `APPS[id]` bullet list and a `USES[id]` list of `[title, desc]` pairs. If it
  has an image, add the file to `assets/` and a key to `IMAGES`, then set
  `img:'KEY'`.
- **Change / add an image:** drop the `.jpg` in `assets/` and point its `IMAGES`
  key at `assets/<file>.jpg`. Don't base64-inline it.
- **Re-skin:** edit the `:root` tokens above.
- **Repo links:** there is no repo/link field on projects yet — add one to
  `PROJECTS` and render it in the card template if you want per-project links.

## Brand placeholders to replace

- Title / name: **"Rubix Defence"** (`<title>` and headings)
- Contact: **contact@rubixdefence.in**

Swap these for the real company name / contact when finalizing.
