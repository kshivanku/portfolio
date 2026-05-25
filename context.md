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
| 1 | **Hero** | Editorial header with animated large name, description, and hero-only nav under the description |
| 2 | **Project 1** | Vertical project section: intro panel + 3 captioned video panels |
| 3 | **Project 2** | Vertical project section: intro panel + 3 captioned video panels |
| 4 | **Project 3** | Vertical project section: intro panel + 2 captioned video panels |
| 5 | **Resume** | Work experience and education from the attached CV, plus a Google Drive resume download link and contact footer |

**Five main sections total** (hero counts as the first). The hero is an editorial header: a large "Shivanku Kumar" heading whose visible letters use a GSAP masked text reveal on load, then release into a playful randomized drift across the available hero area. Released letters keep the active theme text color and use `mix-blend-mode: difference` for a negative-style overlap effect. Hover/focus over the heading pulls all letters quickly back into place; pointer/touch taps briefly assemble the name before it drifts again. The hero also includes concise product design description and hero-only navigation links for Work, Resume, and Contact directly below the description. A top-left menu icon appears only after leaving the hero, remains visible through the resume/contact area, and opens a full-screen menu page with large centered Work, Resume, and Contact links; the same menu icon morphs into an X to close the menu. The menu overlay fades open/closed smoothly and each menu link uses the same masked reveal, animating upward from `yPercent: 100` over `0.8s` with `power3.out` and a small stagger; transitions are disabled for `prefers-reduced-motion`. Hero, project, resume, and contact titles use the Anton Google Font with Impact/sans-serif fallback. The hero is `80vh` / `80dvh` tall so the first project peeks into the initial viewport. The animated heading keeps an accessible `aria-label` and disables the letter animation and drift for `prefers-reduced-motion`. The site uses a single vertical scroll axis. On mobile, each project section stacks the intro panel followed by its captioned video panels. On desktop (`min-width: 960px`), each project section becomes a two-column layout: the intro panel stays sticky on the left at `34vw`, uses a white background in light mode, and the video panels scroll vertically on the right. In light mode, Project 1 video panels use an animated WhatsApp background with mostly `#FFFFE9` and subtle moving `#3FAA55`; Projects 2 and 3 use animated SoFi backgrounds with mostly `#E9FCFF` and subtle moving `#00AFD3`. In dark mode, all video panels use a solid dark grey `#151515` background with no gradient layer. Video panels keep centered caption text, subtle metadata pills below the caption, and a compact "Case study" link using placeholder `#case-study-placeholder` URLs that open in a new tab. Launched status pills use a subtle green hue, and in-development status pills use a subtle yellow hue. The intro panel displays the project title, description, and four work experience rows (Year, Company, Role, Platform) with labels on the left and values on the right. Project 1 video order is `UplevelCTA.mov`, `FoAMediaEdited.mov`, then `Quicksends.mov`; Project 2 video order is `vHubDemo.mov`, `VHubReviewDemo.mov`, then `AdditionalIncomeReview.mov`; Project 3 video order is `Pulsecheck.mov`, then `HLHub.mov`. Project videos are paused and reset while offscreen; video panels only play from the beginning when vertically visible in the active project section. Tapping/clicking a video, or focusing it and pressing Enter/Space, opens it in native fullscreen where supported. The project/video carousel indicator sits below the menu icon at top-left, matches the menu icon width, and appears only while one of the three project sections is active; it marks projects 1, 2, or 3 and shows active-video dots under the active project number. The resume section follows the projects with a minimal work experience and education layout using plain rows and separators instead of cards, with a subtle download link to the Google Drive resume. The contact footer lives at the end of the resume scroll area as a full-viewport, full-bleed footer with a centered `KSHIVANKU@GMAIL.COM` mail link whose matching GSAP masked reveal triggers when the footer enters view, plus a social button row for LinkedIn, Instagram, GitHub, and X.

### Planned (not built yet)

- Final project names, descriptions, video captions, status/impact metadata, and case study URLs
- Unique image or video per project
- Optional: peek of next frame, in-project overlay scroll (see `experiment/scroll-frame-portfolio` branch for a prior attempt)

---

## Scroll model (current)

- **CSS scroll snap** — `scroll-snap-type: y mandatory` on `html`
- Hero is **`80vh` / `80dvh`** so the first project peeks into the initial viewport; media panels remain **`100vh` / `100dvh`**
- Outer project sections are not snap targets; their intro and video panels provide the snap stops so videos land flush at the top
- Project sections are vertically stacked; no horizontal scroll is used for project media
- In light mode, Project 1 video panels have an animated WhatsApp background using mostly `#FFFFE9` with subtle moving `#3FAA55`; Projects 2 and 3 video panels have animated SoFi backgrounds using mostly `#E9FCFF` with subtle moving `#00AFD3`
- In dark mode, all video panels use a solid dark grey `#151515` background with no gradient layer
- On desktop, project intro panels are sticky left columns while the video panels scroll vertically on the right
- Resume section follows the projects and scrolls internally if the work and education rows exceed the viewport
- Contact footer is inside the resume section, after education, but outside the constrained resume content wrapper; it spans the full viewport with a top border, large centered email link whose GSAP masked reveal triggers on viewport entry, and a social button row for LinkedIn, Instagram, GitHub, and X; the Contact nav item scrolls to this footer
- **No JavaScript scroll snapping logic** — vertical snap is native CSS
- **JavaScript media visibility control only** — the most vertically visible project section is active; video panels start from `0:00` only when vertically visible in that section; all other videos pause/reset
- Top-left menu icon is hidden on the hero, visible from projects through the resume/contact area, opens a full-screen menu overlay with Work, Resume, and Contact links, and morphs into an X while open; the overlay fades smoothly and each link uses a GSAP masked reveal with slight stagger; tapping it or pressing Escape closes the overlay
- Project indicator uses the same active project detection as media playback, sits below the menu icon at the same width, appears only after a project section reaches the viewport top, hides on the hero and resume sections, and shows active-video dots under the active project number
- Project videos are focusable and open in native fullscreen on click/tap, Enter, or Space; iOS Safari uses `webkitEnterFullscreen` fallback
- **No text overlays** yet
- **`prefers-reduced-motion`:** snap degrades to `proximity`; GSAP text reveals and hero letter drift are skipped

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
| Site | Single `index.html` (CSS + theme/media/menu JS only) |
| Animation | GSAP 3.12.5 from jsDelivr for masked text reveals |
| Typography | Inter Google Font for body text; Anton Google Font for hero/project/resume titles |
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

*Last updated: Removed forced white color from drifting hero letters so blend uses theme color.*
