# patterns.fscss

FSCSS pattern module — apply styling by describing it in plain English.

This is a demo module built on top of FSCSS's `pattern()` method. It does not
attempt to cover every possible style. It exists to show how a pattern
library works, and to serve as a starting point for your own.

MIT License.

## What this is

FSCSS `pattern()` matches a phrase written in a stylesheet against a stored
description, and injects the associated CSS when the similarity score meets
a defined threshold. This module wraps a set of pattern descriptions and
their CSS inside two reusable `@define` blocks:

- `pattern-root(sel: root)` — declares the design tokens (colors, radii,
  shadows, transitions) as CSS custom properties on a selector, `:root` by
  default.
- `patterns(thr: 0.65)` — declares the pattern library itself: cards,
  buttons, hover effects, animations, and a few common UI components.

## Usage

### Import the published module

```fscss
@import((pattern-root, patterns) from patterns)

@pattern-root()
@patterns(0.7)

.card {
  beautiful gradient hello world card
}

.btn-primary {
  solid purple primary button with white label
}
```

### Fork and import locally

Fork this repo, drop `patterns.fscss` into your project, and import from the
local path instead:

```fscss
@import((pattern-root, patterns) from "./patterns.fscss")
```

Forking is the recommended path if you plan to add your own patterns rather
than just consume the demo set — see Contributing below.

### Compiling

```bash
npm install -g fscss
fscss style.fscss style.css
```

Or via CDN in runtime mode:

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.1.26/e/exec.min.js" async></script>
```

### Pro tip

Install the [FSCSS VS Code extension](https://marketplace.visualstudio.com/items?itemName=Figsh.fscss) for syntax highlighting, autocompilation
on save, and pattern suggestions as you type.


## How pattern() works

```css
pattern(threshold: "description", `
  css
`)
```

- `threshold` — minimum similarity score (0 to 1) a phrase must reach to
  trigger the pattern. Defaults to 1 (near-exact match) if omitted.
- `description` — the plain-English phrase being matched against.
- `css` — the CSS injected when a phrase clears the threshold.

A phrase in your stylesheet does not need to match a description word for
word. It is scored against every pattern's description, and the
highest-scoring pattern that also clears its own threshold wins.

```css
pattern(0.5: "Beautiful card", `
  background: #667eea;
`)

.card {
  beautiful card
}
```

## What's included

- **Cards** — gradient card, soft elevated card
- **Buttons** — white button with purple label, solid purple primary
  button, ghost outline button
- **Hover effects** — lift on hover, scale on hover, glow on hover
- **Animations** — fade in up, gentle pulse, spin, plus their matching
  `@keyframes`
- **Components** — input field with focus ring, pill badge, glass
  morphism, dark tooltip

Full descriptions and their CSS are in [`patterns.fscss`](./patterns.fscss).

## Contributing

This module is intentionally small. It grows through contribution, not a
fixed roadmap. If you want to add a pattern:

1. Write the CSS you want to reuse.
2. Write a plain-English description for it, phrased the way a developer
   would naturally describe the result, not the implementation.
3. Keep the description lexically distinct from existing ones. Two
   descriptions that share most of their words (for example, two button
   variants both saying "primary button with white text") make matching
   ambiguous. Give each pattern at least one or two words nothing else
   uses.
4. Add it to the appropriate section in `patterns.fscss`, following the
   existing `pattern(@use(thr): "description", "css")` format.
5. Open a pull request describing what the pattern produces.

Patterns that only make sense for one specific project are better kept in
that project's own module. This repo is for patterns broadly useful across
projects.

## License

MIT
