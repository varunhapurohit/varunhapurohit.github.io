# Claude Instructions — Varunha Portfolio

This file is read by Claude at the start of every session. It contains everything Claude must know before touching this project.

---

## Project Overview

Portfolio website for **Varunha Purohit** — Digital Designer & Motion Artist.  
Live at: `varunhapurohit.github.io`  
Repo: `varunhapurohit/varunhapurohit.github.io`  
Branch strategy: features on `feature_YYYYMMDD` → merge to `main` → auto-deploys in ~60s.

---

## Stack

| Layer | Choice |
|---|---|
| HTML | Single `index.html` — no framework, no build step |
| CSS | Inline `<style>` block in `index.html` |
| JS | Inline `<script>` only if needed — no bundler |
| Hosting | GitHub Pages via GitHub Actions (source: Actions, not branch) |
| Images | Figma MCP asset URLs during dev; move to `/assets/images/` when final |
| Videos | Vimeo embeds only (GitHub 100MB limit, no streaming) |
| Fonts | New York/Georgia (system serif) for marquee; Inter from Google Fonts for everything else |

---

## Figma Connection

- **Figma MCP server** is always active in this project
- **File key:** `LZk8oZ8WdWuGqcVo286I5U` (Varunha-portfolio)
- **Account:** varunhapurohit@gmail.com
- **Before building any section:** always call `get_design_context` + `get_screenshot` on the specific node
- **Node links** must include `node-id` in the URL — section-level reads require it
- **Figma asset URLs expire after 7 days** — when a section is finalized, download assets and save to `/assets/images/`

---

## Sizing Rules (MANDATORY)

Full rules in [docs/FIGMA_SIZING.md](docs/FIGMA_SIZING.md). Key rules:

### Canvas Reference
| Property | Value |
|---|---|
| Figma canvas width | 1440px |
| Hero section height | 1024px |
| Portrait width (Figma) | 761px → `52.85vw` |
| Portrait height (Figma) | 854px → `83.4svh` |

### Conversion Formulas
- `px` width → `vw`: `(px / 1440) * 100`
- `px` height → `svh`: `(px / 1024) * 100`
- Font sizes: always `clamp(MIN, preferred-vw, MAX)` — never fixed `px`
- Spacing/padding: `clamp(40px, 5vw, 120px)` pattern
- Absolute positions: `left: calc((figma_x / 1440) * 100%)`

### Height-First Rule (CRITICAL)
Portrait and character images MUST be sized by height, not width:
```css
/* ✅ Correct */
.hero-portrait { height: 84svh; width: auto; max-width: 56vw; }

/* ❌ Wrong — portrait becomes too short on widescreen */
.hero-portrait { width: 53vw; height: auto; }
```

### Always use `svh` not `vh`
`svh` = small viewport height — handles iOS Safari browser chrome correctly. `vh` causes overflow bugs on mobile.

---

## Pre-Commit Checklist

Before committing any section change, Claude must verify:

- [ ] No hardcoded `width`/`height` in `px` on images or major layout blocks (except ≤16px utility values)
- [ ] All heading/display fonts use `clamp()`
- [ ] Portrait and character images use `height: Xsvh; width: auto`
- [ ] Section padding uses `clamp()` or `vw`/`vh`
- [ ] Absolute-positioned elements use `%` or `calc(%)` not raw `px`
- [ ] Screenshot compared against Figma before reporting done

---

## Sections Status

| Section | Status | Figma Node |
|---|---|---|
| Navbar (glassmorphism pill) | ✅ Live | `24:109` |
| Hero (marquee + portrait) | ✅ Live | `1:2` |
| About (black band) | ✅ Live | `31:210–31:212` |
| Work / Grey section | 🔲 Placeholder | — |
| Skills / Expertise | 🔲 Not started | — |
| Testimonials | 🔲 Not started | — |
| Contact / Footer | 🔲 Not started | — |

---

## Key CSS Patterns (Already in index.html)

### Glassmorphism Nav Pill
```css
.nav {
  position: fixed; top: 32px; left: 50%; transform: translateX(-50%);
  width: min(1070px, calc(100% - 80px)); height: 87px;
  background: rgba(255,255,255,0.15); backdrop-filter: blur(24px);
  border: 1px solid rgba(255,255,255,0.3); border-radius: 30px;
}
```

### Seamless Marquee
```css
.marquee-inner { display: inline-flex; animation: marquee-scroll 28.45s linear infinite; }
@keyframes marquee-scroll { from { transform: translateX(0); } to { transform: translateX(-50%); } }
```
HTML: text block duplicated twice inside `.marquee-inner` so the loop is seamless.

### Height-Driven Portrait
```css
.hero-portrait { position: absolute; left: 50%; transform: translateX(-50%); bottom: 0; height: 84svh; width: auto; max-width: 56vw; }
.hero-portrait img { height: 100%; width: auto; object-fit: contain; object-position: bottom center; }
```

---

## Deployment

**GitHub Actions** — `.github/workflows/deploy.yml`  
- Push to `main` → auto-deploys  
- Manual trigger via GitHub Actions UI → "Run workflow" dropdown (branch selector is built-in, no input needed)  
- `workflow_dispatch` only shows in GitHub UI when the workflow file is on `main`

**GitHub Pages setup required once:**  
Settings → Pages → Source → change to "GitHub Actions" (not "Deploy from a branch")

---

## Workflow for Adding New Sections

1. User shares Figma node-specific URL (must have `node-id`)
2. Claude calls `get_design_context` + `get_screenshot` on that node
3. Claude extracts motion data via `get_motion_context` if animations exist
4. Claude builds the section following sizing rules above
5. Claude creates a feature branch `feature_YYYYMMDD`
6. User reviews → approves → Claude merges to `main`

User-facing guide in [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md).

---

## Asset URLs in Current index.html (Expire ~7 days from July 21, 2026)

| Asset | URL |
|---|---|
| Glimmer overlay | `https://www.figma.com/api/mcp/asset/bae35404-2f29-4a77-9f79-02cd31b11162` |
| Portrait | `https://www.figma.com/api/mcp/asset/72e46c66-9949-43e8-9ede-94694d955289` |
| Scroll arrow icon | `https://www.figma.com/api/mcp/asset/3bc332c5-8629-4800-888a-b6f0046e1074` |

When these expire, call `mcp__figma__download_assets` to re-fetch them or save permanently to `/assets/images/`.

---

## What NOT to Do

- Do NOT rewrite `index.html` wholesale when updating a section — edit surgically
- Do NOT use `vh` — always `svh`
- Do NOT use fixed `px` font sizes on headings — always `clamp()`
- Do NOT use `width`-driven sizing for portrait/character images — always `height`-driven
- Do NOT push to `main` without user approval
- Do NOT commit Figma asset URLs as permanent — they expire in 7 days
- Do NOT add a framework (React, Next.js, etc.) — this is intentionally static HTML
