# Decisions

One line per non-obvious technical choice: `YYYY-MM-DD · choice · reason`. Devin appends; the founder can veto.

- 2026-09-24 · earth1.co is a static site with no accounts or data · It's the "why" site; all app features live on e1-4.com (see DOMAINS.md).
- 2026-09-24 · Default stack: Next.js static export + Tailwind + TypeScript · Same stack as e1-4.com; one thing for the founder to learn; shared brand tokens. **Pending founder confirmation in Task 1.**
- 2026-09-24 · Default host: Cloudflare Pages · Free tier allows commercial use; DNS planned on Cloudflare already. **Pending founder confirmation.**
- 2026-09-24 · KaTeX rendered at build time · No client math JS; fits the performance budget.
- 2026-09-24 · No analytics by default · Nothing to disclose or leak; revisit if the founder asks.
- 2026-09-24 · Product legal pages on e1-4.com, company privacy notice and imprint on earth1.co · Meta requires app-domain URLs; earth1.co never touches voice data.
