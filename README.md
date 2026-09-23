
# patterns.fscss

FSCSS pattern module — style by plain English, customize with phrase inputs via `@match` (FSCSS **1.2.4+**).

Demo library, not a full design system. Fork it, extend it, or use it as a template for your own pattern packs.

[GitHub](https://github.com/fscss-ttr/patterns.fscss) · [pattern() docs](https://fscss.devtem.org/pattern) · MIT

---

## What this is

FSCSS `pattern()` scores a phrase in your stylesheet against each pattern description. When the score clears the threshold, the template CSS is injected.

This module ships two `@define` helpers:

| Define | Role |
|--------|------|
| `pattern-root(sel:root)` | Design tokens (`--pattern-*`) on `:root` (or another selector) |
| `patterns(thr:0.65)` | Pattern library — cards, buttons, hovers, motion, layout, components, text, a11y |

Templates use **cascade defaults + `@match` overrides**. A default is always emitted first. When a match succeeds, the next declaration overrides it. When a match fails, the empty declaration is invalid and discarded — the default stays.

```css
background: var(--pattern-accent);           /* default */
background: @match((?:bg|background):\s*([#\w().%-]+));  /* override or discard */
```

---

## Description convention: role, not color

Descriptions name a **role** ("solid filled primary button", "ghost outline button"), never a literal color word. Color lives in tokens or arrives through `@match`.

```css
/* Good — role-based, generalizes */
"solid filled primary button"
"ghost outline button with accent border"

/* Avoid — bakes one color pair into match text */
"solid purple primary button with white label"
```

Baking colors into descriptions causes two problems:

1. **It goes stale.** If the accent in `pattern-root` changes, the description still talks about a color the pattern no longer produces.
2. **It pollutes matching.** `pattern()` scores against the description text, so color words become part of the vocabulary a phrase must resemble even though color is an input, not the concept.

Role words age fine. Color words don't.

---

## Requirements

- **FSCSS ≥ 1.2.4** (balanced `@match`, safer regex)
- CLI or runtime from the same major line

```bash
npm install fscss@1.2.4
```

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.4/runtime.min.js" defer></script>
```

VS Code: [Figsh.fscss](https://marketplace.visualstudio.com/items?itemName=Figsh.fscss)

---

## Usage

### Local

```css
@import((pattern-root, patterns) from "./patterns.fscss")

@pattern-root()
@patterns(0.4)   /* sample threshold — forgiving enough for short paraphrases */

.card {
  vibrant two tone gradient card from: #667eea to: #764ba2 color: #fff
}

.btn-primary {
  solid filled primary button bg: #344466
}

.btn-primary {
  lift up on hover with stronger shadow lift: -6px
}
```

### Published module (when published)

```css
@import((pattern-root, patterns) from patterns)
```

### Compile

```bash
fscss demo.fscss demo.css
```

Or open `demo.html` — the browser runtime processes `<link type="fscss">` live.

---

## Prompt inputs (`@match`)

| Phrase includes | Result |
|-----------------|--------|
| `bg: #0ea5e9` | override applied |
| omits `bg:` | default token kept |

Labels used across the module: `bg` / `background`, `color` / `text` / `label`, `from` / `to`, `border`, `radius`, `lift`, `scale`, `glow`, `duration` / `time`, `blur`, `width`, `size`, `ring`, `min` / `minwidth`, `max` / `maxwidth`, `pad` / `padding`, `gap`, `margin`, `thick` / `thickness`, `thumb`, `track`, `fill`, `lines`.

```css
.btn {
  solid filled primary button
}

.btn {
  solid filled primary button bg: #0ea5e9 color: #0f172a
}
```

---

## How `pattern()` works

```css
pattern(0.65: "solid filled primary button", `
  background: var(--pattern-accent);
  background: @match((?:bg|background):\s*([#\w().%-]+));
  color: var(--pattern-text-on-accent);
  color: @match((?:color|label|text):\s*([#\w().%-]+));
  /* … */
`)
```

- **threshold** — minimum similarity (shared via `@patterns(0.4)` → `@use(thr)`).
- **description** — plain English, role-based.
- **template** — CSS with cascade defaults + `@match` overrides.

---

## What's included

- **Cards** — gradient (from/to/color), soft elevated (bg/radius)
- **Buttons** — surface, solid primary, ghost outline (bg/color/border)
- **Hover** — lift, scale, glow (lift/scale/glow)
- **Motion** — fade-in-up, pulse, spin, shimmer + namespaced `pattern*` keyframes (duration)
- **Layout** — centered container (max/pad), auto-fit grid (min/gap), sticky translucent header, divider
- **Components** — input, pill badge, glass, tooltip, avatar + ring, skeleton, toggle track, progress track/fill, status alert, underline tab indicator, custom scrollbar
- **Text** — single-line truncate, multiline clamp (lines)
- **Accessibility** — keyboard focus ring, disabled muted state

---

## Sample threshold

The demo calls `@patterns(0.4)`.

| Threshold | Feel | Use for |
|-----------|------|---------|
| 1.0 (default if omitted) | Near-exact | Short generic labels |
| 0.7–0.9 | Strict | Named components |
| **0.4–0.5** | Forgiving | Demo / longer role phrases |
| ≤ 0.35 | Very open | Risky collisions |

Keep descriptions lexically distinct. Lower thresholds need unique nouns/verbs so the wrong template does not win.

---

## Contributing

1. Prefer **role-based** descriptions. No literal color words in the description text.
2. Keep descriptions **lexically distinct**.
3. Add `@match` only for values people actually vary; always emit a **token default first**.
4. Keep regexes simple (1.2.4 rejects hostile nested quantifiers). Prefer `[#\w().%-]+` for colors; avoid `\s` inside the capture group unless the value is intentionally multi-token (e.g. `rgba(...)`).
5. Prefix new `@keyframes` with `pattern` so they do not collide with other modules.
6. PR with a short note on the phrase and the CSS it produces.

Project-specific patterns belong in your own module. This repo is for broadly reusable demos.

---

## License

MIT
