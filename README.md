# karaboga.dev

Personal site of **Cihat Karaboğa** — mobile software engineer, Türkiye.

A single static page. `index.html` is self-contained — inline CSS and a small
inline script, no build step. Built for speed and SEO with semantic HTML,
Open Graph + Twitter cards, JSON-LD structured data, a sitemap and a manifest.
Type is Geist and Geist Mono (Google Fonts) with system-font fallbacks.

Sections: hero, selected work, experience, stack, about.

## Themes and language

Light/dark follows the system preference and is overridable from the header;
the choice is stored in `localStorage` and applied before first paint.

The page ships in English so crawlers index a complete English document.
Turkish is applied on top from an inline dictionary keyed by `data-i18n`
attributes — every value is plain text set through `textContent`. Language
resolution order is `?lang=` → `localStorage` → `navigator.language` → English,
and switching rewrites `?lang=` so a Turkish link is shareable. There is no
`hreflang`: both languages live at the same URL, so advertising them as
alternates would be a duplicate signal rather than a useful one. Separate
`/tr/` pages would be the fix if real bilingual SEO is ever needed.

To add or change a string, add the `data-i18n` attribute to the element and the
matching key to the `STRINGS.tr` object — the English side is snapshotted from
the markup at load, so it never needs a second dictionary.

## Develop locally

Serve the folder so root-absolute paths (`/favicon.svg`, `/og.png`, …) resolve:

```bash
python3 -m http.server 8080
# → http://localhost:8080
```

## Deploy

### Docker / nginx

```bash
docker build -t karaboga-dev .
docker run --rm -p 8080:80 karaboga-dev
```

`nginx.conf` adds gzip, long-cache headers for static assets, and wires up the
custom `404.html`.

### GitHub Pages

Push to GitHub, then **Settings → Pages → Source: `main` / root**. Set the
custom domain to `karaboga.dev` (this adds a `CNAME` file).

## Files

| File | Purpose |
|------|---------|
| `index.html` | The page — markup, inline CSS, SEO metadata, JSON-LD |
| `404.html` | Themed not-found page |
| `og.png` | 1200×630 social share image |
| `favicon.svg`, `favicon.ico`, `apple-touch-icon.png`, `icon-*.png` | Icons |
| `manifest.webmanifest` | PWA manifest |
| `robots.txt`, `sitemap.xml` | Crawler directives |
| `nginx.conf`, `Dockerfile` | Container deployment |
| `og.html` | Build source for `og.png` — not deployed |

## Regenerate the OG image

`og.png` is a 1200×630 render of `og.html`. After editing `og.html`, re-render
and commit `og.png` — the `Dockerfile` `COPY`s it directly:

```bash
chrome --headless --window-size=1200,630 --force-device-scale-factor=1 \
  --hide-scrollbars --virtual-time-budget=5000 \
  --screenshot=og.png "file://$PWD/og.html"
```

On macOS, `chrome` is `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`.
