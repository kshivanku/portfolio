# Portfolio — project context

Use this file to ramp up quickly. It describes **what we are building**, **how it should behave**, and **what exists today**.

---

## Agent instructions (read first)

**Keep this file updated.** Any agent working on this project — including you — must update `context.md` whenever you change the website in a meaningful way.

Update it in the **same session** as the code change, before you consider the work done. Do not leave the codebase and this file out of sync.

**Update `context.md` when you change any of the following:**

- Scroll behavior, frame transitions, or peek logic
- Site structure (hero, projects, footer, new sections)
- Theme, layout, or interaction patterns
- Files, assets, deploy setup, or repo layout
- What is **done** vs **planned** (checklists, future work)
- Copy, defaults, or product decisions that future agents should know

**How to update it:**

1. Edit the relevant sections — do not only append notes at the bottom.
2. Refresh **Current state vs planned** if scope changed.
3. Update the **Last updated** line at the bottom with a short note or commit reference when helpful.

If you are unsure whether a change warrants an update, **update it anyway**. An slightly over-documented `context.md` is better than a stale one.

---

## Overview

A personal **portfolio website** for **Shivanku** (`kshivanku` on GitHub). The site is a **static, single-page** experience hosted on **GitHub Pages**.

| Item | Value |
|------|--------|
| **Local path** | `~/Documents/Projects/portfolio` |
| **GitHub repo** | [github.com/kshivanku/portfolio](https://github.com/kshivanku/portfolio) |
| **Live URL** | [https://kshivanku.github.io/portfolio/](https://kshivanku.github.io/portfolio/) |
| **Default branch** | `main` |
| **Deploy** | GitHub Pages — source: `main`, folder: `/` (root) |

No build step, no framework. Everything ships as static files.

---

## Product vision

The site should feel like **short-form video apps** (TikTok, Reels, Shorts): **vertical scroll moves you between full-screen “frames”**, not long traditional web pages.

### Site structure (target)

1. **Hero** — intro only (“Hi, I’m Shivanku…”).
2. **Project 1** — first case study.
3. **Project 2** — second case study.
4. **Project 3** — third case study.
5. **Project 4** — fourth case study.
6. **Footer** (planned, not built yet) — contact or closing content after Project 4.

**Four projects** for now. Each project will eventually have its own image or video and real copy. During prototyping, the **same placeholder image** is reused everywhere to validate **interaction and scroll behavior** before investing in final assets.

---

## Scroll model (core UX)

There are **two layers** of scrolling. Understanding both is essential.

### Layer A — Frame transitions (between hero and projects)

- Scrolling advances to the **next full-screen frame** (hero → project 1 → project 2 → …).
- Each transition should feel like a **frame change** — a new “page” has arrived.
- Content from the frame you are leaving **blurs and fades out completely** (`opacity: 0`). It must **not** remain as a ghost in the background.
- **Peek rule:** On every frame **except the last**, the user sees roughly **100px** of the **next** frame at the bottom of the viewport, **rotated ~20°**, as an invite to scroll. When that frame becomes active, the media **rotates to 0°** and fills the viewport.

### Layer B — In-project scroll (inside each project)

Each project is **taller than one viewport**:

| Phase | Height (current) | What happens |
|--------|------------------|--------------|
| **Land / enter** | First `100vh` of the project section | Project media snaps in full-screen (0° rotation). |
| **Story / overlays** | Second `100vh` (`+1` extra viewport) | **Text cards** scroll up **over** the pinned media. Cards have a **semi-opaque background** (and light blur) so text stays readable on top of the image. |
| **Exit** | End of overlay scroll | Full-frame transition to the **next** project; peek of the following project visible where applicable. |

**Hero** is only **one viewport** tall — no in-frame overlay zone. It has the heading plus a peek into Project 1.

**Project 4** has **no peek** at the bottom (last project). A footer will be added later.

### Text overlays per project

- **Two cards per project**, ~**100 characters** each (dummy copy for now).
- Cards are offset left/right slightly for visual rhythm.
- Readable overlay: background + border + `backdrop-filter` blur, themed for light/dark mode.

---

## Visual / interaction details

### Theme

- **Default:** dark mode (`data-theme="dark"` on `<html>`).
- **Toggle:** fixed top-right, monochrome sun/moon icons (de-emphasized, no color).
- Preference persisted in `localStorage` under key `theme`.
- First visit defaults to dark; user choice overrides.

### Hero copy (current)

> Hi, I’m Shivanku. I design and build products that feel simple, intuitive and are used by millions globally.

### Media

- Placeholder asset: `assets/ios-mp-collections.png` (tall iOS screenshot, ~1508×3256).
- Used for hero peek and all four projects until real assets exist.
- `object-fit: contain` on full-frame media.

### Motion

- Scroll-driven animations via vanilla JS (`requestAnimationFrame` + scroll listener).
- `prefers-reduced-motion`: simplified layout, minimal blur/rotation.
- `scroll-snap-type: y proximity` on `html` for a slightly “snapped” feel between frames.

---

## Tech stack

| Layer | Choice |
|--------|--------|
| Markup / style / behavior | Single `index.html` (embedded CSS + JS) |
| Assets | `assets/` directory |
| Hosting | GitHub Pages |
| Git remote | `https://github.com/kshivanku/portfolio.git` |
| GitHub CLI | `gh` (user auth as `kshivanku`) |

**Intentionally not used (yet):** React, bundlers, npm, Jekyll, GitHub Actions.

---

## Repository layout

```text
portfolio/
├── context.md          ← this file (agent onboarding)
├── index.html          ← entire site (structure, styles, scroll logic, theme)
└── assets/
    └── ios-mp-collections.png   ← placeholder for all projects + peeks
```

---

## Implementation notes (for agents)

**Maintain `context.md`.** See [Agent instructions](#agent-instructions-read-first) above — updating this file is part of the task, not optional follow-up.

### HTML structure (conceptual)

- `.site` — main scroll container.
- `.frame.frame--hero` — `100vh`.
- `.frame.frame--project` — `200vh` each (`100vh` sticky stage + `100vh` overlay zone).
- `.frame__sticky` — pinned `100vh` stage (media + optional peek).
- `.frame__heading` — hero title only.
- `.frame__media` — full project image.
- `.frame__peek` — clipped `100px` strip + `.frame__peek-inner` at 20° for “next frame” hint.
- `.frame__overlays` — scrollable text zone; `.text-blob` cards.

### Scroll logic (high level)

JS computes per-frame:

- **`enter`** (0→1) — first viewport of a project: media rotates 20°→0°, fades in.
- **`overlay`** (0→1) — second viewport: text blobs animate in; peek of next project shows when `enter` is complete.
- **Hero exit** — heading blur + full fade on first viewport scroll.
- **Cross-project exit** — previous project’s media and blobs blur/fade when the next project’s `enter` begins.

Do not reintroduce a **200vh hero** or **system theme as default** unless the user asks. Hero = **one viewport**; default theme = **dark**; heading exit = **fully gone**, not ghosted.

### Git / deploy workflow

- User may say **“push it”** when they want changes published.
- Only commit when asked.
- After push, site updates in ~1–2 minutes at the GitHub Pages URL.

### Environment

- Project folder: user’s **`Documents/Projects`** (not `~/Projects`).
- GitHub auth is via **`gh`** on the user’s Mac (`kshivanku`).

---

## Current state vs planned

### Done

- [x] GitHub Pages live on `main`
- [x] Hero with intro + peek into Project 1
- [x] Four project frames with placeholder image
- [x] Frame transitions (blur out, media enter at 0°)
- [x] Peek of next project on hero and projects 1–3
- [x] In-project scroll with two text cards per project
- [x] Light / dark mode toggle
- [x] Dark mode default, centered hero text

### Not done / future

- [ ] Unique image or video per project
- [ ] Real case study copy (replace dummy blobs)
- [ ] Footer after Project 4
- [ ] Image optimization (placeholder PNG is ~1 MB)
- [ ] Optional: stronger scroll snap, URL hash per project, analytics
- [ ] Optional: `git config` global user.name / user.email (commits currently use machine identity)

---

## Design principles (from project owner)

1. **Behavior before polish** — validate scroll and frame transitions with placeholders first.
2. **Minimal stack** — prefer the smallest change that matches the vision.
3. **Frame-like navigation** — discrete full-screen steps between hero and projects; gradual scroll only *inside* a project for story text.
4. **Readable overlays** — never place bare text on busy screenshots without a background card.

---

## Quick commands

```bash
cd ~/Documents/Projects/portfolio

# Local preview
python3 -m http.server 8765
# open http://localhost:8765/

# GitHub auth check
gh auth status

# Push (when user requests)
git add -A && git commit -m "…" && git push
```

---

## Questions to ask the user if unclear

- Per-project scroll length: still **one extra viewport**, or more?
- Peek rotation direction or size tweaks?
- `object-fit: contain` vs `cover` for final full-frame media?
- Footer content and layout when ready to build it?

---

*Last updated: added agent instruction to keep this file in sync with code changes.*
