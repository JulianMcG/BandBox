# mybandbox.app

The website for **BandBox**, a free iPhone app that helps school band directors
track school-owned instruments and uniforms with printed QR labels.

Static HTML. No build step, no dependencies, no JavaScript, no trackers.

## Layout

```
site/                 what gets deployed (Vercel outputDirectory)
  index.html          /            one-screen card: what the app is + links
  support/index.html  /support     App Store Connect "Support URL"
  privacy/index.html  /privacy     App Store Connect "Privacy Policy URL"
  terms/index.html    /terms       terms of service
  schools/index.html  /schools     notes for districts about student information
  404.html                         not-found page
  assets/styles.css                the only stylesheet
  assets/*.svg                     wordmark, hexagon mark, favicon
  robots.txt, sitemap.xml
brand/                original logo files
docs/                 working notes, not published
vercel.json           output dir, clean URLs, security headers
```

Clean URLs come from the folder-plus-`index.html` layout, so `/privacy` works on
Vercel, Netlify, Cloudflare Pages, GitHub Pages, and any Apache/nginx default.

## Local preview

```bash
cd site && python3 -m http.server 8080
```

Then open <http://localhost:8080>.

## Deploying with GitHub + Vercel

1. Create the repo and push:

   ```bash
   git remote add origin git@github.com:<you>/bandbox-website.git
   git push -u origin main
   ```

2. In Vercel, **Add New Project** and import the repo. Vercel reads
   `vercel.json`, so leave the framework preset on **Other** and leave the build
   command empty — `outputDirectory` already points at `site/`.

3. Add `mybandbox.app` under **Settings → Domains** and follow the DNS
   instructions. Vercel issues the TLS certificate automatically.

Every push to `main` redeploys. Pull requests get their own preview URL.

### What vercel.json does

- `outputDirectory: site` — publishes `site/`, so `brand/` and `docs/` stay out
  of the deployed site.
- `cleanUrls` + `trailingSlash: false` — `/privacy` rather than `/privacy/`,
  matching the `<link rel="canonical">` on each page.
- A strict `Content-Security-Policy`. The site loads one stylesheet and a few
  local SVGs and runs no JavaScript, so `default-src 'none'` is accurate. **If
  you ever add a script or an external font, the page will break until you widen
  this.**
- HSTS, `nosniff`, a referrer policy, and a `Permissions-Policy` that turns off
  camera, microphone, and geolocation — none of which the site uses.

## Before submitting to the App Store

- The app is iPhone-only. Set the device family in App Store Connect to iPhone
  and leave iPad off, so the listing matches what these pages say.
- Verify `mybandbox.app` in Google Search Console. Google will not complete OAuth
  consent-screen verification for a domain you have not proven you own.
- Bump the `Last updated` line on `/privacy` and `/terms` if the copy changes.
- Leave the Marketing URL blank in App Store Connect unless you build out a real
  landing page.

### Why there is a root page at all

Apple does not require one — App Store Connect only needs a Privacy Policy URL
and a Support URL. Google does require one: because the app offers Google
sign-in, Google wants a publicly accessible home page on a domain you own that
describes the app and links to the privacy policy. So `index.html` is
deliberately one screen. If you ever want a real landing page, that is the file
to grow.

## Editing

Each page is a standalone HTML file sharing one stylesheet. The floating nav and
the footer are repeated in every inner page — change the nav and you change it in
`support`, `schools`, `privacy`, `terms`, and `404.html`. The home page has
neither, by design.

Design tokens (colours, the `#ff5a3c` accent, spacing) are CSS custom properties
at the top of `assets/styles.css`, with a dark-mode block underneath.
