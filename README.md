# Demo MCP — hand-made Figma to code

The desktop frame of a hand-built Figma file, converted into plain HTML and CSS.

Part of working through [moonlearning.io](https://moonlearning.io)'s *Figma and code with AI* demo, step 1: **Figma → code**.

## What's here

| File | What it is |
|---|---|
| `index.html` | Structure and content only — no styling |
| `styles.css` | Design tokens and all styling |
| `gaps.md` | What doesn't match the Figma file, and why |
| `assets/` | Images exported from Figma |

No frameworks, no build step, no JavaScript. Open `index.html` in a browser.

## The design system

The Figma file's variables are mirrored as CSS custom properties in two layers, the same way Figma does it:

- **Primitives** — `--color-brand-500`, `--space-400`, `--font-size-600`
- **Semantic** — `--text-default`, `--surface-raised`, `--action-primary-default`, each one an alias of a primitive

63 variables across 5 collections, plus 11 text styles.

### Responsive type

Figma can't do media queries, so the original file builds every component three times — once per breakpoint — and swaps a variable mode (`lg` / `md` / `sm`) to change the sizes.

Here there's **one** of each component. Two `@media` blocks redefine the `--font-size-*` variables at 1280px and 800px, and every text style using them updates automatically.

### Dark mode

The Figma `color` collection has light and dark modes, so both are built. It follows your system setting. Untested against a Figma frame — the photographs are all light, so it looks odd.

## Known gaps

See [`gaps.md`](gaps.md). The short version:

- The About portrait is cropped to the photo's own shape rather than Figma's square slot (41px height difference)
- The mobile navigation shows wrapped links instead of a hamburger, which would need JavaScript
- One spacing value, the 95px gap between the About text and photo, is still off the design system's space scale

## Local preview

Open `index.html` directly, or serve the folder:

```bash
python3 -m http.server 8765
```

Then visit <http://localhost:8765>.
