# Theme Switcher Design

## Summary

Add a multi-theme system to the dateme site. Gothic is the default. Iron Man is the first alternate theme. A theme icon row on page 1 lets the user switch. More themes can be added later by extending a `THEMES` object.

## Architecture

All theme data lives in a `THEMES` JS object keyed by theme name. Each entry contains CSS variable overrides and all theme-specific text/emoji strings. `setTheme(name)` applies CSS vars to `:root` and swaps all text nodes. No page reload needed.

Theme state is stored in `localStorage` so it persists on reload.

## Theme Switcher UI

- Top-right corner of page 1, `position: fixed` so it doesn't shift layout
- Icon row: one button per theme (`💀` gothic, `⚡` ironman)
- Active theme button: accent-colored border + glow
- Inactive buttons: dimmed border

## Gothic Theme (default — no change to existing vars)

| Variable | Value |
|---|---|
| `--bg` | `#0d0d0d` |
| `--bg-secondary` | `#1a1a1a` |
| `--accent` | `#8b0000` |
| `--accent2` | `#6a0dad` |
| `--text` | `#e8e8e8` |
| `--text-dim` | `#888` |
| `--btn-yes-bg` | `#8b0000` |
| `--btn-yes-hover` | `#a80000` |
| `--btn-no-bg` | `#1a1a1a` |
| `--btn-no-border` | `#555` |
| `--font-heading` | `'Cinzel Decorative', serif` |

## Iron Man Theme

| Variable | Value |
|---|---|
| `--bg` | `#0a0a1a` |
| `--bg-secondary` | `#1a1a3e` |
| `--accent` | `#c0392b` |
| `--accent2` | `#e8c84a` |
| `--text` | `#f0f0f0` |
| `--text-dim` | `#e8c84a` |
| `--btn-yes-bg` | `#c0392b` |
| `--btn-yes-hover` | `#e74c3c` |
| `--btn-no-bg` | `#1a1a3e` |
| `--btn-no-border` | `#e8c84a` |
| `--font-heading` | `'Impact', 'Arial Black', sans-serif` |

## Text/Emoji Overrides Per Theme

| Element | Gothic | Iron Man |
|---|---|---|
| Page 1 heading | `Will you date me? 💀` | `Will you date me? ⚡` |
| Page 1 subtext | `Even death can't stop this feeling.` | `I am Iron Man.` |
| Yes button | `Yes 💀` | `Assemble ⚡` |
| No button | `No` | `Retreat` |
| Page 2 heading | `What we feeling? 💀` | `Pick your mission. ⚡` |
| Page 3 heading | `When? 💀` | `When's the mission? ⚡` |
| Page 3 subtext | `Pick your moment of doom.` | `Set the coordinates.` |
| Page 3 button | `It's a date 💀` | `Lock it in ⚡` |
| Page 4 heading | `It's official.` | `Mission accepted.` |
| Page 4 sub | `Don't be late. 💀` | `Don't be late. 🔴` |
| Page 4 icon | `🖤` (heartbeat) | `⚡` (heartbeat) |
| Copy button | `Copy 🖤` | `Copy 🔴` |
| Copy message | `I said yes 🖤 [food] on [date]. Don't be late. 💀` | `Mission accepted ⚡ [food] on [date]. Don't be late. 🔴` |
| Background particle | `🖤` | `⚡` |
| Title skull | `💀` (pulsing) | `⚡` (pulsing) |

## Implementation Notes

- All text content uses `data-gothic` / `data-ironman` attributes on elements — `setTheme` reads the right attribute and sets `textContent`
- CSS vars applied via `document.documentElement.style.setProperty`
- `localStorage.setItem('theme', name)` on switch; read on page load
- Background particle spawner reads from theme config, not hardcoded emoji
- `#title-skull` span content swapped on theme change
