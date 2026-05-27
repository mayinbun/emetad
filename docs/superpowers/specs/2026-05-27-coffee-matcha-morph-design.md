# Coffee → Matcha Morph Feature

## Summary

When the user selects the Coffee card on page 2, it morphs into Matcha via a fade crossfade animation, then prompts a confirmation. Declining the confirmation permanently replaces the card with a Pineapple-Mango Smoothie instead of restoring coffee.

## Flow

1. User clicks Coffee card (☕)
2. Coffee emoji fades out, Matcha emoji (🍵) fades in on same card; label updates to "Matcha"
3. Confirmation text: `"Are you sure? 💀"` with two buttons: **Yes** / **No**
4. **Yes** → proceeds to page 3 with message `"She wants Matcha. You better deliver. 💀"`
5. **No** → card resets to 🍍🥭 "Smoothie"; coffee is gone, no way back

## Scope

- Only the Coffee card is affected
- Burger, Sushi, Pizza cards: no change
- No new pages or routes — all within existing page 2 logic

## Implementation

- Fade crossfade: CSS `opacity` transition on the emoji `<span>` (~300ms)
- Confirmation buttons injected below `#food-confirmation` div when coffee is picked
- On **No**: swap card `data-food`, emoji, and label; hide confirmation buttons
- On **Yes**: hide buttons, call existing `showPage3()` after short delay

## States

| State | Card shows | Confirmation area shows |
|-------|-----------|------------------------|
| Initial | ☕ Coffee | nothing |
| After click | 🍵 Matcha (animating) | "Are you sure? 💀" + Yes/No |
| After Yes | 🍵 Matcha (selected) | proceeds to page 3 |
| After No | 🍍🥭 Smoothie (unselected) | cleared |