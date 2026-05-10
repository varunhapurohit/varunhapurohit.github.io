# Design → Development Workflow

This document explains how the design and development process works for this portfolio website. Read this before making any changes in Figma.

---

## The Stack

| What | Where |
|---|---|
| Website code | This GitHub repo → auto-deploys to `varunhapurohit.github.io` |
| Design source | Figma file (PORTFOLIO-website) |
| Videos | Vimeo (upload there, share the link) |
| Images | Figma assets → downloaded and hosted in `/assets/` |

---

## How the Loop Works

```
You design in Figma
       ↓
You share the Figma section link + describe what's new
       ↓
Claude reads the design, builds the code
       ↓
Code is pushed → live on the website in ~1 minute
       ↓
You review in the browser, annotate feedback in Figma
       ↓
Repeat until final
```

---

## Your Responsibilities as Designer

### 1. Mark what's ready for development

When a section is **ready to be coded**, right-click the frame in Figma → **Copy link to selection**. Share that specific URL — not the file URL.

Example of what to share:
```
https://www.figma.com/design/d4SDrsntP2kyQ48w57Bvn0/PORTFOLIO-website?node-id=5101-1920
```

Do not share the whole file link and say "it's updated" — the node-specific link tells Claude exactly what to look at.

### 2. One section at a time

Share one section per message. Example:

> "New hero section is ready: [link]"
> "Updated services section: [link]"

This keeps feedback tight and avoids large rewrites.

### 3. Annotate interactions in Figma

Claude can read layout, colors, fonts, and spacing from the design. It **cannot** infer behaviour. Add a Figma comment or annotation for anything like:

- "On hover, this card lifts with a shadow"
- "This button links to the contact section"
- "This section auto-scrolls / has a parallax effect"
- "Video plays on loop, muted, no controls"

### 4. Use Figma Styles and Variables

Set up your colors, fonts, and spacing as **Figma Styles** (or Variables). Claude reads these directly — it makes the code cleaner and easier to update globally.

### 5. For videos

Upload the video to **Vimeo**. Share the Vimeo video URL alongside the Figma design link. Claude will embed it with the right settings (autoplay, muted, loop for background videos).

### 6. For images

Keep your project images inside the Figma file itself (as fills on frames). Claude pulls them directly from Figma during development. Once the design is final, proper image files will be downloaded and stored in `/assets/`.

---

## Feedback After Reviewing the Live Site

When you review the website and want changes:

- **Design changes** → update in Figma first, then share the new link
- **Copy/text changes** → you can describe them in plain text, no Figma update needed
- **Bug or alignment issue** → screenshot it and describe what's wrong

Do not give feedback like *"the spacing looks off"* — be specific: *"the gap between the headline and the subtext should be tighter, matching the Figma"*.

---

## What Claude Does Each Round

1. Fetches fresh design context from your Figma link
2. Takes a screenshot of the section for visual reference
3. Edits only the relevant part of the code — existing sections are not touched
4. Pushes the change → live on GitHub Pages within ~1 minute

---

## Folder Structure

```
varunhapurohit.github.io/
├── index.html          ← the entire website (single file for now)
├── assets/
│   ├── images/         ← project images (added when design is final)
│   └── fonts/          ← self-hosted fonts if needed
├── DESIGN_WORKFLOW.md  ← this file
└── README.md
```

---

## Rules

- Never push directly to `main` without reviewing. All changes go through Claude.
- The Figma file is the source of truth for design. The code follows the design — not the other way around.
- If something looks different between Figma and the live site, the Figma version wins.

---

## Quick Reference — What to Share Each Round

| Situation | What to share |
|---|---|
| New section designed | Figma node link + any interaction notes |
| Section updated | Figma node link + what changed |
| Video section | Figma node link + Vimeo video URL |
| Text/copy change | Just describe it in plain text |
| Bug or layout issue | Screenshot + description |
