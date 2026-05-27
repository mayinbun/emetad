# Gothic DateMe Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file static website that asks "Will you date me?" with a gothic theme, an escaping Yes button, a shrinking No button, a skull celebration, and a food-picker page.

**Architecture:** Single `index.html` with all CSS in a `<style>` tag and all JS in a `<script>` tag. Two "pages" are `<div>` sections toggled with `display:none/block`. The CSS `:root` block is the entire theme — future variants swap only that block.

**Tech Stack:** HTML5, CSS3 (custom properties, keyframe animations, transitions), vanilla JS (no dependencies), Google Fonts (Cinzel)

---

## File Structure

- Create: `index.html` — entire app (HTML structure + `<style>` + `<script>`)

---

### Task 1: HTML skeleton + theme CSS

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create `index.html` with page 1 and page 2 shell**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Will You Date Me? 💀</title>
  <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&display=swap" rel="stylesheet">
  <style>
    /* THEME — swap this block for new variants */
    :root {
      --bg: #0d0d0d;
      --bg-secondary: #1a1a1a;
      --accent: #8b0000;
      --accent2: #6a0dad;
      --text: #e8e8e8;
      --text-dim: #888;
      --btn-yes-bg: #8b0000;
      --btn-yes-hover: #a80000;
      --btn-no-bg: #1a1a1a;
      --btn-no-border: #555;
      --font: 'Cinzel', serif;
      --celebration-emojis: '💀🦇🥀🖤';
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    #page1, #page2 {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      width: 100%;
      padding: 2rem;
      text-align: center;
    }

    #page2 { display: none; }
  </style>
</head>
<body>
  <div id="page1">
    <p>Page 1 placeholder</p>
  </div>
  <div id="page2">
    <p>Page 2 placeholder</p>
  </div>
  <script>
    // JS goes here
  </script>
</body>
</html>
```

- [ ] **Step 2: Open `index.html` in browser and verify**

Open file in browser. Should see dark background, "Page 1 placeholder" in white Cinzel font. No errors in console.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add html skeleton with gothic theme tokens"
```

---

### Task 2: Page 1 content + button layout

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace page 1 placeholder with real content**

Replace `<div id="page1">` contents with:

```html
<div id="page1">
  <h1 class="main-heading">Will you date me? 💀</h1>
  <p class="subtext">Even death can't stop this feeling.</p>
  <div class="button-area">
    <button id="btn-yes" onclick="handleYesClick()">Yes 💀</button>
    <button id="btn-no" onclick="handleNoClick()">No</button>
  </div>
</div>
```

- [ ] **Step 2: Add CSS for headings and buttons inside `<style>`**

Add after the body rule:

```css
.main-heading {
  font-size: clamp(2rem, 6vw, 4rem);
  font-weight: 900;
  color: var(--text);
  margin-bottom: 1rem;
  text-shadow: 0 0 20px var(--accent);
}

.subtext {
  font-size: clamp(0.9rem, 2vw, 1.2rem);
  color: var(--text-dim);
  margin-bottom: 3rem;
  font-style: italic;
}

.button-area {
  position: relative;
  display: flex;
  gap: 2rem;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 120px;
}

#btn-yes {
  position: absolute;
  padding: 1rem 2.5rem;
  font-family: var(--font);
  font-size: 1.2rem;
  font-weight: 700;
  background: var(--btn-yes-bg);
  color: var(--text);
  border: none;
  cursor: pointer;
  transition: left 0.35s cubic-bezier(.25,.8,.25,1), top 0.35s cubic-bezier(.25,.8,.25,1), background 0.2s;
  text-transform: uppercase;
  letter-spacing: 2px;
}

#btn-yes:hover { background: var(--btn-yes-hover); }

#btn-no {
  position: relative;
  padding: 1rem 2.5rem;
  font-family: var(--font);
  font-size: 1.2rem;
  font-weight: 400;
  background: var(--btn-no-bg);
  color: var(--text-dim);
  border: 1px solid var(--btn-no-border);
  cursor: pointer;
  transition: transform 0.3s ease, opacity 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 2px;
}
```

- [ ] **Step 3: Reload browser**

Both buttons visible on dark background. Heading glows faintly red. Subtext is dimmed. No console errors.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add page 1 content, heading, and button styles"
```

---

### Task 3: Yes button escape behavior

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Initialize Yes button position and add escape JS**

The Yes button uses `position: absolute` inside `.button-area`. On first render we need to place it. Add inside `<script>`:

```javascript
const btnYes = document.getElementById('btn-yes');
const btnNo = document.getElementById('btn-no');

// Place Yes button at center on load
function initYesButton() {
  const area = document.querySelector('.button-area');
  const areaRect = area.getBoundingClientRect();
  btnYes.style.left = (areaRect.width / 2 - btnYes.offsetWidth / 2) + 'px';
  btnYes.style.top = (areaRect.height / 2 - btnYes.offsetHeight / 2) + 'px';
}

function escapeYes() {
  const area = document.querySelector('.button-area');
  const areaRect = area.getBoundingClientRect();
  const maxX = areaRect.width - btnYes.offsetWidth;
  const maxY = areaRect.height - btnYes.offsetHeight;
  const newX = Math.random() * maxX;
  const newY = Math.random() * maxY;
  btnYes.style.left = newX + 'px';
  btnYes.style.top = newY + 'px';
}

btnYes.addEventListener('mouseenter', escapeYes);
window.addEventListener('load', initYesButton);
```

- [ ] **Step 2: Expand `.button-area` height so Yes has room to escape**

Update `.button-area` in CSS:

```css
.button-area {
  position: relative;
  display: flex;
  gap: 2rem;
  align-items: center;
  justify-content: center;
  width: min(600px, 90vw);
  height: 200px;
}
```

- [ ] **Step 3: Verify in browser**

Hover over "Yes 💀" button — it should slide away smoothly. Hover again — slides somewhere new. It never leaves the button-area box.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: yes button escapes on hover with smooth transition"
```

---

### Task 4: No button shrink-to-death

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add `handleNoClick` function inside `<script>`**

```javascript
let noClickCount = 0;
const NO_MAX_CLICKS = 5;

function handleNoClick() {
  noClickCount++;
  const scale = Math.max(0, 1 - noClickCount * 0.18);
  btnNo.style.transform = `scale(${scale})`;
  btnNo.style.opacity = scale;

  if (noClickCount >= NO_MAX_CLICKS) {
    btnNo.style.transition = 'transform 0.3s ease, opacity 0.5s ease';
    btnNo.style.opacity = '0';
    setTimeout(() => btnNo.remove(), 500);
  }
}
```

- [ ] **Step 2: Verify in browser**

Click "No" repeatedly. Button shrinks each click. After 5 clicks it fades out and disappears. Only Yes button remains.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: no button shrinks and disappears after 5 clicks"
```

---

### Task 5: Yes celebration sequence

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add celebration CSS keyframes inside `<style>`**

```css
@keyframes floatUp {
  0%   { transform: translateY(0) rotate(0deg); opacity: 1; }
  100% { transform: translateY(-100vh) rotate(720deg); opacity: 0; }
}

@keyframes fadeInScale {
  0%   { opacity: 0; transform: scale(0.5); }
  100% { opacity: 1; transform: scale(1); }
}

#celebration-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.85);
  z-index: 100;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

#celebration-overlay.active {
  display: flex;
}

#celebration-text {
  font-family: var(--font);
  font-size: clamp(2rem, 8vw, 5rem);
  font-weight: 900;
  color: var(--text);
  text-shadow: 0 0 40px var(--accent), 0 0 80px var(--accent2);
  text-align: center;
  animation: fadeInScale 0.6s ease forwards;
  padding: 1rem;
}

.float-emoji {
  position: fixed;
  font-size: 2rem;
  pointer-events: none;
  animation: floatUp linear forwards;
  z-index: 99;
}
```

- [ ] **Step 2: Add celebration overlay HTML before closing `</body>`**

```html
<div id="celebration-overlay">
  <div id="celebration-text">SHE SAID YES 💀🖤</div>
</div>
```

- [ ] **Step 3: Add `celebrate` and `handleYesClick` JS functions inside `<script>`**

```javascript
const CELEBRATION_EMOJIS = ['💀', '🦇', '🥀', '🖤', '🕸️', '☠️'];

function spawnEmoji() {
  const el = document.createElement('span');
  el.className = 'float-emoji';
  el.textContent = CELEBRATION_EMOJIS[Math.floor(Math.random() * CELEBRATION_EMOJIS.length)];
  el.style.left = Math.random() * 100 + 'vw';
  el.style.bottom = '-2rem';
  const duration = 1.5 + Math.random() * 2;
  el.style.animationDuration = duration + 's';
  document.body.appendChild(el);
  setTimeout(() => el.remove(), duration * 1000);
}

function celebrate() {
  const overlay = document.getElementById('celebration-overlay');
  overlay.classList.add('active');

  // Spawn emojis in bursts
  let spawned = 0;
  const interval = setInterval(() => {
    for (let i = 0; i < 5; i++) spawnEmoji();
    spawned++;
    if (spawned >= 10) clearInterval(interval);
  }, 200);

  // Transition to page 2 after 2.5s
  setTimeout(showPage2, 2500);
}

function handleYesClick() {
  celebrate();
}
```

- [ ] **Step 4: Add `showPage2` JS function inside `<script>`**

```javascript
function showPage2() {
  document.getElementById('page1').style.display = 'none';
  document.getElementById('celebration-overlay').style.display = 'none';
  document.getElementById('page2').style.display = 'flex';
}
```

- [ ] **Step 5: Verify in browser**

Catch and click Yes. Screen should flood with gothic emojis floating upward. "SHE SAID YES 💀🖤" appears centered. After 2.5s transitions to (still-placeholder) page 2.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: yes button triggers gothic skull celebration then transitions to page 2"
```

---

### Task 6: Page 2 — food picker

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace page 2 placeholder with food picker HTML**

Replace `<div id="page2">` contents:

```html
<div id="page2">
  <h1 class="main-heading">What we feeling? 💀</h1>
  <p class="subtext">Choose wisely.</p>
  <div class="food-grid">
    <div class="food-card" data-food="Burger" onclick="pickFood(this)">
      <span class="food-emoji">🍔</span>
      <span class="food-label">Burger</span>
    </div>
    <div class="food-card" data-food="Sushi" onclick="pickFood(this)">
      <span class="food-emoji">🍣</span>
      <span class="food-label">Sushi</span>
    </div>
    <div class="food-card" data-food="Pizza" onclick="pickFood(this)">
      <span class="food-emoji">🍕</span>
      <span class="food-label">Pizza</span>
    </div>
    <div class="food-card" data-food="Coffee" onclick="pickFood(this)">
      <span class="food-emoji">☕</span>
      <span class="food-label">Coffee</span>
    </div>
  </div>
  <p id="food-confirmation" class="food-confirmation"></p>
</div>
```

- [ ] **Step 2: Add food picker CSS inside `<style>`**

```css
.food-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.food-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
  padding: 2rem 2.5rem;
  background: var(--bg-secondary);
  border: 1px solid #333;
  cursor: pointer;
  transition: border-color 0.2s, box-shadow 0.2s, transform 0.15s;
  user-select: none;
}

.food-card:hover {
  border-color: var(--accent);
  transform: translateY(-3px);
}

.food-card.selected {
  border-color: var(--accent2);
  box-shadow: 0 0 20px var(--accent2), 0 0 40px var(--accent);
}

.food-emoji { font-size: 3rem; }

.food-label {
  font-family: var(--font);
  font-size: 1rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--text);
}

.food-confirmation {
  font-family: var(--font);
  font-size: 1.1rem;
  color: var(--accent2);
  text-shadow: 0 0 10px var(--accent2);
  min-height: 2rem;
  font-style: italic;
  animation: fadeInScale 0.4s ease forwards;
}
```

- [ ] **Step 3: Add `pickFood` JS function inside `<script>`**

```javascript
function pickFood(card) {
  // Deselect all
  document.querySelectorAll('.food-card').forEach(c => c.classList.remove('selected'));
  // Select clicked
  card.classList.add('selected');
  const food = card.dataset.food;
  const el = document.getElementById('food-confirmation');
  el.textContent = `She wants ${food}. You better deliver. 💀`;
}
```

- [ ] **Step 4: Verify full flow in browser**

1. Page loads → dark page with question and two buttons
2. Hover Yes → button escapes
3. Click No 5 times → No vanishes
4. Catch and click Yes → skull flood, celebration text, auto-transition
5. Page 2 shows food grid
6. Click a card → card glows purple, confirmation message appears below

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add food picker page 2 with gothic card styles and confirmation"
```

---

### Task 7: Polish + `.gitignore`

**Files:**
- Modify: `index.html`
- Create: `.gitignore`

- [ ] **Step 1: Add `.gitignore`**

```
.superpowers/
```

- [ ] **Step 2: Add subtle skull background texture via CSS**

Add inside `:root` and after `body` rule:

```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image: radial-gradient(ellipse at 20% 50%, rgba(139,0,0,0.08) 0%, transparent 60%),
                    radial-gradient(ellipse at 80% 20%, rgba(106,13,173,0.06) 0%, transparent 50%);
  pointer-events: none;
  z-index: 0;
}

#page1, #page2 { position: relative; z-index: 1; }
```

- [ ] **Step 3: Make Yes button position more stable on mobile**

Wrap `escapeYes` with a guard so button never escapes off-screen on small viewports — the existing `maxX/maxY` clamp already handles this, but add a minimum of 0:

```javascript
function escapeYes() {
  const area = document.querySelector('.button-area');
  const areaRect = area.getBoundingClientRect();
  const maxX = Math.max(0, areaRect.width - btnYes.offsetWidth);
  const maxY = Math.max(0, areaRect.height - btnYes.offsetHeight);
  const newX = Math.random() * maxX;
  const newY = Math.random() * maxY;
  btnYes.style.left = newX + 'px';
  btnYes.style.top = newY + 'px';
}
```

- [ ] **Step 4: Final browser check — full flow**

Walk through entire flow one more time. Check: no console errors, buttons look good, celebration fires, page 2 works, food confirmation shows.

- [ ] **Step 5: Commit**

```bash
git add index.html .gitignore
git commit -m "feat: add ambient background glow and gitignore"
```

---

### Task 8: Page 3 — Date & Time Picker

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add page 3 HTML shell after `#page2`**

```html
<div id="page3">
  <h1 class="main-heading">When? 💀</h1>
  <p class="subtext">Pick your moment of doom.</p>
  <div class="date-picker-container">
    <label class="date-label" for="date-input">Date</label>
    <input id="date-input" type="date" class="gothic-input">
    <label class="date-label" for="time-input">Time</label>
    <input id="time-input" type="time" class="gothic-input">
    <button class="confirm-btn" onclick="confirmDate()">It's a date 💀</button>
  </div>
  <p id="date-confirmation" class="food-confirmation"></p>
</div>
```

- [ ] **Step 2: Add `#page3` to CSS (reuse same hidden pattern)**

```css
#page3 { display: none; }
```

- [ ] **Step 3: Add gothic input + confirm button CSS inside `<style>`**

```css
.date-picker-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  width: min(360px, 90vw);
  margin-bottom: 2rem;
}

.date-label {
  font-family: var(--font);
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 3px;
  color: var(--text-dim);
  align-self: flex-start;
}

.gothic-input {
  width: 100%;
  padding: 0.9rem 1.2rem;
  background: var(--bg-secondary);
  border: 1px solid #333;
  color: var(--text);
  font-family: var(--font);
  font-size: 1rem;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
  color-scheme: dark;
}

.gothic-input:focus {
  border-color: var(--accent);
  box-shadow: 0 0 12px var(--accent);
}

.confirm-btn {
  margin-top: 1rem;
  padding: 1rem 2.5rem;
  font-family: var(--font);
  font-size: 1rem;
  font-weight: 700;
  background: var(--btn-yes-bg);
  color: var(--text);
  border: none;
  cursor: pointer;
  text-transform: uppercase;
  letter-spacing: 2px;
  transition: background 0.2s, box-shadow 0.2s;
}

.confirm-btn:hover {
  background: var(--btn-yes-hover);
  box-shadow: 0 0 20px var(--accent);
}
```

- [ ] **Step 4: Update `pickFood` to transition to page 3 after short delay**

Replace existing `pickFood` function:

```javascript
function pickFood(card) {
  document.querySelectorAll('.food-card').forEach(c => c.classList.remove('selected'));
  card.classList.add('selected');
  const food = card.dataset.food;
  const el = document.getElementById('food-confirmation');
  el.textContent = `She wants ${food}. You better deliver. 💀`;
  setTimeout(showPage3, 1800);
}
```

- [ ] **Step 5: Add `showPage3` and `confirmDate` JS functions**

```javascript
function showPage3() {
  document.getElementById('page2').style.display = 'none';
  document.getElementById('page3').style.display = 'flex';
}

function confirmDate() {
  const date = document.getElementById('date-input').value;
  const time = document.getElementById('time-input').value;
  if (!date || !time) {
    document.getElementById('date-confirmation').textContent = 'Pick both a date and time, mortal. 💀';
    return;
  }
  const formatted = new Date(date + 'T' + time).toLocaleString('en-US', {
    weekday: 'long', month: 'long', day: 'numeric',
    hour: '2-digit', minute: '2-digit'
  });
  document.getElementById('date-confirmation').textContent = `${formatted}. Don't be late. 💀`;
}
```

- [ ] **Step 6: Verify full flow in browser**

1. Page 1 → catch Yes → celebration → Page 2
2. Pick food → confirmation → 1.8s pause → Page 3
3. Try confirm without filling fields → error message
4. Fill date + time → click confirm → formatted date confirmation appears

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add page 3 gothic date/time picker"
```
