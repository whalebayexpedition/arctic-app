# Greenland Sailing 2026 - Web App

## What is this?

A premium mobile-first web app for **Whalebay Expeditions' 2026 Greenland Arctic sailing trips**. Converted from a high-end PDF brochure into a single-page scrolling website, optimized for **WeChat's in-app browser** (primary audience is Chinese luxury travelers).

Chinese name: 帆向格陵兰 · 2026北极私属远征

## Tech Stack

- **Pure static site** — single `index.html` with all HTML, CSS, and JS inline (no build step, no frameworks)
- **63 JPEG images** in `assets/` extracted from the original PDF brochure
- **Fonts:** OPPO Sans (body, via 3rd-party CDN), Noto Sans SC (headings), Noto Serif SC (diary sections), Cinzel (English decorative)
- **No JavaScript frameworks** — vanilla JS only, for WeChat browser compatibility

## Hosting & Deployment

- **Cloudflare Pages** project name: `greenland`
- **Account:** Whalebayexpedition@gmail.com (Account ID: `30ebb84974d1bfa09da734203afdbb43`)
- **Live URL:** https://greenland-b5t.pages.dev
- **Custom domain:** `whalebay.world` (registered at GoDaddy, DNS via Cloudflare)
  - Nameservers: `olivia.ns.cloudflare.com` / `razvan.ns.cloudflare.com`
- **Auto-deploy:** Connected to this GitHub repo — push to `main` triggers deployment

### Manual deploy (if needed)

```bash
npm install -g wrangler
wrangler login          # authenticate to Whalebayexpedition@gmail.com
wrangler pages deploy . --project-name greenland
```

### Local testing

```bash
python -m http.server 9000
# Open http://localhost:9000
```

## Key Design Decisions

- **Single HTML file** — everything inline for simplicity; no bundler needed
- **Base font size:** `html { font-size: 18px }` — optimized for mobile reading
- **Color scheme:** Dark ocean (`#0a0f1a`), gold accents (`#c8a86e`), red sail (`#c0392b`)
- **CSS custom properties** defined in `:root` for the full color palette
- **Intersection Observer** drives fade-in animations (`.fade-in` class)
- **Loading screen** with progress bar hides until fonts + hero image are ready
- **WeChat-specific:** `-webkit-overflow-scrolling: touch`, no Service Workers, `tel:` links, QR code with white padding for long-press recognition

## File Structure

```
greenland-app/
├── index.html          # The entire app (HTML + CSS + JS inline, ~55KB)
├── CLAUDE.md           # This file — Claude Code project context
├── DEPLOY-NOTES.md     # Detailed deployment & font notes (bilingual)
└── assets/             # 63 JPEGs from the PDF
    ├── page1_img1.jpeg # Cover: red sailboat + iceberg (also og:image)
    ├── page24_img3.jpeg # WeChat QR code
    └── ...
```

## Important Notes & Gotchas

1. **og:image uses relative path** — currently `assets/page1_img1.jpeg`. Once `whalebay.world` domain is active, update to absolute URL: `https://whalebay.world/assets/page1_img1.jpeg` (same for `twitter:image`)

2. **OPPO Sans font is from a 3rd-party CDN** (`db.quike.com.cn`) — may go down. Self-hosting backup plan is in DEPLOY-NOTES.md. The font files could be downloaded to `assets/fonts/`.

3. **Google Fonts for China** — if `fonts.googleapis.com` is slow in China, swap to `fonts.googleapis.cn` mirror (just change the `<link>` href).

4. **Windows zip backslash bug** — PowerShell's `Compress-Archive` creates zips with backslash paths (`assets\img.jpeg`) which Cloudflare Pages can't resolve. Always use `wrangler pages deploy` (the CLI) or create zips via .NET `ZipFile` API with explicit forward slashes.

5. **WeChat JS-SDK** — share card hooks are pre-wired in the `<script>` block at the bottom of index.html but require backend signature generation (appId + appSecret). See DEPLOY-NOTES.md section 1.1.

6. **All content is in Chinese** (zh-CN) — the brochure targets high-net-worth Chinese travelers for a private arctic sailing expedition (8 seats per trip, 4 departures in 2026).
