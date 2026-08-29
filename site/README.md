# mybandbox.app

Static site. No build step, no dependencies, no JavaScript, no trackers.
Upload the contents of this folder as the site root.

```
index.html          /            one-screen card: what the app is + links
support/index.html  /support     App Store Connect "Support URL"
privacy/index.html  /privacy     App Store Connect "Privacy Policy URL"
terms/index.html    /terms       terms of service
schools/index.html  /schools     notes for districts about student information
404.html                         not-found page
assets/styles.css                the only stylesheet
assets/logolong.svg              wordmark
assets/logomark.svg              hexagon mark
assets/favicon.svg               favicon, in the accent colour
robots.txt, sitemap.xml
```

Clean URLs come from the folder-plus-`index.html` layout, so `/privacy` works on
Netlify, Vercel, Cloudflare Pages, GitHub Pages, and any Apache/nginx default —
no redirect rules needed.

## Why there is a root page at all

Apple does not require one. App Store Connect only requires a Privacy Policy URL
and a Support URL, and the Marketing URL is optional — leave it blank and App
Review never looks at the domain root.

Google does require one. Because the app offers Google sign-in, the OAuth client
is a production app, and Google requires a publicly accessible home page on a
domain you own that describes the app's functionality and links to the privacy
policy. Getting "BandBox" and the logo onto the consent screen instead of a bare
project identifier is brand verification, which checks that page.

So `index.html` is deliberately one screen: a description, the specs, and links.
It satisfies the Google rule without being a marketing site. If you ever want to
add a real landing page, that is the file to grow.

## Deploying

Netlify:

```bash
npx netlify-cli deploy --dir=site --prod
```

Cloudflare Pages:

```bash
npx wrangler pages deploy site --project-name=bandbox
```

GitHub Pages: serve this folder as the site root and add a `CNAME` file
containing `mybandbox.app`.

Whichever host: point `mybandbox.app` at it and make sure HTTPS is on before
submitting to App Review — Apple checks that the privacy and support URLs
resolve.

## Before submitting

- The app is iPhone-only. Set the device family in App Store Connect to iPhone
  and leave iPad off, so the listing matches what the pages say.
- Verify `mybandbox.app` in Google Search Console. Google will not complete OAuth
  consent-screen verification for a domain you have not proven you own.
- Bump the `Last updated 29 August 2026` line on `/privacy` and `/terms` if the
  copy changes.
- Leave the Marketing URL blank in App Store Connect unless you build out a real
  landing page.

## Editing

Each page is a standalone HTML file sharing one stylesheet. The floating nav and
the footer are repeated in each inner page — change the nav and you change it in
`support`, `schools`, `privacy`, `terms`, and `404.html`. The home page has
neither, by design.

Design tokens (colours, the `#ff5a3c` accent, spacing) live at the top of
`assets/styles.css` as CSS custom properties, with a dark-mode block underneath.
