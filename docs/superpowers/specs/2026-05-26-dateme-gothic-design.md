# "Will You Date Me?" — Gothic Edition

## Overview

Single-file static website. Two-page flow. Gothic theme with skull motifs. Built for maximum chaos and fun.

## Theme System

All visual tokens live in a single CSS `:root` block. Swapping themes = replacing one block.

```css
:root {
  --bg: #0d0d0d;
  --bg-secondary: #1a1a1a;
  --accent: #8b0000;
  --accent2: #6a0dad;
  --text: #e8e8e8;
  --font-heading: 'Cinzel', serif;
  --font-body: 'Cinzel', serif;
  --emoji-skull: '💀';
  --emoji-extra: '🦇 🥀 🖤';
}
```

Future variants (cute-pink, space, cottagecore) override this block only. No other code changes needed.

## Page 1 — The Ask

**Layout:** Centered vertically and horizontally. Dark full-viewport background.

**Content:**
- Heading: "Will you date me? 💀"
- Subtext: "Even death can't stop this feeling."
- Two buttons: `Yes 💀` and `No`

### "Yes" Button Behavior

- On `mouseenter`: button animates to a random position within viewport bounds (CSS `transition` + JS random x/y)
- Keeps escaping every hover
- On `click` (when finally caught): triggers celebration sequence

### "No" Button Behavior

- Each `click`: button scales down by ~20% (CSS transform scale)
- After 5 clicks: button fades out and is removed from DOM
- Only `Yes` remains — player must catch it

### Yes Celebration Sequence

1. Screen floods with skull/bat/rose emojis via JS-generated DOM elements (absolute positioned, CSS keyframe fall/float animation)
2. Dark confetti burst (pure CSS or lightweight JS — no external lib)
3. Bold overlay text: `SHE SAID YES 💀🖤`
4. After 2.5s auto-transition to Page 2

## Page 2 — "What We Feeling?"

**Layout:** Same centered dark layout. Replaces Page 1 content (no page reload — JS show/hide).

**Content:**
- Heading: "What we feeling? 💀"
- Subtext: "Choose wisely."
- 4 option cards in a 2×2 grid:
  - 🍔 Burger
  - 🍣 Sushi
  - 🍕 Pizza
  - ☕ Coffee

**On card click:**
- Card highlights (accent border glow)
- Confirmation text appears below grid: `"She wants [X]. You better deliver. 💀"`

## Architecture

Single `index.html` file. No build step, no dependencies, no CDN (except Google Fonts for Cinzel).

```
index.html
  <head>
    - Google Fonts: Cinzel
    - <style> — all CSS including :root theme block
  </head>
  <body>
    - #page1 — ask flow
    - #page2 — food picker (hidden initially)
    - <script> — all JS inline
  </body>
```

### JS Responsibilities

- `escapingYes()` — calculates random safe position within viewport, applies via `style.left/top`
- `shrinkNo()` — tracks click count, applies scale transform, removes at 5
- `celebrate()` — spawns emoji elements, triggers confetti, shows overlay, calls `showPage2()` after delay
- `showPage2()` — hides `#page1`, shows `#page2`
- `pickFood(choice)` — highlights card, renders confirmation message

### CSS Responsibilities

- `:root` theme block — all tokens
- Keyframe animations: `@keyframes floatUp` (celebration emojis), `@keyframes fadeIn` (page transitions)
- Button escape: `transition: left 0.3s ease, top 0.3s ease; position: absolute;`
- No button scales: `transform: scale(var(--no-scale))` controlled by CSS variable updated via JS

## Out of Scope

- Backend, analytics, form submission
- Mobile layout (desktop-first, mobile acceptable if it works)
- Sound effects
- Multiple simultaneous themes — one active at a time
