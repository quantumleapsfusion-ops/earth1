# Founder Checklist: what only you can do

Devin can't do these. Tick them off as you go; Devin will ask when it's blocked on one.

## Before Task 1 (skeleton)

- [ ] Connect Devin to this GitHub repo (`quantumleapsfusion-ops/earth1`) with permission to open PRs.
- [ ] Create a **Cloudflare** account (free) if you don't have one. Add Devin or give it an API token scoped to **Cloudflare Pages** for this project only.
- [ ] Confirm the stack defaults in `DEVIN.md` section 4 (Next.js static site on Cloudflare Pages), or tell Devin what to change.

## Before Task 2 (brand shell)

- [ ] Put the two logo files in `public/brand/`, exactly as you have them:
  - Logo A: chalk-on-chalkboard mark
  - Logo B: white-outline-on-black refined mark (an SVG is ideal; if you only have PNG, send the largest one you have)

## Before production launch (Task 7)

- [ ] Move the **earth1.co** nameservers to Cloudflare at your domain registrar (and e1-4.com too, when its build starts).
- [ ] Approve the copy marked **DRAFT, FOUNDER TO APPROVE** in `docs/CONTENT.md` (mission lines, 404, meta, links).
- [ ] Supply the imprint details: legal entity type, registered address, jurisdiction, registration number (if any), contact email.
- [ ] Have a lawyer glance at `/legal/privacy` and `/legal/imprint`.
- [ ] Review and merge the DNS/launch PR yourself.

## Open decisions (from your brief)

- [ ] **Company email:** `@earth1.co` for you/staff/press/investors and `@e1-4.com` for the app's system mail (default proposal in `DOMAINS.md` §3). Yes / change?
- [ ] **Equations section:** stays at two cards, or becomes a running series (entanglement, superposition, …)? If a series: you write or approve each piece; Devin builds it (milestone M5).
- [ ] **Legal entity:** what kind of company is Earth One Global Coalescent, and where is it registered?
- [ ] **Analytics on earth1.co:** none (default) or Cloudflare Web Analytics (cookieless)?
- [ ] **e1-4 repo:** create a separate repo for e1-4.com (recommended) and tell Devin its name.

## Accounts you own (never Devin's personal accounts)

Domain registrar, Cloudflare, GitHub, Google Workspace. Keep 2-factor on all of them. Give Devin scoped tokens, never your passwords.
