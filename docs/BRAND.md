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

## 3. Mark and logo

The mark is **Ψ over π, divided by a hairline rule**. The founder has two supplied versions:

- **Logo A: chalkboard mark** (chalk texture, hand-drawn). Use for large, expressive moments only (e.g. the Open Graph image, possibly a large footer mark).
- **Logo B: refined mark** (clean white outline on black). The primary production logo: nav, favicon, touch icons, social avatars.

Files live in `public/brand/` exactly as supplied. Suggested names: `logo-chalk.png`, `logo-refined.svg` (or `.png`).

**Do not:** redraw, trace, vectorize, AI-generate, recolor (other than to chalk `#f1ede1` if supplied as white), rotate, stretch, add gradients/glows/shadows, or substitute a generic icon. If a vector version is needed and only a raster exists, **ask the founder** for one rather than tracing it.

**If the files aren't in the repo yet:** use the text wordmark "Earth One" in Fraunces, chalk color, and leave a clearly named component (`<Mark />`) ready to swap. Don't draw a stand-in Ψ/π glyph.

Clear space around the mark: at least the height of the π. Minimum size: 24px tall in the nav.

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
