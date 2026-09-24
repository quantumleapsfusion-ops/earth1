# Devin Build Instructions: earth1.co

**Site:** earth1.co — the corporate and philosophy site of **Earth One Global Coalescent**
**Sister product:** e1-4 (e1-4.com), the voice-only social network
**Motto (earth1.co):** *Global Citizenship For All.*
**Motto (e1-4.com):** *Think.*

**How to use this doc.** Read it top to bottom before your first task. Items marked **(default)** are proposed choices the founder can change. Anything in **[BRACKETS]** is still to be decided by the founder: ask, don't guess. Section 10 links the ready-to-paste tasks. Section 11 lists what only the founder can do.

Companion files (read them when a task points to them):

| File | What it holds |
| --- | --- |
| `docs/CONTENT.md` | Every word that appears on the site. Use it verbatim. |
| `docs/BRAND.md` | Colors, type, logo rules, spacing, motion. |
| `docs/CICERO_REVEAL.md` | Full spec for the hero animation, the one bold gesture on the site. |
| `docs/DOMAINS.md` | earth1.co vs e1-4.com, DNS, email, cross-linking contract. |
| `docs/devin/TASKS.md` | Ready-to-paste tasks, one per session, in build order. |
| `docs/devin/E1-4-ADDENDUM.md` | Updates to the e1-4 master spec that came out of the earth1.co work. |
| `docs/FOUNDER_CHECKLIST.md` | Things only the founder can do. |
| `docs/DECISIONS.md` | Log of technical decisions. You append to it. |

---

## 0. Read this first (instructions to Devin)

- You are building **earth1.co**, not the e1-4 app. **Never build social-network features, sign-in, recording, or user accounts on this domain.** Those belong to e1-4.com. earth1.co is a fast, quiet, mostly static site that makes the argument for why e1-4 should exist.
- The founder is not a professional engineer and relies on you for sound technical decisions inside the guardrails below.
- Work in **small, reviewable pull requests** (ideally under ~400 changed lines). Never commit directly to `main`.
- When something is ambiguous, **stop and ask** if it's listed in section 8 ("Always ask first"). Otherwise follow the stated default and note it in your PR.
- If you are stuck on the same problem after **3 attempts**, stop, write up what you tried and what you think is wrong, and wait for guidance. Do not loop.
- Prefer the simplest thing that works. Do not add features, libraries, pages, or abstractions that are not in the current task.
- **Copy is fixed.** Every visible word comes from `docs/CONTENT.md`. Do not write new marketing copy, taglines, or filler. If a page needs words that aren't in `CONTENT.md`, ask. (This is the site's thesis: nothing on it is filler. Placeholder text anywhere in production is a bug.)
- **Never redraw, regenerate, or "improve" the logo.** Use the founder-supplied files only (see `docs/BRAND.md`). If they aren't in the repo yet, use the text-only wordmark and ask.
- Keep `docs/DECISIONS.md`: for every non-obvious technical choice, add one line with date, choice, and reason.
- Verify current requirements in official docs (hosting, DNS, fonts, KaTeX) rather than relying on memory. They change.

---

## 1. What earth1.co is for

earth1.co **argues**; e1-4.com **builds**.

earth1.co doesn't sell the product. It carries the founding philosophy, the Cicero material, and the physics content that anchor the company's worldview. Someone should be able to read this site and understand why a voice-only, borderless communication platform matters **before** they ever open e1-4.com. Every page routes naturally to e1-4.com for "what" and "how".

**Hierarchy:**

```
EARTH ONE GLOBAL COALESCENT      (company)
        earth1.co                (why: philosophy, corporate, legal entity)
            │
            ▼
          e1-4                   (product)
        e1-4.com                 (what and how: the app)
            │
   ┌────────┼──────────────┬───────────────┐
Voice Stream  Da Vinci  Infinity Chalkboard  Gravity Board
```

**Principles (use these to settle tradeoffs):**

1. **One bold gesture per page.** On the home page that's the Cicero reveal. Everything else is quiet.
2. **Nothing said is filler.** No lorem ipsum, stock photos, fake testimonials, "coming soon" padding, or invented stats.
3. **Restraint.** Jony Ive / Steve Jobs-style seamlessness: fewer elements, more space, perfect type.
4. **Physics doesn't carry a passport.** Physical law ignores borders; so should communication. The equations exist to make that argument, not to decorate.
5. **Fast and accessible for everyone, everywhere.** "Global Citizenship For All" includes people on slow phones, screen readers, and reduced-motion settings.

---

## 2. Site spec

### 2.1 Pages

| Route | Purpose | MVP? |
| --- | --- | --- |
| `/` | Home: nav, Cicero hero, mission, equations, philosophy, footer | Yes |
| `/legal/privacy` | Privacy notice for earth1.co itself (what this site collects, which is almost nothing) | Yes |
| `/legal/imprint` | Company details for Earth One Global Coalescent | Yes, with **[FOUNDER: registered name, address, jurisdiction]** |
| `/404` | Designed not-found page (copy in `CONTENT.md`) | Yes |
| `/equations/[slug]` | Longer write-ups (entanglement, superposition…) | **No.** Only if the founder approves the series (see M5) |

No blog, no careers page, no newsletter sign-up, no contact form, no cookie banner (we set no non-essential cookies, so none is needed). Ask before adding any page.

### 2.2 Home page structure (in order)

1. **Nav:** mark + "Earth One" wordmark (left); one link, "e1-4 →", to `https://e1-4.com` (right). Nothing else.
2. **Hero:** the scramble-to-Cicero reveal, then the English translation, then the citation. Full spec in `docs/CICERO_REVEAL.md`.
3. **Mission:** the "nobody wrote it to be read" argument, two short lines.
4. **Equations:** two cards (Schrödinger, Einstein field equations), each with its caption, plus a pointer to e1-4.com.
5. **Philosophy:** "Global Citizenship For All." as a large standalone statement, one supporting line.
6. **Footer:** "Earth One Global Coalescent · earth1.co", link to e1-4.com, links to the legal pages, © year.

All copy for each section is in `docs/CONTENT.md`.

### 2.3 Equations

- Render with **KaTeX at build time** (server-side, output HTML + MathML). No client-side math JS.
- Equations are set in the display face (see `BRAND.md`) where KaTeX allows; otherwise KaTeX's default fonts. Don't hand-draw equations as images.
- Each card also has an accessible label (the equation read aloud in words, provided in `CONTENT.md`).
- **Never alter an equation or invent a new one.** If the founder adds more, check them against a reliable reference and flag anything that looks wrong.

### 2.4 What earth1.co must not do

- No user accounts, sign-in, or voice recording (that's e1-4.com).
- No third-party trackers, ad pixels, social embeds, or chat widgets.
- No cookies except strictly necessary ones (there should be none).
- No external requests at runtime except to the host itself (self-host fonts; see 4.2).

---

## 3. Design principles

The brand is shared with e1-4.com. Full tokens are in `docs/BRAND.md`. Summary:

- **Palette:** blackboard `#0e1a13` background, chalk `#f1ede1` text, dust `#93a294` secondary, ochre `#d3a34c` single accent.
- **Type:** Fraunces (serif) for display, Latin, and equations; Space Grotesk (sans) for nav, UI, and body.
- **Mark:** Ψ over π, divided by a hairline rule. Founder-supplied files only.
- **Dark only.** The site is a chalkboard. Do not build a light theme.
- **Motion:** only the Cicero reveal and quiet fades. Everything respects `prefers-reduced-motion`.
- **Mobile-first.** Designed at 375px wide, then scaled up. No horizontal scroll at any width.
- **Accessibility:** semantic HTML, WCAG 2.2 AA contrast, visible focus states, keyboard reachable, screen readers get the real Cicero text (not the scramble).

---

## 4. Architecture and stack

**Proposed defaults (confirm with the founder in Task 1; record any change in DECISIONS.md):**

| Layer | Default | Why |
| --- | --- | --- |
| Framework | Next.js (App Router) + TypeScript, **static export** (`output: 'export'`) | Same stack as e1-4.com, so the founder learns one thing and brand code can be shared later. Static output means no server to run or secure. |
| Styling | Tailwind CSS with the brand tokens from `BRAND.md` as the theme | Matches e1-4. |
| Fonts | Fraunces and Space Grotesk, self-hosted via `next/font` | No runtime call to Google; faster and more private. |
| Math | KaTeX, rendered at build time | No client math JS. |
| Animation | Plain TypeScript + `requestAnimationFrame`, no animation library | The reveal is small; a library would outweigh it. |
| Hosting | **Cloudflare Pages** (default) | Free tier allows commercial use; DNS is already planned on Cloudflare (see `DOMAINS.md`). Vercel is the alternative if the founder prefers one host for both domains; note that Vercel's free Hobby plan is non-commercial. |
| DNS | Cloudflare, for both earth1.co and e1-4.com | Per the master spec. |
| Analytics | **None** for MVP (default). If the founder wants numbers: Cloudflare Web Analytics (cookieless). Ask first. | Nothing to disclose, nothing to leak. |
| CI | GitHub Actions: install, lint, typecheck, test, build, Lighthouse CI | Every PR gets checked and a preview URL. |

**Constraints**

- Everything reproducible from the repo. `README.md` must let a new developer run the site locally in under 10 minutes.
- No secrets are needed for the site itself. If one ever is, it goes in the host's environment variables and `.env.example`, never in the repo.
- Target **$0/month** infrastructure excluding the domain renewals. Ask before adding anything paid.

**Performance budget (enforced in CI via Lighthouse CI on the home page, mobile profile):**

- Lighthouse Performance, Accessibility, Best Practices, SEO: **≥ 95** each.
- Total JavaScript shipped to the home page: **≤ 60 KB gzipped** (the reveal itself should be a few KB).
- Largest Contentful Paint ≤ 2.0 s, Cumulative Layout Shift ≤ 0.02 (the reveal must not move the page; see the spec).
- Fonts: subset to Latin + the Greek and math glyphs we use (Ψ, π, ħ, ∂, μ, ν, Λ). `font-display: swap`.

---

## 5. Engineering conventions

- **Repo layout** (default):
  ```
  app/                 routes (page.tsx, legal/, not-found.tsx)
  components/          Nav, Footer, CiceroReveal, EquationCard, …
  lib/                 reveal engine (pure functions), content loader
  content/             copy as typed data, generated from docs/CONTENT.md
  public/brand/        founder-supplied logo files (never edited)
  tests/               unit (Vitest) + end-to-end (Playwright)
  docs/                these instructions + DECISIONS.md
  ```
- **Branching:** one branch per task (`feature/…`, `fix/…`). Never commit to `main` directly.
- **PRs:** describe what, why, how to test; attach mobile and desktop screenshots for any visual change, and a short screen recording for anything animated.
- **Code style:** Prettier + ESLint, enforced in CI. TypeScript `strict`.
- **Tests:**
  - Unit tests for the reveal engine (it's a pure function; see `CICERO_REVEAL.md`).
  - Playwright end-to-end: page renders, final Cicero text is correct after the reveal, reduced-motion shows it immediately, no-JS shows it, links to e1-4.com work, no console errors.
  - A **content test** that fails the build if the strings "lorem", "ipsum dolor sit amet, consectetur adipiscing" or "TODO" appear anywhere in the built HTML **outside** the reveal's scrambled starting layer.
- **Docs:** keep `README.md` and `docs/DECISIONS.md` current.

---

## 6. MVP scope

**In scope:** home page (all six sections), Cicero reveal, equation cards, legal pages, 404, SEO/social metadata, favicon/icons from the supplied mark, deploy to earth1.co over HTTPS, `www` → apex redirect, cross-links to e1-4.com.

**Out of scope (do NOT build unless asked):** blog, CMS, newsletter, contact form, careers, investor portal, light mode, multiple languages, analytics beyond the default, the equations series pages, anything that needs a database or login.

---

## 7. Milestones (build in this order)

Each milestone ends with a deployed preview URL and a short summary for the founder. Don't start the next one until the current one meets its criteria and the founder approves.

**M0 · Skeleton and deploy**
Repo, CI, lint/format/typecheck, Lighthouse CI, static export, deploy pipeline with preview URLs, README.
*Done when:* a placeholder page (blackboard background, "Earth One" in chalk; no lorem ipsum) deploys automatically from `main`, every PR gets a preview URL, and README works from a clean machine.

**M1 · Brand system and page shell**
Tokens, fonts, logo slot, Nav, Footer, section scaffolding, legal pages, 404.
*Done when:* Nav and Footer match `BRAND.md` at 375px, 768px and 1440px; fonts are self-hosted; the mark appears from the supplied files (or the text wordmark if not supplied yet); the legal pages and 404 render with the copy from `CONTENT.md`; contrast checks pass.

**M2 · The Cicero reveal**
The hero, exactly as specced in `docs/CICERO_REVEAL.md`.
*Done when:* every acceptance criterion in that doc passes, including no layout shift, reduced-motion, no-JS, and screen-reader behavior, with a screen recording on a real phone.

**M3 · Mission, equations, philosophy**
The remaining home-page sections with KaTeX at build time.
*Done when:* equations render crisply at all widths with no client math JS; captions and accessible labels match `CONTENT.md`; the whole home page passes the performance budget.

**M4 · Launch readiness**
SEO and social metadata (title, description, Open Graph and Twitter images built from the brand, canonical URLs, `robots.txt`, `sitemap.xml`), favicons and touch icons from the mark, security headers (see `DOMAINS.md`), production DNS for earth1.co and `www` redirect, final cross-browser pass (iOS Safari, Android Chrome, desktop Chrome/Firefox/Safari/Edge).
*Done when:* https://earth1.co serves the site, `www.earth1.co` 301s to it, the Lighthouse budget passes on production, sharing the URL shows the correct preview card, and securityheaders.com gives an A or better.

**M5 · Equations series (only if the founder approves)**
Turn the equations section into a running series with `/equations/[slug]` write-ups (entanglement, superposition, …) authored as MDX from founder-supplied text.
*Done when:* the founder has supplied and approved each write-up; each page follows the one-bold-gesture rule; equations are verified against a reference.

---

## 8. Working agreement

**Devin may decide on its own**
- Implementation details inside the current milestone (file structure, naming, small dev-only libraries).
- Test design and refactors inside the current task.
- Exact spacing and sizing within the tokens in `BRAND.md`.

**Always stop and ask first**
- Any new visible word, page, section, or link not in `CONTENT.md` / this doc.
- Any change to the logo, palette, fonts, or the reveal's behavior.
- Adding analytics, tracking, cookies, or any third-party script or embed.
- Anything that costs money or changes the host or DNS provider.
- Touching DNS records for e1-4.com, or MX/email records for either domain.
- Anything that would put app functionality (accounts, recording) on earth1.co.

**Requires founder review before merge (do not self-merge)**
- DNS, hosting, redirects, and security-header changes.
- Legal pages.
- The Cicero reveal (M2) and any change to it.

**Definition of done for any task**
1. Acceptance criteria met and shown (screenshots, recordings, or test output).
2. CI green: lint, typecheck, tests, build, Lighthouse budget.
3. Checked at 375px and on a real phone for anything visual.
4. No placeholder text, secrets, or trackers in the build.
5. Docs updated (`README.md`, `DECISIONS.md`).
6. PR description written, plus a two-to-three-line summary for the founder in plain language.

---

## 9. Relationship to e1-4.com

- earth1.co links to e1-4.com from the nav, the equations section, and the footer. e1-4.com links back from its nav, its CTA ("Built by Earth One Global Coalescent — global citizenship for all") and its footer. See the contract in `docs/DOMAINS.md`.
- The two sites share one brand. When e1-4.com is built, extract the tokens (colors, type scale, spacing) into a shared package or copy them verbatim, and never let them drift. Note which in `DECISIONS.md`.
- Corrections and additions to the e1-4 master spec are in `docs/devin/E1-4-ADDENDUM.md`. Apply them when you work on e1-4.

---

## 10. Tasks

Ready-to-paste tasks are in **`docs/devin/TASKS.md`**. Run them one per session, in order. Each one follows this template:

```
TASK TITLE:
GOAL (one or two sentences):
CONTEXT (sections of DEVIN.md and docs/ to read):
ACCEPTANCE CRITERIA (checkable, numbered):
FILES / AREAS TO TOUCH:
FILES / AREAS NOT TO TOUCH:
HOW TO TEST IT:
REQUIRES FOUNDER REVIEW BEFORE MERGE? (yes/no; see section 8)
QUESTIONS TO ASK BEFORE STARTING:
```

---

## 11. What only the founder can do

See **`docs/FOUNDER_CHECKLIST.md`**. In short: domain and DNS access, hosting account, logo source files, legal entity details, and approval of every PR that touches DNS, legal pages, or the reveal.
