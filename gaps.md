# Gaps, decisions and things worth knowing

Notes from converting the `<1280 (Desktop)` frame into `index.html` + `styles.css`.

**Updated** after the Figma fixes — see section 0.

---

## 0. What you fixed, and what the code now does ✅

### Spacing — fixed, and the CSS now uses tokens instead of raw numbers

| Where | Was | Now | CSS |
|---|---|---|---|
| Nav: link group → button | 54px | **48px** | `var(--space-700)` |
| Nav: between the three links | ~35.5px | **32px** | `var(--space-600)` |
| Card: headline / text / link | 19px | **16px** | `var(--space-400)` |
| About: section padding | 66 / 52 / 52.875 / 64 | **64 / 48 / 48 / 64** | `var(--space-800)` + `var(--space-700)` |

No hard-coded pixel values are left in those four places.

### Secondary button — fixed properly

The button's stroke is now **bound** to `action/secondary/border`, and you re-pointed that token from `color/neutral/300` to `color/neutral/800`.

That's the better of the two possible fixes: the rendered colour is unchanged (`#2C2C2C`), but it now flows from the right token instead of coincidentally matching the primary fill. The CSS went from `var(--action-primary-default)` to `var(--action-secondary-border)`.

### font/link/md — fixed

Letter spacing went from **−2% to −0.5%**, matching the other 16px styles. The CSS is now `-0.005em`.

### Two more changes I caught while re-reading

You also adjusted two tokens I hadn't flagged. Both are now in the CSS:

| Token | Light: was → now | Dark: was → now |
|---|---|---|
| `action/secondary/hover` | neutral/100 → **neutral/200** | neutral/800 (unchanged) |
| `action/secondary/pressed` | neutral/200 → **neutral/300** | neutral/700 (unchanged) |
| `action/secondary/border` | neutral/300 → **neutral/800** | neutral/700 → **neutral/200** |

---

## 1. Still off the space scale: one value

**`About` → `container`, the gap between the text and the photo, is still 95px.**

Your space scale is 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 80. The nearest token is `space/900` (80).

Worth knowing *why* it survived: you set **80px on the `About` instance itself**, but that frame has only one child (`container`), so its gap has nothing to space apart and no visible effect. The gap that actually separates the text from the photo lives one level down, on `container`, and is still 95.

The CSS keeps `gap: 95px` so the page matches what Figma currently renders. Change it on `container` in Figma and I'll swap the CSS to `var(--space-900)`.

---

## 2. Verified against Figma ✅

Measured in a browser at 1424px wide and compared to the frame:

| Thing | Figma | Code |
|---|---|---|
| Container width | 1200px | 1200px |
| Card image / text columns | 584 / 584 | 584 / 584 |
| Card image box | 584 × 329 | 584 × 329 |
| Hero headline | 80px, uppercase, 120% | 80px, uppercase, 96px (= 1.2) |
| Card headline | 40px | 40px |
| "About me" / "Skills" | 48px | 48px |
| Skill headline | 24px | 24px |
| Body text | 16px, 170% | 16px, 27.2px (= 1.7) |
| Card content gap | 16px | 16px |
| Nav gaps | 48 / 32 | 48 / 32 |
| About padding | 64 / 48 / 48 | 64 / 48 / 48 |
| Link letter spacing | −0.5% | −0.08px (= 16 × −0.005) |
| Secondary button border | 3px `#2C2C2C` | 3px `rgb(44, 44, 44)` |
| Logo orange | `#FF6330` | `rgb(255, 99, 48)` |
| Alternating card bg | `#FFFFFF` / `#F7F7F7` | same |
| Footer bar | `#161616` | same |
| Skill top rule | 1px `#161616` | same |

Total page height: **3050px** in code vs **3091px** in Figma. The 41px difference is one deliberate change — see 3.1.

---

## 3. Things I changed on purpose

### 3.1 The About portrait is cropped differently

**In Figma:** the media slot is a 553 × 553 square, but the photo inside is 559 × 519 and set to CROP. The photo doesn't fill its square, leaving an empty strip along the bottom.

**In code:** I used the photo's own shape (1112 × 1008) so it fills its box cleanly. This is the entire 41px height difference.

**To match Figma exactly**, in `styles.css`:
```css
.about__media img { aspect-ratio: 1112 / 1008; }   /* current */
.about__media img { aspect-ratio: 1 / 1; }         /* matches Figma */
```

### 3.2 The mobile navigation is different

**In Figma:** the mobile Navigation (375 × 80) is the logo plus a hamburger — the `Menu` component, with `open` and `close` variants. The three links are hidden.

**In code:** the links stay visible and wrap onto a second line.

**Why:** a working hamburger needs JavaScript, which was out of scope for this test.

### 3.3 The hero text is typed in normal case

Figma's `font/display/md` applies `textCase: UPPER` as a *style*, not as typed capitals, so the CSS uses `text-transform: uppercase` and the HTML keeps the real words. Screen readers, search engines and copy-paste all get "Agentic AI." rather than shouting capitals.

---

## 4. Tokens defined but never used on this page

Not errors — your system is simply bigger than one page needs.

- **Type:** `font-size/500` (56px)
- **Spacing:** `space/100`, `space/200`, `space/300`, `space/900`
- **Brand ramp:** 200, 300, 400, 600, 700, 800, 900 — only `brand/500` is used
- **Neutrals:** 500, 999
- **Semantic:** `surface/accent-subtle`, `border/default`, `border/accent`, `text/muted`, `text/disabled`, and the `action/primary` hover / pressed / disabled states

All of them are in `styles.css` regardless, ready for the next page.

Note `space/900` only appears here because of the 95px gap in section 1. Fix that and it gets used.

---

## 5. Still worth fixing in the Figma file

- **Three of the four cards have default names** — "Component 4", "Component 2", "Component 3". Only one is named `ProjectCard`. They're all the same component.
- **All four cards share identical placeholder text** ("I run moonlearning.io…").
- **All three SkillItems** say "Headline" with the same body copy.
- **`font/button/md` and `font/navigation/md` use AUTO line height**; every other text style sets an explicit percentage.
- **`font/link/inline`** (the underlined style) isn't used anywhere on this page.
- I did not re-check the `fon/link/inline` label typo on the Variables & Styles Overview page.

---

## 6. Dark mode is included but untested

Your `color` collection has light and dark modes, so I built both, including the three `action/secondary/*` changes above. Switch your Mac to dark mode and the page follows.

**Treat it as unverified.** There is no dark frame in Figma to compare against, and all five photographs are light-background images, so it looks odd. The token values are taken straight from your file.

To remove it: delete the `@media (prefers-color-scheme: dark)` block in section 2 of `styles.css`.

---

## 7. One component, not three

**Figma has `Hero`, `ProjectCard`, `About`, `Skills`, `Navigation`, `Footer` built three times each** — once per breakpoint — because Figma can't do media queries.

**The code has one of each.** Font sizes swap via two `@media` blocks that redefine the `--font-size-*` variables, mirroring your `lg` / `md` / `sm` modes.

**Caveat:** I matched the *stacking* and *padding* of the tablet and mobile frames, but didn't rebuild them layer by layer. Comparing the code at 375px against the Figma mobile frame will show differences beyond the navigation.

---

## 8. Not included

- **No working links.** Every `href` is `#`.
- **Images are the raw Figma uploads**, not optimised. `about-portrait.png` alone is 1MB.
- **No `state=focused` Button styling.** Your Button has a focused variant; I used a generic orange focus ring (`border/focus`) page-wide instead.
- **The Menu component** (hamburger) is not built.

---

## 9. Files

```
Demo MCP/
├── index.html      structure and content
├── styles.css      tokens and styling
├── gaps.md         this file
├── assets/         5 images exported from Figma
└── .claude/
    └── launch.json config for the local preview server
```
