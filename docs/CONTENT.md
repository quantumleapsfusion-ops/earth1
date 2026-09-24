# earth1.co — Site Copy

Every visible word on earth1.co comes from this file. Use it **verbatim**: same spelling, punctuation, capitalization, and diacritics. Don't add, trim, or "improve" copy. If a layout needs words that aren't here, ask the founder.

Status markers:
- **FINAL:** from the founder's brief; ship as is.
- **DRAFT, FOUNDER TO APPROVE:** assembled from the founder's own wording in the brief; ship on preview, but get a yes before production.

---

## Global

| Key | Text | Status |
| --- | --- | --- |
| `site.name` | Earth One | FINAL |
| `company.legalName` | Earth One Global Coalescent | FINAL |
| `site.domain` | earth1.co | FINAL |
| `site.motto` | Global Citizenship For All. | FINAL |
| `product.name` | e1-4 | FINAL |
| `product.url` | https://e1-4.com | FINAL |

---

## 1. Nav

| Key | Text | Status |
| --- | --- | --- |
| `nav.wordmark` | Earth One | FINAL |
| `nav.productLink` | e1-4 → | DRAFT, FOUNDER TO APPROVE |
| `nav.productLink.ariaLabel` | Go to e1-4.com, the product | DRAFT, FOUNDER TO APPROVE |

---

## 2. Hero: the Cicero reveal

Behavior is specified in `docs/CICERO_REVEAL.md`. The strings below are exact. Note the commas in the Cicero sentence: they are intentional and must be kept.

**`hero.scrambled`** (the starting state; FINAL; only ever appears inside the reveal's animated layer):

> Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.

(The reveal uses only the first 198 characters of this, to match the length of the Cicero sentence. See the spec.)

**`hero.latin`** (the restored sentence; FINAL; 198 characters):

> Neque porro quisquam est, qui dolorem ipsum, quia dolor sit, amet, consectetur, adipisci velit, sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat voluptatem.

**`hero.translation`** (FINAL):

> Nor is there anyone who loves pain itself, who seeks it out and wants to have it, simply because it is pain — but because, now and then, hard circumstance brings, through toil and pain, some great pleasure.

**`hero.citation`** (FINAL):

> Cicero, *De Finibus Bonorum et Malorum* I.32, 45 BC

**`hero.srIntro`** (screen-reader-only sentence read before the Latin; DRAFT, FOUNDER TO APPROVE):

> The placeholder text “Lorem ipsum” is a scrambled fragment of a real sentence by Cicero. Restored, it reads:

---

## 3. Mission

Two short lines. DRAFT, FOUNDER TO APPROVE (taken from the founder's brief):

> **`mission.line1`** The internet has spent decades filling empty space with words nobody was meant to read.
>
> **`mission.line2`** Earth One exists to reverse that habit. Nothing said is filler. Nothing is left half-finished.

---

## 4. Equations

**`equations.heading`** (DRAFT, FOUNDER TO APPROVE): Physics doesn't carry a passport.

### Card 1: Schrödinger equation (FINAL)

| Key | Value |
| --- | --- |
| `eq1.name` | Schrödinger equation |
| `eq1.latex` | `i\hbar \frac{\partial \Psi}{\partial t} = \hat{H}\Psi` |
| `eq1.caption` | A wavefunction doesn't carry a passport. |
| `eq1.ariaLabel` | i h-bar, times the partial derivative of psi with respect to time, equals H-hat psi. |

### Card 2: Einstein field equations (FINAL)

| Key | Value |
| --- | --- |
| `eq2.name` | Einstein field equations |
| `eq2.latex` | `G_{\mu\nu} + \Lambda g_{\mu\nu} = \frac{8\pi G}{c^{4}} T_{\mu\nu}` |
| `eq2.caption` | Spacetime curves the same way on every side of every border. |
| `eq2.ariaLabel` | G mu nu, plus lambda g mu nu, equals eight pi G over c to the fourth, times T mu nu. |

**`equations.pointer`** (DRAFT, FOUNDER TO APPROVE): See the ideas drawn out loud on e1-4 →
Links to `https://e1-4.com`.

---

## 5. Philosophy

| Key | Text | Status |
| --- | --- | --- |
| `philosophy.statement` | Global Citizenship For All. | FINAL |
| `philosophy.support` | One voice at a time. | DRAFT, FOUNDER TO APPROVE (from the e1-4 page's closing line) |

---

## 6. Footer

| Key | Text | Status |
| --- | --- | --- |
| `footer.entity` | Earth One Global Coalescent · earth1.co | FINAL |
| `footer.productLink` | e1-4.com | FINAL |
| `footer.privacy` | Privacy | FINAL |
| `footer.imprint` | Imprint | FINAL |
| `footer.copyright` | © {current year} Earth One Global Coalescent | FINAL |

---

## 7. 404 page

DRAFT, FOUNDER TO APPROVE:

> **`404.heading`** Nothing here.
>
> **`404.body`** Not even lorem ipsum.
>
> **`404.link`** Back to Earth One →

(This is the one place outside the reveal where the word "lorem" may appear. Whitelist `/404` in the content test.)

---

## 8. Legal pages

### `/legal/privacy`: DRAFT, FOUNDER AND LAWYER TO REVIEW

> **Privacy**
>
> earth1.co is the website of Earth One Global Coalescent. It has no accounts, no forms, and no advertising or tracking.
>
> Our hosting provider processes standard technical data (such as IP address and browser type) to deliver the site and keep it secure. We don't set cookies and we don't use this data to identify you.
>
> Our product, e1-4, has its own privacy policy at e1-4.com/privacy.
>
> Questions: [FOUNDER: contact email, e.g. privacy@earth1.co]
>
> Last updated: [DATE OF LAUNCH]

(If analytics are ever added, this page must be updated in the same PR.)

### `/legal/imprint`: FOUNDER TO SUPPLY

> **Imprint**
>
> Earth One Global Coalescent
> [FOUNDER: legal entity type, e.g. LLC / Ltd / Inc.]
> [FOUNDER: registered address]
> [FOUNDER: jurisdiction and registration number, if any]
> Contact: [FOUNDER: email]

Until the founder supplies these, the page is **not linked in production** and Devin should flag it in every launch-readiness summary.

---

## 9. Metadata

| Key | Text | Status |
| --- | --- | --- |
| `meta.title` | Earth One — Global Citizenship For All | DRAFT, FOUNDER TO APPROVE |
| `meta.description` | Earth One Global Coalescent: the philosophy behind e1-4, a voice-only, borderless way to communicate. | DRAFT, FOUNDER TO APPROVE |
| `meta.ogImageText` | Global Citizenship For All. | FINAL |

---

## Notes for Devin

- Straight vs curly quotes: use curly quotes (“ ” ’) and a real em dash (—) in rendered text.
- The Latin is set in Fraunces. The translation in Fraunces italic. The citation in Space Grotesk, small, dust color.
- Do not translate, paraphrase, or "fix" the Latin. It's quoted exactly as Cicero wrote it (with the traditional punctuation used in the lorem ipsum literature).
