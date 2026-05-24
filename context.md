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
| 2 | **Project 1** | Full-viewport project video |
| 3 | **Project 2** | Full-viewport project video |
| 4 | **Project 3** | Full-viewport project video |

**Four frames total** (hero counts as the first). `FoAMediaEdited.mov` is reused on all project frames until unique assets exist. Project videos are paused and reset while offscreen; when a project frame comes into view, its video starts from the beginning.

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
- **JavaScript media visibility control only** — `IntersectionObserver` starts the visible project video from `0:00` and pauses/resets offscreen videos
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
- [x] Same project video on all project frames
- [x] Project videos only play while in view and restart from the beginning on entry
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

*Last updated: Project videos now play only while in view and restart from the beginning on entry; hero + 3 projects, no overlay text yet.*
