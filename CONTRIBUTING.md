# Contributing to patterns.fscss

Thanks for helping improve this demo pattern library. The goal is a small, reusable set of **role-based** patterns that work with FSCSS **1.2.4+** — not a full design system.

---

## Before you start

- Read the [pattern() docs](https://fscss.devtem.org/pattern)
- Use **FSCSS ≥ 1.2.4** (balanced `@match`, safer regex)
- Prefer a local fork of `patterns.fscss` + the demo for experiments

```bash
npm install fscss@1.2.4
fscss demo.fscss demo.css
# or open demo.html with the browser runtime
```

---

## What belongs here

| In scope | Out of scope |
|----------|----------------|
| Broadly reusable UI roles (card, button, alert, grid, …) | Project-specific or brand-only patterns |
| Role-based descriptions + token / `@match` values | Color words baked into descriptions |
| Cascade defaults + safe `@match` overrides | Empty intermediate custom properties that block fallbacks |
| Namespaced `pattern*` keyframes | Generic keyframe names that collide with other modules |

If a pattern only makes sense for one product, keep it in that product’s own module.

---

## Description rules

1. **Role, not color**  
   Name what the thing *is* (`solid filled primary button`), not a literal color (`solid purple button`). Colors belong in `pattern-root` tokens or in the caller’s phrase via `@match`.

2. **Lexically distinct**  
   Unique nouns/verbs so low thresholds do not pick the wrong template. Prefer “render chart” vs “create array” style contrast when adding similar ideas.

3. **Phrase people would type**  
   Short, result-oriented English. Avoid jargon that only the author would use.

```css
/* Good */
"soft elevated surface card with subtle shadow"
"ghost outline button with accent border"

/* Avoid */
"soft elevated surface card with subtle shadow bg white radius 16"
"solid purple primary button with white label"
```

---

## Template rules

### Cascade defaults (required)

Always emit a working default **first**. Put `@match` on a following declaration so a failed match produces an empty/invalid line that the cascade discards.

```css
/* Correct */
background: var(--pattern-accent);
background: @match((?:bg|background):\s*([#\w().%-]+));

/* Avoid — empty --_bg blocks the fallback */
--_bg: @match((?:bg|background):\s*([#\w().%-]+));
background: var(--_bg, var(--pattern-accent));
```

### Safe `@match` regexes

- Prefer simple character classes: `[#\w().%-]+` for colors/tokens, `[\d.]+(?:px|rem|em|%)?` for lengths.
- **Do not** put `\s` inside the capture group unless the value is intentionally multi-token (e.g. `rgba(...)`).
- One clear capturing group per `@match`.
- 1.2.4 rejects hostile nested quantifiers and overlong sources — keep patterns simple.

Labels already in use: `bg` / `background`, `color` / `text` / `label`, `from` / `to`, `border`, `radius`, `lift`, `scale`, `glow`, `duration` / `time`, `blur`, `width`, `size`, `ring`, `min` / `minwidth`, `max` / `maxwidth`, `pad` / `padding`, `gap`, `margin`, `thick` / `thickness`, `thumb`, `track`, `fill`, `lines`.

### Keyframes

Prefix every new animation name with `pattern` (e.g. `patternFadeInUp`) so imports from other modules do not collide.

### Threshold

Patterns are registered with `@use(thr)`. Callers set the shared threshold via `@patterns(0.4)` (or similar).

| Range | Typical use |
|-------|-------------|
| 0.7–0.9 | Strict, short component names |
| 0.4–0.5 | Demo / longer role phrases |
| ≤ 0.35 | Easy collisions — avoid unless descriptions are unique |

Do not lower the library default (`thr:0.65` in the `@define`) without a clear reason; callers can pass a lower value when they need it.

---

## Adding a pattern

1. Add the `pattern(@use(thr): "…", \`…\`)` block inside `@define patterns(...)`.
2. Use cascade defaults + `@match` as above.
3. Exercise it in `demo.fscss` (or a small test sheet) with and without optional inputs.
4. Compile and check that:
   - Defaults apply when the phrase has no value keywords
   - Overrides apply when `bg:`, `color:`, etc. are present
   - No empty `--_*` custom properties remain in the output
5. Document the phrase and any new `@match` labels in the PR description.

---

## Demo and docs

- Keep `demo.fscss` / `demo.html` as a realistic showcase, not a token dump.
- Sample threshold in the demo is **0.4** — change it only if you update the README table to match.
- README “What’s included” and “Prompt inputs” lists should stay in sync with the library.

---

## Pull requests

1. One focused change per PR when possible (one new pattern family, or one regex/cascade fix).
2. In the description, include:
   - The pattern description phrase(s)
   - Example call lines (with and without `@match` inputs)
   - A short note on the CSS produced
3. Confirm FSCSS 1.2.4+ behavior (CLI or runtime).

---

## Code of conduct

Be respectful. This is a small open demo library — assume good intent, keep discussion technical, and prefer clear examples over long debate.

---

## License

By contributing, you agree that your contributions are licensed under the MIT license of this project.
