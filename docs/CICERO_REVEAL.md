# The Cicero Reveal: Hero Spec

This is the one bold gesture on earth1.co. Build it exactly as described; any change to its behavior needs the founder's approval.

## 1. The idea

"Lorem ipsum dolor sit amet, consectetur adipiscing elit…", the placeholder on nearly every unfinished web page for decades, isn't gibberish. It's a scrambled, truncated fragment of one real sentence from Cicero's *De Finibus Bonorum et Malorum* (The Extremes of Good and Evil), sections 1.10.32–33, 45 B.C.

The page loads looking **unfinished**: the familiar lorem ipsum in muted grey, as if nobody had written the site yet. As the visitor scrolls, it **decodes** letter by letter into Cicero's real sentence. Then the English translation fades in beneath it, then the citation.

The metaphor: the internet fills empty space with words nobody was meant to read. Earth One reverses that. The Cicero line itself (toil and difficulty endured now for something worth having later) is also the story of building the company.

## 2. Strings

From `docs/CONTENT.md`:

- `SOURCE` = the first **198** characters of `hero.scrambled`
- `TARGET` = `hero.latin` (exactly 198 characters)
- then `hero.translation`, then `hero.citation`

Add a build-time assertion that `TARGET.length === 198` and `SOURCE.length === TARGET.length`, so a copy edit can't silently break the alignment.

## 3. Sequence

| Phase | What the visitor sees |
| --- | --- |
| **Load** | `SOURCE` in Fraunces `display-l`, color `--dust` at ~70% opacity. Translation and citation not visible. A small `↓` scroll cue in `--dust` fades in after 3 s if the visitor hasn't scrolled. |
| **Decode** (scroll-linked) | As the visitor scrolls through the hero, characters resolve roughly left to right with slight jitter. Each character briefly cycles through random letters, then locks to its `TARGET` character and turns `--chalk`. |
| **Resolved** | The full Cicero sentence in chalk. The scroll cue is gone. |
| **Translation** | Fades in (500 ms) below the Latin, Fraunces italic, `body` size, chalk. |
| **Citation** | Fades in 300 ms after the translation. Space Grotesk `small`, `--dust`. |

**Scroll mapping (default):** the hero is a tall section (~200svh on mobile, ~180vh on desktop) with a sticky inner panel (`position: sticky; top: 0; height: 100svh`). Decode progress `p` goes from 0 to 1 as the visitor scrolls through the first ~70% of that section. The last ~30% holds the resolved sentence while the translation and citation appear.

- **Monotonic:** progress only moves forward. Scrolling back up never re-scrambles. Once resolved, it stays resolved for the page view.
- If the page loads already scrolled past the hero (refresh, back button, anchor link), show the final state immediately.
- Keyboard scrolling (Space, Page Down, arrows) drives it the same way, because it's just scroll.

## 4. The engine (pure function)

Put the logic in `lib/reveal.ts` as a **pure, deterministic function** so it can be unit-tested without a browser:

```ts
type CellState = 'source' | 'cycling' | 'resolved';
interface Cell { char: string; state: CellState }

function revealFrame(
  source: string,
  target: string,
  progress: number, // 0..1, clamped
  tick: number,     // integer, advances every ~50 ms while cycling; used only to pick random glyphs
  seed: number,     // fixed constant, so every visitor sees the same choreography
): Cell[];
```

Rules:

1. For each index `i`, compute a resolve point `r_i = 0.05 + 0.85 * (i / L) + jitter_i`, where `jitter_i` is in ±0.08 from a seeded PRNG (e.g. mulberry32), clamped to [0.02, 0.98].
2. A cell is **cycling** while `r_i - 0.10 <= progress < r_i`, **resolved** when `progress >= r_i`, otherwise **source**.
3. `source` cells show `source[i]`. `resolved` cells show `target[i]`.
4. `cycling` cells show a letter picked from `abcdefghijklmnopqrstuvwxyz` by hashing `(seed, i, tick)`, matching the case of `target[i]`. **Exception:** if `target[i]` is a space or punctuation, skip cycling and switch straight from source to target at `r_i`.
5. At `progress = 0` the output text equals `SOURCE`. At `progress = 1` it equals `TARGET` exactly.

The component (`components/CiceroReveal.tsx`) only maps scroll to `progress`, advances `tick` on `requestAnimationFrame` while any cell is cycling, and renders cells.

## 5. Rendering and layout

- Render one `<span>` per character inside the visible layer; update only spans whose char or state changed (no full re-render per frame).
- `source` and `cycling` cells: `--dust`. `resolved` cells: `--chalk`, with a 150 ms color transition.
- **No page layout shift:** reserve the hero text box's height as the larger of the two measured heights (`SOURCE` and `TARGET` at the current width), and re-measure on resize. The words inside may reflow as letters change (that's part of the effect), but nothing outside the box moves.
- Only run the `requestAnimationFrame` loop while the hero is on screen (IntersectionObserver) and not yet resolved. Remove scroll listeners after resolution. Use a passive scroll listener.

## 6. Accessibility

- The animated layer is `aria-hidden="true"`.
- A visually hidden block always in the DOM holds what a screen reader should hear: `hero.srIntro`, then the Latin in an element with `lang="la"`, then the translation and citation. Screen-reader users never hear the scramble or the random letters.
- The visible translation uses real text (not an image) and the Latin block has `lang="la"`.
- **`prefers-reduced-motion: reduce`:** no scramble, no cycling, no sticky scroll section. Render `TARGET` in chalk with the translation and citation immediately, in a normal-height hero.
- **No JavaScript:** the server-rendered HTML shows `SOURCE` in the visible layer (so the page still looks "unfinished" while JS loads), plus a `<noscript><style>` block that hides that layer and shows the final `TARGET`, translation, and citation. A visitor without JS sees the restored Cicero, never only lorem ipsum.

## 7. Performance

- The reveal code should be a few KB gzipped. No animation libraries.
- Must hold 60 fps on a mid-range Android phone. If it can't, reduce the cycling rate (every 80 ms instead of 50 ms) rather than drop frames.
- Fonts must be loaded before the decode starts to avoid a font swap mid-animation (`document.fonts.ready`).

## 8. Acceptance criteria

1. On load, the hero shows the first 198 characters of lorem ipsum in dust grey, and the rest of the page is untouched.
2. Scrolling through the hero decodes it; at the end, the visible text is **exactly** `hero.latin` (Playwright asserts the string).
3. Scrolling back up never re-scrambles.
4. The translation then the citation fade in after resolution.
5. Reloading mid-page (past the hero) shows the final state immediately.
6. With reduced motion emulated, the final state appears immediately with no animation.
7. With JavaScript disabled, the final Cicero text, translation, and citation are visible.
8. Screen reader (VoiceOver on iOS, and NVDA or VoiceOver on desktop) reads the intro, the Latin, the translation, and the citation, and nothing from the animated layer. Record the test in the PR.
9. Lighthouse CLS ≤ 0.02 on mobile; no layout shift outside the hero box during the reveal (check in the Performance panel and include a screenshot).
10. Smooth on a real mid-range Android phone and a real iPhone in Safari; include a screen recording of each.
11. Unit tests for `revealFrame`: output at `p=0` equals `SOURCE`, at `p=1` equals `TARGET`, resolved count never decreases as `p` increases, same inputs give same output, spaces and punctuation never cycle.
