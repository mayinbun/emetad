# Theme Switcher Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a multi-theme system with Gothic (default) and Iron Man themes, switchable via an icon row on page 1, persisted in localStorage.

**Architecture:** A `THEMES` JS object holds all CSS var overrides and text/emoji strings per theme. `setTheme(name)` applies CSS vars to `:root` and updates all `data-gothic`/`data-ironman` attributed elements. The background particle spawner reads emoji from the active theme config. Theme switcher is a fixed icon row top-right of page 1.

**Tech Stack:** Vanilla HTML/CSS/JS, Google Fonts (already loaded), localStorage.

---

### Task 1: Add data-theme attributes to all themeable text/emoji elements

**Files:**
- Modify: `index.html` (HTML section, lines 389–446)

This task adds `data-gothic` and `data-ironman` attributes to every element whose text changes between themes. No visual change yet.

- [ ] **Step 1: Update page 1 elements**

Find and replace the page 1 section:

```html
  <div id="page1">
    <div class="portrait-frame">
      <img src="me.png" alt="" class="portrait-img">
    </div>
    <h1 class="main-heading" id="main-title">Will you date me? <span id="title-skull" data-gothic="💀" data-ironman="⚡">💀</span></h1>
    <p class="subtext" data-gothic="Even death can't stop this feeling." data-ironman="I am Iron Man.">Even death can't stop this feeling.</p>
    <div class="button-area">
      <button id="btn-yes" onclick="handleYesClick()" data-gothic="Yes 💀" data-ironman="Assemble ⚡">Yes 💀</button>
      <button id="btn-no" onclick="handleNoClick()" data-gothic="No" data-ironman="Retreat">No</button>
    </div>
  </div>
```

- [ ] **Step 2: Update celebration overlay**

```html
  <div id="celebration-overlay">
    <div id="celebration-text" data-gothic="SHE SAID YES 💀🖤" data-ironman="AVENGERS ASSEMBLE ⚡🔴">SHE SAID YES 💀🖤</div>
  </div>
```

- [ ] **Step 3: Update page 2**

```html
  <div id="page2">
    <h1 class="main-heading" data-gothic="What we feeling? 💀" data-ironman="Pick your mission. ⚡">What we feeling? 💀</h1>
    <p class="subtext" data-gothic="Choose wisely." data-ironman="Choose your target.">Choose wisely.</p>
```

- [ ] **Step 4: Update page 3**

```html
  <div id="page3">
    <h1 class="main-heading" data-gothic="When? 💀" data-ironman="When's the mission? ⚡">When? 💀</h1>
    <p class="subtext" data-gothic="Pick your moment of doom." data-ironman="Set the coordinates.">Pick your moment of doom.</p>
```

Find the confirm-btn inside page 3 and update:
```html
      <button class="confirm-btn" onclick="confirmDate()" data-gothic="It's a date 💀" data-ironman="Lock it in ⚡">It's a date 💀</button>
```

- [ ] **Step 5: Update page 4**

```html
  <div id="page4">
    <div class="confirmed-skull" data-gothic="🖤" data-ironman="⚡">🖤</div>
    <h1 class="main-heading confirmed-heading" data-gothic="It's official." data-ironman="Mission accepted.">It's official.</h1>
    <p id="confirmed-date" class="confirmed-date"></p>
    <p class="confirmed-sub" data-gothic="Don't be late. 💀" data-ironman="Don't be late. 🔴">Don't be late. 💀</p>
    <button id="copy-btn" class="confirm-btn" style="margin-top:1.5rem" onclick="copyResult()" data-gothic="Copy 🖤" data-ironman="Copy 🔴">Copy 🖤</button>
  </div>
```

- [ ] **Step 6: Verify page still looks identical in browser**

```bash
open http://localhost:8765
```

Expected: no visual change — all text same as before. Check all 4 pages by clicking through.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add data-gothic/data-ironman attributes to themeable elements"
```

---

### Task 2: Add THEMES object and setTheme function

**Files:**
- Modify: `index.html` (script block, after `let pickedFood = '';`)

- [ ] **Step 1: Add Google Fonts link for Impact fallback (already system font — no change needed)**

Impact is a system font on all major OS. No additional font loading required.

- [ ] **Step 2: Add THEMES object and setTheme after `let pickedFood = '';`**

```js
    const THEMES = {
      gothic: {
        vars: {
          '--bg': '#0d0d0d',
          '--bg-secondary': '#1a1a1a',
          '--accent': '#8b0000',
          '--accent2': '#6a0dad',
          '--text': '#e8e8e8',
          '--text-dim': '#888',
          '--btn-yes-bg': '#8b0000',
          '--btn-yes-hover': '#a80000',
          '--btn-no-bg': '#1a1a1a',
          '--btn-no-border': '#555',
          '--font-heading': "'Cinzel Decorative', serif",
        },
        particle: '🖤',
        copyPrefix: 'I said yes 🖤',
        copySuffix: "Don't be late. 💀",
      },
      ironman: {
        vars: {
          '--bg': '#0a0a1a',
          '--bg-secondary': '#1a1a3e',
          '--accent': '#c0392b',
          '--accent2': '#e8c84a',
          '--text': '#f0f0f0',
          '--text-dim': '#e8c84a',
          '--btn-yes-bg': '#c0392b',
          '--btn-yes-hover': '#e74c3c',
          '--btn-no-bg': '#1a1a3e',
          '--btn-no-border': '#e8c84a',
          '--font-heading': "'Impact', 'Arial Black', sans-serif",
        },
        particle: '⚡',
        copyPrefix: 'Mission accepted ⚡',
        copySuffix: "Don't be late. 🔴",
      },
    };

    let activeTheme = localStorage.getItem('theme') || 'gothic';

    function setTheme(name) {
      const theme = THEMES[name];
      if (!theme) return;
      activeTheme = name;
      localStorage.setItem('theme', name);

      // Apply CSS vars
      const root = document.documentElement;
      Object.entries(theme.vars).forEach(([k, v]) => root.style.setProperty(k, v));

      // Swap text content on all data-attributed elements
      document.querySelectorAll(`[data-${name}]`).forEach(el => {
        el.textContent = el.getAttribute(`data-${name}`);
      });

      // Update switcher button active states
      document.querySelectorAll('.theme-btn').forEach(btn => {
        btn.classList.toggle('theme-btn-active', btn.dataset.theme === name);
      });
    }
```

- [ ] **Step 3: Apply saved theme on page load — add at end of script, before closing `</script>`**

```js
    // Apply theme on load
    setTheme(activeTheme);
```

- [ ] **Step 4: Verify in browser — switch theme via console**

Open http://localhost:8765, open devtools console, run:
```js
setTheme('ironman')
```
Expected: colors change to navy/crimson/gold, all text updates (headings, buttons, page 4 icon). Run `setTheme('gothic')` to revert.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add THEMES object and setTheme function with localStorage persistence"
```

---

### Task 3: Update spawnBat and copyResult to use active theme

**Files:**
- Modify: `index.html` (script block — `spawnBat` and `copyResult` functions)

- [ ] **Step 1: Update spawnBat to read emoji from active theme**

Find:
```js
    function spawnBat() {
      const el = document.createElement('span');
      el.className = 'float-emoji';
      el.textContent = '🖤';
```

Replace:
```js
    function spawnBat() {
      const el = document.createElement('span');
      el.className = 'float-emoji';
      el.textContent = THEMES[activeTheme].particle;
```

- [ ] **Step 2: Update copyResult to use theme copy strings**

Find:
```js
    function copyResult() {
      const date = document.getElementById('confirmed-date').textContent;
      const text = `I said yes 🖤 ${pickedFood} on ${date}. Don't be late. 💀`;
```

Replace:
```js
    function copyResult() {
      const date = document.getElementById('confirmed-date').textContent;
      const t = THEMES[activeTheme];
      const text = `${t.copyPrefix} ${pickedFood} on ${date}. ${t.copySuffix}`;
```

- [ ] **Step 3: Verify particle and copy use theme values**

1. Open http://localhost:8765, run `setTheme('ironman')` in console
2. Wait 4s — particle should be ⚡ not 🖤
3. Click through to page 4, click Copy — paste somewhere and verify text starts with "Mission accepted ⚡"
4. Run `setTheme('gothic')`, wait — particle back to 🖤, copy text back to "I said yes 🖤"

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: particle and copy text read from active theme config"
```

---

### Task 4: Add theme switcher UI to page 1

**Files:**
- Modify: `index.html` (CSS style block + page 1 HTML)

- [ ] **Step 1: Add theme switcher CSS before closing `</style>`**

```css
    .theme-switcher {
      position: fixed;
      top: 1rem;
      right: 1rem;
      display: flex;
      gap: 8px;
      z-index: 100;
    }

    .theme-btn {
      background: transparent;
      border: 2px solid #444;
      border-radius: 8px;
      padding: 6px 10px;
      font-size: 1.1rem;
      cursor: pointer;
      color: #888;
      transition: border-color 0.2s, box-shadow 0.2s, color 0.2s;
    }

    .theme-btn:hover {
      border-color: var(--accent);
      color: var(--text);
    }

    .theme-btn-active {
      border-color: var(--accent);
      color: var(--text);
      box-shadow: 0 0 10px var(--accent);
    }
```

- [ ] **Step 2: Add theme switcher HTML inside `<body>`, before `<div id="page1">`**

```html
  <div class="theme-switcher">
    <button class="theme-btn theme-btn-active" data-theme="gothic" onclick="setTheme('gothic')">💀</button>
    <button class="theme-btn" data-theme="ironman" onclick="setTheme('ironman')">⚡</button>
  </div>
```

- [ ] **Step 3: Verify switcher in browser**

Open http://localhost:8765.
- 💀 button top-right, highlighted with red glow (active)
- Click ⚡ — colors shift to Iron Man, ⚡ button gets gold glow, 💀 dims
- Click 💀 — back to gothic
- Reload page — last selected theme restores from localStorage

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add theme switcher icon row to page 1"
```

---

### Task 5: Push to GitHub

- [ ] **Step 1: Push**

```bash
git push
```

- [ ] **Step 2: Verify on GitHub Pages** (allow ~1 min)

Open https://mayinbun.github.io/emetad/ — theme switcher visible top-right, both themes work end-to-end.