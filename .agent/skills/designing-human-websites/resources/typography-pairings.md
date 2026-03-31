# Typography Pairings — Curated Reference

A good font pairing creates contrast through **personality**, **weight**, or **era** — not similarity.
All fonts below are from Google Fonts (free, load with a single `<link>` tag).

---

## Tier 1 — Editorial / Premium

| Display | Body | Character |
|---|---|---|
| Playfair Display | Inter | Classic meets modern. High contrast serif + neutral grotesque. |
| DM Serif Display | DM Sans | Same family, different modes. Instantly cohesive. |
| Cormorant Garamond | Outfit | Old-world elegance + geometric friendliness. |
| Fraunces | Source Sans 3 | Optical quirky serif + workhorse body. |

**Use for**: portfolios, agencies, luxury brands, editorial blogs

---

## Tier 2 — Tech / Product

| Display | Body | Character |
|---|---|---|
| Syne | Inter | Geometric, spaced display + neutral body. Confident. |
| Space Grotesk | Manrope | Both geometric grotesques but different weights. Clean SaaS feel. |
| Cabinet Grotesk | Inter | Wide, modern display. Strong hierarchy contrast. |
| Epilogue | Epilogue | Single variable font, use weight contrast only. Precise. |

**Use for**: SaaS, developer tools, tech startups, apps

---

## Tier 3 — Expressive / Startup

| Display | Body | Character |
|---|---|---|
| Plus Jakarta Sans | Plus Jakarta Sans | Single family, weight-only contrast. Very contemporary. |
| Clash Display | Satoshi | Strong geometric personality + clean body. |
| General Sans | General Sans | Same family approach. Minimal, confident. |

**Use for**: consumer apps, creative studios, social products

---

## Loading Pattern

```html
<!-- Example: Playfair Display + Inter -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Playfair+Display:ital,wght@0,700;1,700&display=swap" rel="stylesheet">
```

**Rules:**
- Load only the weights you will actually use (reduce bundle size)
- Always include `display=swap` to prevent FOIT
- Preconnect to `fonts.googleapis.com` and `fonts.gstatic.com`
- Prefer variable fonts where available (one file, all weights)

---

## Anti-Patterns (Never Do These)

- ❌ Pairing two display serifs together (no hierarchy)
- ❌ Using a decorative script font for body copy (illegible at small sizes)
- ❌ More than 2 typefaces in one project
- ❌ Using system fonts (`Arial`, `Times New Roman`) in a designed context without intent
- ❌ Mixing a condensed and an expanded font without purpose
