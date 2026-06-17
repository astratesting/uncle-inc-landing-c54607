# Uncle Inc. — Marketing Landing Page Build Plan

## 1. PRODUCT

A single-page marketing site for **Uncle Inc.**, an AI-assisted MVP development platform that lets early-stage founders validate, build, and launch a startup idea in days instead of months. The landing page converts visitors into a waitlist by communicating the core promise (skip the technical co-founder, get a validated prototype fast), demonstrating six concrete feature/benefit blocks, showing three honest pricing tiers (Free / Pro $29 / Team $79), answering six founder-flavored FAQs, and capturing an email. Built on Next.js 14 App Router + Tailwind, fully static, fast, accessible, no fake social proof.

## 2. WHO IT'S FOR

**ICP:** Non-technical or semi-technical first-time founders, indie hackers, and small founding teams (2–3 people) who have an idea but no engineering hire. They are skeptical of hype, time-poor, allergic to "AI magic" language without specifics, and judge credibility by clarity, not logos. They open the tab on a laptop, skim in 30–60 seconds, and either scroll-to-CTA or bounce.

**Product implications for tone and layout:**
- **Above-the-fold must answer "what is this" in one glance.** Founder writes "What does it do?" before "Why is it better?" — we lead with the outcome, not the brand.
- **Specifics over adjectives.** "Test your idea with 5 real users before writing code" beats "powerful validation."
- **No fabricated logos, metrics, or quotes.** Placeholders are honest: tier names + prices + feature lists, nothing invented. Social proof is intentionally absent in this build (per constraint) — the copy carries the load.
- **One primary CTA repeated.** Waitlist email capture appears in the hero, mid-page, and final CTA. No competing actions.
- **Light, confident enterprise UI** (per design spec) — readable on a 13" laptop, generous spacing, no dark patterns.

## 3. LOOK & FEEL

**Vibe / positioning:** Calm, precise, operator-grade. "Crisp Operator" archetype — feels like Linear meets a serious founder tool, not a crypto landing page. Light theme, white-dominant surfaces, navy ink, cobalt for links/highlights, green only for primary CTAs.

**Color tokens (Tailwind config + CSS vars):**
- `brand-navy` `#1A3A5C` — primary text, headings on light, footer
- `brand-cobalt` `#4A90D9` — secondary buttons, link text, section accents
- `brand-green` `#22C55E` — primary CTA fill, success states, the "on" indicator dot
- `bg` `#F8FAFC` — page background
- `surface` `#FFFFFF` — cards, nav, modal
- `text-primary` `#0F172A`
- `text-secondary` `#64748B`
- `border` `#E2E8F0`

**Typography:**
- Headings: **Inter**, weights 600/700. Tight tracking on h1 (`-0.02em`).
- Body: **IBM Plex Sans**, weights 400/500/600. Line-height 1.6.
- Mono: **Source Code Pro** — used sparingly for inline `<code>` and version strings in the FAQ.
- Scale: h1 56/64, h2 36/44, h3 24/32, h4 20/28, body 16/26, small 14/22.

**Spacing / layout:** 4px base unit, 8px grid. Container `max-w-7xl` (1280px), content reads at `max-w-6xl`. Section vertical padding `py-20 md:py-28`. Border-radius `rounded-md` (4px) for cards/inputs, `rounded-full` only for the CTA pill and the logo dot.

**Iconography:** Lucide icons (`lucide-react`). Stroke 1.75, color `brand-navy` or `brand-cobalt`. One icon per feature card. No emoji.

**Imagery:** Zero stock photos. Visual interest comes from:
1. A small SVG switchboard motif in the hero (a stylized 3×3 grid of dots connected by lines, navy + green) — inline SVG, no external asset.
2. Subtle dotted-grid background pattern in the hero, `bg-[radial-gradient(...)]`, very low opacity.
3. A faux product mockup (built in pure HTML/Tailwind, not an image) embedded as a code-style "preview card" inside the hero, showing a small terminal/dashboard snippet in monospace.

**Interaction / motion:**
- CTA button: `hover:bg-brand-green/90 active:scale-[0.98] transition`. Green focus ring `focus-visible:ring-2 ring-brand-green/40`.
- Nav links: cobalt underline that animates in on hover (`after:scale-x-0 hover:after:scale-x-100 after:transition-transform after:origin-left`).
- Feature cards: lift on hover `hover:-translate-y-0.5 hover:shadow-md transition`.
- FAQ items: `details/summary` with a chevron that rotates 90° when open; CSS-only, no JS.
- Scroll reveal: subtle fade+rise on the six feature cards and pricing cards using `IntersectionObserver` in a tiny client component (`whileInView`-style, but framer-motion is **not** added — implemented with raw `useEffect` + `IntersectionObserver` to keep the bundle lean).
- No parallax, no auto-playing video, no animated background gradients.

---

### Screen-by-screen layout

This site is one long page with a sticky nav and seven anchored sections. The "screens" below are the named sections top-to-bottom.

#### Sticky Top Nav (`<header>`)
- Height 64px, `bg-white/80 backdrop-blur border-b border-border`.
- Left: switchboard icon (32×32 SVG) + "Uncle" wordmark (Inter 600, 20px) in navy.
- Center (≥md): nav links — Features, How it works, Pricing, FAQ — each `text-sm text-text-secondary hover:text-brand-navy`, cobalt underline on hover.
- Right: "Join waitlist" button (ghost navy outline on this nav, full green pill appears on the final CTA section).
- Mobile (<md): hamburger reveals a full-width sheet with the same links stacked + the waitlist button at the bottom.

#### Section 1 — Hero (`#top`)
- Two-column on `md+`, single-column stacked on mobile.
- **Left column (60% on desktop):**
  - Eyebrow pill: `border border-border bg-white text-text-secondary text-xs uppercase tracking-wider px-3 py-1 rounded-full` reading "Private beta · 2026".
  - **H1 (Inter 700, 56px desktop / 36px mobile):** "Validate Before You Build."
  - One-line value prop (body 20px, `text-text-secondary`): "Uncle turns your startup idea into a tested prototype in days — no technical co-founder required."
  - **Waitlist form:** inline on `md+`, stacked on mobile. Email input (`h-12 rounded-md border-border`, placeholder `you@startup.com`) + green "Join the waitlist" button. Below it, micro-helper text: "We'll email you once when invites open. No drip campaign." — explicitly sets expectations and builds trust in lieu of fake social proof.
  - Trust line (text-xs, text-secondary): "Used in private beta by solo founders and small teams." — generic, not a fake number.
- **Right column (40%):** a "preview card" — a 380×280 rounded-md white card with a thin border, a fake macOS-style traffic-light row, and a monospace "build log" snippet, e.g.
  ```
  $ uncle validate "marketplace for dog walkers"
  → scanning 12 competitor pages…
  → drafting user interview script (8 Qs)
  → building clickable prototype…
  ✓ ready in 4 min
  ```
  This is hand-typed JSX, not a screenshot, so it's crisp at any DPR.

#### Section 2 — How it works (3-step strip, `#how`)
- Section eyebrow `text-xs uppercase tracking-wider text-brand-cobalt`: "How it works".
- H2: "From idea to validated prototype in three steps."
- Three equal cards (`grid md:grid-cols-3 gap-6`), each card: navy number badge ("01" / "02" / "03" in Inter 700, 32px, on a `bg-brand-navy/5` rounded-md square), short h3, two-line description.
  1. **Describe your idea** — Plain-English prompt; Uncle decomposes it into assumptions, user segments, and risks.
  2. **Run validation** — Auto-generated interview script, landing page, and 5-user test plan, ready in minutes.
  3. **Build the prototype** — Uncle scaffolds a clickable, deployable MVP you can share with real users.

#### Section 3 — Features grid (`#features`)
- H2: "Everything you need to go from idea to launch."
- Subhead: "Six focused tools. No bloated suite."
- 6 cards in a `grid md:grid-cols-2 lg:grid-cols-3 gap-6`. Each card:
  - 40×40 icon block (`bg-brand-navy/5 rounded-md` + Lucide icon in `text-brand-navy`).
  - H3 (Inter 600, 20px).
  - Body (IBM Plex Sans, 16px, text-secondary) — two short sentences, no marketing fluff.
  - Card content (exact copy):
    1. **AI Rapid Prototyping** — *Describe the product. Get a working clickable prototype the same afternoon, not next sprint.*
    2. **Built-in User Testing** — *Generate an interview script, recruit test users, and capture structured feedback without leaving Uncle.*
    3. **Launch Analytics** — *See who visited, where they clicked, and what they skipped — the signals that decide what to build next.*
    4. **Guided Validation** — *Uncle flags your riskiest assumptions first so you spend time on the questions that actually matter.*
    5. **No Code Required** — *Point, click, and prompt. If you can write an email, you can ship an MVP.*
    6. **Iterate with Data** — *Each user test updates a prioritized backlog. The next prototype starts where the last one left off.*
- Hover: card lifts 2px, shadow-md, border darkens to `border-brand-navy/20`.

#### Section 4 — Pricing teaser (`#pricing`)
- H2: "Simple pricing. Free while in beta."
- Subhead: "All paid tiers include unlimited prototypes and tests."
- Three cards (`grid md:grid-cols-3 gap-6`), middle card highlighted with `ring-2 ring-brand-cobalt` and a small "Most popular" badge on top (`bg-brand-cobalt text-white text-xs px-2 py-0.5 rounded-full`).
  1. **Free — $0/mo**
     - "For solo founders kicking the tires."
     - List: 1 active prototype, 5 user tests / month, Community support.
     - CTA: ghost button "Start free".
  2. **Pro — $29/mo** *(highlighted)*
     - "For founders shipping their first MVP."
     - List: Unlimited prototypes, 50 user tests / month, Launch analytics, Email support.
     - CTA: green "Join waitlist for Pro".
  3. **Team — $79/mo**
     - "For 2–3 person founding teams."
     - List: Everything in Pro, Shared workspace, 3 seats included, Priority support.
     - CTA: ghost button "Join waitlist for Team".
- Below the three cards, a single line of text-secondary: "Prices shown in USD. Final pricing may change before public launch — waitlist members get locked-in rates."

#### Section 5 — FAQ (`#faq`)
- H2: "Questions, answered."
- `<details>`/`<summary>` list, max-width 3xl, divided by `border-b border-border`.
- Six questions (chevron-right rotates on `[open]`):
  1. **Do I need to know how to code?** — *No. Uncle generates and deploys the prototype. You describe what you want in plain English and iterate through prompts.*
  2. **What does "validated prototype" actually mean?** — *A clickable, shareable web app that real users can use, paired with a structured 5-person test and a report of what to change next.*
  3. **How is this different from a no-code tool?** — *No-code tools give you building blocks. Uncle gives you a finished first version — and the validation plan to know whether to keep building it.*
  4. **Can I export my code?** — *Yes. Every prototype ships with a clean repository you can download or hand to a developer to extend.*
  5. **When will invites open?** — *We're in private beta. Waitlist members are invited in small cohorts as capacity grows — typically a few weeks after signup.*
  6. **Is my idea safe?** — *Your projects are private to your workspace. We don't train models on your prompts or prototypes.*

#### Section 6 — Final CTA (`#cta`)
- Full-width band with `bg-brand-navy text-white` background, a faint switchboard SVG motif in the bottom-right at 10% opacity.
- Centered content:
  - H2 (Inter 700, white, 44px): "Stop guessing. Start validating."
  - Body (`text-white/70`, 18px): "Join the waitlist and be the first to ship a real prototype this quarter."
  - Waitlist form (same component as hero): email + green "Join the waitlist" button.
  - Below: micro-helper text in `text-white/50`, 13px: "No spam. One email when invites open."
- Above the CTA, a slim two-column reassurance row:
  - Left: "Built for first-time founders" + Lucide `rocket` icon.
  - Right: "Open export — your code is yours" + Lucide `download` icon.

#### Footer
- `bg-white border-t border-border`, `py-12`.
- Four-column grid on `md+`, stacked on mobile:
  - Col 1: switchboard icon + "Uncle" wordmark, then `text-text-secondary text-sm`: "Validate, build, launch."
  - Col 2 — **Product**: Features, How it works, Pricing, Changelog (last two non-functional — links scroll-anchor to `#features` and `#pricing`; Changelog links to `/changelog` which renders a "Coming soon" page).
  - Col 3 — **Company**: About (`/about` placeholder), Contact (`mailto:hello@uncle.so`).
  - Col 4 — **Legal**: Privacy (`/privacy`), Terms (`/terms`). Both placeholder pages with the standard skeleton + a single sentence each — explicitly noted as placeholder.
- Bottom row: `text-xs text-text-secondary` — "© 2026 Uncle Inc. All rights reserved."

---

## 4. USER FLOWS

This is a marketing page, so the flows are short and conversion-focused.

### Flow A — Browse → waitlist (primary)
1. Land on `/` → hero visible above the fold, no scroll required to read headline + see the form.
2. Optionally scroll through Features → How it works → Pricing → FAQ.
3. Click any "Join waitlist" button (hero, mid-page after pricing, or final CTA) — all anchor to `#waitlist` or `#cta` so the user lands on the final-CTA form.
4. Submit email → POST to `/api/waitlist` → on success, the form swaps to a confirmation state ("You're on the list. We'll be in touch.") with a check icon. On error (network/duplicate), inline error message under the input, form remains filled.
5. No redirect; the user can keep scrolling. No auto-scroll back to top.

### Flow B — Browse → FAQ → scroll to CTA
1. User scrolls directly to `#faq` via the nav.
2. Opens one or more `<details>` items — pure CSS, no JS state.
3. Hits the question "When will invites open?" → answer references the waitlist → user clicks the final CTA button below → email captured via Flow A.

### Flow C — Mobile nav
1. On `<md`, user taps hamburger.
2. Sheet slides down with nav links + waitlist button.
3. Tap "Join waitlist" → sheet closes, smooth-scroll to `#cta`.

### Flow D — Direct deep link
- `/pricing`, `/features`, `/faq`, `/about`, `/privacy`, `/terms` all exist as real routes (see §5). Pricing/Features/FAQ either redirect to `/` with hash or render the same section content so deep links work.

**Form states (single component, used in hero and CTA):**
- `idle` — empty input, green button enabled.
- `submitting` — button shows spinner, input disabled, button label "Joining…".
- `success` — form replaced with a green-bordered confirmation card + check icon + message.
- `error` — input gets red border (`border-red-500`), helper text below turns red, message varies ("That email doesn't look right." / "You're already on the list." / "Something went wrong — try again.").

---

## 5. PAGES / ROUTES

| Route | Purpose | Layout & key UI |
|---|---|---|
| `/` | Main marketing page | Sticky nav + sections 1–6 + footer. Server-rendered, one client component for the waitlist form (or two — one in hero, one in CTA, sharing a single form component file). |
| `/pricing` | Standalone pricing deep link | Same Pricing section from `/` rendered as a full page with the same nav and footer — for users who share the link. |
| `/features` | Standalone features deep link | Same Features + How-it-works sections rendered full-width. |
| `/faq` | Standalone FAQ deep link | Same FAQ section rendered full-width. |
| `/about` | About placeholder | Single centered card, h1 "About Uncle", one paragraph: "We're a small team building tools to help first-time founders ship. More soon." Honest placeholder, no invented history. |
| `/privacy` | Privacy placeholder | h1 "Privacy", 3 short paragraphs explicitly noting the page is a placeholder pending legal review. |
| `/terms` | Terms placeholder | Same pattern as `/privacy`. |
| `/changelog` | Changelog placeholder | h1 "Changelog", single line "Nothing shipped yet. Check back soon." |
| `/api/waitlist` | POST endpoint | Accepts `{ email: string }`. Validates with a simple regex. Upserts into Supabase `waitlist` table (see §7). Returns `{ ok: true }` or a typed error. Rate-limited via Upstash or a simple in-memory token bucket (best-effort, with a clear comment that production should use Upstash). |

**Redirects:** none required. The deep-link routes share components with `/` so content stays in sync.

---

## 6. CORE FEATURES

1. **Sticky navigation with anchor scroll-spy**
   - Active section is highlighted in the nav as the user scrolls.
   - Implemented with `IntersectionObserver` on each `<section>`; the nav link whose `href` matches the first intersecting section gets `text-brand-navy` and a cobalt underline.
   - Clicking a nav link calls `scrollIntoView({ behavior: 'smooth', block: 'start' })`.

2. **Waitlist form (shared component)**
   - Lives at `components/WaitlistForm.tsx`, used in hero and final CTA.
   - Client component (`'use client'`) with local state: `status: 'idle' | 'submitting' | 'success' | 'error'`, `email: string`, `errorMessage: string`.
   - On submit: POST `/api/waitlist`. On success → swap to confirmation panel. On 409 (duplicate) → success state with different copy ("You're already on the list."). On 400 → red border + "Please enter a valid email." On 5xx → red border + "Something went wrong. Try again."

3. **Section scroll-reveal animation**
   - Single utility `components/Reveal.tsx` — wraps children, observes `intersection`, toggles `opacity-0 translate-y-2` → `opacity-100 translate-y-0` via Tailwind transition classes when 15% visible.
   - Used on each feature card, each pricing card, each how-it-works card.
   - Respects `prefers-reduced-motion` — disables transforms when reduced motion is requested.

4. **FAQ accordion**
   - Native `<details>`/`<summary>` — zero JS. Chevron rotates with `[open] > svg { transform: rotate(90deg) }`.

5. **Mobile nav sheet**
   - Client component `components/MobileNav.tsx` — toggled by hamburger in the header. Uses a simple fixed-position panel with `bg-white border-b border-border`, slides in via `transition-transform translate-y-[-100%] → translate-y-0`. Body scroll locked while open via `document.body.style.overflow = 'hidden'` toggled in an effect.

6. **Switchboard motif SVG**
   - `components/Switchboard.tsx` — renders an inline SVG of a 3×3 dot grid with selective connecting lines. Used in the hero (small, full color) and the final CTA band (large, low opacity).

7. **Footer link group**
   - `components/Footer.tsx` — receives a `variant: 'full' | 'simple'` prop. Full used on `/`, simple used on placeholder pages.

8. **Pricing highlight**
   - `components/PricingCard.tsx` — `featured?: boolean` prop toggles the cobalt ring and the "Most popular" badge.

9. **Feature card**
   - `components/FeatureCard.tsx` — props `{ icon: LucideIcon, title: string, description: string }`. Icon rendered inside a `bg-brand-navy/5` square.

10. **SEO + OpenGraph**
    - `app/layout.tsx` exports `metadata` with title template, description, OG image (a 1200×630 SVG generated at build time via a small Node script that produces `/public/og.svg` — no fabricated images).
    - `app/sitemap.ts` lists all routes.
    - `app/robots.ts` allows all, points to sitemap.

---

## 7. DATA MODEL

Only one persistent entity (waitlist) plus implicit session/analytics data.

### `waitlist` (Supabase Postgres table)
| Field | Type | Notes |
|---|---|---|
| `id` | `uuid` PK, default `gen_random_uuid()` | |
| `email` | `text` unique not null | Lowercased + trimmed before insert |
| `created_at` | `timestamptz` default `now()` | |
| `source` | `text` nullable | One of `hero`, `cta`, `pricing-pro`, `pricing-team`, `nav` — set by client when posting |
| `referrer` | `text` nullable | `document.referrer` at submit time |
| `user_agent` | `text` nullable | Truncated to 256 chars |
| `confirmed` | `boolean` default false | Reserved for future double-opt-in; not used in MVP |

RLS: enabled. Only `service_role` can read/insert; anon key has no access. The `/api/waitlist` route uses the service-role key server-side.

### Index
- Unique on `email` (already enforced by `unique`).
- B-tree on `created_at` for future cohort queries.

### No other entities
- No users, no sessions, no analytics tables — page views go to a future integration; this build doesn't fabricate them.

---

## 8. AUTH

**No user auth on this site.** It's a marketing page with an email waitlist only.

- No Supabase Auth, no NextAuth, no Clerk, no OAuth buttons.
- The `/api/waitlist` endpoint is server-side and uses the Supabase service-role key via env var `SUPABASE_SERVICE_ROLE_KEY` — never exposed to the client.
- Env vars required (documented in `.env.example`):
  - `NEXT_PUBLIC_SUPABASE_URL` — anon-safe URL used to verify Supabase connectivity at build time (optional).
  - `SUPABASE_SERVICE_ROLE_KEY` — server-only, used in `/api/waitlist`.
  - `UPSTASH_REDIS_REST_URL`, `UPSTASH_REDIS_REST_TOKEN` — optional, for rate limiting; falls back to in-memory bucket if absent.
- A `.env.example` ships with the project; README explains how to provision Supabase + (optionally) Upstash.

**Explicit decision:** No Google/GitHub/Apple sign-in buttons anywhere on the site. Per constraint, no social buttons that don't work.

---

## 9. FILES

```
app/
  layout.tsx                 Root layout: fonts, metadata, Tailwind, global wrapper
  page.tsx                   Main landing page composing all sections
  globals.css                Tailwind directives + CSS vars for brand colors + base styles
  sitemap.ts                 Sitemap generator listing all routes
  robots.ts                  robots.txt
  pricing/page.tsx           Standalone pricing deep link (reuses PricingSection)
  features/page.tsx          Standalone features deep link (reuses FeaturesSection + HowItWorks)
  faq/page.tsx               Standalone FAQ deep link (reuses FAQSection)
  about/page.tsx             Placeholder about page
  privacy/page.tsx           Placeholder privacy page
  terms/page.tsx             Placeholder terms page
  changelog/page.tsx         Placeholder changelog page
  api/waitlist/route.ts      POST handler for waitlist signup (validates, upserts to Supabase)

components/
  Nav.tsx                    Sticky top nav with scroll-spy and mobile sheet
  MobileNav.tsx              Client component: hamburger-triggered sheet
  Hero.tsx                   Section 1 — headline, value prop, waitlist form, preview card
  Switchboard.tsx            Reusable inline SVG switchboard motif (props: size, opacity, color)
  WaitlistForm.tsx           Client form: idle/submitting/success/error states, shared by hero + CTA
  PreviewCard.tsx            Faux terminal/build-log card in hero
  HowItWorks.tsx             Section 2 — three-step strip
  Features.tsx               Section 3 — six feature cards grid
  FeatureCard.tsx            Single feature card with icon + title + description
  Pricing.tsx                Section 4 — three pricing cards with featured highlight
  PricingCard.tsx            Single pricing card (featured prop)
  FAQ.tsx                    Section 5 — six native <details> items
  FAQItem.tsx                Single <details>/<summary> with rotating chevron
  FinalCTA.tsx               Section 6 — navy band with second waitlist form
  Footer.tsx                 Footer with link groups (full + simple variants)
  Reveal.tsx                 IntersectionObserver wrapper for fade-in on scroll, respects reduced-motion

lib/
  supabase.ts                Server-only Supabase client (service role)
  rateLimit.ts               Token bucket / Upstash helper for /api/waitlist
  validateEmail.ts           Single regex + length check helper
  analytics.ts               No-op stub that logs to console; placeholder for future PostHog/Plausible

public/
  favicon.svg                Switchboard mark, navy + green
  og.svg                     Generated 1200×630 OG image (navy background, wordmark + headline)
  logo-wordmark.svg          "Uncle" wordmark used in nav and footer

scripts/
  generate-og.mjs            Node script that writes /public/og.svg at build time

tailwind.config.ts           Brand color tokens, font families, container, borderRadius override
postcss.config.mjs           Tailwind + autoprefixer
next.config.mjs              Standard Next 14 config, no special rewrites
.env.example                 SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY, optional UPSTASH_*
README.md                    Setup, env vars, deploy notes, explicit note that no fake social proof is used
tsconfig.json                Standard Next + strict
package.json                 next, react, tailwindcss, @supabase/supabase-js, lucide-react, zod
```

Total: ~35 source files. All routes and components are real, no placeholders for unimplemented features (the `/about`, `/privacy`, `/terms`, `/changelog` pages are intentionally minimal one-paragraph placeholders, explicitly labeled as such in copy).

---

## 10. ACCEPTANCE

**Done and working means all of the following are true:**

- [ ] `pnpm install && pnpm dev` boots the site at `http://localhost:3000` with zero errors and zero TypeScript warnings.
- [ ] `pnpm build` produces a successful production build; `pnpm start` serves it.
- [ ] Lighthouse (mobile) scores: Performance ≥ 95, Accessibility ≥ 95, Best Practices ≥ 95, SEO ≥ 95.
- [ ] All seven sections render on `/` in order: Hero, How it works, Features, Pricing, FAQ, Final CTA, Footer.
- [ ] Sticky nav highlights the section currently in view as you scroll (desktop ≥md).
- [ ] Clicking any nav link smooth-scrolls to the corresponding section.
- [ ] Mobile (<md): hamburger opens a sheet with all nav links and a "Join waitlist" button that closes the sheet and smooth-scrolls to `#cta`.
- [ ] The waitlist form in the hero and the form in the Final CTA both POST to `/api/waitlist` and transition through `idle → submitting → success` (or `error`) with appropriate UI.
- [ ] Submitting a valid email inserts a row into the Supabase `waitlist` table (verifiable in Supabase dashboard).
- [ ] Submitting the same email twice returns the success state with "You're already on the list." copy — no duplicate rows, no error to the user.
- [ ] Submitting an invalid email shows an inline error and does not hit the API.
- [ ] All six FAQ items expand/collapse using only CSS; no JS errors in the console.
- [ ] All six feature cards, three pricing cards, and three how-it-works cards animate in on scroll, and **do not animate** when `prefers-reduced-motion: reduce` is set.
- [ ] Brand colors match the design spec exactly: navy `#1A3A5C`, cobalt `#4A90D9`, green `#22C55E`, background `#F8FAFC`, surface `#FFFFFF`, text `#0F172A` / `#64748B`, border `#E2E8F0`.
- [ ] Fonts: Inter for headings, IBM Plex Sans for body — verified via DevTools computed styles.
- [ ] No fake testimonials, customer quotes, logos, ratings, user counts, revenue figures, or press mentions appear anywhere in the copy or visuals.
- [ ] No Google/GitHub/Apple/any social sign-in buttons exist on the site.
- [ ] No Clerk dependency in `package.json`.
- [ ] `/pricing`, `/features`, `/faq` are reachable and render their corresponding section.
- [ ] `/about`, `/privacy`, `/terms`, `/changelog` render with honest placeholder copy that explicitly says the page is a placeholder.
- [ ] `/sitemap.xml` and `/robots.txt` are generated and accessible.
- [ ] OG image at `/og.svg` renders correctly when sharing the URL on Twitter/LinkedIn (verified via `opengraph.xyz` or similar).
- [ ] No console errors or warnings on any route in production build.
- [ ] Keyboard navigation works: tab order is logical, focus rings are visible (green ring on CTA, navy ring on links), `Esc` closes the mobile nav.
- [ ] All interactive elements have accessible labels (aria-label on icon-only buttons, `<label htmlFor>` on form inputs).
- [ ] `.env.example` is committed; `.env.local` is git-ignored.
- [ ] README documents: required env vars, Supabase table SQL, how to run, how to deploy (Vercel), and explicitly notes that the build ships without fabricated social proof.

---

FILES: ["app/layout.tsx","app/page.tsx","app/globals.css","app/sitemap.ts","app/robots.ts","app/pricing/page.tsx","app/features/page.tsx","app/faq/page.tsx","app/about/page.tsx","app/privacy/page.tsx","app/terms/page.tsx","app/changelog/page.tsx","app/api/waitlist/route.ts","components/Nav.tsx","components/MobileNav.tsx","components/Hero.tsx","components/Switchboard.tsx","components/WaitlistForm.tsx","components/PreviewCard.tsx","components/HowItWorks.tsx","components/Features.tsx","components/FeatureCard.tsx","components/Pricing.tsx","components/PricingCard.tsx","components/FAQ.tsx","components/FAQItem.tsx","components/FinalCTA.tsx","components/Footer.tsx","components/Reveal.tsx","lib/supabase.ts","lib/rateLimit.ts","lib/validateEmail.ts","lib/analytics.ts","public/favicon.svg","public/og.svg","public/logo-wordmark.svg","scripts/generate-og.mjs","tailwind.config.ts","postcss.config.mjs","next.config.mjs",".env.example","README.md","tsconfig.json","package.json"]