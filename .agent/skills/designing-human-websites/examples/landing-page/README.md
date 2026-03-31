---
name: SaaS Landing Page — Editorial Dark
mood: Powerful / Precise / Modern
font_pair: "Syne (display) + Inter (body)"
accent: "Violet-blue hsl(250, 70%, 60%)"
---

# Example: SaaS Landing Page

This is a minimal reference implementation demonstrating the design principles
from the `designing-human-websites` skill.

## Design Decisions (Documented)

### Why Syne + Inter?
Syne is bold and geometric — it signals confidence and technology.
Inter is the most legible grotesque at small sizes. The contrast comes from
personality, not just weight. They don't compete.

### Why dark mode?
This is a dev tool SaaS. Developer-audience products trend dark.
Background is `hsl(250, 12%, 7%)` — not pure black, slightly violet-tinted
to harmonize with the accent hue.

### Why a 60/40 hero split?
A 50/50 split feels passive. The text column dominates (60%), the visual
sits in support (40%). This communicates: words first, proof second.

### Why no drop shadow on the nav?
A shadow on load creates visual weight before the user scrolls.
Nav appears clean; a subtle border appears only after 40px of scroll (JS).

### Why stagger the feature cards?
Staggered entrance (80ms per card) reads as "revealing" rather than "loading".
It creates a sense of intentional disclosure.

---

## Key HTML Patterns

```html
<!-- Nav: transparent → border on scroll -->
<nav class="site-nav" id="site-nav">
  <div class="container nav-inner">
    <a href="/" class="nav-logo">Acme</a>
    <ul class="nav-links">
      <li><a href="#features">Features</a></li>
      <li><a href="#pricing">Pricing</a></li>
      <li><a href="#docs">Docs</a></li>
    </ul>
    <a href="/signup" class="btn btn-accent">Get started →</a>
  </div>
</nav>

<!-- Hero: asymmetric 60/40 -->
<section class="hero section">
  <div class="container hero-grid">
    <div class="hero-text reveal">
      <span class="label">Now in public beta</span>
      <h1>Build faster.<br>Ship with confidence.</h1>
      <p class="lead">The deployment platform built for teams who care about observability, not just uptime.</p>
      <div class="hero-actions">
        <a href="/signup" class="btn btn-accent">Start free trial</a>
        <a href="/demo" class="btn btn-ghost">Watch demo ↗</a>
      </div>
    </div>
    <div class="hero-visual reveal" style="--reveal-delay: 200ms">
      <!-- Terminal or product screenshot, not stock imagery -->
    </div>
  </div>
</section>
```

```css
.hero-grid {
  display: grid;
  grid-template-columns: 60fr 40fr;
  gap: var(--space-16);
  align-items: center;
  min-height: 90vh;
}

.hero h1 {
  font-size: var(--text-4xl);
  font-family: var(--font-display);
  letter-spacing: -0.04em;
  line-height: 1.05;
  margin-top: var(--space-4);
  margin-bottom: var(--space-6);
}

.label {
  font-size: var(--text-xs);
  font-weight: 600;
  letter-spacing: var(--tracking-wide);
  text-transform: uppercase;
  color: var(--color-accent);
}

.lead {
  font-size: var(--text-md);
  line-height: 1.6;
  color: var(--color-fg-muted);
  max-width: 44ch;
}
```

```js
// Nav border on scroll
const nav = document.getElementById('site-nav');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 40);
}, { passive: true });

// Scroll-triggered reveal
const observer = new IntersectionObserver(
  (entries) => entries.forEach(e => {
    if (e.isIntersecting) {
      const delay = e.target.style.getPropertyValue('--reveal-delay') || '0ms';
      setTimeout(() => e.target.classList.add('visible'), parseInt(delay));
      observer.unobserve(e.target);
    }
  }),
  { threshold: 0.15 }
);
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

---

## Anti-AI Checks Applied ✓

- ✅ Body line-height: `1.7` (not browser default `1.2`)
- ✅ Heading letter-spacing: `-0.04em` (optically tightened)
- ✅ Background: `hsl(250, 12%, 7%)` (tinted, not pure black)
- ✅ Shadow uses tinted `hsl(...)` not `rgba(0,0,0,...)`
- ✅ Nav transition targets specific properties, not `all`
- ✅ Hero is NOT centered text + gradient button
- ✅ Color roles: 3 only (bg / fg / accent)
- ✅ Staggered animations via `--reveal-delay` CSS custom property
