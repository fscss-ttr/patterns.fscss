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
| `patterns(thr:0.65)` | Pattern library — cards, buttons, hovers, motion, components |

Templates use **`@match(regex)`** so callers can pass colors, radii, durations, etc. **in the phrase**. If a match is missing, a **token default** still applies.

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
@patterns(0.7)

.card {
  beautiful gradient hello world card from: #667eea to: #764ba2 color: #fff
}

.btn-primary {
  solid purple primary button with white label bg: #5b21b6 color: #f8fafc
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

Descriptions keep **stable keywords** for scoring. Optional slots in the **call phrase** are harvested with `@match`. Typical labels:

| Label in phrase | Used for |
|-----------------|----------|
| `bg:` / `background:` | Fill |
| `color:` / `text:` / `label:` | Foreground |
| `from:` / `to:` | Gradient stops |
| `border:` | Border color |
| `radius:` | Border radius |
| `lift:` | Hover translateY |
| `scale:` | Hover scale factor |
| `glow:` | Hover glow color |
| `duration:` / `time:` | Animation length |
| `blur:` | Glass blur |
| `width:` | Border width |

Example — defaults if you omit slots:

```fscss
.btn {
  solid purple primary button with white label
}
```

Example — override in prose:

```fscss
.btn {
  solid purple primary button with white label bg: #0ea5e9 color: #0f172a
}
```

`@match` runs on the **caller line**, not the stored description. First capturing group wins; aliases can be chained so `bg:` or `background:` both work.

---

## How `pattern()` works

```fscss
pattern(0.65: "solid purple primary button with white label bg color", `
  background: @match(bg:?\s*([#\w()-]+)) var(--pattern-accent);
  color: @match(color:?\s*([#\w()-]+)) var(--pattern-text-on-accent);
`)
```

- **threshold** — minimum similarity (via `@patterns(0.7)` shared as `@use(thr)`).
- **description** — plain English + optional cue words (`bg`, `color`) so inputs stay in vocabulary.
- **template** — CSS with `@match` and token fallbacks.

---

## What's included

- **Cards** — gradient (from/to/color), soft elevated (bg/radius)
- **Buttons** — surface, solid primary, ghost (bg/color/border)
- **Hover** — lift, scale, glow (lift/scale/glow)
- **Motion** — fade-in-up, pulse, spin + keyframes (duration)
- **Components** — input, pill badge, glass (blur), tooltip, border initial

---

## Contributing

1. Prefer **result-oriented** descriptions developers would type.
2. Keep descriptions **lexically distinct** (unique nouns/verbs).
3. Add `@match` only for values people actually vary; always leave a **token default**.
4. Keep regexes simple (1.2.4 rejects hostile nested quantifiers).
5. PR with a short note on the phrase + what CSS it produces.

Project-specific patterns belong in your own module; this repo is for broadly reusable demos.

---

## License

MIT
