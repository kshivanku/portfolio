# Portfolio — project context

Use this file to ramp up quickly. It describes **what we are building**, **how it should behave**, and **what exists today**.

---

## Agent instructions (read first)

**Keep this file updated.** Any agent working on this project — including you — must update `context.md` whenever you change the website in a meaningful way.

Update it in the **same session** as the code change, before you consider the work done. Do not leave the codebase and this file out of sync.

**Update `context.md` when you change any of the following:**

- Scroll behavior, frame transitions, or snap logic
- Site structure (hero, projects, footer, new sections)
- Theme, layout, or interaction patterns
- Files, assets, deploy setup, or repo layout
- What is **done** vs **planned** (checklists, future work)
- Copy, defaults, or product decisions that future agents should know

**How to update it:**

1. Edit the relevant sections — do not only append notes at the bottom.
2. Refresh **Current state vs planned** if scope changed.
3. Update the **Last updated** line at the bottom with a short note or commit reference when helpful.

If you are unsure whether a change warrants an update, **update it anyway**.

---

## Overview

A personal **portfolio website** for **Shivanku** (`kshivanku` on GitHub). Static single-page site on **GitHub Pages**.

| Item | Value |
|------|--------|
| **Local path** | `~/Documents/Projects/portfolio` |
| **GitHub repo** | [github.com/kshivanku/portfolio](https://github.com/kshivanku/portfolio) |
| **Live URL** | [https://kshivanku.github.io/portfolio/](https://kshivanku.github.io/portfolio/) |
| **Default branch** | `main` |
| **Deploy** | GitHub Pages — `main`, root `/` |

No build step, no framework.

---

## Product vision

The site should feel like **short-form video apps** (TikTok, Reels, Shorts): **one scroll gesture = one full-screen frame**. Scrolling down loads the next “page”; scrolling up snaps back to the previous frame.

### Frames (current)

| # | Frame | Content |
|---|--------|---------|
| 1 | **Hero** | Intro heading |
| 2 | **Project 1** | Horizontal project strip: intro panel + 3 captioned video panels |
| 3 | **Project 2** | Horizontal project strip: intro panel + 3 captioned video panels |
| 4 | **Project 3** | Horizontal project strip: intro panel + 3 captioned video panels |

**Four vertical frames total** (hero counts as the first). Each project frame contains a horizontal scroll-snap strip with a short intro panel followed by three video panels, each with a short caption below the video. Non-final media panels are `min(100vw - 40px, 520px)`; intro panels use the same mobile sizing but can grow to `680px` on desktop. Mobile keeps about a 40px next-panel peek, while wider desktop viewports can show multiple capped panels at once. `FoAMediaEdited.mov` is reused for all video panels until unique assets exist. Project videos are paused and reset while offscreen; video panels that are at least half visible in the active project frame play from the beginning.

### Planned (not built yet)

- Final project names, descriptions, and video captions
- Unique image or video per project
- Footer after the last project
- Optional: peek of next frame, in-project overlay scroll (see `experiment/scroll-frame-portfolio` branch for a prior attempt)

---

## Scroll model (current)

- **CSS scroll snap** — `scroll-snap-type: y mandatory` on `html`
- Each `.frame` is **`100vh` / `100dvh`** with `scroll-snap-align: start` and `scroll-snap-stop: always`
- Project frames contain a native horizontal scroll area with `scroll-snap-type: x mandatory`
- Horizontal media panels are full height and capped at 520px wide; intro panels are capped at 680px; smaller screens preserve a 40px right-side preview of the next panel, while larger screens may show multiple panels
- **No JavaScript scroll snapping logic** — vertical and horizontal snap are native CSS
- **JavaScript media visibility control only** — horizontal video panels at least half visible in the active project frame start from `0:00`; all other videos pause/reset
- **No text overlays** yet
- **`prefers-reduced-motion`:** snap degrades to `proximity`

Do not invert vertical direction. Scrolling down goes to the next project frame; scrolling up returns to the previous frame. Avoid long multi-viewport vertical sections unless the user asks.

---

## Theme

- **Default:** dark mode (`data-theme="dark"`)
- **Toggle:** fixed top-right, monochrome sun/moon icons
- Persisted in `localStorage` key `theme`

---

## Tech stack

| Layer | Choice |
|--------|--------|
| Site | Single `index.html` (CSS + theme JS only) |
| Assets | `assets/FoAMediaEdited.mov` (shared project video), `assets/ios-mp-collections.png` (old placeholder, no longer used by current frames) |
| Hosting | GitHub Pages |

---

## Repository layout

```text
portfolio/
├── context.md
├── index.html
└── assets/
    ├── FoAMediaEdited.mov
    └── ios-mp-collections.png
```

### Branches

| Branch | Purpose |
|--------|---------|
| **`main`** | Current TikTok-style scroll snap (this doc) |
| **`experiment/scroll-frame-portfolio`** | Prior attempt: peek hints, blur transitions, in-project text overlays — preserved, not deleted |

---

## Current state vs planned

### Done

- [x] Hero + 3 project frames with scroll snap
- [x] Each project frame has a horizontal intro + 3 captioned video panel strip
- [x] Horizontal panels are max-width capped on desktop and show a 40px preview of the next panel when one exists
- [x] Same project video reused across all video panels
- [x] Project videos only play while at least half visible in the active project frame and restart from the beginning on entry
- [x] Light / dark theme toggle, dark default
- [x] Bidirectional snap (scroll up returns to previous frame)

### Not done

- [ ] Bottom-of-frame text per project
- [ ] Unique media per project
- [ ] Footer
- [ ] Peek / rotation transitions (was on experiment branch only)

---

## Quick commands

```bash
cd ~/Documents/Projects/portfolio
python3 -m http.server 8765   # http://localhost:8765/
gh auth status
```

---

*Last updated: Intro panels are wider than media panels on desktop, and multiple visible desktop video panels can play together.*
