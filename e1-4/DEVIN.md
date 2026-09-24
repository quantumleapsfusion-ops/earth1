# Devin Build Instructions: e1-4

**Product:** e1-4 (e1-4.com), a voice-only social network. "e1-4" is short for "earth life-forms".
**Company:** Earth One Global Coalescent (corporate site: earth1.co)
**Motto:** *Think.*

**How to use this doc.** Read it in full before your first task. Items marked **(default)** are proposed choices the founder can change. Anything in **[BRACKETS]** is still to be decided by the founder: ask, don't guess. Sections 1–3 are the philosophy and product spec to absorb; sections 4–9 are rules; section 10 is the task process; `TASKS.md` has the ready-to-run tasks; `SETUP.md` lists what only the founder can do.

This doc consolidates the founder's master spec (the "Devin Build Instructions: e1-4" draft assembled from Claude, Gemini and ChatGPT output), the founder's e1-4 brief, and later decisions. Where they conflict, this doc wins.

---

## 0. Read this first

- You are building the first version of e1-4. The founder is not a professional engineer and relies on you for sound technical decisions inside the guardrails below.
- Work in **small, reviewable pull requests** (ideally under ~400 changed lines). One milestone = several PRs. Never commit to `main` directly.
- **Stop and ask** on anything in section 8 ("Always ask first"). Otherwise follow the stated default and note it in the PR.
- Stuck on the same problem after **3 attempts**? Stop, write up what you tried and what you think is wrong, and wait. Don't loop.
- Simplest thing that works. No features, libraries, or abstractions beyond the current task.
- Keep `docs/DECISIONS.md`: one line per non-obvious choice (date, choice, reason).
- Verify current requirements in each provider's official docs (Google, Meta, Microsoft, Supabase, Deepgram, Anthropic, Vercel). They change; don't rely on memory.
- **No placeholder text in production.** All visible marketing copy comes from `CONTENT.md`; UI microcopy follows the voice in `docs/BRAND.md` §6.
- **Never redraw the logo.** Use the founder's Ψ/π files only (`docs/BRAND.md` §3.2).

---

## 1. Mission and philosophy

**Mission:** replace text-based platforms by 2030, so the world communicates by speaking. Purely voice, no typing anywhere on the platform. That's the north star, not MVP scope. Every decision asks: *does this make speaking a better way to share ideas than typing?*

**Strategy:** not aimed at one competitor's users. The goal is to change how people communicate broadly and bring as wide an audience as possible.

**One-sentence pitch:** e1-4 is a social network where every post, reply and message is spoken, every voice becomes a living, animated transcript, and an assistant called Da Vinci draws out and analyses the ideas being said.

**Community self-description:** "audiophiles in audiofiles".

**Who it's for:** first the founder and early testers, using it to talk through hard ideas (theoretical physics, quantum mechanics: black holes, anti-matter, gravity, bending light). Long term, everyone: a new user should understand it within 30 seconds.

**Principles (settle tradeoffs with these):**
1. **Voice is the only input.** No typing in the end-user product (§2.10).
2. **Voice Stream is the flagship.** If a decision helps or hurts Voice Stream, that wins.
3. **Make speech visible.** The transcript is a creative, animated rendering, not captions.
4. **Privacy and deletion are core features.** Users can always destroy their voice data.
5. **Simple chrome, creative core.** Simple homepage, profile, navigation. The creativity lives in transcription and visualization.
6. **Boring technology for plumbing.**
7. **Think.** Reward deep, unhurried thought, not fast reaction.

**We will not** (default; founder to confirm):
- sell voice data or share it with advertisers
- use recordings or transcripts to train any AI model (ours or a provider's)
- create or store voiceprints, or use voice to identify people
- use engagement-bait mechanics or manipulative notifications

---

## 2. Product spec

### Feature map

| Feature | What it is | MVP? |
| --- | --- | --- |
| **Voice Stream** (flagship) | One continuous, pausable recording per person; their audio diary | Yes (M2) |
| **Living transcript** | Animated speech-to-text | Yes (M3) |
| **Da Vinci** | Assistant that transcribes, analyses, and draws what you're saying | Yes (M5) |
| **Infinity Chalkboard** | Pan-and-zoom board where Da Vinci lays out the ideas; 2D in MVP, 3D/4D later | 2D yes (M5); 3D/4D later (M8) |
| **Gravity Board** | The chalkboard's edge case: several interpretations held "in superposition" until one is chosen | **No (default):** fast-follow (M8). [FOUNDER: launch or fast-follow?] |

### 2.1 Voice Stream (flagship)

One long, continuous voice recording the user can **pause and resume at any time**: a single stream of thought, like an audio diary. Not a discrete "post".

- One big mic button starts, pauses, and resumes. Ending the stream is a separate, deliberate action.
- Each pause creates a **segment marker** in the same stream. Playback is one continuous piece with visible markers.
- A stream can run for hours. Upload audio in **small chunks (~10–30 s)** so nothing is lost if the tab closes, the phone dies, or the network drops.
- Uploads are **resumable and retry automatically**. Chunks queue in IndexedDB while offline.
- Use the Screen Wake Lock API while recording.
- **Platform limit:** mobile browsers (especially iOS Safari) may stop recording when backgrounded or locked. Detect it, keep everything captured, tell the user clearly, let them resume. Test on real devices; record findings in DECISIONS.md. Don't promise background recording the browser can't deliver.
- Chrome/Android gives WebM/Opus; Safari/iOS gives MP4/AAC. Store original chunks; produce one normalized playable file server-side (background job with ffmpeg).
- Each stream has: auto title (from Da Vinci), duration, segment markers, transcript, animation data, privacy setting, optional Da Vinci output and chalkboard.
- **New streams default to Private.**

### 2.2 Living transcript

Speech-to-text that **animates what someone is saying**.

- Word-level timestamps: each word appears as it's spoken, live and identically on playback.
- Style words by how they were said: volume, pace, pitch/emphasis, pauses. Emphasis gets bigger or bolder; fast passages compress; pauses make space.
- An ambient visual layer reacts to the audio (waveform, particles, or a shape driven by amplitude via Web Audio).
- **Three styles** to start. Default proposal: **Chalk** (default, on-brand), **Ink**, **Constellation**. [FOUNDER TO APPROVE NAMES]
- The animation is a **pure function of (transcript, word timings, style, playback time)**: replayable anywhere and deleted cleanly with the stream.
- 60 fps on a mid-range phone; degrade (fewer effects) rather than stutter.
- `prefers-reduced-motion` → calm static style. A plain-text transcript view is always available.
- "Translation" in the brief means speech → text → animated visual. Language-to-language translation is out of MVP scope, but store a language code per transcript so it can be added.

### 2.3 Da Vinci

Reads a stream's transcript and produces **analysis and visuals** matching what was said.

- **Triggers:** on pause, on stream end, and optionally every [60] s of continuous speech. Never per word. Batch to control cost.
- **Pipeline:** transcript segment → Claude → **structured JSON validated against a strict Zod schema** → client renders only validated data.
  - Fields: `title`, `summary`, `concepts[]`, `relations[]` (A → B with label), `equations[]` (LaTeX), `visuals[]` (template name + parameters), `feedback[]` (`type`: clarification | question | possible_error | suggestion, `text`, `confidence`).
- **Never execute model output as code.** No eval, no model-written HTML or scripts.
- **Visual templates (MVP):** parametric SVG/Canvas, the model picks the template and fills parameters: concept map; gravity well / spacetime curvature; orbits; photon path bending near a mass; black hole (horizon, singularity); particle–antiparticle annihilation; wave/interference.
- **Honesty rules:** separate established science from speculation; state confidence; never invent equations, sources, or citations; say politely when a claim conflicts with established physics, and why. A thinking partner, not a flatterer and not an authority.
- Store all Da Vinci output with the stream (deleted with it). Per-user daily limits during alpha; log cost per stream.

### 2.4 Infinity Chalkboard

A **pan-and-zoom canvas** where Da Vinci lays out a stream's ideas: concept nodes, connecting lines, KaTeX equations, visual templates.

- **MVP:** one board per stream, auto-laid-out, pan and zoom, **tap a node to hear the moment it was said** (jump to timestamp). 2D only.
- **Later (M8):** 3D and 4D views as an idea needs more room (WebGL, e.g. three.js, loaded only on those routes, falling back to 2D on weak devices and reduced motion); boards spanning several streams; shared boards.
- Primary demo scenario: talking through black holes and quantum mechanics.

### 2.5 Gravity Board (M8; don't build until approved)

The Infinity Chalkboard's edge case, built around black-hole physics. When an idea gets dense enough, it "collapses past its event horizon": the system generates **several candidate interpretations** of what the speaker meant and holds them side by side ("superposition") until the speaker picks one, or Da Vinci resolves it from what they say next. The chosen interpretation is rendered as topology: 3D/4D spacetime mock-ups, animation, analysis.

Engineering reality: the "stochastic intelligence" is **multiple sampled Da Vinci outputs** for the same passage, each validated against the same schema and stored as siblings until one is chosen. No new model type is needed. Speculative interpretations are labeled as such.

### 2.6 Voice timeline and pages

- **Public landing page** (logged out): see `CONTENT.md` and M0.5.
- **Home (logged in):** a timeline of streams. Each card: avatar, auto title, duration, a short looping animated-transcript preview, play button. Order: chronological from people you follow, plus a public Discover section (default). No algorithmic feed.
- **Profile:** avatar, display name, **voice intro** (a spoken bio), the user's streams visible to the viewer.
- Simple UI throughout.

### 2.7 Sharing, voice notes, friends and strangers

- Per-stream privacy: **Private (only me)**, **Specific people**, **Friends**, **Public**.
- **Voice notes:** short spoken replies and direct messages, each with its own transcript, between friends and strangers.
- **Stranger safety:** messages from non-connections go to a **Requests** inbox. Users can decline, block, or turn off stranger contact.
- Follow (one-way, public streams) and friend (mutual, friends-level access).

### 2.8 Sign-in

- **Google, Facebook, Microsoft**, on web and the installed PWA, via **Supabase Auth** (default). No home-made passwords or sessions.
- Don't auto-link accounts across providers unless the provider marks the email verified; ask the founder before enabling linking.
- Meta requires a privacy policy URL and a data-deletion mechanism. Implement both (`/privacy`, `/data-deletion`) per Meta's current docs.

### 2.9 Profile avatar

JPEG/PNG/WebP up to 5 MB. Validate by content, not extension. Strip EXIF (including location). Resize to standard sizes. Friendly default avatar.

### 2.10 Privacy and destruction of data (core feature)

- **Delete any voice item** (stream, segment, voice note): removes audio chunks, normalized audio, transcripts, timings, animation data, Da Vinci output, chalkboard data, related notifications.
- **Delete all my data / delete my account:** everything above plus profile, avatar, provider links.
- Removal from all views is immediate. Hard delete from primary storage within [24 hours]; backups purge within [30 days] (stated in the privacy policy). CDN caches invalidated.
- Request deletion from every processor that received audio or text (speech-to-text, Claude). Configure providers for **no retention and no training** where available, and record each processor's terms in `docs/PROCESSORS.md`.
- Recipients lose access when a shared item is deleted. (The privacy policy must say e1-4 can't delete copies made outside the platform.)
- **Automated test:** after deletion, no rows or storage objects tied to the item remain. Every new data type that stores user content gets this test **from M2 on**, not just in M6.
- Keep only a minimal deletion record (no content) where legally required. [FOUNDER TO CONFIRM WITH LAWYER]

### 2.11 The no-typing rule

- No free-text inputs in the end-user product. Sign-in uses the providers' screens. Search, replies, reports, profile setup: voice, taps, pick-lists, confirmations.
- Display name/handle: from the sign-in provider (default). [FOUNDER: allow a spoken name confirmed by tap?]
- Reporting: tap a category, optionally add a voice note.
- **The admin console is exempt.**
- Known tradeoff for the founder: a strict no-typing rule limits deaf, hard-of-hearing, and speech-impaired users. Transcripts help them view but not contribute. Track it; revisit before public launch.

### 2.12 Core objects

| Object | Description |
| --- | --- |
| User | Account, avatar, display name, voice intro, provider links |
| Stream | One continuous recording: privacy, title, duration |
| Segment | Span of a stream between pauses |
| AudioChunk | Uploaded piece of audio with sequence number |
| Transcript | Text, word-level timings, language code |
| AnimationSettings | Style and parameters for the living transcript |
| DaVinciOutput | Validated JSON analysis, feedback, visual specs (Gravity Board: several siblings, one chosen) |
| Chalkboard | Nodes, relations, equations, visuals for a stream |
| VoiceNote | Short spoken message or reply, with transcript |
| Relationship | Follow, friend, block, mute |
| Report | Report of content or a user |
| DeletionRequest | Record of a delete action and third-party deletion status |
| UsageLog | Per-user, per-stream cost of transcription and Da Vinci calls |

---

## 3. Design principles

- **Brand:** `docs/BRAND.md` (shared with earth1.co, copied verbatim; never let them drift). Blackboard `#0e1a13`, chalk `#f1ede1`, dust `#93a294`, one ochre accent `#d3a34c`; Fraunces + Space Grotesk; the Ψ-over-π mark. **Dark only.** Jony Ive / Steve Jobs-style seamlessness.
- **Mobile-first PWA.** Test iOS Safari, Android Chrome, and desktop Chrome, Edge, Firefox, Safari.
- **Voice-first UI:** one prominent mic button, large touch targets, obvious recording state, live level meter.
- **No exposed plumbing:** mask latency with calm motion (e.g. the mark softly glowing while Da Vinci thinks), never raw JSON, spinners everywhere, or error codes.
- **Subtraction over addition:** fix a UX problem by removing friction before adding a button.
- **Speed:** primary screens load in under 2 s on a typical mobile connection.
- **Accessibility:** semantic HTML, keyboard-operable, screen-reader labels, WCAG 2.2 AA, reduced-motion support, plain transcript always available.
- **Empty and error states are designed.** First run gets the user speaking within 2 minutes of signing in.
- Copy tone: calm, plain, in the spirit of *Think.*

---

## 4. Architecture and stack

**Defaults (confirm with the founder in Task 1; record changes in DECISIONS.md):**

| Layer | Default |
| --- | --- |
| Frontend | Next.js (App Router, Server Actions) + TypeScript (strict) + Tailwind; installable PWA |
| Hosting | **Vercel** (Pro) with preview deploys on every PR |
| Database | **Supabase Postgres**, accessed with **Prisma** (schema + versioned migrations) |
| Auth | **Supabase Auth**: Google, Facebook, Microsoft (Azure) providers |
| File storage | **Supabase Storage** to start: bucket `voice` (private) and `avatars`. All access goes through a `StorageProvider` interface so it can move to **Cloudflare R2** later without touching features (if audio download volume gets expensive; ask first). Audio only via short-lived signed URLs. |
| Background jobs | **Trigger.dev** (added in M2, when audio normalization is needed; not in M0/M1) for audio normalization (ffmpeg), transcription post-processing, Da Vinci calls, deletion fan-out. Never run long work in short-lived serverless functions. |
| Audio capture | MediaRecorder with timeslice chunks; Web Audio API for levels; Wake Lock API |
| Speech-to-text | Behind a `TranscriptionProvider` interface, **stubbed from M0** so the app runs without a key and shows a calm "transcription not enabled yet" state. Default **Deepgram** (streaming, word timestamps). [FOUNDER CONFIRMS] Must support word-level timestamps, streaming, and no-retention/no-training settings. |
| Da Vinci (LLM) | Behind a `DaVinciProvider` interface, **stubbed from M0** the same way. Default **Anthropic Claude** via the official `@anthropic-ai/sdk`, model **`claude-opus-5`**, structured outputs (`output_config.format` / `messages.parse`) validated with **Zod**. Check `stop_reason` (including `"refusal"`) before reading content. Stream long requests. **No LangChain or LlamaIndex**: one direct SDK call per job is simpler and cheaper to maintain. Verify model IDs and parameters in Anthropic's current docs at build time. |
| Visuals | SVG/Canvas for the living transcript and templates; **Konva** (or similar) for chalkboard pan/zoom; **KaTeX** for equations; three.js only for M8 |
| Email | **Resend**, from `no-reply@e1-4.com` (account and security notices only) |
| Monitoring | **Sentry** for errors (with PII scrubbing); privacy-respecting product analytics only if the founder approves |
| Rate limiting | Postgres-based or Upstash Redis (free tier); your call, noted in DECISIONS.md |
| DNS | Cloudflare (same account as earth1.co) |

**Constraints**
- `SUPABASE_SERVICE_ROLE_KEY` and every provider API key live only in server environment variables: never `NEXT_PUBLIC_…`, never in the repo, never pasted into chat. Only the Supabase URL and anon key may be public.
- Provider API keys **never** reach the browser. For live transcription from the browser, the server issues short-lived tokens.
- Authorization is enforced **in server code on every route**. If you also use Supabase Row Level Security, it's a second layer, not a replacement.
- Cost scales with **audio minutes** and **Da Vinci calls**: log cost per stream and per user, enforce per-user daily caps in alpha, and ask before adding any paid service.
- Monthly cost targets: infrastructure under $[X]; usage-based speech + LLM capped at $[Y]. [FOUNDER]
- No self-hosted servers.
- Everything reproducible: README gets a new developer running locally in under 15 minutes; `.env.example` lists every variable; secrets only in environment variables (Vercel / Trigger.dev), never in the repo.
- Data region: [FOUNDER, with lawyer: US or EU]. Supabase, storage, and processors should match where possible.

---

## 5. Engineering conventions

- **Repo:** `app/`, `components/`, `lib/` (providers, reveal/animation engines as pure functions), `db/` (Prisma schema, migrations), `jobs/` (Trigger.dev), `docs/`, `tests/`, `public/brand/`.
- **Branches:** one per task (`feature/…`, `fix/…`).
- **PRs:** what, why, how to test; screenshots or short recordings for UI; list new dependencies and why.
- **Style:** Prettier + ESLint in CI; TypeScript strict.
- **Tests:** unit for logic; integration for API routes and jobs; at least one Playwright end-to-end test per key flow; an automated **deletion test** for every user-content data type. CI must pass before merge.
- **Database:** versioned Prisma migrations only.
- **Docs:** keep `README.md`, `docs/ARCHITECTURE.md`, `docs/DECISIONS.md`, `docs/PROCESSORS.md` current.

---

## 6. MVP scope

**In:** public landing page; Google/Facebook/Microsoft sign-in; profile with avatar and voice intro; Voice Stream (record, pause, resume, resumable chunked upload, playback with markers); living transcript (3 styles), live and on playback; voice timeline; per-stream privacy, follow/friend, voice notes and replies, Requests inbox; Da Vinci analysis and a 2D Infinity Chalkboard per stream; delete stream/note, delete all data, delete account, export data; report, block, mute, admin review; consent screen, privacy policy, terms, data-deletion page, age gate.

**Out (don't build unless asked):** native iOS/Android apps; language-to-language translation; live audio rooms or calls; 3D/4D chalkboard and Gravity Board (M8); multi-stream or shared boards; advanced voice search; monetization, payments, ads, tokens/coins; algorithmic feeds; video or image posts (avatars only); multiple UI languages; any text posting or messaging.

---

## 7. Milestones (in order)

Each ends with a deployed preview and a short plain-language demo summary. Don't start the next until the current one meets its criteria and the founder approves.

**M0 · Skeleton.** Repo, CI, lint/format/typecheck/tests, Vercel deploys with PR previews, Supabase project wired (Prisma connected, first migration), README, `.env.example`, PWA shell, brand tokens copied from earth1.co.
*Done when:* a blank on-brand app deploys from `main`, CI runs on every PR, README works from a clean machine.

**M0.5 · Public landing page.** The logged-out home page per `CONTENT.md`, plus `/privacy`, `/terms`, `/data-deletion` pages (drafts, clearly marked for legal review) that Meta and Google need before sign-in can be registered. e1-4.com served over HTTPS.
*Done when:* https://e1-4.com shows the landing page on a phone, cross-links to earth1.co work, Lighthouse ≥ 90 on mobile.

**M1 · Sign-in and profile.** Supabase Auth with Google, Facebook, Microsoft; profile page; avatar upload (validation, EXIF strip, resize); default avatar.
*Done when:* a new user can sign in with each provider on a phone, gets a profile on first login, can upload an avatar (bad files rejected, EXIF stripped); logged-out users can't reach protected pages or API routes; tests cover auth and unauthorized access.

**M2 · Voice Stream core.** Consent screen; mic permission handling; record/pause/resume; chunked resumable upload to the `voice` bucket; offline queue; wake lock; normalization job; playback with seek and markers; delete stream.
*Done when:*
1. Start a stream, pause/resume 10+ times; it plays back as one stream with markers.
2. A 60-minute stream records and plays on desktop Chrome and Android Chrome; iOS Safari tested on a real device and documented (including screen lock).
3. Offline for 2 minutes mid-stream loses no audio after reconnect.
4. Closing the tab loses at most the last ~30 s.
5. Mic permission denied shows clear guidance.
6. Deleting a stream removes all audio objects and rows (automated test).

**M3 · Living transcript.** Streaming transcription behind `TranscriptionProvider`; word timings stored; animated player with 3 styles; reduced-motion fallback; plain-text view; cost logging and daily caps.
*Done when:* words appear within ~2 s of being spoken while recording and within ~200 ms of the audio on playback; animation reproducible from stored data alone; smooth on a mid-range phone (documented); transcripts deleted with the stream (test).

**M4 · Timeline, sharing, voice notes.** Home timeline; per-stream privacy; follow/friend; voice notes and replies with transcripts; Requests inbox; in-app notifications.
*Done when:* sharing with specific people gives access to only those people (tests try unauthorized access); stranger voice notes land in Requests; blocked users can't see or contact the blocker.

**M5 · Da Vinci and Infinity Chalkboard (2D).** `DaVinciProvider` with Claude, Zod schema, visual templates, pan/zoom board, tap-to-jump-to-timestamp, honesty rules, cost logging.
*Done when:* a 5-minute spoken explanation of black holes produces a board with concepts, relations, at least one correct visual template, and feedback with confidence labels; malformed model output is rejected safely (tests feed bad JSON); Da Vinci output is deleted with the stream (test).

**M6 · Privacy, deletion, moderation, legal.** Delete all data, delete account, export, third-party deletion requests, admin review page, reporting, audit log, consent management, final legal pages naming Earth One Global Coalescent, age gate.
*Done when:* the deletion suite confirms nothing remains after account deletion; a reported item appears in admin; the founder has had the legal pages reviewed by a lawyer.

**M7 · Alpha polish.** Onboarding, empty/error states, performance and accessibility pass, analytics events if approved (sign-up, first stream, day-1/day-7 return, shares), rate limiting on all public endpoints, backups verified, error alerts, load test at [X] concurrent users.
*Done when:* 20–50 invited testers use it end to end without help and the founder signs off.

**M8 · Gravity Board and 3D/4D Infinity Chalkboard (fast-follow; only when the founder approves).** Per §2.4 and §2.5.

---

## 8. Working agreement

**Devin may decide alone:** implementation details inside the current milestone (file structure, small libraries, naming); refactors inside the task; test design, error messages, styling within the brand tokens.

**Always stop and ask first:**
- adding or changing any paid service, or anything that changes monthly cost
- choosing or switching the speech-to-text or LLM provider or model
- anything that changes what audio, transcript, or personal data goes to a third party
- changing the core stack (§4)
- enabling account linking across providers
- a feature not in the current milestone (including anything to do with tokens, coins, or payments)
- any change to the meaning of Voice Stream or the no-typing rule
- deleting data or dropping tables in any shared environment

**Founder review before merge (never self-merge):** auth, sessions, permissions; privacy, sharing/visibility, deletion, export; database migrations; moderation, reporting, blocking; security config (CORS, rate limits, headers, secrets); anything that sends user content to a third party.

**Definition of done:**
1. Acceptance criteria met and shown (screenshots, recordings, test output).
2. CI green; new logic tested; new user-content types have deletion tests.
3. Works on a phone-sized screen (and iOS Safari where audio is involved).
4. No secrets, keys, or personal data in the repo or logs; no audio or transcript content in logs.
5. Docs updated.
6. PR description written, plus a 2–3 line plain-language summary for the founder.

**Resource guardrails:** one task per session; if a task runs far longer than expected, stop and report; justify every new dependency.

---

## 9. Security, privacy, moderation, legal

*(Engineering requirements, not legal advice. The founder must have a lawyer review legal pages and data handling before public launch. Voice recordings are personal data and in some laws can be sensitive or biometric data.)*

**Security:** validate and sanitize all input server-side; escape output; parameterized queries/ORM only; authorize every route on the server; audio and transcripts private by default, served only via short-lived signed URLs after an authorization check; rate-limit sign-in, uploads, voice notes, reports, Da Vinci calls; never render or execute model output as code; HTTPS only, secure cookies, security headers, dependency scanning in CI; never log audio, transcripts, or tokens.

**Privacy:** collect the minimum; no voiceprints or speaker identification; consent screen before the first recording (what's recorded, who processes it, how long it's kept, how to delete it); export and delete anytime; `docs/PROCESSORS.md` lists every processor and its data terms; minimum age [default 18].

**Content policy: "free speech absolute", reconciled with the law:**
- **Lawful** speech is never removed for being offensive, unpopular, or emotionally intense. Users shape their own experience with block, mute, and stranger-contact settings.
- **Unlawful** content (e.g. child sexual abuse material, credible threats of violence, content a court orders removed) must still be handled; some of it carries legal reporting duties. The report flow and admin page exist for this.
- Automated transcript checks **flag for human review only** and never auto-remove lawful speech.
- The admin page shows audio and transcript, can hide content and suspend accounts, and keeps an audit log.
- The written policy is [FOUNDER + LAWYER]. Devin builds the tools, not the policy.

---

## 10. Relationship to earth1.co

e1-4.com **builds**; earth1.co **argues**. Cross-link constantly (contract in `docs/DOMAINS.md`, copied from the earth1 repo). All app code, auth callbacks, and APIs live on e1-4.com only. Product legal pages live on e1-4.com and name Earth One Global Coalescent as operator.

---

## 11. Task process

Ready-to-paste tasks for M0–M2 are in `TASKS.md`. For every later milestone, the founder pastes the **"Plan the next milestone"** prompt from `TASKS.md`; you propose a breakdown into PR-sized tasks using the template there, and wait for approval before building.
