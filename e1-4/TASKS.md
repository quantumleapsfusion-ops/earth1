# e1-4: Ready-to-Paste Devin Tasks

Paste **one task per Devin session**, in order. Each assumes the previous one is merged. Before each task, check `SETUP.md` for anything you need to set up first.

| # | Task | Milestone | Founder review before merge? |
| --- | --- | --- | --- |
| 1 | Skeleton, CI, Vercel, Supabase | M0 | Yes |
| 2 | Public landing page and legal stubs | M0.5 | Yes (legal pages, DNS) |
| 3 | Sign-in (Google, Facebook, Microsoft), profile, avatar | M1 | Yes |
| 4 | Voice Stream recorder | M2 (first slice) | Yes |
| – | Plan the next milestone (reuse for M2 rest → M8) | any | – |

Every task uses this template:

```
TASK TITLE:
GOAL:
CONTEXT (sections of DEVIN.md / docs to read):
ACCEPTANCE CRITERIA (numbered, checkable):
FILES / AREAS TO TOUCH:
FILES / AREAS NOT TO TOUCH:
HOW TO TEST IT:
REQUIRES FOUNDER REVIEW BEFORE MERGE?:
QUESTIONS TO ASK BEFORE STARTING:
```

---

## Task 1: Skeleton, CI, Vercel, Supabase (M0)

```
Read DEVIN.md in full, then SETUP.md and docs/BRAND.md, before doing anything.

TASK TITLE: e1-4 project skeleton and deploy pipeline

GOAL: Create the e1-4 app foundation (repo structure, CI, automatic deploys, database connection) so every later task has a safe place to land.

CONTEXT: DEVIN.md sections 0, 4, 5, M0.

ACCEPTANCE CRITERIA:
1. Next.js (App Router) + TypeScript strict + Tailwind app runs locally; `npm run dev|build|test|lint` work.
2. Prettier, ESLint, Vitest, Playwright set up; GitHub Actions runs lint, typecheck, unit, build, and Playwright on every PR.
3. Deploys on Vercel: `main` → production, every PR → preview URL posted on the PR.
4. Prisma connected to the Supabase Postgres project; one initial migration (e.g. an empty `User` table stub) applied via CI or a documented command. No Supabase keys in the repo.
5. Installable PWA shell (manifest, service worker, icons from public/brand/ if present, else a plain chalk "e1-4" wordmark; never draw a Ψ/π substitute).
6. Brand tokens from docs/BRAND.md in one tokens file (Tailwind theme + CSS variables); Fraunces and Space Grotesk self-hosted via next/font. Dark only.
7. The only page: blackboard background with "e1-4" and "Think." in chalk. No lorem ipsum or starter content.
8. `TranscriptionProvider` and `DaVinciProvider` interfaces exist with stub implementations selected when their keys are absent; the app builds and runs with only the Supabase variables set.
9. README (run locally in under 15 minutes), `.env.example` listing every variable, docs/ARCHITECTURE.md, docs/DECISIONS.md, docs/PROCESSORS.md (empty table) created.

FILES / AREAS TO TOUCH: whole repo.
FILES / AREAS NOT TO TOUCH: DEVIN.md, SETUP.md, CONTENT.md, docs/BRAND.md, docs/DOMAINS.md.

HOW TO TEST IT: open a trivial PR; CI goes green and a preview URL appears; open it on a phone and "Add to Home Screen".

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (deploy and secrets configuration).

QUESTIONS TO ASK BEFORE STARTING: confirm the stack in DEVIN.md section 4 (Vercel, Supabase Auth + Postgres + Storage, Prisma; Trigger.dev from M2); confirm the Supabase data region; confirm you have Vercel and Supabase access.
```

---

## Task 2: Public landing page and legal stubs (M0.5)

```
Read DEVIN.md (sections 0, 2.6, 3, 10, M0.5), CONTENT.md, docs/BRAND.md, docs/DOMAINS.md.

TASK TITLE: e1-4.com public landing page, legal page stubs, production domain

GOAL: Give e1-4.com a public home page and the privacy / terms / data-deletion URLs that Google and Meta require before sign-in can be registered.

ACCEPTANCE CRITERIA:
1. Landing page sections in order: Nav, Hero ("Think."), Product ("four ways to speak": Voice Stream, Da Vinci, Infinity Chalkboard, Gravity Board), CTA, Footer, all copy verbatim from CONTENT.md.
2. Gravity Board card has slightly heavier visual weight than the other three, and shows the "Coming next" label while it's not live.
3. Hero waveform motif: small, quiet, SVG/Canvas; static under prefers-reduced-motion; no microphone access on this page.
4. Cross-links to https://earth1.co exactly as in docs/DOMAINS.md section 4 (same tab, no tracking parameters).
5. /privacy, /terms, /data-deletion exist with the draft text from CONTENT.md, each showing a visible "Draft, pending legal review" note until the founder approves removal.
6. e1-4.com (apex) on Vercel with HTTPS; www redirects to apex. Only the records needed for this; do not touch MX/email records.
7. Security headers (HSTS, CSP, nosniff, Referrer-Policy, Permissions-Policy allowing microphone only for self).
8. Lighthouse mobile ≥ 90 in all categories; zero axe violations; screenshots at 375px, 768px, 1440px.

FILES / AREAS TO TOUCH: app/(marketing)/, components for landing sections, legal routes, headers config, Vercel domain settings.
FILES / AREAS NOT TO TOUCH: auth, database schema.

HOW TO TEST IT: open https://e1-4.com on a phone; follow the earth1.co links both ways; toggle reduced motion.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (legal pages, DNS).

QUESTIONS TO ASK BEFORE STARTING: is e1-4.com on Cloudflare DNS yet? Has the founder approved the DRAFT copy in CONTENT.md, including whether to show the feature subtitles?
```

---

## Task 3: Sign-in, profile, avatar (M1)

```
Read DEVIN.md (sections 2.8, 2.9, 2.11, 8, 9, M1) and SETUP.md ("Before Task 3").

TASK TITLE: Google, Facebook, and Microsoft sign-in, profile, and avatar

GOAL: Let a user sign in with Google, Facebook, or Microsoft, land on a simple profile, and upload an avatar.

CONTEXT: The founder registers the three OAuth apps and enters their credentials in Supabase (SETUP.md). Ask if any provider is missing; build and test the others meanwhile.

ACCEPTANCE CRITERIA:
1. Sign-in works with all three providers via Supabase Auth, on desktop and a phone, including from the installed PWA. First login creates a User and profile (display name and default avatar from the provider).
2. No auto-linking of accounts across providers.
3. Protected pages redirect and protected API routes return 401 when logged out; authorization checked in server code.
4. Avatar upload to the Supabase `avatars` bucket via the StorageProvider interface: JPEG/PNG/WebP up to 5 MB, type checked by file content, EXIF stripped, resized to standard sizes, served via signed or cache-safe URLs; other files rejected with a clear message; default avatar when none.
5. Deleting the avatar removes every size from storage (automated test).
6. Simple logged-in home placeholder and profile page; no free-text fields.
7. Sign-out works everywhere; sessions use secure cookies.
8. Tests cover each auth flow (mocked where needed), unauthorized access, and avatar validation.

FILES / AREAS TO TOUCH: auth, middleware, profile, avatar upload and storage, Prisma schema (User/Profile), basic logged-in layout.
FILES / AREAS NOT TO TOUCH: landing page, CI/deploy config unless required.

HOW TO TEST IT: sign in with a test account for each provider on a phone; upload a valid image, a renamed .exe, and a 20 MB file; sign out and try a protected URL.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (authentication, permissions, migrations).

QUESTIONS TO ASK BEFORE STARTING: are all three providers configured in Supabase? Do the `voice` and `avatars` buckets exist?
```

---

## Task 4: Voice Stream recorder (M2, first slice)

```
Read DEVIN.md (sections 2.1, 2.10, 3, 9, M2) and SETUP.md ("Before Task 4").

TASK TITLE: Voice Stream recorder with pause/resume and resumable chunk upload

GOAL: Let a signed-in user record one long stream, pause and resume freely, have it upload safely in chunks, and play it back as one continuous stream. This is the flagship; reliability beats features.

CONTEXT: Transcription and animation are NOT part of this task (M3).

ACCEPTANCE CRITERIA:
1. One mic button starts, pauses, and resumes; a separate deliberate action ends the stream. Recording state and a live level meter are obvious.
2. A consent screen appears before the first recording (what's recorded, where it's stored, how to delete it); consent is stored.
3. Audio uploads to the private `voice` bucket (via StorageProvider) in ~10–30 s chunks with sequence numbers; uploads are resumable and retry automatically.
4. Offline for 2 minutes mid-stream: no audio lost after reconnect (chunks queued in IndexedDB).
5. Closing the tab loses at most the last ~30 s.
6. Screen Wake Lock while recording; backgrounding or locking the phone is detected and clearly communicated, and the user can resume.
7. A Trigger.dev job normalizes chunks (WebM/Opus and MP4/AAC) into one playable file; originals kept until normalization succeeds.
8. Playback plays the stream as one continuous audio with visible pause markers and seek.
9. New streams default to Private; audio served only via short-lived signed URLs after an authorization check.
10. Deleting a stream removes all chunks, the normalized file, and all rows (automated integration test).
11. Tested on desktop Chrome, Android Chrome, and iOS Safari on real devices; results, including limits, in docs/DECISIONS.md.

FILES / AREAS TO TOUCH: recorder UI, upload API, storage, jobs/, playback, Prisma (Stream, Segment, AudioChunk).
FILES / AREAS NOT TO TOUCH: auth, landing page, transcription, Da Vinci.

HOW TO TEST IT: record 60 minutes with 10 pauses; airplane mode for 2 minutes mid-stream; close and reopen the tab; lock the phone; delete the stream and check the bucket.

REQUIRES FOUNDER REVIEW BEFORE MERGE? Yes (privacy, authorization, migrations).

QUESTIONS TO ASK BEFORE STARTING: any browser limits you already know about that would change this plan?
```

---

## Plan the next milestone (reuse for every milestone after Task 4)

Paste this, replacing `M_` with the milestone (M2 remainder, then M3, M4, M5, M6, M7, M8):

```
Read DEVIN.md in full, then docs/DECISIONS.md and the merged PRs since the last milestone.

TASK: Plan milestone M_ from DEVIN.md section 7. Don't write code yet.

1. Summarize what exists today that M_ builds on, and anything from earlier milestones that's incomplete.
2. Break M_ into 2–5 PR-sized tasks using the task template in TASKS.md. Each task needs numbered, checkable acceptance criteria that together cover M_'s "Done when" list, plus deletion tests for any new user-content data.
3. List every question or founder decision M_ needs (providers, costs, copy, legal), and every setup step from SETUP.md the founder must finish first.
4. Estimate new monthly cost, if any.

Wait for the founder's approval of the plan, then build the tasks one per session.
```
