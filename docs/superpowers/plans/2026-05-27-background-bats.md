# Background Bats Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Continuously spawn 🦇 bats floating up from the bottom of the screen across all pages as ambient gothic background atmosphere.

**Architecture:** Reuse existing `floatUp` keyframe and `.float-emoji` CSS class. Add a `spawnBat` function and start a `setInterval` on page load that spawns 1–2 bats every 800ms. Bats use a lower z-index (1) than page content so they never obstruct interaction.

**Tech Stack:** Vanilla HTML/CSS/JS, no dependencies.

---

### Task 1: Add bat spawner and start on page load

**Files:**
- Modify: `index.html` — add `spawnBat` function and interval start in `<script>` block

- [ ] **Step 1: Add `spawnBat` function and auto-start interval**

In `index.html`, find the `<script>` block. After the `const CELEBRATION_EMOJIS` line (around line 433), add:

```js
function spawnBat() {
  const el = document.createElement('span');
  el.className = 'float-emoji';
  el.textContent = '🦇';
  el.style.left = Math.random() * 100 + 'vw';
  el.style.bottom = '-2rem';
  el.style.fontSize = (1.5 + Math.random()) + 'rem';
  el.style.zIndex = '1';
  const duration = 3 + Math.random() * 3;
  el.style.animationDuration = duration + 's';
  document.body.appendChild(el);
  setTimeout(() => el.remove(), duration * 1000);
}

setInterval(() => {
  const count = Math.random() < 0.5 ? 1 : 2;
  for (let i = 0; i < count; i++) spawnBat();
}, 800);
```

- [ ] **Step 2: Verify page content sits above bats**

Check that `.float-emoji` z-index default in CSS is `99` (already set). The bat override sets `z-index: 1` on each bat element inline — page content (`z-index` not set = auto = above z-index 1) will render on top.

Confirm in `index.html` around line 263:
```css
.float-emoji {
  position: fixed;
  font-size: 2rem;
  pointer-events: none;
  animation: floatUp linear forwards;
  z-index: 99;
}
```
Celebration emojis keep `z-index: 99` (via class). Bats override to `1` inline. No CSS change needed.

- [ ] **Step 3: Open app and verify bats**

```bash
open http://localhost:8765
```

Expected: bats spawn from bottom, float upward, vary in size (1.5–2.5rem), random horizontal position, disappear at top. 1–2 per ~800ms. Visible on page 1 immediately on load.

- [ ] **Step 4: Navigate through all pages and verify bats persist**

1. Click **Yes** → page 2: bats still flying
2. Pick a food → page 3: bats still flying
3. Confirm date → page 4: bats still flying

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add ambient background bat particles on all pages"
```

---

### Task 2: Push to GitHub

- [ ] **Step 1: Push**

```bash
git push
```

- [ ] **Step 2: Verify on GitHub Pages** (allow ~1 min)

Open https://mayinbun.github.io/emetad/ — bats should be flying immediately on load.