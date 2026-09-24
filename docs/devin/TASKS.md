# earth1.co: Ready-to-Paste Devin Tasks

Paste **one task per Devin session**, in order. Each one assumes the previous one is merged. Every task starts with the same line so Devin reads the rules first.

| # | Task | Milestone | Founder review before merge? |
| --- | --- | --- | --- |
| 1 | Skeleton, CI, preview deploys | M0 | Yes |
| 2 | Brand system, Nav, Footer | M1 | No |
| 3 | Content pipeline, legal pages, 404 | M1 | Yes (legal) |
| 4 | Reveal engine (pure function + tests) | M2 | No |
| 5 | Cicero reveal hero | M2 | Yes |
| 6 | Mission, equations, philosophy | M3 | No |
| 7 | Launch: metadata, icons, headers, DNS | M4 | Yes |
| 8 | Equations series (only if approved) | M5 | Yes |

---

## Task 1: Skeleton, CI, preview deploys (M0)

```
Read DEVIN.md in full, then docs/DECISIONS.md, before doing anything.

TASK TITLE: earth1.co skeleton, CI, and preview deploys

GOAL: Create the earth1.co project so that every later task has a safe, automatically checked and deployed place to land.

CONTEXT: DEVIN.md sections 0, 4, 5, M0. docs/DOMAINS.md section 2.

ACCEPTANCE CRITERIA:
1. Next.js (App Router) + TypeScript (strict) + Tailwind, configured for static export (output: 'export'). `npm run dev`, `npm run build`, `npm test`, `npm run lint` all work.
2. Prettier, ESLint, Vitest, and Playwright set up. GitHub Actions runs lint, typecheck, unit tests, build, Playwright, and Lighthouse CI (mobile) on every PR.
3. Lighthouse CI asserts the budget in DEVIN.md section 4 (≥95 in all four categories, CLS ≤ 0.02). Fails the build if missed.
4. A content test fails the build if "lorem", "ipsum dolor sit amet, consectetur adipiscing" or "TODO" appears in the built HTML (the reveal and /404 get whitelisted in later tasks).
5. Deploys to Cloudflare Pages: `main` → production project URL (*.pages.dev for now, not earth1.co yet); every PR gets a preview URL posted on the PR.
6. The only page: blackboard (#0e1a13) background with "Earth One" in chalk (#f1ede1). No lorem ipsum, no Next.js starter content, no default favicon.
7. README.md: what this repo is, how to run it locally in under 10 minutes, how deploys work. `.nvmrc` pins the Node LTS version.
8. docs/DECISIONS.md updated with anything you chose.

FILES / AREAS TO TOUCH: whole repo, except docs/ content other than DECISIONS.md.
FILES / AREAS NOT TO TOUCH: DEVIN.md, docs/CONTENT.md, docs/BRAND.md, docs/CICERO_REVEAL.md, docs/DOMAINS.md.

HOW TO TEST IT: open a trivial PR and confirm CI runs green and a preview URL appears; open that URL on a phone.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (deploy configuration).

QUESTIONS TO ASK BEFORE STARTING: confirm Next.js static export on Cloudflare Pages (DEVIN.md section 4), and that you have a Cloudflare Pages token.
```

---

## Task 2: Brand system, Nav, Footer (M1)

```
Read DEVIN.md, docs/BRAND.md, docs/CONTENT.md sections 1 and 6.

TASK TITLE: Brand tokens, fonts, mark, Nav and Footer

GOAL: Put the shared Earth One / e1-4 brand system in place and build the page shell every section will sit in.

CONTEXT: DEVIN.md section 3 and M1. docs/BRAND.md in full.

ACCEPTANCE CRITERIA:
1. Colors, type scale, spacing, and motion tokens from BRAND.md defined once (Tailwind theme + CSS variables) in a single file that could later be copied into e1-4.com unchanged.
2. Fraunces and Space Grotesk self-hosted via next/font, subset as BRAND.md says, font-display: swap. No request to fonts.googleapis.com at runtime (verify in the network panel).
3. `<Mark />` uses the Earth One mark from public/brand/ (earth-one-mark.webp, or the transparent SVG if the founder has supplied it), following docs/BRAND.md §3.1 including its known-limits rules. If only the black-background raster exists, say in the PR how you handled it. Never redraw, trace, or knock out the background of the mark.
4. Nav: mark + "Earth One" left, "e1-4 →" right linking to https://e1-4.com (same tab). Nothing else.
5. Footer: copy from CONTENT.md section 6; Privacy and Imprint links point to /legal/privacy and /legal/imprint (pages come in Task 3).
6. Home page has empty, correctly spaced <section> landmarks for hero, mission, equations, philosophy, each with an id. No placeholder text in them.
7. Visible focus rings in ochre; skip-to-content link; passes axe with zero violations.
8. Screenshots at 375px, 768px, 1440px in the PR.

FILES / AREAS TO TOUCH: app/layout.tsx, app/page.tsx, components/Nav, components/Footer, components/Mark, styles/tokens, tailwind config.
FILES / AREAS NOT TO TOUCH: CI and deploy config.

HOW TO TEST IT: preview URL on a phone; keyboard-tab through the page; run axe.

REQUIRES FOUNDER REVIEW BEFORE MERGE? No.

QUESTIONS TO ASK BEFORE STARTING: are the logo files in public/brand/ yet?
```

---

## Task 3: Content pipeline, legal pages, 404 (M1)

```
Read DEVIN.md, docs/CONTENT.md in full, docs/DOMAINS.md section 5.

TASK TITLE: Typed site copy, legal pages, and 404

GOAL: Make docs/CONTENT.md the single source for every visible string, and ship the legal pages and 404.

CONTEXT: DEVIN.md sections 2.1 and 5. docs/CONTENT.md.

ACCEPTANCE CRITERIA:
1. content/site.ts exports every string from CONTENT.md as typed constants (keys as in the doc). Components import from there; no hard-coded visible strings in components.
2. A unit test asserts each FINAL string matches CONTENT.md exactly (parse the doc or keep a fixture; your call, note it in DECISIONS.md), and that hero.latin is 198 characters.
3. /legal/privacy renders the draft text from CONTENT.md. /legal/imprint renders but is excluded from the sitemap and footer in production until the founder supplies the details (feature flag or env var; bracketed placeholders must never ship to production).
4. /404 (not-found) renders the 404 copy with a link home. The content test whitelists "lorem" on /404 only.
5. Curly quotes and em dashes rendered correctly.

FILES / AREAS TO TOUCH: content/, app/legal/, app/not-found.tsx, tests/.
FILES / AREAS NOT TO TOUCH: brand tokens, CI config (except the content-test whitelist).

HOW TO TEST IT: visit /legal/privacy, /legal/imprint, and a bad URL on the preview.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (legal pages).

QUESTIONS TO ASK BEFORE STARTING: has the founder supplied imprint details yet?
```

---

## Task 4: Reveal engine (M2, part 1)

```
Read DEVIN.md and docs/CICERO_REVEAL.md in full.

TASK TITLE: Pure reveal engine for the Cicero hero

GOAL: Implement revealFrame() exactly as specified in docs/CICERO_REVEAL.md section 4, fully unit-tested, with no UI yet.

CONTEXT: docs/CICERO_REVEAL.md sections 2, 4, 8 (item 11).

ACCEPTANCE CRITERIA:
1. lib/reveal.ts exports revealFrame(source, target, progress, tick, seed) and a seeded PRNG. No DOM, no globals, no Math.random.
2. Build-time/unit assertion: SOURCE and TARGET are both 198 characters.
3. Unit tests: p=0 → SOURCE; p=1 → TARGET exactly; resolved count is non-decreasing across p from 0 to 1 in steps of 0.01; identical inputs give identical outputs; spaces and punctuation never enter the cycling state; cycling letters match the target's case.
4. A tiny dev-only page (/dev/reveal, excluded from the production build) with a range slider to scrub progress, for eyeballing the choreography. Attach a short recording.
5. Under 2 KB gzipped for the engine.

FILES / AREAS TO TOUCH: lib/reveal.ts, tests/, a dev-only route.
FILES / AREAS NOT TO TOUCH: the home page.

HOW TO TEST IT: npm test; scrub the slider on the dev page.

REQUIRES FOUNDER REVIEW BEFORE MERGE? No.
```

---

## Task 5: Cicero reveal hero (M2, part 2)

```
Read DEVIN.md, docs/CICERO_REVEAL.md, docs/BRAND.md sections 2 and 5.

TASK TITLE: Cicero reveal hero on the home page

GOAL: Build the hero exactly as docs/CICERO_REVEAL.md describes, using the engine from Task 4. This is the site's one bold gesture; it has to feel effortless.

CONTEXT: docs/CICERO_REVEAL.md in full.

ACCEPTANCE CRITERIA: every item in docs/CICERO_REVEAL.md section 8, 1 through 10. Specifically include in the PR:
- Screen recordings on a real iPhone (Safari) and a real mid-range Android phone (Chrome).
- Playwright tests: final text equals hero.latin after scrolling; no re-scramble when scrolling back; reduced-motion shows the final state immediately; JS-disabled shows the final state; reload past the hero shows the final state.
- Screen-reader test notes (what VoiceOver/NVDA actually read).
- A Performance panel screenshot showing no layout shift outside the hero box.
- The content test whitelists the scrambled SOURCE inside the aria-hidden reveal layer only.

FILES / AREAS TO TOUCH: components/CiceroReveal.tsx, the hero section of app/page.tsx, tests/.
FILES / AREAS NOT TO TOUCH: lib/reveal.ts (except bug fixes, with a test), other sections.

HOW TO TEST IT: preview URL on both phones; toggle reduce motion in the OS; disable JS in desktop browser dev tools.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (the one bold gesture).

QUESTIONS TO ASK BEFORE STARTING: none, unless something in the spec is impossible on a real device. If so, stop and describe it before improvising.
```

---

## Task 6: Mission, equations, philosophy (M3)

```
Read DEVIN.md sections 2.2 and 2.3, docs/CONTENT.md sections 3-5, docs/BRAND.md.

TASK TITLE: Mission, equation cards, and philosophy sections

GOAL: Finish the home page's quiet sections, with equations rendered by KaTeX at build time.

ACCEPTANCE CRITERIA:
1. Mission: the two lines from CONTENT.md, display-m, generous space, fade in on first view (none under reduced motion).
2. Equations: heading, then two <EquationCard>s (name, equation, caption) from CONTENT.md, and the pointer link to https://e1-4.com. Cards stack on mobile, sit side by side from ~900px.
3. KaTeX renders at build time to HTML + MathML. Zero KaTeX JavaScript shipped to the browser (verify the bundle). KaTeX CSS and fonts self-hosted, only what's used.
4. Each equation has its aria-label from CONTENT.md; the LaTeX strings match CONTENT.md exactly (unit test).
5. Philosophy: "Global Citizenship For All." at display-xl as a standalone statement, one supporting line below in dust.
6. Whole home page still meets the Lighthouse budget and has zero axe violations.
7. Screenshots at 375px, 768px, 1440px.

FILES / AREAS TO TOUCH: components/EquationCard, the mission/equations/philosophy sections, KaTeX setup.
FILES / AREAS NOT TO TOUCH: the hero, Nav, Footer.

HOW TO TEST IT: preview URL; zoom to 200% and check the equations stay crisp; check the network tab for any katex.js.

REQUIRES FOUNDER REVIEW BEFORE MERGE? No.
```

---

## Task 7: Launch: metadata, icons, headers, DNS (M4)

```
Read DEVIN.md M4, docs/DOMAINS.md in full, docs/FOUNDER_CHECKLIST.md.

TASK TITLE: Launch earth1.co

GOAL: Make earth1.co production-ready and serve it on the real domain.

ACCEPTANCE CRITERIA:
1. Metadata from CONTENT.md section 9: title, description, canonical URL (https://earth1.co), Open Graph + Twitter card. OG image (1200×630) generated at build time from the brand: blackboard background, "Global Citizenship For All." in Fraunces chalk, the Earth One mark (only if a transparent or high-resolution version has been supplied; otherwise text only). No other text.
2. Favicon, apple-touch-icon, and icon set generated from the Earth One mark (no redrawing; cropping to the mark with even padding is fine). robots.txt and sitemap.xml (excluding /legal/imprint until it's filled in, and /dev/*).
3. Security headers from DOMAINS.md section 2 via public/_headers. securityheaders.com grade A or better. No CSP violations in the console.
4. Custom domain: earth1.co (apex) on the Cloudflare Pages project, HTTPS, www.earth1.co → 301 → https://earth1.co. Only the records listed in DOMAINS.md section 1; nothing touching MX/email or e1-4.com.
5. Cross-browser pass: iOS Safari, Android Chrome, desktop Chrome, Firefox, Safari, Edge. Note results in the PR.
6. Lighthouse budget passes against production.
7. Every DRAFT string in CONTENT.md is either approved by the founder or listed in the PR as still pending. Nothing bracketed ([FOUNDER…]) is visible in production.
8. README updated with how production DNS and deploys are set up.

FILES / AREAS TO TOUCH: app metadata, public/, public/_headers, Cloudflare Pages settings.
FILES / AREAS NOT TO TOUCH: page content and the reveal.

HOW TO TEST IT: open https://earth1.co and https://www.earth1.co on a phone; paste the URL into a messaging app to see the share card; run securityheaders.com.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (DNS, headers, launch).

QUESTIONS TO ASK BEFORE STARTING: are the earth1.co nameservers on Cloudflare yet? Which DRAFT copy has the founder approved?
```

---

## Task 8: Equations series (M5, only if the founder approves)

```
Read DEVIN.md M5, docs/BRAND.md, docs/CONTENT.md section 4.

TASK TITLE: Equations series pages

GOAL: Turn the equations section into a running series, each entry a short page drawing a line from a physical law to "Global Citizenship For All."

CONTEXT: The founder supplies the text for each entry (entanglement, superposition, …). Devin does not write the essays. These are distinct from the Gravity Board, which is an e1-4.com product feature.

ACCEPTANCE CRITERIA:
1. MDX content collection in content/equations/, one file per entry with: title, equation (LaTeX), caption, founder-supplied body, optional link to a related e1-4.com page.
2. /equations/[slug] pages, statically generated, same brand and performance budget; one quiet page each, no new bold gestures unless the founder asks.
3. The home equations section shows the two original cards plus "More →" to an /equations index.
4. Every equation is checked against a reliable reference; anything that looks wrong is flagged in the PR, not "fixed" silently.
5. Sitemap updated.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (new public content).

QUESTIONS TO ASK BEFORE STARTING: which entries, and is the founder's text for each ready?
```
