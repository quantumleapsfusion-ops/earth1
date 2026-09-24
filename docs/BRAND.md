# Earth One / e1-4 — Brand System

Shared by **earth1.co** and **e1-4.com**. Both sites use these exact tokens. If you change one, change it in both, and only with the founder's approval.

## 1. Color

Chalkboard-derived, pulled from the chalk-on-chalkboard logo, not a generic dark theme.

| Token | Hex | Use |
| --- | --- | --- |
| `--blackboard` | `#0e1a13` | Page background. The only background color. |
| `--chalk` | `#f1ede1` | Primary text, the mark, the resolved Cicero sentence. |
| `--dust` | `#93a294` | Secondary text, captions, citation, the scrambled lorem ipsum state. |
| `--ochre` | `#d3a34c` | The single accent: links on hover/focus, focus rings, one highlight per section at most. |
| `--rule` | `rgba(241, 237, 225, 0.14)` | Hairline rules and card borders (derived from chalk). |

Rules:
- One accent. Never introduce a second accent color, gradients, or pure black/white.
- Contrast: chalk on blackboard and dust on blackboard both pass WCAG AA for body text; verify ochre for any text use (it's fine at large sizes and for focus rings).
- Dark only. No light theme.

## 2. Type

| Role | Family | Notes |
| --- | --- | --- |
| Display, headlines, Latin, equations, big statements | **Fraunces** (variable) | Use optical size (`opsz`) high for big type. Light-to-regular weights. Italic for the translation. |
| Nav, UI, body, captions, citation | **Space Grotesk** (variable) | Regular and medium only. |

Self-host both via `next/font` (no runtime calls to Google Fonts). Subset to Latin plus the glyphs we use: Ψ π ħ ∂ μ ν Λ Ĥ →.

**Type scale (default; fluid with `clamp`)**

| Token | Mobile (375px) | Desktop (1440px) | Use |
| --- | --- | --- | --- |
| `display-xl` | 44px | 112px | "Global Citizenship For All." |
| `display-l` | 26px | 48px | Cicero Latin |
| `display-m` | 22px | 32px | Mission lines, equations |
| `body` | 17px | 19px | Translation, body |
| `small` | 13px | 14px | Captions, citation, footer |

Line length: 60–75 characters for body text; the Latin can run wider (up to ~34em).

## 3. Marks and logos

There are **two marks**, one per domain. They are siblings, not substitutes: never swap them.

### 3.1 Earth One mark (earth1.co)

The company mark. Supplied by the founder on 2026-09-24 as `public/brand/earth-one-mark.webp`.

What it is: a heavy white vertical bar rising through a thin white horizontal bar set high on it, inside two concentric circles (a thin solid grey outer ring and a dashed grey inner ring), on a black field. Treat that description as identification only, **not** as instructions to rebuild it.

Uses on earth1.co: nav, favicon and touch icons, Open Graph image, footer.

**Known limits of the supplied file (founder to resolve; Devin must not "fix" them by redrawing):**
- It's a 679×398 raster with a **solid black (#000) background**. On the blackboard background (`#0e1a13`) that black shows as a visible box. We need a version with a **transparent background**, ideally an **SVG**.
- The mark sits in a wide canvas with a lot of padding, so it will look small unless it's cropped. Cropping to the mark with even padding (no other change) is allowed.
- At 679px wide it's too small for a sharp OG image or large display. An SVG solves this too.
- Until the founder supplies a transparent SVG: use the file in the nav and footer **only on a black-backed element sized to the image** (a small black tile is acceptable), or fall back to the "Earth One" text wordmark, and flag it in the PR. Don't knock out the background with filters or blend modes, and don't trace it.

Clear space: at least half the outer circle's radius on every side. Minimum size: 28px tall in the nav (the dashed ring disappears below that; at favicon sizes it's acceptable for the dashed ring to blur, but never remove it).

### 3.2 e1-4 mark (e1-4.com)

The product mark: **Ψ over π, divided by a hairline rule**. The founder has two versions (files not yet in any repo):

- **Chalkboard version** (chalk texture, hand-drawn): large, expressive moments only.
- **Refined version** (clean white outline on black): nav, favicon, touch icons, social avatars on e1-4.com.

On earth1.co the Ψ/π mark appears **only** if the founder asks (e.g. next to the e1-4 link). Clear space: at least the height of the π.

### 3.3 Rules for both marks

Files live in `public/brand/` exactly as supplied.

**Do not:** redraw, trace, vectorize, AI-generate, recolor (other than white to chalk `#f1ede1` in an SVG the founder supplies), rotate, stretch, add gradients/glows/shadows, or substitute a generic icon. If a vector or transparent version is needed and only a raster exists, **ask the founder** for one.

**If a file isn't in the repo yet:** use the text wordmark ("Earth One" or "e1-4") in Fraunces, chalk color, behind a `<Mark />` component ready to swap.

## 4. Space and layout

- Base unit 8px. Section vertical padding: 96px mobile, 160px desktop.
- Max content width 1120px; text columns narrower (see line length).
- Hairline rules (`--rule`, 1px) separate sections sparingly. Cards have a 1px `--rule` border, no shadow, radius 2px at most (chalkboards don't have rounded corners).
- Generous whitespace is the design. When in doubt, remove.

## 5. Motion

- The only big motion on earth1.co is the **Cicero reveal** (`docs/CICERO_REVEAL.md`).
- Everything else: opacity fades of 400–600ms, `cubic-bezier(0.2, 0, 0, 1)`, no bounces, no parallax, no scroll-jacking.
- `prefers-reduced-motion: reduce` → no animation at all; content shown in its final state.

## 6. Voice (for any copy the founder approves later)

Calm, exact, unhurried. Short sentences. No hype words ("revolutionary", "disrupt", "AI-powered"). No exclamation marks. The site should feel like chalk on a board after a good lecture.
