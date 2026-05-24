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
| 2 | **Project 1** | Full-viewport project image |
| 3 | **Project 2** | Full-viewport project image |
| 4 | **Project 3** | Full-viewport project image |

**Four frames total** (hero counts as the first). Placeholder image is reused on all project frames until real assets exist.

### Planned (not built yet)

- Text elements at the **bottom** of each frame
- Unique image or video per project
- Footer after the last project
- Optional: peek of next frame, in-project overlay scroll (see `experiment/scroll-frame-portfolio` branch for a prior attempt)

---

## Scroll model (current)

- **CSS scroll snap** — `scroll-snap-type: y mandatory` on `html`
- Each `.frame` is **`100vh` / `100dvh`** with `scroll-snap-align: start` and `scroll-snap-stop: always`
- **No JavaScript scroll logic** — native snap only
- **No text overlays** yet
- **`prefers-reduced-motion`:** snap degrades to `proximity`

Do not reintroduce long multi-viewport sections or gradual in-frame scroll unless the user asks. The current direction is **strict one-frame-per-snap**.

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
| Assets | `assets/ios-mp-collections.png` (placeholder) |
| Hosting | GitHub Pages |

---

## Repository layout

```text
portfolio/
├── context.md
├── index.html
└── assets/
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
- [x] Same placeholder image on all project frames
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

*Last updated: TikTok-style mandatory scroll snap — hero + 3 projects, placeholder image, no overlay text yet.*
