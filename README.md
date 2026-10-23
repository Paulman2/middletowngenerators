# middletowngenerators.com

Static site for **Middletown Generator Service**. No build step, no dependencies — plain HTML/CSS.
Content and offer are owned by the Blackout agent; the build, design and deploy are owned by Blueprint.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site. Styles are inline in `<head>`; JSON-LD is at the bottom. |
| `404.html` | Not-found page. |
| `robots.txt` / `sitemap.xml` | Indexing. |

No `CNAME` file yet — see *Deploy* below.

## Compliance rules — read before editing a single line

These are hard rules from Paul, not style preferences. Several carry real legal or
licensing exposure. Do not soften, reinterpret or "improve" them.

**Never publish:**

- Any **license number**, or the words *licensed electrician* / *electrical contractor*.
  Claiming the credential while omitting the number is worse than silence, and advertising
  as an electrical contractor is the part NJ's board enforces.
- **"Insured"** or any insurance claim. Coverage is unconfirmed as of 2026-10-22.
- Any **manufacturer name used as a credential** — no *authorized*, *certified*,
  *factory-trained*, *dealer*, no manufacturer logos, nothing implying warranty service.
  There is zero dealer status and zero certification. (Twin Lights hit this exact trap
  with Sharkfin.) Neutral "major manufacturers" phrasing is the ceiling.
- Any **response-time promise** — no *same-day*, no *24-hour*, no *we'll be there fast*.
  The approved words are **"priority scheduling."** After a Sandy-scale event the promise
  is physically impossible to keep.
- **Installs** of any kind — generators or transfer switches. Copy must stay on the
  testing/maintenance side of that line.
- A **street address**. Home-based, service-area business. Locality only: "Middletown, NJ".
- Paul's **personal name**, an owner bio or a founder photo.
- **`paul@bluecollarlabs.org`** — that's the 501(c)(3)'s address and cannot appear on a
  for-profit site.
- **Photos of work that hasn't been done.** There are no real job photos yet. The site is
  deliberately type-driven with no hero image; AI-generated hero art was rejected as
  obviously AI. Drop in real photos off Paul's phone once service Saturdays start.
- **"First month free"** — that's a direct-mail offer and its deadline isn't set.

**Currently blank on purpose:** business email (none exists), logo (wordmark only for now;
designer brief lives in the Blackout workspace), job photos, any payment link. Signup is
phone only — Stripe links don't exist yet, so there are no checkout buttons to fake.

## Facts as published

- Name: **Middletown Generator Service** (domain intentionally `middletowngenerators.com` — the mismatch is Paul's call, don't "fix" it)
- Phone: **(908) 476-9612**, always labelled **"Call or text"**
- Area: Middletown, NJ 07748 primary; "Serving Monmouth County" secondary.
  Rumson / Fair Haven / Little Silver are round-two mail targets, **not** committed service
  towns — no town pages for them yet.
- Pricing: Basic **$29/mo**, Priority **$39/mo**, both 12-month terms; prepay **$399/yr** for Priority
- First service visit: 60–90 days after signup

## Deploy

Target is GitHub Pages. `gh` (v2.101.0) is authenticated as **Paulman2** with `repo` +
`workflow` scope, so this needs no new credentials.

**There is no working `git` on this Mac.** `/usr/bin/git` is the Xcode shim, and there is no
Homebrew, no Command Line Tools and no Xcode. `python3` is the same stub — to serve locally,
use a small `node:http` script, not `python3 -m http.server`. So deploys go through the
**GitHub Contents API via `gh api`**, not `git push`:

```sh
gh repo create Paulman2/middletowngenerators --public -d "Middletown Generator Service"
# then for each file, base64 the content and PUT it:
gh api -X PUT /repos/Paulman2/middletowngenerators/contents/index.html \
  -f message="…" -f content="$(base64 -i index.html)"
# then enable Pages on main / root:
gh api -X POST /repos/Paulman2/middletowngenerators/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

A **project** Pages site serves at `https://paulman2.github.io/middletowngenerators/`, i.e.
under a subpath — which is why internal links are `./`, not `/`. Don't change them back.

**Preview before approval:** add `<meta name="robots" content="noindex">` to `index.html`
for the preview deploy and strip it once Paul approves, so an unreviewed site never gets
crawled under the business name.

Domain is registered at **Cloudflare** (bought 2026-10-22) with Cloudflare nameservers and
**no A record yet**. To put it on the real domain:

1. Point DNS in Cloudflare: four `A` records for the apex to GitHub's Pages IPs
   (`185.199.108.153`, `.109.153`, `.110.153`, `.111.153`), plus a `www` CNAME to the
   `github.io` host. Set those records to **DNS only** (grey cloud), not proxied, or the
   Pages certificate won't issue.
2. **Only after DNS resolves**, add a `CNAME` file containing `middletowngenerators.com`
   and enable Enforce HTTPS. Adding it earlier makes the `github.io` preview URL redirect
   to a domain that isn't live yet.
3. Once it's on the apex domain, the `./` links and the absolute URLs in `sitemap.xml`,
   `robots.txt` and the JSON-LD all line up correctly with no further edits.

## Still open

- Confirm insurance coverage before any "Insured" claim ever goes up
- Business email, so the site isn't phone-only forever
- Logo (wordmark is standing in; it must never block launch)
- Real job photos
- Stripe links, which would turn "Call or text to start" into actual checkout
- Google Business Profile as a service-area business, with NAP matching this site exactly
