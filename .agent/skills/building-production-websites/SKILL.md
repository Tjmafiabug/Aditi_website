---
name: building-production-websites
description: Builds dynamic, secure, well-architected websites and web applications from scratch. Use when the user asks to build a website, web app, or full-stack project and wants proper security, clean architecture, and production-ready code. Activated by phrases like "build me a website", "create a web app", "make it secure", "proper architecture", or "full-stack".
---

# Building Production-Quality Websites

## When to Use This Skill
- User wants a complete website or web app built from scratch
- User mentions "secure", "dynamic", "real users", "backend", "database", or "API"
- User wants a full-stack project with proper structure — not a static page
- User needs auth, forms, data persistence, or server-side logic

## Checklist
- [ ] Clarify scope: static site, dynamic frontend, or full-stack?
- [ ] Choose tech stack appropriate to the complexity
- [ ] Scaffold folder structure before writing any code
- [ ] Implement security layer before any feature code
- [ ] Build data layer (models/schema) before UI
- [ ] Validate architecture against the checklist below
- [ ] Run a security audit before declaring done

---

## Phase 1 — Scope Decision

Answer these before choosing a stack:

| Question | Implication |
|---|---|
| Does it need a database? | Yes → need a backend or BaaS |
| Will users log in? | Yes → need auth (do NOT build custom) |
| Will data change after page load? | Yes → need JS fetching or a framework |
| Will multiple people edit content? | Yes → need a CMS or admin panel |
| Will it handle payments? | Yes → Stripe only, never custom card handling |

### Stack Decision Matrix

| Project Type | Recommended Stack |
|---|---|
| Static marketing site | HTML + CSS + Vanilla JS |
| Content-heavy / blog | Astro or Next.js (static export) |
| Interactive SPA (no backend) | Vite + React or Vanilla JS |
| Full-stack app with auth + DB | Next.js (App Router) + Supabase |
| API-heavy product | Next.js API routes or Express + PostgreSQL |
| E-commerce | Next.js + Stripe + Supabase |

> Default to the **simplest stack** that satisfies requirements. Over-engineering is a failure mode.

---

## Phase 2 — Folder Architecture

### Standard Full-Stack Project (Next.js)

```
project-root/
├── app/                    # Next.js App Router pages
│   ├── layout.tsx          # Root layout (fonts, global CSS, metadata)
│   ├── page.tsx            # Home route
│   ├── (auth)/             # Route group: login, signup, reset
│   │   ├── login/page.tsx
│   │   └── signup/page.tsx
│   ├── dashboard/          # Protected routes
│   │   └── page.tsx
│   └── api/                # API route handlers
│       └── [...route]/
├── components/             # Reusable UI components
│   ├── ui/                 # Primitives: Button, Input, Card
│   └── layout/             # Nav, Footer, Sidebar
├── lib/                    # Shared logic (no UI)
│   ├── auth.ts             # Auth helpers
│   ├── db.ts               # Database client singleton
│   └── utils.ts            # Pure utility functions
├── hooks/                  # Custom React hooks
├── types/                  # TypeScript type definitions
├── middleware.ts            # Route protection (auth guard)
├── .env.local              # Secrets (never committed)
├── .env.example            # Template for collaborators
└── .gitignore              # Must include .env.local
```

### Static / Vanilla Project

```
project-root/
├── index.html
├── css/
│   ├── tokens.css          # Design system variables
│   ├── layout.css          # Grid, spacing, container
│   └── components.css      # Button, card, form, nav
├── js/
│   ├── main.js             # Entry point, init
│   ├── api.js              # All fetch calls in one place
│   ├── auth.js             # Auth state management
│   └── utils.js            # Pure helpers
├── assets/
│   └── images/
├── .env                    # Local secrets
└── .gitignore
```

**Rules:**
- UI code never touches the database directly — always through `lib/`
- API keys live only in `.env` files, never in source code
- One file = one responsibility (never mix fetch logic into a component)

---

## Phase 3 — Security Layer (Implement Before Features)

Security is not added at the end. These must be in place before writing feature code.

### 3.1 Environment Variables

```bash
# .env.local — NEVER commit this file
DATABASE_URL="postgresql://..."
NEXTAUTH_SECRET="generate with: openssl rand -base64 32"
SUPABASE_SERVICE_KEY="..."         # Server-only key
NEXT_PUBLIC_SUPABASE_URL="..."     # Safe to expose (public)
NEXT_PUBLIC_SUPABASE_ANON_KEY="..." # Safe to expose (public)
```

```bash
# .gitignore — always include these
.env
.env.local
.env.production
*.pem
```

**Rule**: Any variable prefixed `NEXT_PUBLIC_` is sent to the browser. Never put secrets there.

### 3.2 Authentication (Never Build Custom)

Use established libraries only. Recommended options:

| Option | Best For |
|---|---|
| NextAuth.js / Auth.js | Next.js apps with OAuth or email |
| Supabase Auth | Projects already using Supabase |
| Clerk | Fast integration, built-in UI components |
| Lucia | Lightweight, if you want control |

```ts
// middleware.ts — protect all /dashboard routes
import { withAuth } from 'next-auth/middleware';

export default withAuth({
  pages: { signIn: '/login' },
});

export const config = {
  matcher: ['/dashboard/:path*', '/api/private/:path*'],
};
```

**Never:**
- Store passwords without bcrypt (min cost 12)
- Roll your own JWT verification
- Store auth tokens in `localStorage` (use httpOnly cookies)

### 3.3 Input Validation

Every piece of data from the user is untrusted. Validate on the **server**, not just the client.

```ts
// Use Zod for all input validation
import { z } from 'zod';

const ContactSchema = z.object({
  name:    z.string().min(1).max(100).trim(),
  email:   z.string().email(),
  message: z.string().min(10).max(2000).trim(),
});

// In your API route:
const result = ContactSchema.safeParse(req.body);
if (!result.success) {
  return Response.json({ error: result.error.flatten() }, { status: 400 });
}
// Only use result.data from here on — never raw req.body
```

### 3.4 Security Headers

```ts
// next.config.ts
const securityHeaders = [
  { key: 'X-DNS-Prefetch-Control', value: 'on' },
  { key: 'X-Frame-Options', value: 'SAMEORIGIN' },        // Prevent clickjacking
  { key: 'X-Content-Type-Options', value: 'nosniff' },    // Prevent MIME sniffing
  { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
  { key: 'Permissions-Policy', value: 'camera=(), microphone=(), geolocation=()' },
  {
    key: 'Content-Security-Policy',
    value: [
      "default-src 'self'",
      "script-src 'self' 'unsafe-inline'",   // Tighten in production
      "style-src 'self' 'unsafe-inline' fonts.googleapis.com",
      "font-src 'self' fonts.gstatic.com",
      "img-src 'self' data: https:",
    ].join('; '),
  },
];

export default {
  async headers() {
    return [{ source: '/(.*)', headers: securityHeaders }];
  },
};
```

### 3.5 Database Security

- **Always use parameterized queries** — never string-concatenate user input into SQL
- Use Row Level Security (RLS) if using Supabase — enable on every table
- Database credentials must only live on the server (never `NEXT_PUBLIC_`)

```ts
// Safe parameterized query (Supabase example)
const { data } = await supabase
  .from('posts')
  .select('*')
  .eq('user_id', session.user.id);   // user.id comes from verified session, not request

// NEVER do this:
const query = `SELECT * FROM posts WHERE user_id = '${userId}'`; // SQL injection risk
```

### 3.6 Rate Limiting

Protect all API routes from abuse:

```ts
// Simple in-memory rate limiter for API routes
import { Ratelimit } from '@upstash/ratelimit';
import { Redis } from '@upstash/redis';

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, '10 s'), // 10 requests per 10 seconds
});

// In your API handler:
const identifier = req.headers.get('x-forwarded-for') ?? 'anonymous';
const { success } = await ratelimit.limit(identifier);
if (!success) {
  return Response.json({ error: 'Too many requests' }, { status: 429 });
}
```

---

## Phase 4 — Data Layer

Design data models before building any UI.

### Schema Design Rules
- Every table has an `id` (UUID preferred over auto-increment)
- Every table has `created_at` and `updated_at` timestamps
- Soft-delete with `deleted_at` instead of hard deletes (for recovery)
- Foreign keys for every relationship — never store raw IDs without constraints

```sql
-- Example: posts table
CREATE TABLE posts (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title       TEXT NOT NULL CHECK (char_length(title) <= 200),
  content     TEXT NOT NULL,
  published   BOOLEAN DEFAULT false,
  created_at  TIMESTAMPTZ DEFAULT now(),
  updated_at  TIMESTAMPTZ DEFAULT now(),
  deleted_at  TIMESTAMPTZ                  -- soft delete
);

-- Row Level Security (Supabase)
ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can only see their own posts"
  ON posts FOR SELECT USING (auth.uid() = user_id);
```

---

## Phase 5 — Dynamic Frontend Patterns

### API Fetching Pattern

```ts
// lib/api.ts — centralize ALL fetch calls here
export async function fetchPosts(): Promise<Post[]> {
  const res = await fetch('/api/posts', {
    credentials: 'include',       // send cookies
    headers: { 'Content-Type': 'application/json' },
  });

  if (!res.ok) {
    throw new Error(`API error: ${res.status}`);
  }

  return res.json();
}
```

### Form Handling (with validation)

```ts
// Pattern: optimistic UI + server validation
async function handleSubmit(formData: FormData) {
  const raw = Object.fromEntries(formData);
  
  // 1. Client-side schema check (fast feedback)
  const result = Schema.safeParse(raw);
  if (!result.success) {
    setErrors(result.error.flatten().fieldErrors);
    return;
  }

  // 2. Send to server (which also validates)
  setLoading(true);
  try {
    const res = await fetch('/api/submit', {
      method: 'POST',
      body: JSON.stringify(result.data),
      headers: { 'Content-Type': 'application/json' },
    });
    if (!res.ok) throw new Error(await res.text());
    setSuccess(true);
  } catch (err) {
    setError('Something went wrong. Please try again.');
  } finally {
    setLoading(false);
  }
}
```

---

## Phase 6 — Performance Baseline

Every project ships with these before launch:

- [ ] Images served in WebP or AVIF format, with explicit `width` and `height`
- [ ] Fonts use `font-display: swap` and are preloaded
- [ ] Critical CSS is inlined (above-the-fold styles)
- [ ] No render-blocking scripts (`defer` or `async` on all `<script>` tags)
- [ ] Core Web Vitals measured with Lighthouse (target: green on all three)

```html
<!-- Correct font loading -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap">

<!-- Correct image -->
<img src="hero.webp" alt="Description" width="1200" height="600" loading="lazy">

<!-- Correct script -->
<script src="main.js" defer></script>
```

---

## Phase 7 — Pre-Launch Security Audit

Run this checklist before going live:

### Secrets & Config
- [ ] No secrets in source code or git history (`git log` and `grep -r "API_KEY" .`)
- [ ] `.env` files are in `.gitignore`
- [ ] Production uses different secrets than development

### Authentication
- [ ] Protected routes return 401/403 without a valid session (test by clearing cookies)
- [ ] Logout properly invalidates the server-side session
- [ ] Passwords are hashed with bcrypt (never MD5 or SHA1)

### Input & Output
- [ ] All form inputs validated on the server
- [ ] No raw user content rendered as HTML (XSS)
- [ ] File uploads restricted to allowed MIME types and max size

### Transport
- [ ] HTTPS enforced (HTTP redirects to HTTPS)
- [ ] Security headers present (verify at [securityheaders.com](https://securityheaders.com))
- [ ] Cookies have `Secure`, `HttpOnly`, and `SameSite=Lax` flags

### Database
- [ ] RLS enabled on all tables (if Supabase)
- [ ] No raw SQL string interpolation anywhere
- [ ] Database not publicly accessible (only reachable from server)

---

## Resources
- [resources/env-template.txt](resources/env-template.txt) — environment variable template
- [resources/gitignore-template.txt](resources/gitignore-template.txt) — recommended .gitignore
- [examples/next-supabase-starter/](examples/next-supabase-starter/) — full-stack reference project
