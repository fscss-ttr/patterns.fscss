# patterns.fscss

FSCSS pattern module — style by plain English, **customize with prompt inputs** via `@match` (FSCSS **1.2.4+**).

Demo library, not a full design system. Fork it, extend it, or use it as a template for your own pattern packs.

[GitHub](https://github.com/fscss-ttr/patterns.fscss) · [pattern() docs](https://fscss.devtem.org/pattern) · MIT

---

## What this is

FSCSS `pattern()` scores a phrase in your stylesheet against each pattern description. When the score clears the threshold, the template CSS is injected.

This module ships two `@define` helpers:

| Define | Role |
|--------|------|
| `pattern-root(sel:root)` | Design tokens (`--pattern-*`) on `:root` (or another selector) |
| `patterns(thr:0.65)` | Pattern library — cards, buttons, hovers, motion, layout, components, text, accessibility |

Templates use **`@match(regex)`** so callers can pass colors, radii, durations, etc. **in the phrase**. If a match is missing, a **token default** still applies.

---

## Description convention: role, not color

Descriptions name a **role** ("solid primary button", "ghost outline button"), never a literal color word. The actual color lives in the design tokens, or comes in through `@match` when a caller supplies one.

```fscss
/* Good: role-based, generalizes, still matches loosely typed phrases */
"solid filled primary button"
"ghost outline button with accent border"

/* Avoid: bakes one specific color pair into the match text */
"solid purple primary button with white label"
```

Baking colors into descriptions causes two problems:

1. **It goes stale.** If the accent color in `pattern-root` ever changes, the description keeps describing a color the pattern no longer produces.
2. **It pollutes matching.** `pattern()` scores against the description text, so "purple" and "white" become part of the vocabulary a phrase has to resemble, even though color is meant to be an input, not part of the concept.

Role words age fine. Color words don't. Keep them out of the description and let tokens or `@match` carry the actual value.

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

### Published module

```fscss
@import((pattern-root, patterns) from patterns)

@pattern-root()
@patterns(0.5)

.card {
  vibrant two tone gradient card from: #667eea to: #764ba2 color: #fff
}

.btn-primary {
  solid filled primary button bg: #5b21b6 color: #f8fafc
}

.btn-primary {
  lift up on hover with stronger shadow lift: -6px
}
```

### Local fork

```fscss
@import((pattern-root, patterns) from "./patterns.fscss")
```

### Compile

```bash
fscss style.fscss style.css
```

---

## Prompt inputs (`@match`)

Safe pattern used in this module:

```fscss
--_bg: @match((?:bg|background):\s*([#\w()-]+));
background: var(--_bg, var(--pattern-accent));
```

| Phrase | Result |
|--------|--------|
| includes `bg: #0ea5e9` | `--_bg` set, used |
| omits `bg:` | fallback, `var(--pattern-accent)` |

**Never** write `background: @match(...) var(--token)` (two values when matched).

Labels in use across the module: `bg` / `background`, `color` / `text` / `label`, `from` / `to`, `border`, `radius`, `lift`, `scale`, `glow`, `duration` / `time`, `blur`, `width`, `size`, `ring`, `min` / `minwidth`, `max` / `maxwidth`, `pad` / `padding`, `gap`, `margin`, `thick` / `thickness`, `thumb`, `track`, `fill`, `lines`.

```fscss
.btn {
  solid filled primary button
}

.btn {
  solid filled primary button bg: #0ea5e9 color: #0f172a
}
```

## How `pattern()` works

```fscss
pattern(0.65: "solid filled primary button bg color", `
  background: @match(bg:?\s*([#\w()-]+)) var(--pattern-accent);
  color: @match(color:?\s*([#\w()-]+)) var(--pattern-text-on-accent);
`)
```

- **threshold** — minimum similarity (via `@patterns(0.7)` shared as `@use(thr)`).
- **description** — plain English, role-based, plus optional cue words (`bg`, `color`) so inputs stay in vocabulary.
- **template** — CSS with `@match` and token fallbacks.

---

## What's included

- **Cards** — gradient (from/to/color), soft elevated (bg/radius)
- **Buttons** — surface, solid primary, ghost outline (bg/color/border)
- **Hover** — lift, scale, glow (lift/scale/glow)
- **Motion** — fade-in-up, pulse, spin, shimmer + keyframes (duration), all namespaced `pattern*` to avoid collisions with other imported modules
- **Layout** — centered container (maxwidth/padding), auto-fit grid (minwidth/gap), sticky translucent header, divider
- **Components** — input, pill badge, glass, tooltip, border initial, avatar with ring, skeleton loader, toggle track, progress track/fill, status alert banner, underline tab indicator, custom scrollbar
- **Text** — single line truncate, multiline clamp (lines)
- **Accessibility** — keyboard focus ring, disabled muted state

---

## Contributing

1. Prefer **result-oriented, role-based** descriptions developers would type. No literal color words in the description text, colors are tokens or `@match` inputs.
2. Keep descriptions **lexically distinct** (unique nouns/verbs).
3. Add `@match` only for values people actually vary; always leave a **token default**.
4. Keep regexes simple (1.2.4 rejects hostile nested quantifiers).
5. Prefix any new `@keyframes` name with the module namespace (`pattern*`) so it doesn't collide with other imported modules.
6. PR with a short note on the phrase and what CSS it produces.

Project-specific patterns belong in your own module, this repo is for broadly reusable demos.

---

## License

MIT
