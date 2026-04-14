# Nuuk · 北极追光 — page design spec

**Status:** approved for implementation plan
**Date:** 2026-04-14
**File target:** `arctic-app/nuuk/index.html` (single-file inline HTML)
**Deploy target (future, out of scope for this task):** `arctic.whalebay.world/nuuk`

---

## 1 · Context

Whalebay Expeditions sells a Nuuk Fjord autumn voyage — 13 total days with a 5-night ship-stay aboard the **Tulu**, a family-run Inuit expedition boat (father = captain, mother = cook, son = first mate). Dates target **September–October**, positioned as an aurora-chase product during the polar-night shoulder when days are still long enough for hiking and kayaking.

This page is one chapter of a larger brand structure:

```
whalebay.world                           → Brand portal + summer Greenland product
arctic.whalebay.world                    → Arctic landing (TODO, separate task)
arctic.whalebay.world/nuuk               → THIS PAGE
arctic.whalebay.world/ilulissat          → TODO
arctic.whalebay.world/wrangel            → TODO
antarctica.whalebay.world                → Antarctica landing + sub-pages (in progress)
```

Siblings (`greenland-app`, `antarctica-app`) share an established design system: single-file inline HTML, WeChat-optimized, Chinese-primary audience, ~15–20 sections each, alternating dark/light/full-bleed photo rhythm, `航行日记` diary sections.

The Nuuk page intentionally **departs** from the siblings on two axes:
1. **Shorter** (~10 sections vs ~20) — an editorial photo-essay, not a full brochure.
2. **Different palette and display typography** — pink-lilac / amber / aurora emerald / 囍 rust with Noto Serif SC headlines, where siblings use red-sail/gold or navy/gold with Noto Sans SC headlines.

These departures give Nuuk its own identity while staying in the family.

## 2 · Goals

1. Make a visitor *feel* an autumn Nuuk voyage within 90 seconds of landing on the hero.
2. Establish the Inuit family-boat differentiator as the emotional peak.
3. Communicate the real itinerary (from PPTX `20251001_努克_yifu妈妈专供`) without turning into a brochure.
4. Credit the guide (姚雪霏) and the authenticity voice ("本页照片全部为我们自己所摄").
5. Convert inquiry via contact block + WeChat QR (no published prices).
6. Work reliably in WeChat's in-app browser.

## 3 · Non-goals

- No summer product content on this page — summer Greenland lives on `whalebay.world` and is linked once at the bottom.
- No published prices, no booking engine, no date picker — inquiry-only, matching the sibling pattern.
- No WeChat JS-SDK signing / share card backend (skip until a real backend exists).
- No video backgrounds (hostile to WeChat data users).
- No English version (zh-CN primary, matches siblings).
- No framework, no build step, no component libraries.
- No deployment on this task — local preview at `file://` or `python -m http.server` only.

## 4 · Title and positioning

- **Main title:** `北极追光`
- **Subtitle:** `格陵兰 · 纳努克峡湾`
- **Tagline stack:** `13 天 · 船宿 5 夜 · 8–10 家人密友` / `2026 秋 · 限定期程 · 追光大年` / `ARCTIC · GREENLAND · 2026`
- **Brand parent line:** `鲸湾探索 · WHALEBAY EXPEDITIONS`

Rationale: verbs + places matches sibling naming (`帆向格陵兰`, `纵帆南极`) but `北极追光` pushes `北极` first per user direction and keeps `格陵兰` for the subtitle. `追光大年` is lifted verbatim from the PPTX selling line.

## 5 · Architecture

**Single-file static HTML**, same pattern as siblings:

```
arctic-app/nuuk/
├── index.html          # Entire app — HTML + CSS + JS inline, ~1100 lines target
└── assets/             # Existing 130+ photos, curated to ~25 used on-page
```

- **Fonts:** OPPO Sans (body, same 3rd-party CDN as greenland-app), Noto Serif SC (headlines — the Nuuk departure), Noto Sans SC (secondary/tagline), Cinzel (English decorative captions)
- **CSS:** inline, uses `:root` custom properties for the 8-color palette, `@font-face` block same as greenland-app for OPPO Sans
- **JS:** ~80 lines total — loading screen hider, IntersectionObserver for `.fade-in`, one toggle for aurora shimmer pause when off-screen
- **No dependencies:** no bundler, no npm, no framework
- **Base font size:** `html { font-size: 18px }` (mobile-optimized, same as siblings)
- **Viewport:** mobile-first; desktop is 1 column with max-width 680px centered, same as siblings

## 6 · Visual identity

### Palette (`:root` CSS custom properties)

| Token | Hex | Name | Role |
|---|---|---|---|
| `--lilac` | `#E7B6C4` | 黄昏紫 Dusk Lilac | hero accent, aurora afterlight, dawn transitions |
| `--amber` | `#D89A4E` | 苔原金 Tundra Amber | primary warm accent, gold-line dividers, eyebrows, section numbers |
| `--emerald` | `#6FBF8E` | 极光碧 Aurora Emerald | aurora section only, use sparingly |
| `--slate` | `#4A6A7C` | 峡湾青 Fjord Slate | body text on dark, secondary backgrounds |
| `--indigo` | `#0E1620` | 极夜蓝 Polar Indigo | dominant dark background (hero, diary, aurora, family) |
| `--rust` | `#9A4A35` | 囍红 Lantern Rust | family section ONLY — 囍 charm, red lantern accent |
| `--bone` | `#F2EBE1` | 骨白 Warm Bone | light-section background (tundra, sites, contact) |
| `--sand` | `#D9C8A5` | 驯鹿沙 Reindeer Sand | light-section secondary, muted dividers |

Plus inherited from siblings: `--text-dark`, `--text-body`, `--text-light`, `--text-muted`, `--divider` tokens matching greenland-app's `:root`.

### Typography

- **Hero display (H1):** Noto Serif SC weight 200, letter-spacing 0.22–0.25em — this is the Nuuk-specific departure from sibling sans displays
- **Section titles (H2):** Noto Serif SC weight 300
- **Subtitles (H3):** Noto Sans SC weight 300, amber color
- **Body:** OPPO Sans weight 400, line-height 1.95, color slate/body on light, bone on dark
- **Diary (手记):** Noto Serif SC weight 300, line-height 2.0, color bone on dark indigo
- **Captions:** Cinzel regular, 10–11px, letter-spacing 0.2em, uppercase, amber color

## 7 · Section-by-section spec

Ten sections, ordered for emotional arc. Each is ~1 scroll-screen on mobile. Section-number chips follow the antarctica-app convention (`.section-number` / `.section-number-light`).

### Section 00 · Hero — `data-section="0"`

- **Class:** `.hero.section`
- **Background:** `assets/Aurora.jpg` full-bleed, darkened 45% via overlay
- **Content layout:** centered vertical flex, min-height 100dvh
- **Brand top:** `鲸湾探索` (Noto Sans SC 300) / `Whalebay Expeditions` (Cinzel)
- **H1:** `北极追光` (Noto Serif SC 200, 0.25em tracking, #fff)
- **Subtitle:** `格陵兰 · 纳努克峡湾` (Noto Serif SC 300, lilac color)
- **Tagline stack (3 lines):** see section 4
- **Year marker:** `ARCTIC · GREENLAND · 2026` (Cinzel, amber, small)
- **Scroll indicator:** 1px amber vertical line, 60px, with amber dot

### Section 01 · Full-bleed opening — `data-section="1"`

- **Class:** `.full-image-section.section`
- **Background:** `IMG_20251010_185027_edit_44860496520757.jpg` (pink-lilac sunset through wheelhouse window)
- **Overlay gradient:** bottom-heavy, `linear-gradient(180deg, rgba(14,22,32,0.1) 0%, rgba(14,22,32,0.5) 50%, rgba(14,22,32,0.9) 100%)`
- **No section number**
- **Floating text**, anchored bottom 25%, centered, Noto Serif SC 300, white:
  > 九月，极光已至。<br>十月，峡湾未封冻。<br>一年里最短的窗口，最长的追光。
- **Bottom-right caption:** `* 纳努克峡湾 · October 2025` (Cinzel)

### Section 02 · 航行日记 01 · 在纳努克醒来 — `data-section="2"`

- **Class:** `.section.dark-section`
- **Section number:** `01` (light variant)
- **H2-like label:** `航行日记 · 01`
- **Diary frame** following antarctica-app `.diary-entry` structure:
  - `.diary-date`: `航行日记 ︱ 2025-10-03 · 纳努克 Nuuk`
  - `.diary-location`: `纳努克峡湾 · 清晨的港口`
  - `.diary-text`: ~150 字 first-person diary. Beats to hit:
    - Morning light hitting the harbor from an apartment window (IMG_20250928_065642.jpg visual)
    - The Tulu tied up below, yellow hull in grey dawn
    - The 囍 charm already hanging in the guest cabin before you board — a detail noted but not yet explained
    - Closing line that sets up the aurora night
  - `.diary-author`: `鲸湾探索 · 姚雪霏 · 极地向导`
- **Image below text:** `IMG_20250928_065642.jpg` class `image-rounded fade-in fade-in-delay-2`

**TODO marker:** the 150-word diary text is placeholder until user supplies or approves. Leave `<!-- CONTENT-TODO: diary-01 -->` in the HTML.

### Section 03 · 追光之夜 · Aurora Nights — `data-section="3"`

- **Class:** `.full-image-section.section`
- **Background:** `assets/Aurora.jpg` (or `aurora ZS.jpg` as alternate)
- **Ambient shimmer:** CSS `@keyframes` slow gradient pan on a pseudo-element, 8s loop, respects `prefers-reduced-motion`
- **Section number:** `02`
- **H2:** `追光之夜` (Noto Serif SC 300, white)
- **Three-movement poem** in three stacked blocks:
  - `一 · 船长的敲门声`
  - `二 · 甲板上的羽绒服与惊叹`
  - `三 · 绿帘子落下，没有人说话`
- **Attributed quote**, amber rule above:
  > 「今年是极光大年，追光成功概率极高。」
  > — 姚雪霏 · 极地向导
- **Photo strip bottom:** 3 thumbnails (aurora variations + night deck if available)

### Section 04 · 金色苔原 · Golden Tundra — `data-section="4"`

- **Class:** `.section.content-section` — **MAJOR TONAL BREAK**, first light section after three dark ones
- **Background:** `--bone`
- **Section number:** `03`
- **H2:** `金色苔原`
- **Gold line divider**
- **Body** (~100 字): autumn tundra, 蓝莓 / 岩高兰 / 鹿蹄草 / 驯鹿, hiking above the fjord, the hours before the aurora returns. Factual + sensory, not poetic.
- **Large image:** `IMG_20251002_174204.jpg` (wooden swing set on tundra), full content-width, rounded
- **3-column strip below** (stacks on mobile):
  - `徒步 · 苔原高处看峡湾` (hiking)
  - `手钓 · 船边静水` (fishing)
  - `冰峡湾 BBQ · 甲板烤炉` (BBQ in icefjord)
- Each with 1 photo + 1 short line

### Section 05 · 囍 · 远方的家 — `data-section="5"`

- **Class:** `.section.dark-section` — the page's emotional peak
- **Section number:** `04`
- **H2:** `囍 · 远方的家`
- **Subhead:** `一艘船，一个家。`
- **Body paragraph 1** (~80 字): introduce the family — the Inuit father captains, the mother runs the galley, the son is first mate. Restrained tone, no exoticizing. Frame as "a working family who has been hosting guests in their own boat for years."
- **Body paragraph 2** (~80 字): the 囍 detail + food intimacy:
  > 船上有中国的辣椒面，老干妈，泡面和酸辣粉（数量有限）。<br>
  > 客舱窗边挂着红灯笼和金鱼剪纸 —— 去年某位客人留下的。<br>
  > 老船长没摘下来。
- **Hero image:** `IMG_20251002_132109.jpg` (the Chinese-New-Year-decorated cabin) — this photo stops the scroll
- **囍红 accent:** one thin rust-colored vertical rule next to the image, or a small 囍 mark as a list bullet

**TODO marker:** `<!-- CONTENT-TODO: family-paragraph — confirm wording with user before publish -->`

### Section 06 · 七日简谱 · Seven-Day Score — `data-section="6"`

- **Class:** `.section.dark-section`
- **Section number:** `05`
- **H2:** `七日简谱`
- **Subhead:** `从哥本哈根出发，回到哥本哈根。十一天，六夜船宿。`
- **Copenhagen bookend top strip**: `Day 1–2 · 哥本哈根 · 落地休整` (small, muted)
- **Day ribbon** — 7 rows for the Nuuk portion (Days 3–9 from the PPTX):

  | Day | Date | Place | Single-line description |
  |---|---|---|---|
  | 03 | 10.03 | 登船 · Qoornoq 无人村 | 船长欢迎晚宴，北极冰海 |
  | 04 | 10.04 | Qoornoq · 冰峡湾 | 无人村徒步，甲板BBQ，冰川 |
  | 05 | 10.05 | Sulussugutip 峡湾 | 因纽特野外射击课程 |
  | 06 | 10.06 | Qooqqut 峡湾 | 徒步与手钓，隐秘峡湾餐厅 |
  | 07 | 10.07 | 返航努克 | 告别午餐，入住酒店 |
  | 08 | 10.08 | 努克市内 | 博物馆，海边栈道，渔夫市场 |
  | 09 | 10.09 | 飞返哥本哈根 | GL782 GOH→CPH |

- Each row: left = `DAY 03` (Cinzel) + `10.03` (month.day, i.e. October 3rd) / middle = place name (Noto Serif SC 400) / right = 1-line description
- Amber divider between rows
- **Copenhagen bookend bottom strip**: `Day 10–11 · 哥本哈根 · 回国 · CA878` (small, muted)

Dates are indicative and reference the 2025 pilot run. Copy near the ribbon should state `示意日程 · 以实际出发为准` or equivalent — actual 2026 departures TBD.

### Section 07 · 峡湾图景 · Fjord Sites — `data-section="7"`

- **Class:** `.section.content-section`
- **Background:** `--bone`
- **Section number:** `06`
- **H2:** `峡湾图景`
- **Map graphic:** inline SVG, simplified Greenland west coast with Nuuk pin + 4 site pins connected by a dotted route line. Style: thin amber strokes on warm bone, similar to antarctica-app map section.
- **Sites grid** (2×2 on mobile, 4×1 desktop cards):
  1. `Qoornoq · 无人村` — abandoned settlement, colorful houses, hiking
  2. `Sulussugutip 峡湾 · 因纽特野外射击课` — wildlife tracking, hunting tradition
  3. `Qooqqut 峡湾 · 隐秘餐厅` — hike and fish, fjord restaurant meal
  4. `Sermitsiaq 神山 · 老鹰之巢` — sacred mountain, captain's secret eagle nest
- Each card: 1 photo + name + 1-line description

**TODO marker:** `<!-- CONTENT-TODO: confirm 4 site photos from nuuk/assets/ -->`

### Section 08 · 九月·十月 · Why Now — `data-section="8"`

- **Class:** `.section.midnight-section` (the darker variant from antarctica-app)
- **Section number:** `07`
- **Background:** `IMG_20251003_083946.jpg` (pink-dawn porthole shot) as the `background-image`, with a dark indigo overlay at 80% opacity on top so only ~20% of the image shows through
- **H2:** `为何是九月十月`
- **Three-column ribbon** (stacks on mobile):
  - **白昼** — 10–12 hours daylight, hiking navigable, autumn tundra at peak color
  - **黑夜** — aurora KP window active, 追光大年
  - **冰况** — fjord navigable, no ice lockout, first snow rare
- **Closing line**, large:
  > 夏日喧哗已散，冬日门扉未闭。<br>只有这两个月。

### Section 09 · 鲸湾 · Contact — `data-section="9"`

- **Class:** `.section.content-section`
- **Background:** `--bone`
- **Section number:** `08`
- **H2:** `与我们同行`

**Guide profile block:**
- Photo of 姚雪霏 (placeholder, TODO)
- Name: `姚雪霏 · Yao Xuefei`
- Role: `探险向导 · 野生动物学者`
- Bio (3 lines from the PPTX):
  - 牛津大学动物生态学博士，专注喜马拉雅雪豹与高寒生态系统
  - 2010 年起参与南北极探险，担任野生动物向导、冲锋舟驾驶员、急救与救援人员
  - 科考地区：藏北、珠峰、帕米尔、昆仑山、祁连山、阿尔金山

**Contact column:**
- WeChat QR (placeholder, **TODO from user**)
- `问询 · whalebayexpedition@gmail.com`
- Photo credit voice line: `本页照片全部为我们自己所摄，大部分出自一部手机。`

**Inclusion strip** — placed as a single full-width row below both the guide profile and the contact column. Internally the strip is split into two sub-columns (`行程包含` left, `行程不含` right) that stack vertically on mobile.
- `行程包含` (left):
  - 哥本哈根 ↔ 格陵兰 航班
  - 哥本哈根 机场酒店
  - Tulu 探险船独家使用（含船员与港口税费）
  - 探险向导 2 位
  - 格陵兰境内酒店、船宿、餐饮
  - 讲座与户外活动
  - 境外旅行人身保险、延误取消险
- `行程不含` (right):
  - 国内 ↔ 丹麦 航班
  - 签证相关费用
  - 当地船员小费（€20/天/人，全程 €160/人）

**Outbound sibling link**, at the very bottom:
- `夏日峡湾 · 另一面纳努克 →` → `https://whalebay.world`

### Footer
- `© 2026 鲸湾探索 · Whalebay Expeditions`
- 4 small sibling links: `whalebay.world` · `arctic.whalebay.world` · `antarctica.whalebay.world` · `鲸湾探索微信`
- Minimal, muted, one line

## 8 · Motion & interactions

**Inherited from siblings:**
- Loading screen + progress bar, hides once fonts + hero image ready
- IntersectionObserver-driven `.fade-in` with `.fade-in-delay-1/2/3` staggers
- Smooth scroll, no scroll-jacking
- WeChat compat: `-webkit-overflow-scrolling: touch`, `format-detection`, `tel:` links
- `loading="eager"` hero, `loading="lazy"` everything else

**Nuuk-specific:**
- **Aurora ambient shimmer** — section 03 only. CSS `@keyframes` very-slow gradient pan, 8s loop. `prefers-reduced-motion` → disabled. IntersectionObserver pauses when off-screen.
- **Lilac-to-indigo transition** — sections 01 → 02 fade a CSS variable `--scroll-bg-tint` from lilac to indigo on scroll entry.
- **囍 section entry** — the cabin photo in section 05 scales from 1.08 → 1.0 on enter (1.2s ease-out), diary text fades in 400ms after.
- **Day-ribbon reveal** — section 06 rows stagger-fade on scroll, 80ms between rows, using existing `.fade-in-delay-*` classes.
- **Gold-line divider draw** — CSS `@keyframes` on `transform: scaleX(0 → 1)` when the parent enters viewport, 600ms ease-out.

**Explicit non-goals:**
- No GSAP / Framer Motion / anime.js
- No sticky header
- No modal dialogs
- No video backgrounds
- No hover parallax on mobile

## 9 · Assets used

Source folder: `arctic-app/nuuk/assets/` (already exists, 130+ photos). This spec commits to roughly 25 photos on-page. The actual selection happens during implementation.

**Confirmed / required:**
- `Aurora.jpg` — hero + section 03 background
- `aurora ZS.jpg` — section 03 photo strip
- `中山站极光.jpg` — section 03 photo strip
- `IMG_20251010_185027_edit_44860496520757.jpg` — section 01 full-bleed
- `IMG_20250928_065642.jpg` — section 02 harbor window
- `IMG_20251005_111712.jpg` — Tulu in ice (used somewhere, likely section 03 or 05 adjunct)
- `IMG_20251002_132109.jpg` — section 05 cabin hero
- `IMG_20251002_140332.jpg` — section 05 adjunct (alternate cabin shot)
- `IMG_20251002_174204.jpg` — section 04 tundra swing set
- `IMG_20251003_083946.jpg` — section 08 porthole dawn background
- `IMG_20250929_192837.jpg` — section 04 or 09 accent (amber fishing boat)

**TODO — user to supply or confirm:**
- WeChat QR code PNG (section 09, contact)
- Photo of 姚雪霏 (section 09, guide profile) — may already be in nuuk/assets/Camera_1040g* or similar, TBD
- Portraits of the Inuit family if available (captain/cook/son) — optional, section 05
- OG share-card image 1200×630 (meta tags, placeholder until supplied)

## 10 · WeChat / mobile compat

- `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">`
- `<meta name="format-detection" content="telephone=yes">`
- `<meta http-equiv="X-UA-Compatible" content="IE=edge">`
- OG and Twitter meta tags for share cards
- QR code will need white padding to survive WeChat long-press recognition (matches greenland-app gotcha)
- OPPO Sans font loaded from same 3rd-party CDN as greenland-app (known risk: CDN goes down — fallback to `Noto Sans SC`, `PingFang SC`, `Microsoft YaHei`)
- `zh-CN` lang attribute
- No Service Workers, no PWA manifest

## 11 · Out of scope (this task)

- Deployment (Cloudflare Pages, custom domain, DNS)
- Git init + commit of the arctic-app project (user may want to do this themselves)
- Parent page `arctic.whalebay.world` landing — separate task
- Sibling pages: ilulissat, wrangel — separate tasks
- WeChat JS-SDK share signature backend
- Translations / English version
- Date picker, booking engine, pricing publishing
- Analytics / tracking

## 12 · Success criteria

1. Opens in a browser via `file://` or `python -m http.server` and renders fully on mobile and desktop Chrome.
2. All 10 sections visible, scrollable, with the described content in place (copy can be placeholder where TODO is marked).
3. Palette and typography match the palette spec in section 6.
4. Aurora shimmer animation is visible but not performance-destructive on mid-tier mobile.
5. Page weight under 3 MB total (images should be appropriately sized — implementation plan will handle resizing strategy).
6. Page validates against WeChat in-app browser Web UA on at least one real phone check (user validation step).
7. No console errors, no broken image links.

## 13 · Open questions for implementation plan phase

1. **Image resizing** — the source photos in `nuuk/assets/` are mostly raw phone output (2–5 MB each). Implementation plan should decide whether to resize to a web-optimized `nuuk/assets/web/` variant or leave the source files and accept the page weight.
2. **姚雪霏 photo** — if not in existing assets, page ships with placeholder.
3. **Inuit family portraits** — if not available, section 05 uses only the decorated-cabin image without portraits. Acceptable and specified above.
4. **2026 departure dates** — the PPTX shows 2025 dates as historical reference. The page should say "2026 秋 · 限定期程" without specific dates until user supplies them.
5. **Commit / git init** — this task writes to `arctic-app/` which is currently not a git repo. User decides whether to init + commit the spec and resulting page.

---

**Spec author:** Claude (Opus 4.6) via superpowers brainstorming skill
**Next step:** user review → writing-plans skill for implementation plan
