# Domains, DNS, Email, and Cross-Linking

Earth One Global Coalescent runs on two domains. Keep the boundary strict.

| | **earth1.co** | **e1-4.com** |
| --- | --- | --- |
| Role | Company: the philosophy and the argument ("why") | Product: the app ("what" and "how") |
| Motto | Global Citizenship For All. | Think. |
| Contents | This static site, corporate legal pages, company identity | The e1-4 app: sign-in, Voice Stream, Da Vinci, Infinity Chalkboard, Gravity Board, product legal pages |
| Must never host | Accounts, sign-in, recording, user data | Corporate/investor material |
| Repo | `quantumleapsfusion-ops/earth1` (this one) | Its own repo **[FOUNDER: create or name it]** |

## 1. DNS (Cloudflare, default)

Both domains use Cloudflare as their DNS provider (per the master spec). Only the founder can move the nameservers; see `FOUNDER_CHECKLIST.md`.

**earth1.co records Devin sets up (with founder approval, in Task 7):**

| Name | Type | Target | Notes |
| --- | --- | --- | --- |
| `earth1.co` | CNAME (flattened) | Cloudflare Pages project | Apex serves the site |
| `www` | CNAME | Cloudflare Pages project | Then a 301 redirect `www.earth1.co/*` → `https://earth1.co/$1` |

**Devin must not touch** MX, SPF, DKIM, or DMARC records on either domain, or any e1-4.com record, without explicit approval in the task.

## 2. Security headers (earth1.co)

Set via the host's headers file (`public/_headers` on Cloudflare Pages):

- `Strict-Transport-Security: max-age=31536000; includeSubDomains` (add `preload` only after the founder confirms every subdomain is HTTPS)
- `Content-Security-Policy: default-src 'self'; img-src 'self' data:; style-src 'self' 'unsafe-inline'; font-src 'self'; script-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'none'` (adjust only if Next's static output needs a hash for an inline script; prefer hashes over `'unsafe-inline'` for scripts)
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy: camera=(), microphone=(), geolocation=()`: earth1.co never records; the mic belongs to e1-4.com.

## 3. Email

**Open decision (founder):** the brief originally asked for an `@e1-4.com` address; the later master spec put company email on `@earth1.co`. Default proposal, pending the founder's yes:

| Address type | Domain | Examples |
| --- | --- | --- |
| Founder, staff, press, investors, legal | `@earth1.co` via Google Workspace | `zach@earth1.co`, `press@earth1.co`, `privacy@earth1.co` |
| App system mail (sign-in notices, account/security alerts) | `@e1-4.com` via a transactional provider | `no-reply@e1-4.com` |
| Public product contact | `@e1-4.com` (alias forwarding to Workspace) | `hello@e1-4.com` |

Both domains need SPF, DKIM, and DMARC before any mail is sent. The founder adds the Google Workspace MX records; Devin can write the exact records into a checklist from Google's current docs but must not apply them without approval. Domains that never send mail from a given service should still publish a restrictive SPF and DMARC to prevent spoofing.

## 4. Cross-linking contract

Both sites must route visitors to each other naturally. Minimum links:

| From | Where | Text | To |
| --- | --- | --- | --- |
| earth1.co | Nav (right) | e1-4 → | `https://e1-4.com` |
| earth1.co | Equations section | See the ideas drawn out loud on e1-4 → | `https://e1-4.com` |
| earth1.co | Footer | e1-4.com | `https://e1-4.com` |
| e1-4.com | Nav | earth1.co | `https://earth1.co` |
| e1-4.com | CTA section | Built by Earth One Global Coalescent — global citizenship for all | `https://earth1.co` |
| e1-4.com | Footer | Earth One Global Coalescent / earth1.co | `https://earth1.co` |
| e1-4.com | Landing page closing line | Read the philosophy at earth1.co | `https://earth1.co` |

- Same tab, no `target="_blank"` (it's one family of sites).
- No UTM parameters or tracking on these links.
- If a deep page exists on the other side later (e.g. `earth1.co/equations/superposition` ↔ a Gravity Board demo), link the specific page, not just the home.

## 5. Legal pages: which domain

- **earth1.co** hosts the company's own site privacy notice and the imprint (company details).
- **e1-4.com** hosts the **product** privacy policy, terms, and data-deletion instructions. Meta (Facebook Login) needs these URLs on the app's domain, and they cover voice data, which earth1.co never touches.
- Both name **Earth One Global Coalescent** as the operator and link to each other.
