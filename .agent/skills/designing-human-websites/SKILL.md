---
name: designing-human-websites
description: Designs premium, human-feeling websites with intentional visual logic behind every element. Use when the user asks to build a website, landing page, portfolio, or any UI that must not look AI-generated, generic, or templated. Activated by phrases like "make it look real", "not AI-like", "premium design", "stunning UI", or "beautiful website".
---

# Designing Human-Quality Websites

## When to Use This Skill
- User wants a website, landing page, or UI that feels crafted, not generated
- User says "make it beautiful", "premium", "doesn't look AI", "like a real agency made it"
- User wants a portfolio, SaaS homepage, editorial site, or brand presence
- Any web UI where aesthetics and intentionality matter

## Checklist
- [ ] Establish the design intent (what emotion should this evoke?)
- [ ] Choose a type scale with a clear rational base
- [ ] Define a restricted color system (max 3 roles)
- [ ] Set up a spacing system based on a single base unit
- [ ] Lay out visual hierarchy before writing any component code
- [ ] Apply the Anti-AI audit before calling it done
- [ ] Verify on at least one mobile and one desktop viewport

---

## Phase 1 — Design Intent (Before Writing Any Code)

Answer these before touching HTML:

1. **Mood**: Clinical / Editorial / Playful / Powerful / Intimate?
2. **Audience**: Who is reading this — and what do they already know?
3. **Primary action**: What is the ONE thing a visitor should do?
4. **Reference tonality**: (e.g. "Stripe meets Notion", "Raw brutalist meets luxury")

> These answers drive every decision below. Do not skip this step.

---

## Phase 2 — Typography System

Typography is the single highest-leverage design decision. Choose it first.

### The Two-Font Rule
- **Display font**: Used only for headlines. Can be expressive. (e.g. Playfair Display, Syne, DM Serif Display)
- **Body font**: Used for everything else. Must be neutral and highly legible. (e.g. Inter, DM Sans, Outfit)
- Never use more than 2 typefaces in a single project.

### Type Scale (use a 1.25× or 1.333× modular scale)

```css
/* Base: 16px, Scale: 1.25 (Major Third) */
--text-xs:   0.64rem;   /* 10.24px — labels, captions */
--text-sm:   0.8rem;    /* 12.8px  — meta, footnotes */
--text-base: 1rem;      /* 16px    — body copy */
--text-md:   1.25rem;   /* 20px    — lead text */
--text-lg:   1.563rem;  /* 25px    — section labels */
--text-xl:   1.953rem;  /* 31px    — subheadings */
--text-2xl:  2.441rem;  /* 39px    — headings */
--text-3xl:  3.052rem;  /* 48.8px  — hero headlines */
--text-4xl:  3.815rem;  /* 61px    — massive display */
```

**Rules:**
- Body line-height: `1.6–1.75` (never default `1.2` — that's the AI tell)
- Heading line-height: `1.05–1.15` (tight, intentional)
- Letter-spacing on headlines: `−0.02em` to `−0.04em` (optically tighter)
- Letter-spacing on all-caps labels: `+0.08em` to `+0.15em`
- Max line length (measure): `55–75ch` for body, `30–45ch` for headlines

---

## Phase 3 — Color System

### Three-Role Model

```css
/* Every color serves exactly one of these roles */
--color-bg:       /* Surface — what content sits on */
--color-fg:       /* Content — text, icons, primary elements */
--color-accent:   /* Action — one single brand/interactive color */
```

Derive all shades from these three, not from a generic palette picker.

### Choosing an Accent Color
- Pick a hue, then define it in HSL with a fixed saturation
- Derive hover, active, and muted states by adjusting L only
- Never use pure `#FF0000`, `#0000FF`, or `#00FF00` — shift the hue slightly

```css
/* Example: a slate-blue accent */
--accent-hue: 220;
--accent-sat: 70%;

--accent:         hsl(var(--accent-hue), var(--accent-sat), 52%);
--accent-hover:   hsl(var(--accent-hue), var(--accent-sat), 44%);
--accent-muted:   hsl(var(--accent-hue), 30%, 90%);
--accent-on:      hsl(0, 0%, 100%);  /* text on accent bg */
```

### On Dark Mode
- Background should not be pure `#000000` — use `hsl(220, 15%, 8%)` or similar
- Text should not be pure `#FFFFFF` — use `hsl(220, 15%, 92%)` or similar
- The slight hue tint in both bg and fg makes the palette feel cohesive

---

## Phase 4 — Spacing System

All spacing derives from a single base unit. This is what makes layouts feel intentional rather than arbitrary.

```css
--space-unit: 8px;

--space-1:  calc(var(--space-unit) * 0.5);   /* 4px  */
--space-2:  calc(var(--space-unit) * 1);     /* 8px  */
--space-3:  calc(var(--space-unit) * 1.5);   /* 12px */
--space-4:  calc(var(--space-unit) * 2);     /* 16px */
--space-6:  calc(var(--space-unit) * 3);     /* 24px */
--space-8:  calc(var(--space-unit) * 4);     /* 32px */
--space-12: calc(var(--space-unit) * 6);     /* 48px */
--space-16: calc(var(--space-unit) * 8);     /* 64px */
--space-24: calc(var(--space-unit) * 12);    /* 96px */
--space-32: calc(var(--space-unit) * 16);    /* 128px */
```

**Rules:**
- Never use arbitrary pixel values like `padding: 23px` or `margin: 37px`
- Section padding: `--space-24` to `--space-32` (vertical)
- Component internal padding: `--space-4` to `--space-8`
- Whitespace is not empty space — it is a design element with a weight

---

## Phase 5 — Layout Logic

### Grid
- Use a 12-column grid for flexibility
- Content max-width: `1280px` for wide layouts, `768px` for editorial/reading
- Horizontal padding (gutters): `clamp(1.5rem, 5vw, 4rem)`

### Visual Hierarchy Rules
Every screen must have a clear **Z1/Z2/Z3 hierarchy**:
- **Z1 (Dominant)**: One element per section. Largest, highest contrast. Draws the eye first.
- **Z2 (Supporting)**: Subheadings, key labels. Clearly subordinate to Z1.
- **Z3 (Contextual)**: Body copy, metadata. Lowest contrast, smallest size.

If you cannot identify Z1/Z2/Z3 in each section, the layout lacks hierarchy — fix it before continuing.

### Asymmetry as a Signal of Intent
- Perfectly centered, symmetrical layouts feel generic
- Introduce deliberate asymmetry: offset text columns, bleed images, staggered grids
- A 60/40 column split feels more intentional than 50/50

---

## Phase 6 — Motion & Micro-Interaction Design

Animation communicates causality and response. Every animation must have a reason.

```css
/* Timing tokens */
--ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);
--ease-in-out:    cubic-bezier(0.76, 0, 0.24, 1);
--duration-fast:  150ms;
--duration-base:  250ms;
--duration-slow:  400ms;
--duration-enter: 600ms;
```

### Rules
- Hover states: `150ms ease-out` — fast, responsive, confident
- Page/section entrances: `600ms` with `translateY(24px)` and `opacity: 0 → 1`
- Never animate `width` or `height` directly — animate `transform` and `opacity` only (GPU compositing)
- Scroll-triggered animations: use `IntersectionObserver`, trigger at `threshold: 0.15`
- Stagger sibling elements by `80ms` each, not all at once

---

## Phase 7 — The Anti-AI Audit

Before considering the design complete, check every item:

### Typography Tells
- [ ] Line-height on body is NOT the browser default (`1.2`) — must be `1.6+`
- [ ] Headlines are NOT sentence-case by default — deliberately chosen
- [ ] There is NO generic "Lorem ipsum" placeholder text
- [ ] Font pairing has a reason (contrast of weight, or personality, not random)

### Color Tells
- [ ] No pure saturated primaries (`#ff0000`, `#0000ff`, `#00ff00`)
- [ ] Backgrounds are NOT pure white or pure black
- [ ] There are NOT more than 3 color roles in use (bg / fg / accent)
- [ ] Shadows use a tinted color, not `rgba(0,0,0,0.X)`

```css
/* Bad (AI-generated) */
box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);

/* Good (human, intentional) */
box-shadow: 0 4px 24px hsl(220, 30%, 10%, 0.15);
```

### Spacing Tells
- [ ] No arbitrary margins or paddings (e.g. `margin-top: 43px`)
- [ ] Section vertical rhythm is consistent — all from the spacing scale
- [ ] Elements are NOT equidistant from each other (intentional proximity grouping)

### Layout Tells
- [ ] There is NO "card grid with equal cards in a 3-column row" without a reason
- [ ] Hero section is NOT "centered text + gradient button + stock image background"
- [ ] Navigation does NOT have a logo left + links right + CTA button pattern unless it's genuinely the best choice

### Motion Tells
- [ ] Nothing uses `transition: all 0.3s ease` — transitions target specific properties
- [ ] Hover states exist on interactive elements
- [ ] There are NO infinite looping animations on content (only on decorative elements)

---

## Phase 8 — Component Patterns

For common components, apply these human-feel patterns:

### Buttons
```css
/* Avoid: generic pill button */
/* Use: specific geometry with optical padding */
.btn {
  padding: 0.75em 1.75em;  /* more horizontal than vertical */
  border-radius: 4px;       /* subtle, not fully rounded unless intentional */
  font-size: var(--text-sm);
  font-weight: 600;
  letter-spacing: 0.02em;
  transition: background-color var(--duration-fast) var(--ease-out-quart),
              transform var(--duration-fast) var(--ease-out-quart),
              box-shadow var(--duration-fast) var(--ease-out-quart);
}
.btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px hsl(var(--accent-hue), 50%, 20%, 0.25);
}
.btn:active {
  transform: translateY(0);
  box-shadow: none;
}
```

### Cards
- Cards should have **visual tension** — not just a box with padding
- Use: border instead of shadow, or shadow without border. Never both unless deliberate.
- Card hover: lift via `translateY(-4px)` + shadow deepening. NOT background color change.

### Section Dividers
- Avoid `<hr>` or hard lines between sections
- Use: whitespace alone, a subtle background shift (`hsl(220, 15%, 97%)` → `hsl(0,0%,100%)`), or an SVG wave/diagonal cut

---

## Resources
- [resources/css-tokens.css](resources/css-tokens.css) — full design token starter
- [resources/typography-pairings.md](resources/typography-pairings.md) — curated font pair reference
- [examples/landing-page/](examples/landing-page/) — full worked example
