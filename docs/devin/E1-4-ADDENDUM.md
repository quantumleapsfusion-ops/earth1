# e1-4 Master Spec: Addendum

The main e1-4 build spec is the **"Devin Build Instructions: e1-4"** section of the founder's *social network* Google Doc (sections 0–12: mission, product spec, stack, milestones M0–M7, working agreement, first three tasks, founder checklist). That spec still stands. **This addendum updates it** with decisions made since, including the ones that came out of the earth1.co work. Where they conflict, this file wins.

When the e1-4.com repo is created, copy the master spec into it as `DEVIN.md` and this addendum as `docs/ADDENDUM.md`.

---

## A1. Visual style is decided

The master spec leaves "visual style: [FOUNDER TO DESCRIBE]". It's decided: use **`docs/BRAND.md`** from this repo (blackboard `#0e1a13`, chalk `#f1ede1`, dust `#93a294`, ochre `#d3a34c`; Fraunces + Space Grotesk; the Ψ-over-π mark). Copy the tokens file verbatim from earth1.co; the two sites must never drift. The app is dark only.

The founder's logo rules apply in both repos: use the supplied files only; never redraw or AI-generate the mark.

Suggested names for the three living-transcript styles (the spec leaves them open; **founder to approve**): **Chalk** (the default, matches the brand), **Ink**, **Constellation**.

## A2. Four features, not three

The product now has four named features. Map them onto the master spec like this:

| Feature | Master spec section | MVP? |
| --- | --- | --- |
| **Voice Stream** (flagship) | 2.1, M2 | Yes |
| **Da Vinci** | 2.3, M5 | Yes |
| **Infinity Chalkboard** (was "Chalkboard") | 2.4, M5 | Yes, as specced: one 2D pan/zoom board per stream. 3D and 4D views are post-MVP. |
| **Gravity Board** | *new* | **No (default):** fast-follow after M7. **[FOUNDER: ship at launch or fast-follow?]** |

**Gravity Board, for the record (don't build it until approved):** the Infinity Chalkboard's edge case, built around black-hole physics. When an idea gets dense enough it "collapses past its event horizon": the system generates several candidate interpretations of what the speaker meant and holds them in parallel ("superposition") until the speaker picks one or Da Vinci resolves it from what they say next. The chosen interpretation is rendered as topology: 3D/4D spacetime mock-ups and animation. Engineering notes when it's scheduled:
- "Stochastic intelligence" = multiple sampled Da Vinci outputs for the same passage, each validated against the same JSON schema, stored as siblings until one is chosen. No new model type is needed.
- 3D rendering via WebGL (e.g. three.js), loaded only on the Gravity Board route. It must degrade to the 2D chalkboard on weak devices and under reduced motion.
- Same honesty rules as Da Vinci: speculative interpretations are labeled as such.

## A3. Public landing page for e1-4.com (new milestone M0.5)

Before sign-in exists, e1-4.com needs a public home page. It also gives Meta the privacy-policy URL it requires. Build it right after M0, using the earth1.co components (Nav, Footer, tokens).

Structure (from the founder's brief):
1. **Nav:** mark + "e1-4" wordmark; link "earth1.co" to https://earth1.co.
2. **Hero:** "Think." as a large standalone statement, one line of context, and a small waveform motif (the one bold gesture on this page; keep it quiet and reduced-motion safe).
3. **Product:** "four ways to speak": Voice Stream, Da Vinci, Infinity Chalkboard, Gravity Board, using the copy from the founder's **"e1-4"** Google Doc verbatim. Gravity Board gets slightly heavier visual weight (newest, most ambitious); if it isn't live yet, label it "Coming next" (founder to approve wording).
4. **CTA:** "Built by Earth One Global Coalescent — global citizenship for all, one voice at a time." + "Read the philosophy at earth1.co" → https://earth1.co.
5. **Footer:** Earth One Global Coalescent / e1-4.com, link to earth1.co, Privacy, Terms, Data deletion.

**Ask the founder before shipping the parenthetical subtitles** in that doc (e.g. "Da Vinci (Inteligence // ARI // Stochastic I // Schroodinger)"). They read like working notes, and three words are misspelled: Inteligence → Intelligence, Schroodinger → Schrödinger, Topologoical → Topological. Don't publish them as written without a yes.

## A4. Domains and email

- All app code, auth callbacks, and APIs live on **e1-4.com** only. earth1.co never hosts app features. See `docs/DOMAINS.md`.
- OAuth apps (Google, Meta, Microsoft) are registered by the founder under Earth One Global Coalescent, with redirect URIs on **e1-4.com**.
- Product privacy policy, terms, and data-deletion instructions live on e1-4.com and name Earth One Global Coalescent as the operator.
- Company email proposal: people on `@earth1.co`, app system mail on `@e1-4.com` (**founder to confirm**; see `DOMAINS.md` §3).

## A5. Content policy: reconcile two statements

The founder's brief says **"free speech absolute": no removal of lawful speech for being offensive, unpopular, or emotionally intense.** The master spec (section 9) requires reporting, blocking, automated flagging, and an admin page. These fit together as follows, and Devin should build to this:

- **Lawful** speech is never removed for being offensive or unpopular. Users control their own experience with block, mute, and stranger-contact settings.
- **Unlawful** content (e.g. child sexual abuse material, credible threats of violence, content a court orders removed) must still be handled. The report flow and admin page exist for this, and some of it carries legal reporting duties depending on jurisdiction.
- Automated transcript checks **flag for human review only**; they never auto-remove lawful speech.
- The written content policy is **[FOUNDER + LAWYER]**. Devin builds the tools, not the policy.

## A6. Stack clarifications

- **Auth:** the master spec lists "NextAuth.js / Supabase Auth". Pick one in M1 and record why in DECISIONS.md. Default: **Auth.js (NextAuth v5)** if the database is plain managed Postgres; **Supabase Auth** if Supabase is also chosen as the database. Ask before starting M1.
- **Da Vinci model:** the older draft names "Claude 3.5 Sonnet / GPT-4o". Those are out of date. Keep the model behind the `DaVinciProvider` interface and use a current model; check the provider's docs for current model IDs at build time. The provider choice is still **[FOUNDER]**.
- **Speech-to-text:** unchanged (behind `TranscriptionProvider`; founder chooses; must support word-level timestamps, streaming, and no-retention/no-training).
- **Shared brand code:** copy the tokens file from earth1.co (default) rather than a shared package. It's simpler, and a copy is fine for two sites. Revisit if a third site appears.

## A7. Build order across both repos

1. **earth1.co Tasks 1–3** (skeleton, brand shell, content). Small, and they settle the brand tokens e1-4 will reuse.
2. **e1-4.com M0** (skeleton), then **M0.5** (landing page, reusing the earth1.co shell).
3. **earth1.co Tasks 4–7** (reveal, sections, launch) in parallel with **e1-4.com M1** (sign-in, profile).
4. e1-4.com M2 onward, as in the master spec.

One Devin session per task, one repo per session.
