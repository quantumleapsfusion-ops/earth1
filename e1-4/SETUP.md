# e1-4.com: What You Need (Founder Setup)

This is everything you, the founder, must have before and during the Devin build of e1-4. Devin can't create these accounts for you. Create every account **under the company** (Earth One Global Coalescent) with your company email, turn on two-factor login, and give Devin **API keys or scoped access**, never your passwords.

## 1. The short answer to "do I need Vercel or Supabase?"

**For e1-4.com: yes, both** (defaults; Devin will confirm with you in Task 1).

| Need | Service (default) | What it does for e1-4 |
| --- | --- | --- |
| Hosting | **Vercel** (Pro plan) | Runs the Next.js app. Every Devin PR gets its own preview link. |
| Database + sign-in | **Supabase** (Pro plan) | Postgres database and "Sign in with Google / Facebook / Microsoft". |
| Audio and avatar storage | **Cloudflare R2** | Stores voice recordings. No download fees, which matters when people replay long streams. |
| Background work | **Trigger.dev** | Long jobs: stitching audio chunks, transcription clean-up, Da Vinci analysis. |
| Speech-to-text | **Deepgram** (default; you choose) | Live transcription with word-by-word timing, which drives the animated transcript. |
| Da Vinci's brain | **Anthropic (Claude API)** | Reads transcripts and returns the analysis and chalkboard layout. |
| App emails | **Resend** | Account and security emails from `no-reply@e1-4.com`. |
| Error tracking | **Sentry** | Tells Devin when something breaks in production. |
| DNS | **Cloudflare** | Same account as earth1.co. |
| Company email | **Google Workspace** | You at `@earth1.co` (and/or `@e1-4.com`). |
| Code | **GitHub** | A new repo, e.g. `quantumleapsfusion-ops/e1-4`. |
| Builder | **Devin** | Builds it, one task at a time. |

Why not just one service? Each piece is the "boring, reliable" choice for its job, and every one has a free or cheap tier to start with. Devin must ask you before adding any other paid service.

## 2. Rough monthly cost (alpha, a few dozen testers)

| Item | Cost |
| --- | --- |
| Vercel Pro | about $20/month (the free plan isn't allowed for commercial use) |
| Supabase Pro | about $25/month (the free tier is fine while building; upgrade before real users) |
| R2, Trigger.dev, Resend, Sentry | $0 to a few dollars on free tiers at alpha scale |
| Deepgram (speech-to-text) | pay per minute of audio: **set a monthly cap** |
| Claude API (Da Vinci) | pay per use: **set a monthly cap** in the Anthropic Console |
| Google Workspace | about $7 per user per month |
| Domain e1-4.com | about $10–20/year |
| Devin | your plan |

Check each provider's pricing page when you sign up; prices change. The two usage-based items (speech-to-text and Claude) are the ones that can grow, so the build includes per-user daily limits and cost logging, and you set spending caps in each dashboard.

## 3. Checklist, in the order you'll need it

### Before Devin Task 1 (skeleton)
- [ ] You own **e1-4.com** and can log in to its registrar.
- [ ] Create GitHub repo **`e1-4`** under `quantumleapsfusion-ops`. Copy in everything from this repo's `e1-4/` folder, **plus** `docs/BRAND.md` and the Ψ/π logo files (see the e1-4 README).
- [ ] Connect Devin to the new repo.
- [ ] Create a **Vercel** account (Pro) and connect it to the GitHub repo. Give Devin access to the project.
- [ ] Create a **Supabase** project. Pick the data region (US or EU; ask your lawyer if you'll have EU users). Put the keys into Vercel's environment variables (Devin will tell you which).

### Before Task 2 (public landing page)
- [ ] Put the two **Ψ/π logo files** in `public/brand/` (chalkboard version and refined version; SVG with transparent background is ideal).
- [ ] Move **e1-4.com** nameservers to **Cloudflare** (same steps as earth1.co).
- [ ] Approve the landing-page copy in `e1-4/CONTENT.md`.

### Before Task 3 (sign-in). The fiddliest part; Devin can walk you through each screen.
- [ ] **Google:** Google Cloud Console → new project "e1-4" → OAuth consent screen (app name e1-4, logo, privacy policy URL `https://e1-4.com/privacy`, terms URL) → create OAuth client (Web). Redirect URL: the one Supabase shows you under Auth → Providers → Google.
- [ ] **Facebook (Meta):** developers.facebook.com → create app → add Facebook Login → privacy policy URL, **data deletion instructions URL** (`https://e1-4.com/data-deletion`), app icon → redirect URL from Supabase. Meta may require **business verification** and **app review** before the public can use it. Start early; it can take days or weeks.
- [ ] **Microsoft:** portal.azure.com → Microsoft Entra ID → App registrations → new registration, supported accounts "**personal Microsoft accounts and work/school accounts**" → redirect URL from Supabase → create a client secret (note its expiry date and set a calendar reminder).
- [ ] Paste each Client ID and Secret into **Supabase → Authentication → Providers** (never into the code or a chat).

### Before Task 4 (Voice Stream recorder)
- [ ] **Cloudflare R2:** create a bucket for audio and one for avatars; create an API token scoped to those buckets and give it to Devin via Vercel environment variables.
- [ ] **Trigger.dev:** create a project; give Devin the key via environment variables.

### Before milestone M3 (animated transcript)
- [ ] Choose the **speech-to-text provider** (default Deepgram). Create an account, set a spending cap, and turn on any "no data retention" / "don't use my data to improve models" settings they offer.

### Before milestone M5 (Da Vinci + Infinity Chalkboard)
- [ ] **Anthropic Console:** create an organization for Earth One Global Coalescent, create an API key, set a **monthly spend limit**. Ask Anthropic about **zero data retention** for voice transcripts (it requires an agreement with them), and record the answer in `docs/PROCESSORS.md`.

### Before inviting testers (M6–M7)
- [ ] **Lawyer review:** privacy policy, terms, consent screen, deletion process, age limit. Voice recordings are personal data, and in some places (e.g. Illinois, the EU) voice can count as sensitive or biometric data. e1-4 never makes voiceprints, which helps, but get it reviewed.
- [ ] Decide the **minimum age** (default 18, because strangers can send voice notes).
- [ ] Approve the **content policy** (your "free speech absolute" stance, plus how unlawful content is handled; see DEVIN.md §9).
- [ ] **Sentry** and **Resend** accounts (free tiers); verify the e1-4.com sending domain in Resend.
- [ ] Register **Earth One Global Coalescent** details for the legal pages (entity type, address, jurisdiction).

## 4. Decisions only you can make (Devin will ask)

- Speech-to-text provider (default Deepgram)
- Data region (US or EU)
- Minimum age (default 18)
- Display name: taken from Google/Facebook/Microsoft (default) or spoken and confirmed by tap
- Names of the three animated-transcript styles (default proposal: Chalk, Ink, Constellation)
- Gravity Board at launch, or fast-follow (default fast-follow)
- Company email: `@earth1.co`, `@e1-4.com`, or both
- Monthly caps: infrastructure $[X] and usage-based (speech + Claude) $[Y]
