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
| 2 | **Project 1** | Vertical project section: intro panel + 3 captioned video panels |
| 3 | **Project 2** | Vertical project section: intro panel + 3 captioned video panels |
| 4 | **Project 3** | Vertical project section: intro panel + 2 captioned video panels |
| 5 | **Resume** | Work experience and education from the attached CV |

**Five main sections total** (hero counts as the first). The site now uses a single vertical scroll axis. On mobile, each project section stacks the intro panel followed by its captioned video panels. On desktop (`min-width: 960px`), each project section becomes a two-column layout: the intro panel stays sticky on the left at `34vw`, and the video panels scroll vertically on the right. Video panels keep centered caption text and subtle metadata pills below the caption. The intro panel displays the project title, description, and four work experience rows (Year, Company, Role, Platform) with labels on the left and values on the right. Project 1 video order is `UplevelCTA.mov`, `FoAMediaEdited.mov`, then `Quicksends.mov`; Project 2 video order is `vHubDemo.mov`, `VHubReviewDemo.mov`, then `AdditionalIncomeReview.mov`; Project 3 video order is `Pulsecheck.mov`, then `HLHub.mov`. Project videos are paused and reset while offscreen; video panels only play from the beginning when vertically visible in the active project section. Tapping/clicking a video, or focusing it and pressing Enter/Space, opens it in native fullscreen where supported. A subtle right-side project indicator appears only while one of the three project sections is active; it marks projects 1, 2, or 3 and can be clicked to jump between project sections. The active project number shows small dots for that project's videos, with the dot matching the most visible video panel. The resume section follows the projects with a minimal work experience and education layout using plain rows and separators instead of cards.

### Planned (not built yet)

- Final project names, descriptions, video captions, and status/impact metadata
- Unique image or video per project
- Footer after the last project
- Optional: peek of next frame, in-project overlay scroll (see `experiment/scroll-frame-portfolio` branch for a prior attempt)

---

## Scroll model (current)

- **CSS scroll snap** — `scroll-snap-type: y mandatory` on `html`
- Hero and media panels are **`100vh` / `100dvh`** with `scroll-snap-align: start` and `scroll-snap-stop: always`
- Outer project sections are not snap targets; their intro and video panels provide the snap stops so videos land flush at the top
- Project sections are vertically stacked; no horizontal scroll is used for project media
- On desktop, project intro panels are sticky left columns while the video panels scroll vertically on the right
- Resume section follows the projects and scrolls internally if the work and education rows exceed the viewport
- **No JavaScript scroll snapping logic** — vertical snap is native CSS
- **JavaScript media visibility control only** — the most vertically visible project section is active; video panels start from `0:00` only when vertically visible in that section; all other videos pause/reset
- Project indicator uses the same active project detection as media playback, appears only from Project 1 through Project 3, hides on the hero and resume sections, and shows active-video dots under the active project number
- Project videos are focusable and open in native fullscreen on click/tap, Enter, or Space; iOS Safari uses `webkitEnterFullscreen` fallback
- **No text overlays** yet
- **`prefers-reduced-motion`:** snap degrades to `proximity`

Do not invert vertical direction. Scrolling down advances through project intros, project videos, and the resume; scrolling up returns to the previous snap point.

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
| Assets | `assets/UplevelCTA.mov` (Project 1 first video panel), `assets/FoAMediaEdited.mov` (Project 1 second video panel and shared fallback), `assets/Quicksends.mov` (Project 1 third video panel), `assets/vHubDemo.mov` (Project 2 first video panel), `assets/VHubReviewDemo.mov` (Project 2 second video panel), `assets/AdditionalIncomeReview.mov` (Project 2 third video panel), `assets/Pulsecheck.mov` (Project 3 first video panel), `assets/HLHub.mov` (Project 3 second video panel), `assets/ios-mp-collections.png` (old placeholder, no longer used by current frames) |
| Hosting | GitHub Pages |

---

## Repository layout

```text
portfolio/
├── .gitignore
├── context.md
├── index.html
└── assets/
    ├── UplevelCTA.mov
    ├── Quicksends.mov
    ├── vHubDemo.mov
    ├── VHubReviewDemo.mov
    ├── AdditionalIncomeReview.mov
    ├── Pulsecheck.mov
    ├── HLHub.mov
    ├── FoAMediaEdited.mov
    └── ios-mp-collections.png
```

### Branches

| Branch | Purpose |
|--------|---------|
| **`main`** | Current vertical-only scroll model with sticky desktop project intro columns |
| **`codex/desktop-two-panel-peek`** | Merged experiment branch: full-width desktop shows intro + primary video + 80px next-video peek |
| **`experiment/scroll-frame-portfolio`** | Prior attempt: peek hints, blur transitions, in-project text overlays — preserved, not deleted |

---

## Current state vs planned

### Done

- [x] Hero + 3 project sections with scroll snap
- [x] Project sections use a single vertical scroll axis
- [x] Video captions include two subtle metadata pills for status and impact
- [x] Desktop composition: sticky left project intro + vertically scrolling video panels on the right
- [x] Mobile composition: centered intro panel followed by full-height video panels
- [x] Unique video assets assigned for Project 1, Project 2, and both Project 3 video panels
- [x] Project videos only play while vertically visible in the active project section and restart from the beginning on entry
- [x] Project videos open in fullscreen on click/tap or keyboard activation
- [x] Light / dark theme toggle, dark default
- [x] Bidirectional snap (scroll up returns to previous frame)
- [x] Right-side project indicator appears on project sections and highlights the active project/video
- [x] Work experience details (Year, Company, Role, Platform) displayed in intro panel with label-value layout
- [x] Resume section after projects with work experience and education from the CV

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

*Last updated: Added active-video dots under the active project number and made project intro/video panels the project snap targets.*
