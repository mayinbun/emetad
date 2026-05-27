# Coffee → Matcha Morph Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** When the user picks Coffee on page 2, the card morphs to Matcha via a fade animation, shows an "Are you sure?" prompt, and if declined resets the card to a Pineapple-Mango Smoothie.

**Architecture:** All changes are in `index.html` (single-file static site). Add a CSS fade keyframe for the emoji swap, add confirmation buttons to the food-confirmation area, and extend `pickFood()` with coffee-specific branching logic.

**Tech Stack:** Vanilla HTML/CSS/JS — no build tools, no dependencies.

---

### Task 1: Add CSS for emoji fade transition and confirmation buttons

**Files:**
- Modify: `index.html` (inside `<style>` block, around line 174 where `.food-emoji` is defined)

- [ ] **Step 1: Add fade keyframe and transition styles**

Find this in the `<style>` block:
```css
.food-emoji { font-size: 3rem; }
```

Replace with:
```css
.food-emoji {
  font-size: 3rem;
  transition: opacity 0.3s ease;
}

.food-emoji.fade-out { opacity: 0; }
```

Then add after `.food-label { ... }` closing brace (around line 183):
```css
.food-confirm-btns {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-top: 0.75rem;
}

.food-confirm-btns button {
  font-family: var(--font);
  font-size: 0.85rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 2px;
  padding: 0.5rem 1.25rem;
  background: var(--bg-secondary);
  border: 1px solid var(--accent);
  color: var(--text);
  cursor: pointer;
  transition: background 0.2s, box-shadow 0.2s;
}

.food-confirm-btns button:hover {
  background: var(--accent);
  box-shadow: 0 0 10px var(--accent);
}
```

- [ ] **Step 2: Open the app and verify styles load without errors**

```bash
open http://localhost:8765
```

Open browser devtools console — no errors expected.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add CSS for emoji fade and matcha confirm buttons"
```

---

### Task 2: Implement coffee morph logic in `pickFood()`

**Files:**
- Modify: `index.html` (inside `<script>` block, `pickFood` function ~line 437)

- [ ] **Step 1: Replace `pickFood` with the new branching version**

Find and replace the entire `pickFood` function:

```js
function pickFood(card) {
  document.querySelectorAll('.food-card').forEach(c => c.classList.remove('selected'));
  card.classList.add('selected');
  const food = card.dataset.food;

  if (food === 'Coffee') {
    morphCoffeeToMatcha(card);
    return;
  }

  const el = document.getElementById('food-confirmation');
  el.textContent = `She wants ${food}. You better deliver. 💀`;
  removeCoffeeConfirmBtns();
  setTimeout(showPage3, 1800);
}
```

- [ ] **Step 2: Add `morphCoffeeToMatcha` function directly after `pickFood`**

```js
function morphCoffeeToMatcha(card) {
  const emojiEl = card.querySelector('.food-emoji');
  const labelEl = card.querySelector('.food-label');
  const confirmEl = document.getElementById('food-confirmation');

  emojiEl.classList.add('fade-out');

  setTimeout(() => {
    emojiEl.textContent = '🍵';
    card.dataset.food = 'Matcha';
    labelEl.textContent = 'Matcha';
    emojiEl.classList.remove('fade-out');
  }, 300);

  confirmEl.textContent = 'Are you sure? 💀';
  showCoffeeConfirmBtns(card);
}

function showCoffeeConfirmBtns(card) {
  removeCoffeeConfirmBtns();
  const confirmEl = document.getElementById('food-confirmation');
  const btns = document.createElement('div');
  btns.className = 'food-confirm-btns';
  btns.id = 'coffee-confirm-btns';

  const yes = document.createElement('button');
  yes.textContent = 'Yes 💀';
  yes.onclick = () => {
    removeCoffeeConfirmBtns();
    confirmEl.textContent = "She wants Matcha. You better deliver. 💀";
    setTimeout(showPage3, 1800);
  };

  const no = document.createElement('button');
  no.textContent = 'No';
  no.onclick = () => resetCoffeeToSmoothie(card);

  btns.appendChild(yes);
  btns.appendChild(no);
  confirmEl.insertAdjacentElement('afterend', btns);
}

function removeCoffeeConfirmBtns() {
  const existing = document.getElementById('coffee-confirm-btns');
  if (existing) existing.remove();
}

function resetCoffeeToSmoothie(card) {
  const emojiEl = card.querySelector('.food-emoji');
  const labelEl = card.querySelector('.food-label');
  const confirmEl = document.getElementById('food-confirmation');

  emojiEl.classList.add('fade-out');

  setTimeout(() => {
    emojiEl.textContent = '🍍🥭';
    card.dataset.food = 'Smoothie';
    labelEl.textContent = 'Smoothie';
    emojiEl.classList.remove('fade-out');
  }, 300);

  card.classList.remove('selected');
  confirmEl.textContent = '';
  removeCoffeeConfirmBtns();
}
```

- [ ] **Step 3: Verify in browser — happy path**

1. Open http://localhost:8765
2. Click **Yes** on page 1
3. Click **Coffee** card
4. Verify: emoji fades ☕ → 🍵, label → "Matcha", text → "Are you sure? 💀", two buttons appear
5. Click **Yes** → verify proceeds to page 3 after ~1.8s

- [ ] **Step 4: Verify in browser — No path**

1. Repeat steps 1–4 above
2. Click **No** → verify card shows 🍍🥭 "Smoothie", deselected, confirmation cleared, buttons gone

- [ ] **Step 5: Verify other cards unaffected**

Click Burger, Sushi, Pizza — each should proceed normally to page 3 with `"She wants X. You better deliver. 💀"`

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: coffee card morphs to matcha with confirmation; declines reset to smoothie"
```

---

### Task 3: Push to GitHub

- [ ] **Step 1: Push**

```bash
git push
```

- [ ] **Step 2: Verify GitHub Pages reflects change** (allow ~1 min for Pages to rebuild)

Open https://mayinbun.github.io/emetad/ and test the coffee flow.