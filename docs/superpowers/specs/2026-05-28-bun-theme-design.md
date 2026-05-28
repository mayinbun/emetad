# Bun Theme Design

## Summary

Add a third theme: FF14-inspired Bunny Boy. Navy/gold/FF-blue color palette, bunny puns throughout, 🐰 particles, coffee→carrot morph (not matcha), carrot No → lettuce. Switcher gets a 🐰 icon button.

## Colors

| Variable | Value |
|---|---|
| `--bg` | `#0a0d1a` |
| `--bg-secondary` | `#141929` |
| `--accent` | `#c8a84b` |
| `--accent2` | `#4a90d9` |
| `--text` | `#f0ece0` |
| `--text-dim` | `#a09070` |
| `--btn-yes-bg` | `#c8a84b` |
| `--btn-yes-hover` | `#d4b85a` |
| `--btn-no-bg` | `#141929` |
| `--btn-no-border` | `#4a90d9` |
| `--font-heading` | `'Cinzel', serif` |

## Text/Emoji Overrides

| Element | Bun theme |
|---|---|
| Page 1 heading skull | `🐰` |
| Page 1 subtext | `Hop to it.` |
| Yes button | `Yes please! 🐰` |
| No button | `Lettuce not.` |
| Celebration text | `SHE SAID YES 🐰🥕` |
| Page 2 heading | `What's on the menu? 🐰` |
| Page 2 subtext | `Choose your carrot.` |
| Page 3 heading | `When? 🐰` |
| Page 3 subtext | `Pick your burrow time.` |
| Page 3 confirm button | `It's a date 🐰` |
| Page 4 heading | `Hare we go!` |
| Page 4 sub | `Don't be late. 🐰` |
| Page 4 icon | `🐰` |
| Copy button | `Copy 🐰` |
| Copy prefix | `I said yes 🐰` |
| Copy suffix | `Don't be late. 🥕` |
| Particle | `🐰` |

## Coffee Morph — Bun Theme Override

In Bun theme, `morphCoffeeToMatcha` behavior changes:
- Coffee → 🥕 Carrot (not 🍵 Matcha)
- Label → "Carrot"
- `data-food` → "Carrot"
- Confirmation Yes text: `"She wants Carrot. Hop to it. 🐰"`
- No → resets card to 🥬 "Lettuce" (not 🍍🥭 Smoothie)

## Implementation Notes

- Add `bun` key to `THEMES` object in JS
- Add `data-bun` attributes to all themeable elements
- `morphCoffeeToMatcha` reads active theme to decide target emoji/label/reset food
- Add `🐰` button to `.theme-switcher` HTML
- No gif on page 4 for bun theme (skip)
- Tune on page 4: reuse gothic Bach organ (fits the dramatic bunny energy)
