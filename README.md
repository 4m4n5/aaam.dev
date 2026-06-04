# aaam.dev — studio website

Public-facing website for **aaam.dev**, hosted on **Cloudflare Pages**.
Each app gets its own subdirectory (e.g. `/hum/`).

---

## Live URLs

| URL | What |
|---|---|
| `https://aaam.dev` | Studio landing page |
| `https://aaam.dev/hum/` | Hum - rituals marketing page |
| `https://aaam.dev/hum/support.html` | Hum support + FAQ + account deletion |
| `https://aaam.dev/hum/privacy.html` | Hum privacy policy |
| `https://aaam.dev/tir/` | tir - word race marketing page |
| `https://aaam.dev/tir/support.html` | tir support + FAQ |
| `https://aaam.dev/tir/privacy.html` | tir privacy policy |

Legacy `/humm/*` URLs 301-redirect to `/hum/*` via `_redirects`. Update
App Store Connect / Play Console for the **hum** app to use the new
`/hum/...` URLs (the old ones still work, but App Review may eventually
flag the redirect chain).

---

## Accounts & infrastructure

| Service | Account | Details |
|---|---|---|
| **Domain registrar** | Cloudflare | Domain: `aaam.dev`, registered 2026-05-08 |
| **Email routing** | Cloudflare Email Routing | `support@aaam.dev` → `aamd3v@gmail.com` |
| **Website hosting** | Cloudflare Pages | Project: `aaam-dev`, repo: `4m4n5/aaam.dev` |
| **DNS** | Cloudflare (automatic) | Managed via the same Cloudflare account |
| **Gmail (team)** | `aamd3v@gmail.com` | Receives all `support@aaam.dev` email |
| **GitHub repo** | `github.com/4m4n5/aaam.dev` | Public, deploys automatically on push to `main` |

### Apple Developer (individual account)

| Field | Value |
|---|---|
| **Seller / legal name** | Aman Shrivastava |
| **Apple Developer Team ID** | D92AD98B9B |
| **Contact email (App Review)** | `support@aaam.dev` |

### Hum - rituals (first app)

| Field | Value |
|---|---|
| **App Store Connect ID** | `6763845513` |
| **Bundle ID** | `com.humtum.app` |
| **EAS Project ID** | `30c5357d-ddd0-490b-a158-fd22c872392e` |
| **App repo** | `github.com/4m4n5/humm` |
| **Support URL** | `https://aaam.dev/hum/support.html` |
| **Privacy URL** | `https://aaam.dev/hum/privacy.html` |
| **Marketing URL** | `https://aaam.dev/hum/` |

---

## Directory structure

```
aaam.dev/
├── index.html              ← studio landing page
├── robots.txt              ← allows all crawlers (incl. AI bots) + sitemap ref
├── sitemap.xml             ← all 7 pages, for Search Console / Bing
├── llms.txt                ← plain-text site summary for AI answer engines
├── hum/                    ← Hum - rituals store pages (dir is `hum/`; brand is "hum")
│   ├── index.html          ← marketing / product overview
│   ├── support.html        ← support + FAQ + account deletion
│   ├── privacy.html        ← privacy policy
│   ├── style.css           ← shared stylesheet (dark theme)
│   └── images/             ← screenshots, og-cover
└── <future-app>/           ← next app goes here
```

---

## SEO / discoverability

Every page is statically optimized — no build step needed.

- **JSON-LD structured data** (`<script type="application/ld+json">` in each
  `<head>`): the landing page carries `Organization` + `WebSite` + an
  `ItemList` of both apps; each app page carries a `MobileApplication`
  (`tir` is co-typed `VideoGame` + `MobileApplication` so it qualifies for
  Google's app rich result); support pages carry `FAQPage`; sub-pages carry
  `BreadcrumbList`. Entities link via `@id` to `https://aaam.dev/#org`.
- **`robots.txt` / `sitemap.xml` / `llms.txt`** live at the repo root.
- **iOS Smart App Banner** (`<meta name="apple-itunes-app">`) on each app page.
- **No fake `aggregateRating`.** Add real star ratings to the
  `MobileApplication` schema *only* once the apps have genuine App Store
  reviews — fabricated ratings violate Google's guidelines and can earn a
  manual penalty.

**Manual follow-ups (off-page, can't be done in this repo):**

1. Submit `https://aaam.dev/sitemap.xml` in
   [Google Search Console](https://search.google.com/search-console) and
   [Bing Webmaster Tools](https://www.bing.com/webmasters).
2. Validate the structured data with the
   [Rich Results Test](https://search.google.com/test/rich-results) and the
   [Schema.org Validator](https://validator.schema.org/) after each deploy.
3. Earn real editorial backlinks (the legitimate version of what paid
   "press" vendors sell). Once the apps have App Store ratings, add an
   `aggregateRating` to each app's JSON-LD for star rich results.

---

## How to add a new app

1. Create a new directory at the root: `mkdir <app-name>`
2. Add `index.html`, `support.html`, `privacy.html`, `style.css`
3. Add a card to the root `index.html` linking to `/<app-name>/`
4. Push to `main` — Cloudflare Pages auto-deploys
5. Update App Store Connect / Play Console URLs to `https://aaam.dev/<app-name>/...`

---

## How deployment works

- **Automatic**: every push to `main` triggers a Cloudflare Pages build
- **No build step**: the site is static HTML/CSS, deployed as-is
- **Build output directory**: `/` (root of the repo)
- **Custom domain**: `aaam.dev` (configured in Cloudflare Pages → Custom domains)
- **SSL**: automatic via Cloudflare

---

## How email routing works

- Cloudflare Email Routing is enabled on the `aaam.dev` domain
- `support@aaam.dev` forwards to `aamd3v@gmail.com`
- To reply *as* `support@aaam.dev`, set up "Send mail as" in Gmail:
  1. Gmail → Settings → Accounts → "Send mail as" → Add another email
  2. Enter `support@aaam.dev`, uncheck "Treat as an alias"
  3. SMTP server: `smtp.gmail.com`, port 587, your Gmail credentials
  4. Verify via the confirmation email

---

## Local preview

```bash
cd /path/to/aaam.dev && python3 -m http.server 8765
```

Open `http://localhost:8765` for the landing page,
`http://localhost:8765/hum/` for the Hum pages.

---

*aaam.dev · established may 2026*
