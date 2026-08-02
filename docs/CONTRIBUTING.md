# Varunha Portfolio — Contributor Guide

This guide is for anyone who wants to add or update content on this website. You do not need to know how to code. You design in Figma, then bring the Figma link here and ask Claude to build it.

---

## How This Works

```
You design in Figma
       ↓
You open Claude Code (this tool)
       ↓
You paste the Figma link + describe what you want
       ↓
Claude reads the design, fetches assets & animations, builds the code
       ↓
Claude pushes to a preview branch → you review it live
       ↓
You say "looks good" → Claude merges to main → site goes live in ~1 min
```

---

## Before You Start

### What You Need

| Tool | Purpose |
|---|---|
| [Figma](https://figma.com) | Design the page or section |
| Claude Code (you're here) | Turns the design into code |
| GitHub account access | To see the live preview |

### Figma File

The portfolio lives here:
```
https://www.figma.com/design/LZk8oZ8WdWuGqcVo286I5U/Varunha-portfolio
```

Always design new sections inside this file on the page called **"Page 1"**. Do not create a new file — Claude is connected to this specific file.

---

## Step-by-Step: Adding a New Section

### Step 1 — Design it in Figma

1. Open the Figma file above
2. Add your new section inside the existing **"Desktop - 1"** frame (1440px wide)
3. Name your layers clearly — Claude reads the layer names:
   - ✅ `hero_section`, `about_text`, `work_card_01`
   - ❌ `Frame 47`, `Rectangle 12`, `Group`
4. When done, right-click the section frame → **Copy link to selection**
   - The link will look like: `https://www.figma.com/design/LZk8oZ8WdWuGqcVo286I5U/Varunha-portfolio?node-id=XX:XX`
   - The `node-id` part is critical — it tells Claude exactly which section to read

### Step 2 — Describe Interactions (Important)

Claude can read what things look like but **cannot guess how they should behave**. Before asking Claude, write down:

**For every interactive element, answer:**
- What triggers it? (hover / click / scroll / page load)
- What does it do? (fade in / slide up / scale / color change)
- How long does it take? (e.g. 300ms)
- Does it repeat? (infinite loop / once / on scroll)

**Examples of good descriptions:**
```
The work card lifts up (translateY -8px) on hover, shadow gets stronger, over 200ms ease-out.

The section heading fades in from below (translateY 40px → 0, opacity 0 → 1) 
when it enters the viewport, 600ms ease-out, staggered 100ms per word.

The background gradient slowly shifts colors in an infinite loop, 8 seconds per cycle.
```

### Step 3 — Animations in Figma (Auto-Detected)

If you add animations directly in Figma using **Figma Animate** (formerly Smart Animate), Claude will detect and extract them automatically. You do not need to describe these manually.

To add an animation in Figma:
1. Select the frame/layer
2. Click **Prototype** tab (top right)
3. Add an interaction → choose animation type and duration
4. Claude will call `get_motion_context` and extract the exact keyframes, easing curves, and timing

For animations that can't be done in Figma prototype (scroll-triggered, hover, etc.) — describe them in text as shown in Step 2.

### Step 4 — Come to Claude Code and Say:

```
Here is my new [section name] design:
[paste the Figma node link]

Interactions:
- [describe hover/click/scroll behaviors here]
- [describe any animations not in Figma prototype]
```

That is all Claude needs. Claude will:
1. Read the design from Figma (colors, fonts, layout, images)
2. Extract any Figma prototype animations automatically
3. Build the HTML/CSS/JS faithfully
4. Create a preview branch for you to review

### Step 5 — Review the Preview

After Claude builds it, you'll be asked to test it. To see your preview live:

1. Go to [github.com/varunhapurohit/varunhapurohit.github.io](https://github.com/varunhapurohit/varunhapurohit.github.io)
2. Click **Actions** tab
3. Click **"Deploy to GitHub Pages"** on the left
4. Click **"Run workflow"** (grey button, top right)
5. Select your preview branch from the dropdown
6. Click the green **"Run workflow"** button
7. Wait ~60 seconds → visit [varunhapurohit.github.io](https://varunhapurohit.github.io)

### Step 6 — Approve or Request Changes

**If it looks right:**
```
Looks good, merge to main.
```

**If something is off:**
```
The heading is too large on mobile. The card shadow is too strong. 
The animation feels slow — make it 300ms instead.
```
Be specific. Claude will fix and update the preview.

### Step 7 — Go Live

Once you approve, Claude merges to `main`. GitHub automatically deploys within 60 seconds. No other action needed.

---

## Providing Maximum Detail to Claude

The more context you give, the less back-and-forth is needed. Use this checklist when asking Claude to build something:

### Design Checklist
- [ ] Figma node-specific link (with `node-id` in the URL)
- [ ] Is this a new section or an update to an existing one?
- [ ] Where should it appear on the page? (e.g. "below the About section")

### Animation Checklist
- [ ] Page load animations — what animates in when the page loads?
- [ ] Scroll animations — what animates as the user scrolls down?
- [ ] Hover effects — what happens when the user hovers over buttons, cards, links?
- [ ] Click interactions — what happens on click? (open modal, expand, navigate)
- [ ] Looping animations — anything that runs continuously? (marquee, pulse, glow)
- [ ] Transition speed — how fast? (snappy = ~200ms, normal = ~400ms, slow = ~700ms)
- [ ] Easing — how does it feel? (ease-out = natural stop, ease-in-out = smooth, linear = mechanical)

### Content Checklist
- [ ] All text is finalized and spelled correctly in Figma
- [ ] All images/videos are placed in Figma (Claude will extract the URLs)
- [ ] Links — where do buttons/links point to? (external URL, email, section anchor)
- [ ] Videos — is the video on Vimeo? Paste the Vimeo embed URL

### Mobile Checklist
- [ ] Is there a mobile version in Figma? (add a 390px wide frame alongside the desktop one)
- [ ] If not, describe how it should adapt: stack vertically / hide elements / reduce font sizes

---

## Sections Already Built

| Section | Status | Node ID |
|---|---|---|
| Navbar (glassmorphism pill) | ✅ Live | `24:109` |
| Hero (marquee + portrait) | ✅ Live | `1:2` |
| About (black band) | ✅ Live | `31:210–31:212` |
| Work/Grey section | 🔲 Placeholder | — |
| Skills / Expertise | 🔲 Not started | — |
| Testimonials | 🔲 Not started | — |
| Contact / Footer | 🔲 Not started | — |

---

## Videos

Videos are hosted on **Vimeo** (not GitHub — GitHub has a 100MB file limit and no streaming).

To add a video:
1. Upload it to [vimeo.com](https://vimeo.com) on Varunha's account
2. Copy the embed link (looks like `https://player.vimeo.com/video/XXXXXXXXX`)
3. Tell Claude: *"Use this Vimeo embed for the work card: [link]"*

Claude will wrap it in a responsive iframe that matches the Figma design.

---

## Fonts

The site uses:

| Font | Used for | Source |
|---|---|---|
| New York / Georgia (serif) | Hero marquee text | System font (Apple) / Georgia fallback |
| Inter | Everything else (nav, body, headings) | Google Fonts |

If you use a different font in Figma, tell Claude the font name so it can load it from Google Fonts or Fontshare.

---

## Image Assets

Images placed in Figma are automatically extracted by Claude via the Figma MCP. **However, these URLs expire after 7 days.**

When a section is finalized (no more design changes expected):
- Tell Claude: *"Download and save the images for [section name] to the assets folder"*
- Claude will save them to `/assets/images/` and update the code to use permanent URLs

Do not use Figma asset URLs in production long-term.

---

## Branch Naming

When Claude creates a preview branch, it will be named like `feature_YYYYMMDD`. You can also request a specific name:

```
Build this in a branch called feature_work_section
```

---

## Common Requests and How to Phrase Them

| What you want | What to say |
|---|---|
| Add a new section | "Here is my new [name] section design: [figma link]. Interactions: [describe]" |
| Update existing text | "Change the About bio to: [new text]" |
| Change a color | "Change the navbar background to black with white text" |
| Add a hover animation | "When hovering over the work cards, lift them up slightly with a subtle shadow" |
| Add scroll animations | "Animate each section heading to fade up when it enters the viewport" |
| Fix spacing | "The gap between About and the grey section is too large, reduce it" |
| Go live | "Merge to main" |
| Preview a branch | "Deploy [branch name] to GitHub Pages" |
| Revert to previous design | "Roll back main to the previous version" |

---

## Questions?

Ask Claude directly. Claude has full access to the Figma file, the codebase, and deployment — and will tell you if anything is missing.
