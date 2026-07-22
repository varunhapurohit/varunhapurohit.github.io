# Figma → Code Sizing Rules

This file exists because sizing is the most common thing that breaks when translating Figma to HTML.  
Claude must read and follow these rules every time a new section is built or updated.

---

## The Core Problem

Figma uses **absolute pixel values** on a fixed canvas (1440×3408px for this site).  
The web is **fluid** — the browser window can be any size.  
Simply copying pixel values from Figma produces a site that looks wrong at any size other than 1440px wide.

**The fix:** Always convert Figma pixel values to proportional units (`vw`, `vh`, `svh`, `%`, `clamp()`).

---

## Conversion Rules

### Width

| Figma value | Convert to | Formula | Example |
|---|---|---|---|
| Element width in px | `vw` or `%` of parent | `(px / 1440) * 100` | `761px` → `52.85vw` |
| Max width | `min(Xpx, Y%)` | Keep both | `min(761px, 53vw)` |
| Full canvas width | `100%` or `100vw` | — | `1440px` → `100%` |

### Height

| Figma value | Convert to | Formula | Example |
|---|---|---|---|
| Element height relative to viewport | `svh` | `(px / 1024) * 100` | `854px` → `83.4svh` |
| Section min-height | `min-height` in `svh` | same | `518px` → `50.6svh` |
| Fixed decorative height | `px` is fine | — | `1px` divider lines |

> **Always use `svh` not `vh`** — `svh` accounts for mobile browser chrome (address bar). `vh` causes overflow bugs on iOS Safari.

### Font Size

Never use a fixed `px` font size for headings or display text. Always use `clamp()`:

```
clamp(MIN, PREFERRED, MAX)
```

| Figma font size | Clamp formula | Result |
|---|---|---|
| 300px display | `clamp(80px, 20vw, 300px)` | Scales with viewport |
| 96px heading | `clamp(48px, 7.5vw, 96px)` | Never smaller than 48px |
| 24px body | `clamp(16px, 1.8vw, 24px)` | Readable on all screens |
| 20px UI text | `clamp(14px, 1.5vw, 20px)` | — |

### Spacing / Padding / Gap

| Figma value | Convert to | Notes |
|---|---|---|
| Padding on sections | `clamp(40px, 5vw, 120px)` | Shrinks gracefully on mobile |
| Gap between flex/grid items | `clamp(24px, 4vw, 80px)` | — |
| Fixed small spacing (4, 8, 16px) | Keep as `px` | Fine for micro-spacing |

### Positioning (absolute elements)

Convert `left`/`top` from Figma canvas coordinates to `%` or `calc()`:

```
left: calc((figma_x / 1440) * 100%)
top:  calc((figma_y / hero_height) * 100%)
```

Example — Figma says `left: 339px, top: 173px` on a 1440×1024 canvas:
```css
left: calc(23.5%);   /* 339/1440 */
top:  calc(16.9%);   /* 173/1024 */
```

---

## Height-First vs Width-First

**The most common sizing mistake on this site:** using `width` to size an element that should be sized by `height`.

### When to use HEIGHT as the primary dimension

- **Portrait / character images** — the person's body should fill a proportion of the screen height, not width. A width-based portrait becomes too short on widescreen and too wide on tall mobile screens.
- **Full-screen hero elements** — anything meant to be "as tall as the viewport"
- **Vertical decorative shapes**

```css
/* ✅ Correct — portrait fills 84% of hero height */
.hero-portrait {
  height: 84svh;
  width: auto;
  max-width: 56vw; /* safety cap so it doesn't overflow on narrow screens */
}

/* ❌ Wrong — width-based portrait becomes too short on wide screens */
.hero-portrait {
  width: 53vw;
  height: auto;
}
```

### When to use WIDTH as the primary dimension

- Horizontal text elements
- Cards and content blocks
- Navigation bars
- Anything wider than it is tall

---

## This Site's Canvas Reference

| Property | Value |
|---|---|
| Figma canvas width | 1440px |
| Hero section height | 1024px |
| Figma's `12.5%` left margin | 180px (= 1440 × 0.125) |
| Portrait width (Figma) | 761px → `52.85vw` |
| Portrait height (Figma) | 854px → `83.4svh` |
| About section height | 518px → `50.6svh` min-height |
| Marquee font size | 300px → `clamp(80px, 20vw, 300px)` |

---

## Checklist — Before Committing Any Section

Claude must verify these before calling a section done:

- [ ] No hardcoded `width` or `height` in `px` on any image or major layout block (except small utility values ≤ 16px)
- [ ] All heading/display font sizes use `clamp()`
- [ ] Portrait and character images are sized by **height** (`height: Xsvh; width: auto`)
- [ ] Section padding uses `clamp()` or `vw`/`vh`
- [ ] Absolute-positioned elements use `%` or `calc(% of canvas)` not raw `px`
- [ ] Tested mentally at three widths: 375px (iPhone), 768px (tablet), 1440px (desktop)
- [ ] The visual proportions of the built section match the Figma screenshot

---

## How to Verify Sizing Matches Figma

After building a section, Claude should:

1. Call `get_screenshot` on the Figma node
2. Compare the proportions visually against the built output
3. If the portrait / hero / heading looks smaller or larger than in Figma, adjust the `clamp()` or `svh` value
4. Confirm the fix before reporting the section as done

**The screenshot comparison is not optional.** Sizing must be verified against the Figma source, not assumed to be correct.
