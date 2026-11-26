# CLAUDE MASTER WORDPRESS BUILD — Figma → Tokens → WordPress (any builder)

> **ROLE:** You are a senior front-end + PHP / WordPress theme developer.
>
> **PRIMARY TASK:** Build a production-ready, maintainable, secure, responsive WordPress site
> **directly from the Figma design** — no static HTML site is built first. The design is exported
> into `Design/`, every colour, font, size, spacing value, radius and breakpoint is normalised into
> an approved `TOKENS.md`, and the theme is built on that token system with the page builder the
> user chooses: **ACF PRO Flexible Content, Gutenberg (ACF Blocks), Both, Elementor Pro, or any
> other builder.**
>
> **CRITICAL:** Do NOT start building WordPress before the design is exported (A2) and the token
> set is approved (A6). Everything after that gate is built on those numbers.
>
> **ELEMENTOR PROJECTS:** Read the **⭐ Main Requirement in E5.1** first. The Elementor site must
> match the Figma design visually and functionally. That rule wins over everything else for
> Elementor builds.
>
> **SITE ALREADY BUILT, ONLY AN UPDATE?** Use **F4 Quick Update Mode** — read only the matching
> section and go straight to the point.
>
> **ELEMENTOR — LESSONS, BUG LIBRARY, SIGN-OFF:** E5.36–E5.39 apply to **every Elementor build**.
> The numbers in them (gaps 40/30/20, card padding 40/30/25/20, wave height 35–41 px, 2 hero
> slides, 4 s marquee…) are **examples from the Buell Recreation project** — on a new project take
> the values from the new design (`TOKENS.md`). The *rule* is what carries over.

**How to use it:**

```
Read claude-master-wordpress-build/CLAUDE-MASTER-WORDPRESS-BUILD.md and run it.
Figma: <paste figma.com URL — or put the design PDF in Design/>
```

---

## Project folder

```
<Project-Name>/
├── Design/                         filled by Claude from Figma (A2) — or the design PDF (A4)
│   ├── pages/  components/  images/  svg/
│   ├── CONTENT.md  ANIMATIONS.md  EXPORT-LOG.md
├── TOKENS.md                       approved token set (A6)
├── login.md                        the user's own site login (D110) — never commit, never print
└── <wordpress>/wp-content/themes/<project-slug>/   the theme (A7, B9)
```

---

## Contents

| Part | What it covers | Sections |
|---|---|---|
| **A** | Design → tokens → theme front-end system | A1 inputs · A2 Figma export · A3–A4 extract · A5 normalise · **A6 approval gate** · A7 theme assets · A8 style guide page · A9 front-end rules · A10 guardrails |
| **B** | WordPress core — theme structure, header, footer, menus, CPTs, blog, assets, security, SEO, performance, QA | B1–B72 |
| **C** | Existing theme — update / extend mode | C73–C95 |
| **D** | Shared features — one-click setup importer, CSS & global structure, images & media, 404 / search, sitemap, forms, ACF PRO + `login.md`, ACF local JSON, DeveloperAI meta tag, WooCommerce, favicon, linking | D96–D115 |
| **E** | Page builders — E1 choose · E2 ACF Flexible Content · E3 Gutenberg (ACF Blocks) · E4 Both · E5 Elementor Pro · E6 any other builder | E1–E6 |
| **F** | Verify against Figma · final QA · definition of done · quick update mode | F1–F4 |

## Build order (full build)

```text
A1  Ask the start questions ONE AT A TIME (project, pages, Figma, animation, builder, forms, Woo, theme, login)
A2  Export Figma → Design/ (pages, components, images, SVG, CONTENT.md, ANIMATIONS.md) — retry, never skip
A3  Extract values (or A4 from PDF)  →  A5 normalise  →  A6 TOKENS.md approval gate  ⛔ stop until approved
A7  Theme + front-end system (vendor.css, vendor.js, sliders.js, animations.js, enqueue)
A8  Style guide page
B64 WordPress development order (theme, header, footer, menus, CPTs, blog, pages …)
E1  Build sections with the chosen builder (E2 / E3 / E4 / E5 / E6)
D96 One-click setup importer seeded from Design/CONTENT.md + Design/images
F1  Verify every page against the Figma frames at 1600 / 900 / 375
F2  Final QA  →  F3 definition of done  →  report
```

**Do not skip A6.** A wrong type scale caught at A6 costs a minute; caught on page 6 it costs a day.

---

---

# A1. Inputs — ask one question at a time

Establish these before doing anything else.

**Ask questions one at a time (MANDATORY).** Never send a list of questions in one message — the
user needs time to think about each answer, and nothing gets missed.

1. Ask **one question per message**, numbered (*"Question 2 of 7"*), so the user knows how many are
   left.
2. Give the options, mark the default / recommended one, and add one line on what the answer
   changes (e.g. *"GSAP + ScrollTrigger — default"*).
3. **Wait for the answer** before asking the next question. If an answer raises a follow-up, ask that
   next, as its own question.
4. Skip a question only when the answer is already given (in the prompt, the project docs or
   `login.md`) — say in one line what you read instead of asking.
5. After the last answer, show **all answers as one short summary** and ask the user to confirm
   before starting. Write the confirmed answers into the project notes / architecture report.

The questions, in this order — **one per message**:

```text
Start questions (ask one, wait for the answer, then ask the next):

 1. Project name, and the theme slug (folder name under wp-content/themes/).
 2. Which pages are in scope?
 3. Design source: the Figma URL (figma.com/design/… or figma.com/file/…).
    No Figma access? Then the design exported as PDF / screenshots in Design/.
 4. Animation library: GSAP 3.12.5 + ScrollTrigger (default), another library, or none?
 5. How should editors build the pages?
      1. ACF PRO — Flexible Content   (page sections as Flexible Content layouts)
      2. Gutenberg — ACF Blocks       (every section is a Gutenberg block)
      3. Both                         (same sections as Flexible Content layouts AND blocks)
      4. Elementor Pro
      5. Other builder                (name + version, please)
 6. Which forms plugin?  1. Gravity Forms   2. Contact Form 7   3. Elementor Pro Form widget (Elementor builds only)
 7. Is WooCommerce part of this build? If yes: Cart and Checkout as
    Block (WooCommerce default) or Classic (shortcode)?
 8. New theme, or update / extend an existing theme on the site?
 9. Which WordPress site do we build on (URL)? Its admin login goes in login.md —
    please do not paste passwords in chat.
```

What each answer drives:

1. **Project name and theme slug.** Needed for the theme header, the PHP prefix, the style guide
   page (A8), the page index and `<title>`. The theme lives in `wp-content/themes/<slug>/` (A7.1).

2. **Pages in scope.** Seeds the style guide page index (A8) and the importer page list (D96–D102).

3. **Design source.** One of:
   - A `figma.com/design/…` or `figma.com/file/…` URL → use the Figma MCP tools.
   - A folder of exported screenshots / PNGs → read them with the image reader.
   - The design exported as **PDF** (all pages, usually in `Design/`) → read it with the PDF
     reader, max 20 pages per call; treat each PDF page as one artboard.
   - Both → prefer Figma for numbers, screenshots / PDF for layout intent.

   **Is the Figma MCP server connected — and does the link actually work?** Check the available
   tool list for `get_variable_defs`, `get_design_context`, `get_metadata`, `get_screenshot`, then
   make one test call (`get_metadata`) on the link.
   - **Connected and the test call succeeds** → A2 (export into `Design/`), then A3 (exact values,
     no guessing).
   - **Not connected** → A4 (measure from screenshots / PDF pages, then confirm every number with
     the user at A6). Say plainly that values are measured, not read.
   - **Connected but the link fails** (no access, wrong or missing node-id, rate limit, empty
     response) → **fall back to the PDF / screenshots in `Design/`** and go to A4. Tell the user in
     one line: *"The Figma link did not work (<reason>), so I am using the Design/ PDF — values are
     measured, not read."* If `Design/` is empty too, stop and ask for the PDF export. Never guess a
     design you cannot see.

4. **Animation library.** Default to **GSAP 3.12.5 + ScrollTrigger**, loaded from pinned cdnjs URLs
   and driven by `data-anim` attributes through `assets/js/animations.js` (exact URLs, attribute API
   and preset list are fixed in A7.4, A7.6 and A9.8 — use them verbatim). If the user names another
   library, use it **only** when they also supply its exact CDN URLs, attribute API and preset list
   (or its official docs); never invent a URL, attribute or preset name. If the user wants none,
   drop A9.8 (keep A9.8.1 sliders), and do not create or enqueue `animations.js`, GSAP or
   ScrollTrigger.

5. **Page builder.** Always asked, never assumed. ACF PRO Flexible Content → E2. Gutenberg (ACF
   Blocks) → E3. Both → E4. Elementor Pro → E5. Any other builder → E6 (ask for its name and
   version). Shared builder rules are in E1. Options 1–3 need **ACF PRO** — never install SCF (D110).

6. **Forms plugin.** Gravity Forms, Contact Form 7, or — on an Elementor build — the Elementor Pro Form widget (D109). Use only the chosen one for every form.

7. **WooCommerce.** Yes / no, and Cart + Checkout as **Block** or **Classic** (D113).

8. **New theme or existing theme.** New theme → Part B. Existing theme → update / extend mode (C73),
   which changes where files go (child theme, backup first).

9. **WordPress site + login.** The admin login for the user's site goes in **`login.md`** in the
   project folder (D110). Never ask for, print, copy or commit passwords. If a local server is needed
   and its details are not known, ask the user — `login.md` is only for the user's own site.

**Sliders are not a question — they are fixed by the builder chosen in E1.**
- **ACF Flexible Content / Gutenberg (ACF Blocks) / Both:** every slider, carousel or logo strip is
  built with **Slick 1.8.1** (needs **jQuery**), driven by `data-slider` attributes through
  `assets/js/sliders.js` (A7.5). **Swiper** may be used instead — one slider library per site, never
  both, initialised only in `sliders.js` and enqueued in the A7.6 order.
- **Elementor Pro / any other builder:** use the builder's own default slider / carousel widget.
  Never add a new slider JS (no Slick, no Swiper of our own).
- Never use Owl, Splide or hand-written slider JS.

---

# A2. Export the Figma design into Design/

When the Figma link works, export the design into `Design/` (in the project folder, next to
`login.md` and `TOKENS.md`) **before** extracting tokens (use `get_metadata` first to get the frame
tree). This folder is the local copy of the design for the whole build. It is the source of truth
for layout, images, icons, animations and text copy.

```
Design/
├── pages/        one full-frame PNG per page and breakpoint (home-desktop.png, home-mobile.png …)
├── components/   one PNG per component / variant (button-primary.png, card-service.png …)
├── images/       every raster image used in the design: photos, backgrounds, illustrations
├── svg/          every icon, logo and vector shape, exported as SVG
├── ANIMATIONS.md every Figma prototype interaction and animation (see step 4)
├── CONTENT.md    every text in the design, per page and section (see step 5)
└── EXPORT-LOG.md every exported node: name, node-id, file, status
```

1. **Export everything.** Every page frame, every component or variant, every image fill and every
   vector or icon. Raster images at **2x**, icons and logos as **SVG**.
2. **Timeout or failed export → retry. Never skip.** If a call times out, returns empty, or gives a
   broken or 0-byte file, retry it up to **3 times**, waiting longer each time (5s → 15s → 30s).
   If it still fails, export that frame in smaller parts (one child node at a time). Mark the result
   in `EXPORT-LOG.md`. At the end, every node must say `ok`. If anything still says `failed`, list
   it for the user. Do not continue with missing images or SVGs.
3. **Check the files.** Open every exported image once: not blank, not cropped, right size. Open
   every SVG once: it renders and has a valid `viewBox`.
4. **Read the animations too.** For every frame, read its prototype interactions: trigger (on click,
   hover, scroll or after delay), action, Smart Animate / dissolve / move-in / slide, duration,
   easing and direction. Hover and pressed variants count as animations too. Write each one into
   `Design/ANIMATIONS.md` (frame, element, trigger, effect, duration, easing), then map it to the
   closest `data-anim` preset from A9.8. If no preset fits, ask. Never drop a Figma animation
   without telling the user.
5. **Export all the text into `Design/CONTENT.md`.** Read every text node of every page frame and
   write it down **exactly as it is in Figma** (same words, same case, same punctuation), grouped by
   page, then by section, in page order. Include:
   - headings (with their level as designed: H1, H2, eyebrow, card title …)
   - body copy and lists
   - button and link labels, and their target if the design shows one
   - menu items (header, footer, mobile menu), in order
   - form labels, placeholders, help text, success / error messages
   - footer text, copyright line, contact details shown in the design
   - alt-text hints: one short description per image file in `Design/images/`, naming the file
   - repeated items (cards, logos, testimonials, FAQ) as a numbered list, one block per item

   Use this structure:

   ```markdown
   # Page: <Page name> (Design/pages/<page>-desktop.png)

   ## Section: <section name, by role — hero, logo-grid, cta-banner …>
   - Eyebrow: <text>
   - H2: <text>
   - Body: <text>
   - Button: <label> → <target if shown>
   - Image: Design/images/<file>.webp — alt hint: <short description>
   - Items:
     1. Title: <text> · Text: <text> · Icon: Design/svg/<file>.svg
   ```

   `Design/CONTENT.md` is the **only** source for importer seed content (D96–D102) and for any
   placeholder or default value in templates and fields. Never retype copy from a screenshot by
   eye, never shorten it, never invent copy. If a text is missing or unreadable in Figma, write
   `MISSING — ask` in `CONTENT.md` and list it for the user.

### Images in the theme — Figma export only, always sharp

Every image in the theme comes **only** from `Design/images/` (raster) and `Design/svg/` (icons,
logos, vector art). No stock photos, no AI images, no crops of the page PNGs. **Images must never
look blurry:**

- Export from Figma at **2x** the size the image is shown on screen.
- When converting to `.webp`, resize **down only** to 2x the largest display size. Never upscale.
- WebP quality **85 or higher** for photos. Use lossless for images with text or sharp edges.
- Icons, logos and line art stay **SVG**. Never convert them to PNG or WebP.
- Set `width` / `height` on `<img>` from the design, and use `object-fit` so it is never stretched.
- After each page, zoom the screenshot on every image. If any image looks soft, export it again at a
  higher size.

The full image rule is A9.1 rule 16. Media Library upload and naming rules are D104 and D105.

---

# A3. Extract the design values (Figma connected)

Work top-down. Call, in order:

1. `get_metadata` on the file/node → the frame tree, frame widths, page names.
   Frame widths tell you which artboards are desktop / tablet / mobile.
2. `get_variable_defs` on the top frame → **published variables and styles**. If the designer used
   Figma variables, this is your token set almost verbatim — colours, spacing, radii, type. Trust
   it over anything you measure.
3. `get_design_context` on each distinct section frame → per-node fills, font family, font size,
   font weight, line-height, letter-spacing, padding, item spacing (gap), corner radius, effects.
4. `get_screenshot` on the full page frame → visual reference to sanity-check what you extracted.

Collect into four raw buckets, **keeping the count of how many times each value appears** — the
frequency is what separates a real token from a one-off:

```
COLORS      hex → count, and which node types used it (text / bg / border / icon)
TYPE        (family, size, weight, lineHeight) → count, and which node names used it
SPACING     every padding, margin and itemSpacing/gap value → count
GEOMETRY    corner radii, stroke widths, shadow/effect definitions → count
```

Never invent a value that is not in these buckets.

---

# A4. Extract from screenshots or PDF (fallback)

Use this when there is no Figma MCP, or the Figma link failed (A1).

Read every screenshot / every PDF page. For each, record the **image or page width** — that is the
artboard width and tells you the breakpoint the design targets. For a PDF, also record which page
number is which screen (Home desktop, Home mobile, …).

Measure by proportion, not by pixel-guessing: find one element whose size you can anchor (usually
the container width or a known button height), then express everything else relative to it and
convert back to px using the artboard width.

Detect in this order — largest, most repeated things first, they are the most reliable:

1. Container width = distance between the left edge of body text and the right edge of the
   right-most content, on the widest artboard.
2. Section padding = vertical whitespace between the last element of one section and the first of
   the next, halved.
3. H1 → H6 → body sizes, by cap-height ratio against the container.
4. Grid gaps, card padding, radius, then colours (sample directly from the image).

Round everything per A5 and **flag the whole set as "measured — please confirm"** in `TOKENS.md`.

In this mode, still fill `Design/` as far as the source allows: page PNGs in `Design/pages/`, every
text read from the PDF / screenshots into `Design/CONTENT.md` (same structure as A2 step 5), and
every animation the user describes into `Design/ANIMATIONS.md`. Images and icons that cannot be
taken from the PDF as clean files are listed for the user — ask for the original files. Never crop
them out of a page screenshot.

---

# A5. Normalise raw data into tokens

This is the phase that matters. Raw Figma data is messy — 41 slightly different greys, font sizes
of 17.6px. Normalising turns that into a scale a human can hold in their head.

## A5.1 Global normalisation rules

- **Round every px to a whole number.** `17.6px → 18px`.
- **Snap to the nearest 5** for anything ≥ 20px (spacing, large type). `33px → 35px`, `62px → 60px`.
  Below 20px **never snap** — keep whole px (`4, 8, 10, 12, 14, 16, 18`). This rule applies
  everywhere, including the responsive step-down in A5.8.
- **Merge only fractional drift.** Values that round to the same whole px are one token
  (`15.8 → 16`, `16.2 → 16`). **Never merge two distinct whole-px values** — `12 / 14 / 16` type and
  `8 / 10` spacing stay separate tokens. Snap-to-5 above 20px is the only other merge allowed.
- **Drop one-offs.** A value used **once** in the whole design is not a token; it is a mistake or a
  detail. Fold it into the nearest neighbour. Exception: the single largest display size, which is
  usually a deliberate hero number.
- **Colours:** merge two hexes whose channels are all within 8 of each other. Keep the more frequent.

## A5.2 Fonts

```
--font-<family-slug>: "<Family>", <fallback stack>;
```

- Group every text node by family. Normally there are exactly **two**: a display face used by
  `h1`–`h4`, and a text face used by body/`p`/buttons/labels. Three is acceptable, four means you
  merged something wrong — re-check.
- The family used by the **largest** text nodes = display. The family used by the most **paragraph**
  nodes = body.
- Fallback stack: display → `Impact, sans-serif` for condensed/poster faces, otherwise
  `Georgia, serif` for serifs and `system-ui, sans-serif` for the rest. Body → `system-ui, sans-serif`.
- Build the Google Fonts URL from the families and the weights actually present. Do not request
  weights the design never uses. It is enqueued in `inc/enqueue.php` (A7.6), never as a hard-coded
  `<link>`.

## A5.3 Type scale — three tiers

Sort every distinct font-size descending, then split:

| Tier | Range | Token form | Responsive behaviour |
|---|---|---|---|
| **Fixed** | ≤ 40px | `--f<size>: <size>px` (name == value) | stepped down at ≤1199.98 / ≤991.98 / ≤767.98 |
| **Fluid** | 41–70px | `--f<size>: clamp(<min>rem, <base>rem + <slope>vw, <max>rem)` | clamps itself, plus a hard step-down per breakpoint |
| **Display** | > 70px | `--f<size>: <size>px` | stepped down hard at every breakpoint |

Rules:
- **Token name is the desktop px value.** `--f24` is 24px. This is non-negotiable — it is what makes
  the scale readable at a glance six months later.
- Fluid `clamp()` min = **80% of max**, rounded to 5. `--f50` → `clamp(2.5rem, 2.375rem + 0.625vw, 3.125rem)` (50→40px).
- A tier is allowed to be empty. Small marketing sites often have no display tier.

Then map the semantic defaults from the design's real usage:

```
h1 = largest heading size found on a page title
h2 = section heading size
h3 = card / sub-heading size
h4 = smaller sub-heading
h5, h6 = the two label-ish sizes, in the body font
p  = the most frequent paragraph size
body = the most frequent UI/label size
```

## A5.4 Line heights

```
--bodyLh:  most frequent line-height on paragraph nodes, as a unitless ratio, rounded to 1 dp
--lhTight: most frequent line-height on heading nodes, rounded to 2 dp
```
Typical: `1.4` and `1.15`. If a design shows `1.0` on headings, use `1.05` — exactly 1 clips
descenders in most faces.

## A5.5 Colours

Name by **role**, never by appearance where a role exists. The generated names must survive a
rebrand — `--primary` survives, `--orange` does not, which is why both exist: literal names for the
palette, role aliases for everything section CSS actually consumes.

Emit in this order:

```css
/* BRAND */      --primary-*, --accent-*, gradients built from them
/* NEUTRALS */   --black --white --ink --muted --line --surface-* --dark-*
/* ROLE ALIASES */ --text --primary
```

- The most-used non-neutral colour = **brand primary**. The colour on primary buttons confirms it.
- Any colour used only on borders/dividers → `--line`. Only on secondary text → `--muted`.
- If the design uses gradients, capture both stops and emit them as `--gradient1` / `--gradient2`
  (one for default, one for hover — usually the same two stops reversed).
- Every hex appears **exactly once** in `:root` and nowhere else in the codebase. (Gutenberg
  `theme.json` presets and Elementor Global Colors mirror these values — see E3 / E5 — they never
  introduce a new colour.)

## A5.6 Spacing

- Take the spacing bucket, snap to 5, drop one-offs → that is the scale.
- Emit as `--sp<n>` where **name == value**: `--sp10: 10px`.
- Always include `--sp4`, `--sp8`, `--sp10`, `--sp15`, `--sp20` even if the design only shows some
  of them; small gaps always turn up later.
- **`--sectionGap`** = the most frequent vertical padding between sections (the single most
  important spacing number on the site).
- **`--sectiongapHalf`** = half of it, rounded to 5.
- **`--g40`** = the most frequent grid/flex gap between cards. Keep the historical name `--g40`
  only if the value is 40px; otherwise name it `--g<value>`.
- **`--container`** = widest content width. **`--gutter`** = artboard side padding.

## A5.7 Geometry & motion

```
--r-btn / --r-sm / --r-md / --r-lg / --r-xl    from the radius bucket, smallest → largest
--r-full: 9999px                               always
--shadow-sm / --shadow-md / --shadow-lg        from the effect bucket; if the design has none,
                                               omit them rather than inventing shadows
--z-sticky: 200  --z-drop: 210  --z-modal: 300 always, fixed
--ease: .25s ease
--ease-out: .3s cubic-bezier(.2,.7,.3,1)
--transition: all .3s ease-in-out
```

## A5.8 Breakpoints — always these seven, never others

Desktop-first, `max-width`, always `.98`:

```
1599.98 · 1439.98 · 1365.98 · 1199.98 · 991.98 · 767.98 · 575.98
```

This is fixed regardless of what the Figma file contains. Figma artboards tell you *what changes*,
not *where the breakpoint sits*.

Derive the **token step-down table** — the single most valuable thing this phase produces, because
it makes every future layout responsive for free:

| Token group | ≤1599.98 | ≤1439.98 | ≤1365.98 | ≤1199.98 | ≤991.98 | ≤767.98 | ≤575.98 |
|---|---|---|---|---|---|---|---|
| `--container` | −4% | −8% | −16% | −24% | fixed 960px | `100%` | `100%` |
| `--gutter` | — | — | — | — | — | −20% | −25% |
| `--sectionGap` | — | — | — | −40% | — | −50% of base | — |
| `--g40` (grid gap) | — | — | — | −25% | −40% | −50% | −60% |
| Fixed type ≤40px | — | — | — | −8% | −14% | −20% | — |
| Fluid type 41–70 | — | — | — | to clamp min | — | −25% | −40% |
| Display type >70 | — | — | — | −25% | −38% | −55% | −65% |
| `--sp25`…`--sp100` | — | — | — | −20% | −35% | −50% | — |

Round every result with the A5.1 rule: **results ≥ 20px snap to the nearest 5, results below 20px
round to a whole px.** Apply these **minimums** after rounding — never go below them at any
breakpoint:

| Token | Minimum |
|---|---|
| Body text (`p`, `li`, form fields) | **14px** (16px for form inputs, to stop iOS zoom) |
| Small / meta text (captions, labels) | **12px** |
| `h6`–`h4` | never smaller than body text |

Example: body `16px × 0.80 = 12.8` → whole px `13` → minimum **14px**. Only re-declare a token at a
breakpoint where it actually changes — a token that carries through is simply absent from that block.

---

# A6. Approval gate — TOKENS.md

**Do not skip this gate.** Everything downstream is built on these numbers; a wrong type scale caught
here costs a minute, caught at page 6 costs a day.

Write **`TOKENS.md`** in the project folder and stop. It must contain:

1. A one-paragraph summary: *N colours, N font families, N type sizes, N spacing steps detected.*
2. Every token as a table: **token name · value · where it came from** (node name / frequency count).
3. A **"Needs your confirmation"** list — every value that was measured rather than read, every
   near-duplicate you merged, every one-off you dropped.
4. The semantic mapping (`h1 = --f65`, `p = --f18`, …).
5. The step-down table with real numbers filled in.

Then ask: *"Confirm these tokens and I'll build the theme."* **Nothing WordPress-related is built
before the user approves** — no theme folder, no `vendor.css`, no `functions.php`, no field groups,
no importer. If they correct a value, update `TOKENS.md` and re-confirm only that value.

---

# A7. Theme front-end system

After A6 is approved, build the front-end system **inside the theme**. There are no static HTML
pages and no `global.html` at the project root — the theme's templates and the style guide page (A8)
replace them.

## A7.1 Asset folders inside the theme

```
<project folder>/
├── Design/                         Figma export (A2) — never shipped in the theme
├── TOKENS.md                       approved tokens (A6)
├── login.md                        site login (D110) — git-ignored
└── wp-content/themes/<slug>/
    ├── style.css                   theme header only
    ├── functions.php               requires the inc/ files
    ├── inc/
    │   ├── enqueue.php             ALL styles and scripts, in the A7.6 order
    │   └── …                       (structure: B9)
    ├── page-templates/
    │   └── page-style-guide.php    living style guide + page index (A8)
    ├── template-parts/
    │   └── sections/<section>.php  ONE render file per section (A9.6, E1)
    ├── acf-json/                   ACF local JSON (D111)
    └── assets/
        ├── css/
        │   ├── vendor.css          GLOBAL ONLY — tokens, reset, typography, buttons, breakpoints
        │   ├── global/             shared components that are not sections (style-guide, forms…)
        │   ├── sections/           ONE FILE PER SECTION
        │   └── custom.css          Elementor builds only — approved CSS only (E5.18)
        ├── js/
        │   ├── vendor.js           nav toggle, accordions — all init guarded
        │   ├── sliders.js          data-slider → Slick 1.8.1 (A7.5) or Swiper — the ONLY slider JS (ACF / Gutenberg / Both; not for Elementor / other builders, A1)
        │   ├── animations.js       data-anim → GSAP + ScrollTrigger (A7.4) — the ONLY animation JS
        │   └── global/             optional per-feature helpers
        ├── images/                 WebP only (SVG for vector art) — from Design/, optimised, sized to use
        └── fonts/                  only for self-hosted faces
```

In update / extend mode (C73) the same `assets/` structure goes into the child theme or the existing
theme, as C73–C95 decide.

All asset URLs are built with `get_template_directory_uri()` (or `get_stylesheet_directory_uri()`
in a child theme). Never use fragile relative paths (B19).

## A7.2 `assets/css/vendor.css`

Write it in exactly this order. Everything in `{{ }}` comes from the approved `TOKENS.md`;
everything else is boilerplate that is identical in every project.

> **Elementor builds:** E5 adds extra CSS rules (Elementor globals, `custom.css`, widget
> wrappers). Where E5 differs from this file or from A9, **E5 wins** for that build.

```css
/* ===================================================================
   TOKENS
   =================================================================== */
:root {
    /* COLORS */
    {{ --brand and neutral colours, one per line, hex only, in the A5.5 order }}
    {{ --gradient1 / --gradient2 if the design uses gradients }}

    /* ROLE ALIASES */
    --text: var(--ink);
    --primary: var({{ --brand-primary }});

    /* FONTS */
    {{ --font-<display>: "<Family>", <stack>; }}
    {{ --font-<body>:    "<Family>", <stack>; }}

    /* RAW TYPE SCALE (name==value) */
    {{ fixed tier, ascending }}
    {{ fluid tier as clamp(), with a "/* 50 - 40 */" comment showing the real range }}
    {{ display tier }}

    --bodyLh: {{ }};
    --lhTight: {{ }};

    /* CONTAINER & SPACE */
    --container: {{ }};
    --gutter: {{ }};
    --sectionGap: {{ }};
    --sectiongapHalf: {{ }};
    {{ --sp* scale, ascending }}
    {{ --g<n>: grid gap }}

    /* RADIUS / SHADOW / Z / EASE */
    {{ --r-* }}
    {{ --shadow-* }}
    --overlay: rgba(0, 0, 0, .55);
    --z-sticky: 200;
    --z-drop: 210;
    --z-modal: 300;
    --ease: .25s ease;
    --ease-out: .3s cubic-bezier(.2, .7, .3, 1);
    --transition: all .3s ease-in-out;

    /* SCROLLBAR */
    --scrollbar-width: 10px;
    --scrollbar-thumb: var(--primary);
    --scrollbar-thumb-hover: {{ darker primary — a real darker value, not an alias }};

    /* ---------------------------------------------------------------
       GLOBAL BREAKPOINTS — container / gutter / gap / type tokens step
       down here, so every layout using .container or these tokens
       follows automatically. Nested inside :root so all values for a
       given token name live in one place.
       --------------------------------------------------------------- */
    @media (max-width: 1599.98px) { {{ only tokens that change }} }
    @media (max-width: 1439.98px) { {{ … }} }
    @media (max-width: 1365.98px) { {{ … }} }
    @media (max-width: 1199.98px) { {{ … }} }
    @media (max-width:  991.98px) { {{ … }} }
    @media (max-width:  767.98px) { {{ … }} }
    @media (max-width:  575.98px) { {{ … }} }
}

/* ===================================================================
   RESET / BASE
   =================================================================== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

[hidden] { display: none !important; }

html {
    scrollbar-width: thin;
    scrollbar-color: var(--scrollbar-thumb) transparent;
    -webkit-text-size-adjust: 100%;
    overflow-x: clip;
    scroll-behavior: smooth;
}

/* NOT nested under html — WebKit only honours these at the top level. */
::-webkit-scrollbar { width: var(--scrollbar-width); }
::-webkit-scrollbar-thumb {
    background: var(--scrollbar-thumb);
    border-radius: var(--r-full);
    &:hover { background: var(--scrollbar-thumb-hover); }
}

body {
    font-family: var({{ --font-body }});
    font-size: {{ --f<body> }};
    line-height: var(--bodyLh);
    color: var(--text);
    background: var(--white);
    -webkit-font-smoothing: antialiased;
    overflow-x: clip;
}

img, video, svg { display: block; max-width: 100%; height: auto; }
a { color: inherit; text-decoration: none; }
ul { list-style: none; }
button {
    border: 0; background: none; color: inherit;
    font-family: inherit; cursor: pointer;
    transition: var(--transition);
}

/* The one global vertical rhythm rule. Section padding stacks on top of this,
   so the real gap between two sections is padding + sectionGap + padding. */
main section { margin-bottom: var(--sectionGap); overflow-x: clip; }

:focus-visible { outline: 2px solid var(--primary); outline-offset: 2px; }

/* ===================================================================
   TYPOGRAPHY
   =================================================================== */
{{ h1–h4 → display font, weight from Figma, line-height var(--lhTight) }}
{{ h5–h6 → body font, line-height var(--lhTight) }}
{{ p     → body font, size --f<p>, line-height var(--bodyLh) }}
p + p { margin-top: var(--sp20); }

/* ===================================================================
   LAYOUT UTILITIES
   =================================================================== */
.container {
    width: 100%;
    max-width: var(--container);
    margin-inline: auto;
    padding-inline: var(--gutter);
}

.sr-only {
    position: absolute; width: 1px; height: 1px;
    padding: 0; margin: -1px; overflow: hidden;
    clip: rect(0, 0, 0, 0); border: 0;
}

.skip-link {
    position: fixed; inset-block-start: 0; inset-inline-start: 0;
    z-index: var(--z-modal);
    width: 0; height: 0; overflow: hidden;
    background: var(--white); color: var(--primary);
}
.skip-link:focus {
    width: auto; height: auto;
    padding: var(--sp15) var(--sp25);
    font-weight: 600; clip: auto;
}

/* ===================================================================
   BUTTONS
   =================================================================== */
{{ .btn base from the design: size, weight, padding, radius, transition,
   display:block, width:max-content }}
{{ .btn-primary / .btn-white (or the design's real variants) with hover states }}
{{ .btn-icon: flex, centered, gap }}
{{ @media (max-width: 575.98px) inside .btn → full width, reduced padding, centered }}

/* The arrow link. The arrow is a masked SVG on ::after whose background-color is
   currentColor, so it always matches the link's own colour and no section ever
   writes arrow markup or arrow CSS. */
.btn-link {
    position: relative;
    display: flex; align-items: center;
    width: max-content; max-width: 100%;
    font-family: var({{ --font-display }});
    font-size: {{ }};

    &::after {
        flex: 0 0 auto;
        width: 22px; height: 10px;
        margin-left: var(--sp10);
        background-color: currentColor;
        content: '';
        transition: var(--transition);
        -webkit-mask: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 10" fill="none"><path d="M17 9L21 5L17 1M21 5H1" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>') no-repeat center;
                mask: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 10" fill="none"><path d="M17 9L21 5L17 1M21 5H1" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>') no-repeat center;
        -webkit-mask-size: contain;
                mask-size: contain;
    }
    &:hover::after { margin-left: var(--sp20); }

    &.btn-black  { color: var(--black); }
    &.btn-accent { color: var(--primary); }
}

/* ===================================================================
   SECTION TITLES
   =================================================================== */
/* One block for every section heading on the site. Sections never style
   their own title — that is the whole point of this component. */
.section-title-wrap {
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    gap: var(--sp10);
    margin-bottom: {{ --sp<section title gap> }};
    text-align: center;

    &:last-child { margin-bottom: 0; }

    & .title-eyebrow {
        font-weight: 700;
        text-transform: uppercase;
        color: var(--primary);
    }

    & h2:not(.section-title) span { color: var(--primary); }

    & .section-title-desc {
        max-width: {{ measured desc width }};
        margin-top: var(--sp10);
        & p { font-size: {{ --f<lead> }}; }
    }

    &.section-title-left { align-items: flex-start; text-align: left; }
}

/* centred heading with a rule on each side */
.section-title {
    display: flex; align-items: center; justify-content: center;
    gap: var(--sp30);
    width: 100%;
    text-align: center;

    &::before, &::after {
        flex: 1;
        height: 1px;
        max-width: min(50vw, 820px);
        background: var(--black);
        content: "";
    }
}

.heading-accent span { color: var(--primary); }

/* ===================================================================
   SLIDERS — Slick 1.8.1 (core slick.min.css from cdnjs, NO slick-theme:
   it ships an icon font). Arrows are inline SVG buttons in the markup.
   =================================================================== */
[data-slider] {
    --slider-gap: var(--g40);
    --per-view: 1;

    &[data-slider="cards"] { --per-view: 3; }
    &[data-slider="logos"] { --per-view: 6; }

    /* Before Slick runs (and with JS off): one row, same layout as the
       built slider, so slides never flash stacked on top of each other. */
    &:not(.slick-initialized) {
        display: flex;
        gap: var(--slider-gap);
        overflow: hidden;

        & > * {
            flex: 0 0 calc((100% - (var(--per-view) - 1) * var(--slider-gap)) / var(--per-view));
        }
    }

    /* Gap = margin on each slide (Slick subtracts it from the slide width),
       pulled back by a negative margin on the list. Equal-height slides. */
    & .slick-list  { margin-inline: calc(var(--slider-gap) / -2); }
    & .slick-track { display: flex; }
    & .slick-slide {
        height: auto;
        margin-inline: calc(var(--slider-gap) / 2);
    }

    @media (max-width: 1199.98px) {
        &[data-slider="cards"] { --per-view: 2; }
        &[data-slider="logos"] { --per-view: 5; }
    }
    @media (max-width: 991.98px) {
        &[data-slider="logos"] { --per-view: 4; }
    }
    @media (max-width: 767.98px) {
        &[data-slider="cards"] { --per-view: 1; }
        &[data-slider="logos"] { --per-view: 3; }
    }
    @media (max-width: 575.98px) {
        &[data-slider="logos"] { --per-view: 2; }
    }
}

.slider-nav {
    display: flex;
    gap: var(--sp10);
}

{{ .slider-arrow size, border and colours from the design's slider arrows }}
.slider-arrow {
    display: inline-flex; align-items: center; justify-content: center;
    width: 48px; height: 48px;
    border: 1px solid currentColor;
    border-radius: 50%;
    color: var(--black);

    & svg { width: 18px; height: 18px; }
    &:hover { background: var(--primary); border-color: var(--primary); color: var(--white); }
    &.slick-disabled { opacity: .4; pointer-events: none; }
}

.slick-dots {
    display: flex; justify-content: center;
    gap: var(--sp10);
    margin-top: var(--sp30);

    & button {
        width: 10px; height: 10px;
        border-radius: 50%;
        background: currentColor;
        font-size: 0;
        opacity: .3;
    }
    & .slick-active button { background: var(--primary); opacity: 1; }
}

/* ===================================================================
   HELPERS
   =================================================================== */
.txt-primary { color: var(--primary); }
```

**What must never enter `vendor.css` after this point:** anything scoped to one section. The file is
closed the moment A7 ends. New global token or global component → `vendor.css` **and** a demo on the
style guide page (A8). Anything else → the section's own file.

## A7.3 `assets/js/vendor.js`

Keep it small and **guard every init** so one missing element never breaks the page:

```js
document.addEventListener('DOMContentLoaded', () => {
    const nav = document.getElementById('primary-nav');
    if (nav) { /* toggle, backdrop, Esc-to-close, focus trap */ }

    const accordions = document.querySelectorAll('[data-accordion]');
    if (accordions.length) { /* … */ }
});
```

No scroll animation code — that lives only in `animations.js` (A7.4). No slider code — that lives
only in `sliders.js` (A7.5). Every init must also work when the same section appears twice on one
page (E1) — select all matches, never assume one.

## A7.4 `assets/js/animations.js` (default: GSAP + ScrollTrigger)

Copy this file verbatim. It turns `data-anim*` attributes into GSAP animations, so no section ever
needs its own animation JS. Content is fully visible without JS and with `prefers-reduced-motion`.

```js
/* Scroll animations — GSAP 3.12.5 + ScrollTrigger, attribute-driven (A9.8). */
document.addEventListener('DOMContentLoaded', () => {
    if (!window.gsap || !window.ScrollTrigger) return;
    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
    gsap.registerPlugin(ScrollTrigger);

    const PRESETS = {
        'fade-up':    { y: 40,  opacity: 0 },
        'fade-down':  { y: -40, opacity: 0 },
        'fade-left':  { x: 40,  opacity: 0 },
        'fade-right': { x: -40, opacity: 0 },
        'fade-in':    { opacity: 0 },
        'zoom-in':    { scale: 0.92, opacity: 0 },
    };
    const sec = (value, fallbackMs) => (parseFloat(value) || fallbackMs) / 1000;
    const onScroll = (el) => ({ trigger: el, start: 'top 85%', once: true });

    /* Cards carry `transition: var(--transition)` (= `all .3s`) for hover. That CSS transition
       fights GSAP's per-frame inline opacity/transform, so the entrance stalls and never shows.
       Switch the CSS transition off while GSAP animates, then clear every inline prop GSAP wrote
       so the stylesheet's hover transition works again. */
    const reveal = (targets, vars) => {
        gsap.set(targets, { transition: 'none' });
        gsap.from(targets, {
            ...vars,
            ease: 'power2.out',
            clearProps: 'transform,opacity,transition',
        });
    };

    document.querySelectorAll('[data-anim]').forEach((el) => {
        const from = PRESETS[el.dataset.anim];
        if (!from) return;
        reveal(el, {
            ...from,
            duration: sec(el.dataset.animDuration, 700),
            delay: sec(el.dataset.animDelay, 0),
            scrollTrigger: onScroll(el),
        });
    });

    document.querySelectorAll('[data-anim-stagger]').forEach((parent) => {
        const from = PRESETS[parent.dataset.animStagger];
        if (!from || !parent.children.length) return;
        reveal(parent.children, {
            ...from,
            duration: sec(parent.dataset.animDuration, 700),
            stagger: sec(parent.dataset.animStaggerGap, 120),
            scrollTrigger: onScroll(parent),
        });
    });

    document.querySelectorAll('[data-anim-counter]').forEach((el) => {
        const end = parseFloat(el.dataset.animCounter);
        if (Number.isNaN(end)) return;
        const state = { value: 0 };
        gsap.to(state, {
            value: end,
            duration: sec(el.dataset.animDuration, 1500),
            ease: 'power1.out',
            scrollTrigger: onScroll(el),
            onUpdate: () => { el.textContent = Math.round(state.value).toLocaleString(); },
        });
    });

    /* Trigger positions are measured at DOMContentLoaded, before images and fonts change the
       layout. Re-measure once everything has loaded, or lower sections fire at the wrong spot. */
    window.addEventListener('load', () => ScrollTrigger.refresh());
});
```

The scripts are loaded through `inc/enqueue.php` in the order fixed in A7.6 — never with
hard-coded `<script>` tags in `header.php`, `footer.php` or a template. `sliders.js` must load
**before** `animations.js`: Slick builds the slider layout first, then ScrollTrigger measures the
page.

Never change the version in the pinned URLs (A7.6) without updating A9.8.

## A7.5 `assets/js/sliders.js` (Slick 1.8.1 + jQuery)

Copy this file verbatim. It turns `data-slider*` attributes into Slick sliders, so no section ever
needs its own slider JS. It does nothing on a page with no `[data-slider]`.

The Slick core CSS is enqueued **before** `vendor.css` (as a dependency of it, A7.6), so the
project's slider rules win. jQuery + Slick are enqueued before `sliders.js` (A7.6). Do **not** load
`slick-theme.css`. It ships the Slick icon font, which breaks the no-icon-fonts rule.

```js
/* Sliders — Slick 1.8.1 + jQuery, attribute-driven (A9.8.1). */
document.addEventListener('DOMContentLoaded', () => {
    const tracks = document.querySelectorAll('[data-slider]');
    if (!tracks.length || !window.jQuery || !jQuery.fn.slick) return;

    const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

    /* Slick breakpoints apply when the window is NARROWER than the number, so 1200 here
       == `max-width: 1199.98px` in CSS. Keep these in step with the CSS --per-view values. */
    const PRESETS = {
        cards: {
            slidesToShow: 3,
            responsive: [
                { breakpoint: 1200, settings: { slidesToShow: 2 } },
                { breakpoint: 768,  settings: { slidesToShow: 1 } },
            ],
        },
        logos: {
            slidesToShow: 6,
            autoplay: true,
            autoplaySpeed: 3000,
            responsive: [
                { breakpoint: 1200, settings: { slidesToShow: 5 } },
                { breakpoint: 992,  settings: { slidesToShow: 4 } },
                { breakpoint: 768,  settings: { slidesToShow: 3 } },
                { breakpoint: 576,  settings: { slidesToShow: 2 } },
            ],
        },
        single: {
            slidesToShow: 1,
            dots: true,
            adaptiveHeight: true,
        },
    };

    const BASE = {
        slidesToScroll: 1,
        speed: 500,
        infinite: true,
        swipeToSlide: true,
        pauseOnHover: true,
        pauseOnFocus: true,
        accessibility: true,
        rows: 0, // stops Slick 1.8.1 wrapping every slide in two extra <div>s, which breaks BEM selectors
    };

    tracks.forEach((track) => {
        const preset = PRESETS[track.dataset.slider];
        if (!preset) return;

        const d = track.dataset;
        const wrap = track.closest('[data-slider-wrap]') || track.parentElement;
        const prev = wrap.querySelector('[data-slider-prev]');
        const next = wrap.querySelector('[data-slider-next]');
        const dotsBox = wrap.querySelector('[data-slider-pagination]');
        const options = { ...BASE, ...preset };

        // Arrows only when the markup has both buttons (inline SVG, never Slick's default arrows).
        options.arrows = Boolean(prev && next);
        if (options.arrows) { options.prevArrow = prev; options.nextArrow = next; }

        if (d.sliderDots) options.dots = d.sliderDots === 'true';
        if (dotsBox) options.appendDots = dotsBox;
        if (d.sliderLoop === 'false') options.infinite = false;
        if (d.sliderFade === 'true' && options.slidesToShow === 1) options.fade = true;

        const autoplay = parseInt(d.sliderAutoplay, 10);
        if (autoplay > 0) { options.autoplay = true; options.autoplaySpeed = autoplay; }
        if (d.sliderAutoplay === 'false') options.autoplay = false;

        if (reduceMotion) { options.autoplay = false; options.speed = 0; }

        jQuery(track).slick(options);
    });
});
```

A new preset is added to `PRESETS` here, to the `--per-view` rules in the `vendor.css` SLIDERS block
**and** to the preset list in A9.8.1. Never pass Slick options inline from a section.

## A7.6 Enqueue order — `inc/enqueue.php`

Every style and script is loaded here with `wp_enqueue_style()` / `wp_enqueue_script()` and real
dependencies. No hard-coded `<link>` or `<script>` tags anywhere in the theme. `functions.php`
requires this file (B10).

**Order (fixed):**

```
Styles:  Google Fonts → slick.min.css (slider builds) → vendor.css → global/*.css → sections/*.css
Scripts: jQuery → Slick 1.8.1 → GSAP 3.12.5 → ScrollTrigger 3.12.5 → vendor.js → sliders.js → animations.js
```

Rules:

- **Pinned cdnjs URLs only**, exactly these. Never change a version without updating A9.8 / A9.8.1:
  - `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js`
  - `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js`
  - `https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.8.1/slick.min.js`
  - `https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.8.1/slick.min.css`
- **jQuery = WordPress's own `jquery` handle** (WordPress 6.4+ ships jQuery 3.7.1). Never deregister
  it and never load a second jQuery from cdnjs — plugins (forms, WooCommerce) depend on the core
  copy, and two copies break them (B21: do not duplicate libraries).
- **Slick:** if the design has **no** slider anywhere, do not enqueue Slick CSS, Slick JS or
  `sliders.js` (set `{PREFIX}_HAS_SLIDER` to `false`). If it has one, enqueue them on every
  front-end page — editors can place any section on any page (E1), so per-page detection is not
  reliable.
- **No animation library** (A1 answer 4 = none): remove the GSAP, ScrollTrigger and
  `animations.js` lines.
- **Section CSS** is enqueued **after** `vendor.css`, with `vendor.css` as its dependency. One handle
  per file. Section files are scoped by their block class, so loading all of them site-wide is safe.
- All theme scripts load **in the footer** with the `defer` strategy (WordPress 6.3+). Their code
  waits for `DOMContentLoaded`, which fires after deferred scripts run.
- Theme files use `filemtime()` as the version, so a changed file is never served from cache.
- `style-guide.css` loads only on the style guide template (A8).
- Gutenberg builds also load the theme CSS in the editor (E3). Elementor builds add `custom.css` and
  the rules in E5 — where E5 differs, E5 wins.

```php
<?php
/**
 * Front-end assets. Order is fixed (A7.6):
 * Styles:  fonts → slick.min.css → vendor.css → global/*.css → sections/*.css
 * Scripts: jquery → slick → gsap → ScrollTrigger → vendor.js → sliders.js → animations.js
 */

defined( 'ABSPATH' ) || exit;

// true when the design has at least one slider (A1 / Design/). false = Slick is never loaded.
define( '{PREFIX}_HAS_SLIDER', true );

function {prefix}_asset_version( $relative_path ) {
	$file = get_template_directory() . $relative_path;
	return file_exists( $file ) ? (string) filemtime( $file ) : wp_get_theme()->get( 'Version' );
}

function {prefix}_enqueue_assets() {
	$uri    = get_template_directory_uri();
	$dir    = get_template_directory();
	$cdn    = 'https://cdnjs.cloudflare.com/ajax/libs/';
	$footer = array( 'in_footer' => true, 'strategy' => 'defer' );

	/* ---------- Styles ---------- */

	// Google Fonts — only the families and weights the design uses (A5.2). URL from TOKENS.md.
	wp_enqueue_style( '{prefix}-fonts', '{{ Google Fonts URL from TOKENS.md }}', array(), null );

	$vendor_deps = array( '{prefix}-fonts' );

	// Slick core CSS BEFORE vendor.css, so the theme's slider rules win. Never slick-theme.css.
	if ( {PREFIX}_HAS_SLIDER ) {
		wp_enqueue_style( 'slick', $cdn . 'slick-carousel/1.8.1/slick.min.css', array(), null );
		$vendor_deps[] = 'slick';
	}

	wp_enqueue_style(
		'{prefix}-vendor',
		$uri . '/assets/css/vendor.css',
		$vendor_deps,
		{prefix}_asset_version( '/assets/css/vendor.css' )
	);

	// Global components that are not sections (forms …). style-guide.css only on its template.
	foreach ( glob( $dir . '/assets/css/global/*.css' ) ?: array() as $file ) {
		$name = basename( $file, '.css' );
		if ( 'style-guide' === $name && ! is_page_template( 'page-templates/page-style-guide.php' ) ) {
			continue;
		}
		wp_enqueue_style(
			'{prefix}-global-' . $name,
			$uri . '/assets/css/global/' . $name . '.css',
			array( '{prefix}-vendor' ),
			{prefix}_asset_version( '/assets/css/global/' . $name . '.css' )
		);
	}

	// One file per section, always AFTER vendor.css.
	foreach ( glob( $dir . '/assets/css/sections/*.css' ) ?: array() as $file ) {
		$name = basename( $file, '.css' );
		wp_enqueue_style(
			'{prefix}-section-' . $name,
			$uri . '/assets/css/sections/' . $name . '.css',
			array( '{prefix}-vendor' ),
			{prefix}_asset_version( '/assets/css/sections/' . $name . '.css' )
		);
	}

	/* ---------- Scripts ---------- */

	$vendor_js_deps = array();

	// jQuery = WordPress core handle. Slick 1.8.1 depends on it.
	if ( {PREFIX}_HAS_SLIDER ) {
		wp_enqueue_script( 'slick', $cdn . 'slick-carousel/1.8.1/slick.min.js', array( 'jquery' ), null, $footer );
	}

	// GSAP 3.12.5 + ScrollTrigger (A9.8). Remove these when the build has no animation library.
	wp_enqueue_script( 'gsap', $cdn . 'gsap/3.12.5/gsap.min.js', array(), null, $footer );
	wp_enqueue_script( 'gsap-scrolltrigger', $cdn . 'gsap/3.12.5/ScrollTrigger.min.js', array( 'gsap' ), null, $footer );

	wp_enqueue_script(
		'{prefix}-vendor',
		$uri . '/assets/js/vendor.js',
		$vendor_js_deps,
		{prefix}_asset_version( '/assets/js/vendor.js' ),
		$footer
	);

	$animation_deps = array( 'gsap', 'gsap-scrolltrigger', '{prefix}-vendor' );

	// sliders.js BEFORE animations.js: Slick builds the layout, then ScrollTrigger measures.
	if ( {PREFIX}_HAS_SLIDER ) {
		wp_enqueue_script(
			'{prefix}-sliders',
			$uri . '/assets/js/sliders.js',
			array( 'jquery', 'slick', '{prefix}-vendor' ),
			{prefix}_asset_version( '/assets/js/sliders.js' ),
			$footer
		);
		$animation_deps[] = '{prefix}-sliders';
	}

	wp_enqueue_script(
		'{prefix}-animations',
		$uri . '/assets/js/animations.js',
		$animation_deps,
		{prefix}_asset_version( '/assets/js/animations.js' ),
		$footer
	);
}
add_action( 'wp_enqueue_scripts', '{prefix}_enqueue_assets' );
```

Check in the page source after the first template is built: every handle appears once, in the order
above, with no second jQuery and no `slick-theme.css`.

---

# A8. Style guide page

The living style guide. It replaces `global.html`. It is the first page a teammate opens and the
first thing Claude opens before building any section.

**Template:** `page-templates/page-style-guide.php` with `Template Name: Style Guide`. The importer
(D96–D102) creates one page **"Style Guide"** (slug `style-guide`) with this template.

- **Not indexed:** add `noindex, nofollow` through the `wp_robots` filter for this template, and
  keep the page out of the XML sitemap, the sitemap page (D107) and search results (D106).
- **Not in any menu.** Never add it to the header, footer or mobile menu.
- Its own CSS lives in `assets/css/global/style-guide.css` and loads only on this template (A7.6).
- It uses the normal `get_header()` / `get_footer()`, so it shows the real header, footer, fonts and
  scripts.

```php
<?php
/**
 * Template Name: Style Guide
 * Living style guide + page index (A8). Not indexed, not in menus.
 */

defined( 'ABSPATH' ) || exit;

get_header();
?>
<main id="main" class="style-guide">
	<?php // 1. .pages-nav  2. display headings  3. h1–h6 + p  4. heading helpers
	      // 5. buttons + arrow links  6. page shell markup  7. sliders (if the design has one) ?>
</main>
<?php
get_footer();
```

```php
// inc/template-functions.php — keep the style guide out of search engines.
function {prefix}_style_guide_noindex( $robots ) {
	if ( is_page_template( 'page-templates/page-style-guide.php' ) ) {
		$robots['noindex']  = true;
		$robots['nofollow'] = true;
	}
	return $robots;
}
add_filter( 'wp_robots', '{prefix}_style_guide_noindex' );
```

Build it with:

1. **`.pages-nav`** — a table of every page in the build (`#` · Page Name · Link), styled by
   `assets/css/global/style-guide.css`. Seed it with the pages from A1. Links use
   `get_permalink()` of the real WordPress page — never a hard-coded URL (B42).
2. **Display headings** — one row per display token, sized inline (the style guide template is the
   *only* file where inline `style` is allowed, and only for this).
3. **`h1`–`h6` + `p`**, rendered as bare tags so the global CSS is what you see.
4. **Heading helpers** — `.heading-accent`, `.section-title`, `.section-title-wrap` in all three
   forms (with desc, without desc, with eyebrow).
5. **Every button variant** and both arrow-link colours.
6. **The page shell** — skip links → `header#header` → `main#main` → `footer#footer` → the
   scripts from A7.6 (loaded by `wp_footer()`), shown as copyable markup. `header.php` prints the
   skip links right after `wp_body_open()`, always exactly:
   ```html
   <a class="skip-link" href="#main">Skip to content</a>
   <a class="skip-link" href="#footer">Skip to footer</a>
   ```
7. **Sliders** — only if the design has one: a working demo of each slider preset the design uses
   (`cards`, `logos`, `single`), with the arrow buttons and dots, built with the A9.8.1 markup.

Text on the style guide page is sample text for components only. Real page copy always comes from
`Design/CONTENT.md`.

Rule that keeps it alive: **a new component is not done until it is also on the style guide page.**

---

# A9. Front-end rules

These are the permanent front-end rules of the theme. They govern every template, template part and
section from here on. Wherever a rule below says `{{ … }}`, the value comes from the approved
`TOKENS.md` (A6). Write those values into the architecture report (B65), so the rules are complete.

Keep the numbered-rule structure intact. It is what makes the rules quotable in review
("A9.1 rule 6", "A9.3.1") instead of vague.

**This is the single source of truth for how every section of this theme is built.** When a Figma
section / design is given, build it **exactly** as designed — but always using the tokens, headings,
buttons, spacing, colors and breakpoints defined below. Never invent a new value if a token already
exists.

Folder structure: A7.1.

`vendor.css` is closed to section code. Every new section gets its **own** file in
`assets/css/sections/` — see A9.6.1.

## A9.0 Component reference — the style guide page

The style guide page (A8, `page-templates/page-style-guide.php`) is the **living style guide**.
Every ready-made typography and button component is already built there, using the exact classes
and tokens from `vendor.css`.

**Workflow for every new section:**
1. **Open the style guide page first.** Find the heading / paragraph / button that matches the
   design.
2. **Copy that exact markup**, then add the section's own BEM class alongside it.
3. Only if nothing matches, build something new — and **add it to the style guide page too**.

### A9.0.1 Page index — every new page gets a row
The first section of the style guide page is `.pages-nav`, a table of every page in the build.
**Creating a page and adding its row happen in the same commit.** Keep `--num` sequential and
zero-padded; Page Name is the human name from the design; Link is the page's permalink.

List the components that exist on this project's style guide page here (from the built page).

## A9.1 Non-negotiable rules

1. **Design accuracy first.** Match the Figma design pixel-for-pixel — layout, order, spacing,
   sizes, colors, states. Do not "improve" or re-arrange it.
2. **Check the style guide page before writing anything.** Reuse what is there; add anything new to
   it.
3. **Use standard heading tags.** One `<h1>` per page, never skip levels. If the visual size ≠ the
   semantic level, keep the correct tag and override the size with a `--f*` token.
4. **Paragraphs use `<p>`.** Global `p` = `{{ --f<p> }}` / `{{ --font-body }}` (from TOKENS.md).
5. **Every value comes from a token.** No raw hex, no raw px font-sizes in section CSS.
6. **Every section gets its own parent class**, and every child element gets its own class.
7. **Write section CSS with native CSS nesting** under that one parent class (A9.6).
8. **Responsive only through the 7 fixed breakpoints** (A9.7). No custom breakpoints.
9. **No inline `style=""`** in templates (the style guide display-heading rows excepted). The only
   other exception is a dynamic value from the admin passed as a CSS variable (D103).
10. **All motion is {{ animation library — GSAP 3.12.5 + ScrollTrigger by default }},
    attribute-driven** — no custom scroll JS, no entrance keyframes in section CSS (A9.8).
11. **Accessibility is part of "done"**: landmarks, `alt` on every image, `aria-label` on icon-only
    controls, visible focus, keyboard-usable interactions.
12. **Every new section gets its own CSS file** at `assets/css/sections/<section>.css`, enqueued
    after `vendor.css` (A7.6), and its own render file at `template-parts/sections/<section>.php`.
13. **No icon fonts — every icon is an inline SVG** with `fill="currentColor"`.
14. **Section titles are always the `.section-title-wrap` block** — bare `<h2>`, no class, no `id`,
    and never any section CSS for the title (A9.3.1).
15. **Every new page gets a row in the style guide page index** (A9.0.1).
16. **All raster images are WebP and optimised.** Convert every JPG/PNG/GIF to `.webp`, compress
    it with no visible quality loss, keep transparency where the source has it, and size it to the
    largest size it is actually displayed at (2× for retina). Theme images go in `assets/images/`;
    content images are uploaded once to the Media Library (D105). Logos, icons and illustrations
    that are vector art stay SVG. Never ship the original JPG/PNG.
    **Images come from the Figma export only** — every image in the theme is the exported file from
    `Design/images/` (icons and logos inlined from `Design/svg/`). No stock photos, no AI images, no
    crops of the page PNG. **Images must never look blurry:**
    - Export from Figma at **2x** the size the image is shown on screen.
    - When converting to `.webp`, resize **down only** to 2x the largest display size. Never upscale.
    - WebP quality **85 or higher** for photos. Use lossless for images with text or sharp edges.
    - Icons, logos and line art stay **SVG**. Never convert them to PNG or WebP.
    - Set `width` / `height` on `<img>` from the design, and use `object-fit` so it is never stretched.
    - After each page, zoom the screenshot on every image. If any image looks soft, export it again
      at a higher size.
17. **Sliders follow the builder chosen in E1** (A1). ACF / Gutenberg / Both: Slick 1.8.1 with
    `data-slider` presets (A9.8.1), or Swiper — one library per site, initialised only in
    `sliders.js`; no `$(…).slick()` call outside `sliders.js`. Elementor / any other builder: the
    builder's own default slider widget, no new slider JS. Never Owl, Splide or hand-written slider JS.
18. **Every page has the DeveloperAI meta tag in `<head>`, by default.** It is output once, through
    the `wp_head` hook, as D112 defines — never hard-coded in a template. Copy it exactly — do not
    change the name or the content, and never show it on the page:
    ```html
    <meta name="DeveloperAI" content="Developer Skill">
    ```
19. **Text comes from `Design/CONTENT.md`.** Every heading, paragraph, label and menu item is the
    exact Figma text, delivered through fields / the importer — never retyped or invented (A2).

## A9.2 Colors (from `:root`)

Colour tables from TOKENS.md: Brand / Neutrals & Surfaces / Gradients / Role aliases, each row =
Token · Value · Use.

**Rule:** never write a hex in section CSS. If a design colour genuinely does not exist yet, add it
to `:root` first, then use it.

## A9.3 Typography

### Font families
Table from TOKENS.md: Token · Family · Used by.

### Type scale tokens
Fixed: list from TOKENS.md.
Fluid (`clamp`, auto-scales): table from TOKENS.md — Token · Range.
Display (stepped down per breakpoint, see A9.7): list from TOKENS.md.

### Line heights
`--bodyLh` = {{ from TOKENS.md }} (body) · `--lhTight` = {{ from TOKENS.md }} (all headings)

### Global heading defaults (already in `vendor.css` — do not duplicate)
Table from TOKENS.md: Tag · Size · Family · Weight.

### A9.3.1 Section titles — always `.section-title-wrap`

```html
<div class="section-title-wrap">
    <span class="title-eyebrow">Eyebrow</span>   <!-- optional -->
    <h2>Section Heading</h2>
    <div class="section-title-desc">             <!-- only when the design has copy -->
        <p>Supporting copy.</p>
    </div>
</div>
```

- The `<h2>` is **bare** — no class, no `id`. Never `.<section>__title`.
- Drop `.section-title-desc` entirely when there is no copy (in PHP: when the field is empty, print
  nothing — B23).
- **Never write CSS for a section title.** The centring, gap, bottom margin, eyebrow style, desc
  measure and the accent-coloured `<span>` are all already handled.
- Because the `h2` has no `id`, give the `<section>` an **`aria-label`**, not `aria-labelledby`.
- Headings *inside* a component (card title, panel heading) keep their own BEM class — this rule is
  about section-level headings only.

## A9.4 Buttons & links (global — reuse, never re-create)

From TOKENS.md and `vendor.css`: the `.btn` base spec, the variant table with default + hover, the
mobile behaviour, and the `.btn-link` spec.

### A9.4.1 Icons — inline SVG only
```html
<svg class="<section>__icon" width="20" height="20" viewBox="0 0 20 20" fill="none"
     xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <path d="…" fill="currentColor" />
</svg>
```
`fill="currentColor"` (or `stroke="currentColor"` for outlined icons) so the icon tracks the
parent's `color`. Explicit `width`/`height` + matching `viewBox`. Decorative → `aria-hidden="true"`;
icon-only control → `aria-label`. The SVG source is the file in `Design/svg/` (naming and
accessibility: D104).

**The `.btn-link` arrow is the exception** — it is global, on the `::after`. A section writes no
arrow markup and no arrow CSS. Never `content: none` it.

## A9.5 Spacing & layout

### Container
`--container` / `--gutter` table from TOKENS.md.

```html
<section class="about" id="about">
  <div class="container">…</div>
</section>
```

### Vertical rhythm
`--sectionGap` ({{ from TOKENS.md }}) is **always** the section's `padding-block`.
⚠️ `vendor.css` also sets `main section { margin-bottom: var(--sectionGap) }`, so the real gap
between two sections is **padding + {{ --sectionGap }} + padding**. Account for it before adding
more space.

### Spacing scale
`--sp*` list from TOKENS.md.
Spacing between blocks is set by the **parent** (`gap`) or a single-direction `margin-bottom` on the
child. Never mix `margin-top` + `margin-bottom` on siblings.

### Radius / Shadow / Z-index / Motion
Token lists from TOKENS.md.

## A9.6 Naming & CSS authoring

### A9.6.1 One CSS file per section
`assets/css/sections/<section>.css` — filename = the block class, lowercase, hyphenated.
One file = one section = one top-level selector + that section's media queries. Nothing else.
It is enqueued after `vendor.css` (A7.6). The section's markup lives in
`template-parts/sections/<section>.php` — same name as the CSS file.
Every section file starts with:
```css
/* =================================================================== ABOUT section css =================================================================== */
```

### A9.6.2 Class naming — BEM, per section
```
.<section>                block (the section parent)
.<section>__<element>     element
.<section>--<modifier>    block modifier
.<section>__<el>--<mod>   element modifier
```
The `<section>` element carries the block class **and** a matching `id` (the optional Section ID
field overrides the `id` when set, and a repeated section never prints a duplicate `id` — E1).
Every child that is positioned, sized or coloured gets a class — nothing is styled by bare tag name
inside a section (except `p` inside a repeating card, where a class would be pure noise).

### A9.6.3 Markup skeleton every section follows
The markup lives in `template-parts/sections/<section>.php`. This is the **rendered output** every
section must produce:

```html
<!-- ============ ABOUT ============ -->
<section class="about" id="about" aria-label="About {{ Project }}">
  <div class="container">
    <div class="section-title-wrap" data-anim="fade-up">
      <h2>About Us</h2>
    </div>
    <div class="about__grid" data-anim-stagger="fade-up" data-anim-stagger-gap="120">
      <article class="about__card">
        <span class="about__card-icon"><svg …>…</svg></span>
        <h3 class="about__card-title">Card Title</h3>
        <p class="about__card-text">Card copy.</p>
        <a class="btn-link btn-accent" href="#">Learn More</a>
      </article>
    </div>
  </div>
</section>
```

In the PHP file, every text, link and image comes from the section's fields (`$args`), escaped on
output (B23), and an empty field prints no empty markup. The seed values are the exact texts from
`Design/CONTENT.md`. Same section, as the render file:

```php
<?php
/**
 * Section: About — template-parts/sections/about.php
 * Values come in through $args (E1). Seed copy: Design/CONTENT.md.
 */
defined( 'ABSPATH' ) || exit;

$section_id = ! empty( $args['section_id'] ) ? $args['section_id'] : 'about';
$heading    = $args['heading'] ?? '';
$cards      = $args['cards'] ?? array();
?>
<!-- ============ ABOUT ============ -->
<section class="about" id="<?php echo esc_attr( $section_id ); ?>" aria-label="<?php echo esc_attr( $heading ); ?>">
	<div class="container">
		<?php if ( $heading ) : ?>
			<div class="section-title-wrap" data-anim="fade-up">
				<h2><?php echo esc_html( $heading ); ?></h2>
			</div>
		<?php endif; ?>

		<?php if ( $cards ) : ?>
			<div class="about__grid" data-anim-stagger="fade-up" data-anim-stagger-gap="120">
				<?php foreach ( $cards as $card ) : ?>
					<article class="about__card">
						<?php if ( ! empty( $card['title'] ) ) : ?>
							<h3 class="about__card-title"><?php echo esc_html( $card['title'] ); ?></h3>
						<?php endif; ?>
						<?php if ( ! empty( $card['text'] ) ) : ?>
							<p class="about__card-text"><?php echo esc_html( $card['text'] ); ?></p>
						<?php endif; ?>
						<?php if ( ! empty( $card['link']['url'] ) ) : ?>
							<a class="btn-link btn-accent" href="<?php echo esc_url( $card['link']['url'] ); ?>"><?php echo esc_html( $card['link']['title'] ); ?></a>
						<?php endif; ?>
					</article>
				<?php endforeach; ?>
			</div>
		<?php endif; ?>
	</div>
</section>
```

Semantic tags throughout. Every image is an optimised `.webp` (A9.1 rule 16) and gets `alt` +
`width`/`height`; below-fold images get `loading="lazy"`. Media Library images are printed with
`wp_get_attachment_image()` so `alt`, `width`, `height` and `srcset` come with them (B26).

### A9.6.4 CSS — one nested block per section
```css
.about {
    padding-block: var(--sectionGap);

    & .about__grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: var(--g40);
    }

    & .about__card {
        padding: var(--sp30);
        border: 1px solid var(--line);
        border-radius: var(--r-md);
        transition: var(--transition);

        &:hover { box-shadow: var(--shadow-lg); }
    }
}
```
One top-level selector per file. Prefix nested selectors with `&`. **Max 3 levels deep.** States
nest directly under the element they belong to, never in a separate block at the bottom.

### A9.6.5 Property order inside a rule
`position/inset` → `display/flex/grid` → `width/height` → `margin/padding` →
`background/border/radius/shadow` → `font/color/text` → `transition/transform/animation`

## A9.7 Breakpoints (the only ones allowed)

Mobile-first content, desktop-first media queries — always `max-width`, always `.98px`.

```css
@media (max-width: 1599.98px) { }   /* Large desktop */
@media (max-width: 1439.98px) { }   /* Desktop */
@media (max-width: 1365.98px) { }   /* Small desktop / large laptop */
@media (max-width: 1199.98px) { }   /* Laptop */
@media (max-width:  991.98px) { }   /* Tablet */
@media (max-width:  767.98px) { }   /* Mobile landscape / small tablet */
@media (max-width:  575.98px) { }   /* Mobile portrait */
```

### A9.7.1 What the global breakpoints already do
The filled-in step-down table from TOKENS.md (A5.8).

Any layout using `.container`, `--gutter`, `--sectionGap`, `--g40` or the type tokens responds
automatically — **do not re-declare those values in a section.**

### A9.7.2 Where section responsive CSS goes
At the **bottom of that section's own file**, under a comment banner, ordered largest → smallest.
Only write a breakpoint the design actually changes at. Empty queries are not written.

## A9.8 Animation — GSAP 3.12.5 + ScrollTrigger (attribute-driven)

**Setup** — enqueued on every page by `inc/enqueue.php` (A7.6), in the footer, `defer`:
`gsap.min.js` → `ScrollTrigger.min.js` (both `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/`)
→ `assets/js/vendor.js` → `assets/js/sliders.js` (slider builds, A9.8.1) → `assets/js/animations.js`.

**Attribute API** — the only animation interface:

| Attribute | On | Effect |
|---|---|---|
| `data-anim="<preset>"` | any element | Animates the element in when it scrolls into view |
| `data-anim-delay="200"` | with `data-anim` | Delay in ms (keep < 400) |
| `data-anim-duration="700"` | with any `data-anim*` | Duration in ms (400–900) |
| `data-anim-stagger="<preset>"` | a parent | Animates each direct child in turn |
| `data-anim-stagger-gap="120"` | with `data-anim-stagger` | Gap between children in ms |
| `data-anim-counter="579"` | a number element | Counts up to the value; put the same final number in the markup text |

**Valid presets — no others exist:** `fade-up`, `fade-down`, `fade-left`, `fade-right`, `fade-in`,
`zoom-in`. A new preset is added to `PRESETS` in `animations.js` **and** to this list, never inline.

Every animation in `Design/ANIMATIONS.md` is mapped to one of these presets (A2 step 4). Figma hover
and pressed variants are built as CSS `:hover` / `:focus-visible` / `:active` states with
`--transition`, not as `data-anim`.

### Animation rules
1. **Attributes only.** No custom scroll JS, no `IntersectionObserver`, no entrance keyframes in a
   section file. Hover/focus micro-transitions still use `--transition`.
2. Animation attributes live in the **markup** (the section's PHP render file), never in section
   CSS.
3. One animation per element — don't nest an animated element inside a stagger parent that targets it.
4. Prefer **stagger on the parent** over per-child delays.
5. Durations **400–900ms**, delays under **400ms**. A stagger over many children blows past that —
   animate the container as one block instead.
6. **Never animate above-the-fold LCP content** with a delay.
7. **No GSAP code outside `animations.js`** — never call `gsap` from a section file or inline script.
8. Decorative motion must not carry meaning. The page must work with JS off.
9. Never put `opacity`/`transform` start states (e.g. `opacity: 0`) for entrances in section CSS.
   `animations.js` already turns off an animated element's CSS `transition` while it plays and
   clears it afterwards, so cards keep `transition: var(--transition)` for hover.
10. **Sliders:** put `data-anim` on the slider wrap (`[data-slider-wrap]`) and animate it as one
    block. Never put `data-anim-stagger` on a `[data-slider]` track or `data-anim` on a slide.
    Slick moves the slides into its own wrappers and clones them, so the stagger breaks.

### A9.8.1 Sliders — Slick 1.8.1 (attribute-driven)

**Setup — builds with a slider:** `slick.min.css` enqueued before `vendor.css`, and WordPress core
`jquery` → `slick.min.js` (1.8.1) before `vendor.js`. Slick files from
`https://cdnjs.cloudflare.com/ajax/libs/`. Then `assets/js/sliders.js`, before `animations.js`
(all in A7.6). Never load `slick-theme.css`. Never load a second jQuery.

**Attribute API** — the only slider interface:

| Attribute | On | Effect |
|---|---|---|
| `data-slider="<preset>"` | the track (its direct children are the slides) | Builds a Slick slider |
| `data-slider-wrap` | the parent holding the track, arrows and dots | Scopes where arrows/dots are found |
| `data-slider-prev` / `data-slider-next` | two `<button>`s inside the wrap | Arrows. No buttons → no arrows |
| `data-slider-pagination` | an empty `<div>` inside the wrap | Where the dots go (default: after the track) |
| `data-slider-dots="true\|false"` | with `data-slider` | Turns dots on/off for this slider |
| `data-slider-autoplay="4000"` | with `data-slider` | Autoplay interval in ms; `"false"` turns it off |
| `data-slider-loop="false"` | with `data-slider` | Stops at the first/last slide (arrows get `.slick-disabled`) |
| `data-slider-fade="true"` | with a 1-per-view preset | Crossfade instead of slide |

**Valid presets — no others exist:**

| Preset | Desktop → ≤1199 → ≤991 → ≤767 → ≤575 | Defaults |
|---|---|---|
| `cards` | 3 → 2 → 2 → 1 → 1 | no dots, no autoplay |
| `logos` | 6 → 5 → 4 → 3 → 2 | autoplay 3000ms |
| `single` | 1 | dots, adaptive height (testimonials, hero, gallery) |

A new preset goes into `PRESETS` in `sliders.js`, the `--per-view` rules in the `vendor.css`
SLIDERS block **and** this table, never inline.

**Markup** (rendered output of `template-parts/sections/testimonials.php`):

```html
<section class="testimonials" id="testimonials" aria-label="Testimonials">
  <div class="container">
    <div class="section-title-wrap" data-anim="fade-up">
      <h2>What Clients Say</h2>
    </div>
    <div class="testimonials__slider" data-slider-wrap data-anim="fade-up">
      <div class="testimonials__track" data-slider="cards">
        <article class="testimonials__card">…</article>
        <article class="testimonials__card">…</article>
        <article class="testimonials__card">…</article>
        <article class="testimonials__card">…</article>
      </div>
      <div class="slider-nav testimonials__nav">
        <button class="slider-arrow" type="button" data-slider-prev aria-label="Previous slide">
          <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M15 18l-6-6 6-6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
        </button>
        <button class="slider-arrow" type="button" data-slider-next aria-label="Next slide">
          <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M9 6l6 6-6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
        </button>
      </div>
    </div>
  </div>
</section>
```

### Slider rules
1. **Attributes only.** Never call `$(…).slick()` or pass Slick options from a section file or
   inline script. Everything goes through `sliders.js`.
2. Slides are the **direct children** of the `[data-slider]` track. Don't wrap them in extra
   `<div>`s.
3. The section CSS styles the **card** and **where** the arrows/dots sit (`.testimonials__nav`).
   Arrow and dot looks come from the global `.slider-arrow` / `.slick-dots`. Never re-style
   `.slick-list`, `.slick-track` or `.slick-slide`.
4. Arrows are always inline-SVG `<button type="button">`s with an `aria-label`. Never use Slick's
   default text arrows or an icon font.
5. Slide gap comes from `--slider-gap` (default `var(--g40)`). Override it on the track in the
   section CSS if the design differs. Never put a margin on the slide.
6. Images in a slider get `width`/`height` but **not** `loading="lazy"`. Off-screen slides are
   inside an `overflow: hidden` list, so lazy images load late and flash blank when they slide in.
   (`wp_get_attachment_image()` adds `loading="lazy"` by default — pass `'loading' => false` for
   slider images.)
7. Autoplay must pause on hover/focus (default). It is switched off for `prefers-reduced-motion`.

## A9.9 Section delivery checklist

- [ ] Design matches the Figma frame (`Design/pages/`) at every breakpoint
- [ ] The style guide page was checked first and its components reused
- [ ] Any genuinely new component was added to the style guide page
- [ ] A new page has its row in the `.pages-nav` table (A9.0.1)
- [ ] Section has its **own file** `assets/css/sections/<section>.css`, enqueued after `vendor.css`
      (A7.6), and its own render file `template-parts/sections/<section>.php`
- [ ] `vendor.css` untouched
- [ ] Section has its own block class **and** matching `id`; every child has a BEM class
- [ ] Correct semantic heading order; one `h1` per page
- [ ] Section title is `.section-title-wrap` — bare `h2`, no class, no `id`, no title CSS (A9.3.1)
- [ ] Section uses `aria-label` (not `aria-labelledby`)
- [ ] Zero raw hex, zero raw px font-sizes, zero magic numbers — tokens only
- [ ] `padding-block: var(--sectionGap)` on the section
- [ ] CSS is one nested block with `&`, max 3 levels, comment banner at the top
- [ ] Responsive queries at the bottom of the same file, largest → smallest
- [ ] Every raster image is an optimised `.webp` — no JPG/PNG/GIF shipped
- [ ] Every image comes from the Figma export (`Design/images/`, `Design/svg/`) and is sharp, not blurry
- [ ] Every text comes from `Design/CONTENT.md` — nothing retyped or invented
- [ ] Every Figma animation for this section in `Design/ANIMATIONS.md` is built
- [ ] Images have `alt` + `width`/`height`; below-fold images `loading="lazy"`
- [ ] Every icon is an inline `<svg>` with `currentColor` — no icon fonts
- [ ] Hover **and** `:focus-visible` states work; keyboard reachable
- [ ] Motion via `data-anim*` attributes only, valid presets only; GSAP, ScrollTrigger and
      `animations.js` enqueued (A7.6)
- [ ] Any slider uses `data-slider` with a valid preset (A9.8.1). Slick CSS, jQuery, Slick and
      `sliders.js` are enqueued. Arrows are SVG buttons. It works by swipe and keyboard at every
      breakpoint.
- [ ] Every field is escaped; an empty field prints no empty markup (B23)
- [ ] No inline `style` attributes (D103 CSS-variable exception only)
- [ ] No horizontal scroll at 320px
- [ ] The page `<head>` has `<meta name="DeveloperAI" content="Developer Skill">` exactly once (A9.1
      rule 18, D112)

---

# A10. Design guardrails

Things that go wrong if you skip them.

| Trap | What to do instead |
|---|---|
| Inventing a token because the design "probably" wants one | Only tokens that appear in the Figma buckets. Ask if unsure. |
| Naming type tokens `--f-sm` / `--f-lg` | Name == value: `--f24` is 24px. Relative names rot the moment a size is added. |
| Writing section CSS into `vendor.css` "just this once" | One file per section. No exceptions — this is how the global file stays reviewable. |
| A `--x-dark` token aliased to `--x` | A hover token that equals its base produces no visible change. Give it a real darker value. |
| Re-styling a section title | `.section-title-wrap` handles it. A `__title` rule is a bug, not a customisation. |
| Copying a hex from Figma into a section file | Add it to `:root` first, then reference the token. |
| Custom breakpoints because "the design breaks at 1100" | Use the nearest of the 7. Adjust the layout, not the breakpoint list. |
| Stagger across 20+ children | Total delay blows past 400ms. Animate the container as one block. |
| Naming a class after a colour (`.btn-orange`, `.text-blue`) | Name by role: `.btn-accent`, `.heading-accent`. Colour names break on a rebrand. |
| Inventing a CDN URL, animation attribute or preset | Use only the pinned URLs and presets in A7.6 / A9.8. Ask for docs before adding a library. |
| Reaching for Owl / Splide or writing a slider by hand | ACF / Gutenberg / Both: Slick 1.8.1 through `data-slider` (A7.5 / A9.8.1) or Swiper, only in `sliders.js`. Elementor / other builders: the builder's own slider widget (A1). |
| Slider shows stacked slides or nothing at all | Slick not enqueued, `{PREFIX}_HAS_SLIDER` is `false`, or Slick loads after `sliders.js`. Check the A7.6 order and dependencies. |
| `data-anim-stagger` on a slider track | Slick clones and re-wraps the slides. Animate the `[data-slider-wrap]` as one block. |
| Hard-coded `<script>` / `<link>` tags in `header.php`, `footer.php` or a template | Enqueue everything in `inc/enqueue.php` with dependencies (A7.6). |
| Loading a second jQuery from cdnjs, or deregistering core jQuery | Use the WordPress `jquery` handle. Two copies break plugins (A7.6, B21). |
| Mobile text shrinking below 14px | Respect the A5.8 minimums after every step-down. |
| Building any template or section before the style guide page exists | The style guide page is what stops page 2 from re-inventing page 1 (A8). |
| Skipping the A6 approval gate | Every later file is built on those numbers. Confirm once, cheaply. Nothing WordPress-related before approval. |
| Dropping a Figma JPG/PNG export straight into `assets/images/` or the Media Library | Convert it to `.webp`, compress it and resize it first — resize down only, to 2x display size, quality 85+, so it never goes blurry. Vector art stays SVG. |
| Figma export timed out, so the image was skipped or replaced with a placeholder | Retry 3 times, then export in smaller parts. Every node in `EXPORT-LOG.md` must say `ok` (A2). |
| Blurry image on the page | Export at 2x, resize down only, WebP quality 85+, vectors stay SVG (A9.1 rule 16). |
| Retyping copy from a screenshot, or writing "Lorem ipsum" / made-up copy | Use the exact text from `Design/CONTENT.md` (A2 step 5). Missing text → `MISSING — ask`. |
| Figma prototype animation ignored | Write it in `Design/ANIMATIONS.md` and map it to an A9.8 preset (A2). |
| A page without the DeveloperAI meta tag | Every page `<head>` gets `<meta name="DeveloperAI" content="Developer Skill">` once, through the `wp_head` hook (A9.1 rule 18, D112). |

---

# B1. PRIMARY OBJECTIVE

Build the **Figma design** (exported into `Design/`, see A2) as a proper custom WordPress theme,
using the approved `TOKENS.md` and the front-end system and rules of Part A (A7–A9).

The final result must:

- Match the Figma design (frames in `Design/pages/`, components in `Design/components/`).
- Build the markup with the Part A naming rules (BEM section classes, A9) wherever practical.
- Build all CSS on the token system (`vendor.css` tokens + one file per section, A7/A9).
- Build JavaScript with the Part A front-end system (`vendor.js`, `sliders.js`, `animations.js`).
- Build the animations listed in `Design/ANIMATIONS.md` (A9.8).
- Match the responsive behavior of the Figma frames at every breakpoint.
- Use only the images and fonts exported from Figma (`Design/images/`, `Design/svg/`, `TOKENS.md` fonts).
- Use only the third-party libraries Part A defines (jQuery, Slick, GSAP, ScrollTrigger …) or that the design truly needs.
- Build every function the design shows (menus, forms, sliders, filters, search …).
- Make appropriate content dynamic WordPress content.
- Create a clean and maintainable WordPress theme.
- Follow WordPress/PHP best practices.
- Avoid PHP warnings, notices, fatal errors, broken queries, broken URLs, broken assets, and JavaScript errors.
- Never redesign the website unless explicitly requested.

The goal is:

```text
Figma Design (Design/ export + approved TOKENS.md)
        ↓
Analyze Complete Design
        ↓
Plan WordPress Architecture
        ↓
Custom WordPress Theme
        ↓
Same Frontend as the Figma Design
        +
Proper WordPress CMS
```

---

# B2. VERY IMPORTANT: INSPECT FIRST, CODE SECOND

Before creating or modifying any PHP file:

## Step 1 — Inventory the complete design

Inspect the full `Design/` export (A2) and the approved `TOKENS.md`:

```text
Figma pages / frames (Design/pages/) at every breakpoint
Figma components (Design/components/)
Design/CONTENT.md (all text copy)
Design/ANIMATIONS.md (animations / interactions)
Images (Design/images/)
SVGs / icons (Design/svg/)
Fonts (TOKENS.md)
Videos
Colours, type scale, spacing (TOKENS.md)
Third-party libraries / integrations the design needs
Plugins/scripts
Forms
API integrations
Animations
Responsive frames (1600 / 900 / 375)
```

Do not inspect only the Home frame.

The whole design must be understood before building.

---

# B3. CREATE A SITE INVENTORY

Before coding, identify:

### Pages

Example (Figma page frames in `Design/pages/`):

```text
Home
About
Services
Contact
Partners
Partner Detail
Blog
Blog Detail
```

### Common components

Identify (from `Design/components/` and repeated parts across frames):

```text
Header
Desktop Navigation
Mobile Navigation
Footer
Footer Navigation
Announcement Bar
CTA
Breadcrumb
Cookie banner
Modals
Forms
Sliders
Cards
```

### Dynamic-looking content

Identify content that should become WordPress-managed:

```text
Partners
Blog Posts
Menus
Logo
Footer content
Social links
CTA
Contact details
Page content
Images
Categories
Tags
```

---

# B4. HEADER MUST BE INSPECTED BEFORE BUILDING

## DO NOT create a generic WordPress header.

First inspect the Figma header frame(s) at every breakpoint (1600 / 900 / 375) and the header
component(s) in `Design/components/`.

Analyze:

- Exact layer structure
- Header naming (map to Part A BEM classes)
- Logo structure
- Desktop menu
- Mobile menu
- Dropdowns
- Mega menu
- CTA buttons
- Search
- Icons
- Sticky behavior
- Header states
- Scroll behavior
- Mobile breakpoint
- Header JavaScript (interactions in `Design/ANIMATIONS.md`)
- Header CSS (tokens from `TOKENS.md`)
- Header animations
- Header assets

Then build that exact structure into:

```text
header.php
```

### Rule

Build with, and keep stable:

```text
layer structure from Figma
class names (Part A BEM naming)
IDs where necessary
CSS hooks
JS hooks
data attributes (data-anim, data-slider)
ARIA attributes
responsive behavior
```

Only content that admins must edit becomes WordPress dynamic functionality.

---

# B5. FOOTER MUST BE INSPECTED BEFORE BUILDING

## DO NOT create a generic WordPress footer.

First inspect the Figma footer frame(s) at every breakpoint (1600 / 900 / 375) and the footer
component(s) in `Design/components/`.

Analyze:

- Footer layer structure
- Footer columns
- Footer navigation
- Logo
- Description
- Contact details
- Social links
- Newsletter
- CTA
- Legal links
- Copyright
- Footer scripts
- Footer CSS
- Responsive behavior

Then build it into:

```text
footer.php
```

Match the Figma structure and styling.

---

# B6. HEADER DYNAMIC REQUIREMENTS

The header should support WordPress management where applicable.

Possible dynamic fields:

```text
Logo
Mobile Logo
Primary Menu
CTA Text
CTA URL
CTA visibility
Announcement bar
Announcement text
Announcement URL
Search
```

Do not create fields that are not required by the Figma design.

---

# B7. WORDPRESS MENUS

Register WordPress menu locations based on the actual Figma design.

Typical:

```php
register_nav_menus([
    'primary' => __('Primary Menu', 'site-theme'),
    'footer'  => __('Footer Menu', 'site-theme'),
]);
```

If the mobile menu uses the same navigation, do not duplicate the menu unnecessarily.

Use:

```php
wp_nav_menu()
```

instead of hard-coded navigation links.

### Keep

- Menu classes (Part A naming) where possible
- Dropdown behavior
- Markup hooks the JS uses
- Mobile menu behavior
- Active states

If custom walker logic is required to reproduce the Figma menu structure, create a proper custom Walker instead of hacking the output with JavaScript.

---

# B8. FOOTER DYNAMIC REQUIREMENTS

Use WordPress settings/custom fields only for content that needs admin control.

Potential settings:

```text
Footer Logo
Footer Description
Email
Phone
Address
Social Links
Copyright
Footer CTA
```

Footer menu should use a registered WordPress menu.

---

# B9. WORDPRESS THEME STRUCTURE

> **Elementor Pro chosen in E1:** header, footer, pages, singles and archives are Theme Builder templates (E5.9, E5.11, E5.15, E5.16, E5.24); the theme keeps only setup, enqueue and CPT files plus the E5.19 assets — not the PHP page templates below.

Create a proper custom theme in `wp-content/themes/<project-slug>/`.

Recommended:

```text
<project-slug>/
│
├── style.css
├── functions.php
├── index.php
├── front-page.php
├── home.php
├── page.php
├── single.php
├── archive.php
├── category.php
├── tag.php
├── search.php                  ← Search Results, as per design (D106)
├── searchform.php              ← the site search form, submits to search.php (D106)
├── 404.php                     ← 404 page, as per design (D106)
├── header.php
├── footer.php
│
├── single-partner.php
├── archive-partner.php
│
├── page-templates/
│   └── page-sitemap.php        ← HTML Sitemap page, dynamic (D107)
│
├── template-parts/
│   ├── header/
│   ├── footer/
│   ├── content/
│   ├── blog/
│   └── partner/
│
├── acf-json/                   ← ACF field groups as JSON, synced (D111)
│
├── inc/
│   ├── theme-setup.php
│   ├── enqueue.php
│   ├── post-types.php
│   ├── taxonomies.php
│   ├── theme-settings.php
│   ├── custom-fields.php
│   ├── template-functions.php
│   └── helpers.php
│
└── assets/
    ├── css/
    │   ├── vendor.css          ← tokens + global components (A7.2)
    │   └── sections/           ← one <section>.css per section
    ├── js/
    │   ├── vendor.js
    │   ├── sliders.js          ← Slick (A7.5)
    │   └── animations.js       ← GSAP (A7.4)
    ├── images/                 ← WebP + SVG from Design/
    ├── fonts/
    └── vendor/
```

Do not put the entire project into one huge `functions.php`.

---

# B10. FUNCTIONS.PHP

Keep `functions.php` lightweight.

Example:

```php
require_once get_template_directory() . '/inc/theme-setup.php';
require_once get_template_directory() . '/inc/enqueue.php';
require_once get_template_directory() . '/inc/post-types.php';
require_once get_template_directory() . '/inc/taxonomies.php';
require_once get_template_directory() . '/inc/theme-settings.php';
require_once get_template_directory() . '/inc/custom-fields.php';
require_once get_template_directory() . '/inc/template-functions.php';
require_once get_template_directory() . '/inc/helpers.php';
```

Use `require_once`.

---

# B11. PARTNERS — CUSTOM POST TYPE

> **Elementor Pro chosen in E1:** register the CPT the same way; its single and archive are Theme Builder templates (E5.15) — no `archive-partner.php` / `single-partner.php`.

If the Figma design has dedicated Partner pages, create:

```text
partner
```

as a Custom Post Type.

Expected admin:

```text
Partners
├── All Partners
└── Add New Partner
```

Expected URLs:

```text
/partners/
/partners/partner-name/
```

Register with proper:

```text
labels
public
has_archive
rewrite
supports
show_in_rest
```

Use the WordPress template hierarchy.

Required:

```text
archive-partner.php
single-partner.php
```

---

# B12. PARTNER FIELDS

Inspect the Figma Partner frames first (listing + detail, every breakpoint).

Only then determine required fields.

Possible:

```text
Partner Name
Partner Logo
Hero Image
Short Description
Content
Website URL
Location
Partner Type
Gallery
CTA Text
CTA URL
Featured Partner
```

Do not blindly create all fields.

Use ACF PRO for fields (D110 — never SCF, never hand-made meta boxes); register field groups as
local JSON (D111).

Do not recreate ACF functionality manually.

---

# B13. PARTNER TAXONOMIES

Only create taxonomies if the website actually needs them.

Possible:

```text
Partner Type
Partner Industry
Partner Location
```

Do not create unnecessary database structures.

---

# B14. BLOG

> **Elementor Pro chosen in E1:** native Posts as below; archive and single are Theme Builder templates (E5.16).

Blog must remain separate from Partners.

Use the native WordPress Post system:

```text
post
```

Do NOT create a `blog` CPT unless there is a specific requirement.

Use:

```text
home.php
single.php
archive.php
category.php
tag.php
```

based on the Figma design.

---

# B15. BLOG FEATURES

Build the blog features the Figma design shows:

```text
Title
Featured Image
Content
Author
Publish Date
Categories
Tags
Related Posts
Pagination
Search
```

Only implement features that exist in the design or are required.

---

# B16. STANDARD WORDPRESS PAGES

Use native Pages for normal website content:

```text
About
Services
Contact
Privacy
Terms
Other static pages
```

Use:

```text
page.php
```

or specific templates only when necessary.

---

# B17. HOMEPAGE

> **Elementor Pro chosen in E1:** Home is a normal page built with Elementor and set as the static front page — no `front-page.php`.

**ACF Flexible Content / Gutenberg (ACF Blocks) / Both chosen in E1:** use:

```text
front-page.php
```

The homepage must be built from the actual Figma Home frames (1600 / 900 / 375).

Do not redesign.

Homepage sections are built with the page builder chosen in E1. For ACF / Gutenberg / Both,
`front-page.php` loops the sections; it does not hardcode them.

If homepage sections contain dynamic data, connect them to:

```text
Pages
Partners
Posts
Theme Settings
Custom Fields
```

as appropriate.

---

# B18. TEMPLATE HIERARCHY

> **Elementor Pro chosen in E1:** Theme Builder templates + display conditions (E5.24) replace this PHP template hierarchy.

Follow WordPress template hierarchy.

Preferred examples:

```text
front-page.php
page.php
page-{slug}.php

home.php

single.php
single-partner.php

archive.php
archive-partner.php

category.php
tag.php

search.php
404.php
```

Do not create duplicate templates without a reason.

`page-{slug}.php` only when a page needs a different wrapper. Page content always comes from the
page builder chosen in E1 — never hardcoded in a template.

---

# B19. ASSETS

All assets from the `Design/` export must be audited.

Put theme assets into:

```text
assets/
```

For example:

```text
assets/css/
assets/js/
assets/images/
assets/fonts/
assets/vendor/
```

Use WordPress functions to reference them.

Example:

```php
get_template_directory_uri()
```

Do not use fragile relative paths such as:

```text
../../images/
```

---

# B20. CSS

> **Elementor Pro chosen in E1:** CSS follows **E5.18** — cover everything with native Elementor controls first; only when a native control cannot do it, add a CSS class on that element with its rule in `custom.css`.

> **ACF Flexible Content / Gutenberg (ACF Blocks) / Both chosen in E1:** do not rewrite CSS for its
> own sake — reuse the `vendor.css` tokens and the existing global components, and only add what the
> section really needs (D103).

## CRITICAL

All CSS is built on the token system of Part A: `assets/css/vendor.css` holds the tokens from the
approved `TOKENS.md` and the global components (A7.2); each section has its own file in
`assets/css/sections/<section>.css`. Follow the front-end rules in A9.

Keep consistent with the Figma design and `TOKENS.md`:

- Selectors (Part A BEM naming)
- Classes
- Spacing
- Typography
- Breakpoints
- Animations
- Responsive behavior

Do not add global styles that can break other components.

Do not use `!important` unless genuinely necessary.

Do not introduce a CSS framework simply because WordPress is being introduced.

For section classes, CSS variables, global spacing and no inline styles, see **D103**.

---

# B21. JAVASCRIPT

Plan all JavaScript before writing it.

Check what the design needs:

```text
Navigation
Mobile menu
Sliders
Tabs
Accordions
Modals
Animations
GSAP
ScrollTrigger
Three.js
AJAX
Forms
API integrations
Lazy loading
Custom interactions
```

Use the Part A front-end system (`vendor.js`, `sliders.js` A7.5, `animations.js` A7.4).

Load scripts with:

```php
wp_enqueue_script()
```

in `functions.php` / `inc/enqueue.php`, in the order A7.6 defines
(jQuery → Slick → GSAP → ScrollTrigger → sliders.js → animations.js). Never hard-code `<script>` tags.

Do not duplicate libraries.

Respect dependencies and execution order.

---

# B22. GSAP / THREE.JS / ANIMATIONS

> **Elementor Pro chosen in E1:** see **E5.20**.

Animations come from `Design/ANIMATIONS.md`, mapped to `data-anim` presets (A9.8) and run by
`animations.js` (A7.4).

If the design uses:

```text
GSAP
ScrollTrigger
Three.js
Swiper
AOS
or other libraries
```

use the Part A setup (GSAP + ScrollTrigger, Slick). Do not add a different library for an effect
the Part A presets already cover.

Do not replace a working animation with a different implementation unless necessary.

After building verify:

- animation timing
- scroll triggers
- canvas rendering
- model loading
- texture loading
- responsive behavior
- mobile behavior
- console errors

---

# B23. PHP SECURITY

All output must be escaped.

Use:

```php
esc_html()
esc_attr()
esc_url()
wp_kses_post()
```

Sanitize incoming values.

Use nonces for custom admin forms.

Use capability checks.

Never trust:

```text
$_GET
$_POST
$_REQUEST
custom fields
user input
```

without validation/sanitization.

---

# B24. DATABASE / QUERY SAFETY

Use WordPress APIs and `WP_Query` where possible.

Example:

```php
$query = new WP_Query([
    'post_type'      => 'partner',
    'posts_per_page' => 12,
]);
```

Do not write raw SQL unless there is a real requirement.

If raw SQL is required:

- Use `$wpdb`
- Use `$wpdb->prepare()`
- Never concatenate user input into SQL
- Validate input
- Escape output

Avoid N+1 queries.

Do not run unnecessary database queries on every page.

---

# B25. WORDPRESS LOOP

Use proper loops.

Example:

```php
if (have_posts()) :
    while (have_posts()) :
        the_post();

        // Template output

    endwhile;
endif;
```

For custom queries:

```php
$query = new WP_Query($args);

if ($query->have_posts()) :
    while ($query->have_posts()) :
        $query->the_post();

        // Output

    endwhile;
endif;

wp_reset_postdata();
```

Never forget:

```php
wp_reset_postdata();
```

after custom loops.

---

# B26. IMAGE HANDLING

> **See also:** image accessibility, names and alt text → D104; upload every image once → D105.

Use only the images exported from Figma (`Design/images/`, `Design/svg/`). They must be sharp,
never blurry: export at 2x, resize down only, WebP at quality 85+, vectors stay SVG (A9.1 rule 16).

Use WordPress image functions.

Prefer:

```php
the_post_thumbnail()
wp_get_attachment_image()
get_the_post_thumbnail_url()
```

Do not hard-code uploaded image URLs.

Register image sizes if needed.

Do not unnecessarily load full-resolution images.

---

# B27. RESPONSIVE DESIGN

The WordPress version must match the responsive behavior of the Figma frames (1600 / 900 / 375).

Test:

```text
Mobile
Tablet
Laptop
Desktop
Large Desktop
```

Verify:

```text
Header
Menu
Hero
Cards
Images
Partner pages
Blog pages
Footer
Forms
Animations
```

---

# B28. FORMS

Inspect every form in the Figma design.

Determine whether it needs:

```text
HTML
PHP
AJAX
API
CRM
Third-party service
WordPress plugin
```

If the client has an existing backend integration (CRM, API, third-party service), do not change it
without understanding it.

Forms must be:

- Secure
- Validated
- Responsive
- Working
- Spam-protected where appropriate

Build every form with the forms plugin the user chose at the start — Gravity Forms, Contact
Form 7 or (Elementor builds) the Elementor Pro Form widget (D109).

---

# B29. SEO

Build SEO in from the start (and keep any existing SEO if the client has an old live site).

Check:

```text
Title
Meta Description
Canonical
H1
H2/H3 structure
Image ALT
Open Graph
Schema
Sitemap
Robots
URLs
Redirects
```

If an SEO plugin exists, integrate with it instead of duplicating its functionality.

---

# B30. URL MIGRATION

Only when the client has an old live site that this build replaces. Otherwise skip this section.

Create an old-to-new URL mapping.

Example:

```text
/about.html
→
/about/

/partners.html
→
/partners/

/partner-a.html
→
/partners/partner-a/

/blog.html
→
/blog/

/blog-post.html
→
/blog/blog-post/
```

Do not blindly change URLs.

Preserve existing production URLs whenever practical.

Where URLs must change, implement 301 redirects.

---

# B31. 404 AND REDIRECTS

> **Elementor Pro chosen in E1:** 404 and Search are Theme Builder templates (E5.29) — no `404.php` / `search.php`. Redirects below still apply.

Create:

```text
404.php
```

Build `404.php` (and `search.php`) **as per the design**, not as a default theme page. See D106.

If the client has an old live site (B30), review old URLs and add required redirects.

Do not create redirect loops.

Test redirects.

---

# B32. WORDPRESS SETTINGS

Use native WordPress functionality where possible.

Do not build a custom setting for something WordPress already manages.

Use custom Theme Settings only for site-specific fields such as:

```text
Header Logo
Header CTA
Footer Contact
Social Links
Footer Copyright
```

---

# B33. ADMIN UX

The WordPress backend should be understandable.

Recommended:

```text
Dashboard

Posts
Media
Pages
Partners
Comments

Appearance
    Menus / Navigation
    Theme Settings

Users
Tools
Settings
```

Partner editors should not need to edit PHP or HTML.

---

# B34. USE ONLY THE BUILDER CHOSEN IN E1

The theme is always a proper custom theme (or child theme, B55) — never a generic theme with the
design pasted in (B35). **How the pages are built is the E1 answer**, not a default.

Build page sections only with the page builder the user chose in E1 (ACF Flexible Content, Gutenberg
ACF Blocks, Both, Elementor Pro, or another builder — E2–E6). Do not mix in a second builder.

Do not build sections as Elementor widgets unless Elementor Pro was chosen in E1.

The Figma design (with the approved `TOKENS.md`) stays the source of truth for the frontend.

---

# B35. DO NOT USE A GENERIC THEME

Do not install a generic theme and place the design inside it.

Build a proper custom theme from the Figma design and the Part A front-end system.

---

# B36. NO REDESIGN RULE

This project builds the Figma design as it is. It is not a redesign. Match the Figma frames.

Do NOT:

- Change colors
- Change typography
- Change spacing
- Change layout
- Change animations
- Change component appearance
- Replace icons
- Replace images
- Replace navigation
- Change responsive breakpoints

unless explicitly required.

---

# B37. CODE QUALITY

Write production-quality PHP.

Requirements:

- Clean naming
- Small reusable functions
- Proper comments for complex logic
- No dead code
- No duplicated code
- No unnecessary globals
- No debug output
- No `var_dump()`
- No `print_r()`
- No test code in production
- No hard-coded environment-specific URLs
- No hard-coded database credentials

---

# B38. DEBUGGING

During development, WordPress debugging may be enabled.

Use:

```php
WP_DEBUG
WP_DEBUG_LOG
```

where appropriate.

Do not display PHP errors to end users in production.

Before completion:

- Fix PHP warnings
- Fix PHP notices
- Fix deprecated calls
- Fix fatal errors
- Fix JS console errors
- Fix broken links
- Fix missing assets

---

# B39. ACCESSIBILITY

Build in and improve:

```text
Semantic HTML
ARIA attributes
Keyboard navigation
Focus states
Alt text
Form labels
Button labels
Link labels
Heading hierarchy
```

Do not leave out useful accessibility attributes the Part A components define.

---

# B40. PERFORMANCE

Optimize without changing the design.

Check:

```text
CSS loading
JS loading
Image sizes
Lazy loading
Font loading
Third-party scripts
Duplicate libraries
Database queries
```

Do not load every library on every page if it is only required on a specific page.

Example:

```text
Three.js
→ load only on pages that require Three.js
```

---

# B41. THIRD-PARTY SCRIPTS

Inventory every external script the project needs (from the client brief, the old live site if any,
and the design).

Examples:

```text
Google Analytics
Tag Manager
Maps
Chat widgets
Tracking
CDNs
Libraries
APIs
```

Do not remove them without understanding their purpose.

Use WordPress enqueue/hooks appropriately.

---

# B42. NO HARDCODED DOMAIN

Do not use:

```text
https://example.com/
```

inside theme code for internal links.

Use:

```php
home_url()
site_url()
get_permalink()
get_template_directory_uri()
```

where appropriate.

---

# B43. INTERNAL LINKS

> **See also D115:** every clickable element must be linked (link map, no `#` / empty links).

Every internal link (buttons, cards, CTAs drawn in Figma) points to a WordPress permalink when the
destination is a WordPress page. Never use static links like:

```html
<a href="/about.html">
```

Prefer:

```php
get_permalink($page_id)
```

or:

```php
get_permalink()
```

Do not hard-code URLs unnecessarily.

---

# B44. MENU LINK HANDLING

> **See also D115** (menus and every other link set, final link check).

The WordPress menu should handle:

```text
Pages
Custom Links
Partner Archive
Blog
Categories
```

Do not hard-code the navigation in `header.php`.

---

# B45. PARTNER ARCHIVE

> **Elementor Pro chosen in E1:** Theme Builder Archive template + Loop Grid (E5.15) — not `archive-partner.php`.

Partner archive should reproduce the Figma partner listing design.

Use:

```text
archive-partner.php
```

Match:

```text
Cards
Images
Typography
Spacing
Hover effects
Animations
Filters
Pagination
```

Only implement filters if the design requires them.

---

# B46. PARTNER SINGLE

> **Elementor Pro chosen in E1:** Theme Builder Single template with Dynamic Tags (E5.15) — not `single-partner.php`.

Partner detail should reproduce the Figma Partner detail frame.

Use:

```text
single-partner.php
```

Dynamic fields must populate the markup built from that frame.

Do not create a completely new partner layout.

---

# B47. BLOG ARCHIVE

> **Elementor Pro chosen in E1:** Theme Builder Archive template + Loop Grid (E5.16).

Blog listing must reproduce the Figma blog listing frame.

Use:

```text
home.php
archive.php
category.php
```

as appropriate.

Use WordPress Posts.

---

# B48. BLOG SINGLE

> **Elementor Pro chosen in E1:** Theme Builder Single Post template (E5.16).

Blog detail must reproduce the Figma blog detail frame.

Use:

```text
single.php
```

or a specific post template if required.

Dynamic:

```text
Title
Featured Image
Author
Date
Category
Content
Related Posts
```

---

# B49. REUSABLE TEMPLATE PARTS

> **Builder sections:** page sections follow the E1 builder (E2–E6; one shared render file per section for ACF builders). **Elementor Pro chosen:** saved templates / Global Widgets / Loop Items (E5.8) — not `template-parts/` PHP.

If the design contains repeated sections or components (`Design/components/`), extract them.

Example:

```text
template-parts/
├── blog/
│   └── blog-card.php
├── partner/
│   └── partner-card.php
└── content/
    └── content-card.php
```

Do not create unnecessary abstraction.

The goal is reusable and readable code.

---

# B50. CSS CLASS NAMING AND STABILITY

> **Elementor Pro chosen in E1:** native Elementor controls first; add a class only when a native control cannot do it, with its rule in `custom.css` (E5.18). Any class that is added still follows the Part A naming.

Use the Part A naming and token rules (A9) for every class. Do not invent names that drift from
the Figma component/section names.

If JavaScript depends on:

```text
class names
IDs
data attributes (data-anim, data-slider)
```

do not rename them without updating the JavaScript.

Before removing a class, search the whole theme for its usage.

---

# B51. JAVASCRIPT DEPENDENCY CHECK

Before modifying JS:

Search for:

```text
querySelector
querySelectorAll
getElementById
jQuery selectors
classList
data attributes
IDs
```

Ensure the PHP templates do not break selectors.

---

# B52. AJAX

If the design or the client's integrations need AJAX:

- Identify endpoint.
- Identify request method.
- Identify payload.
- Identify response.
- Identify authentication.
- Identify nonce/security requirements.

Build it with proper WordPress AJAX or REST API architecture.

Do not break an existing external API integration.

---

# B53. REST API

If custom REST endpoints are required:

- Register with `register_rest_route()`.
- Use namespaces.
- Validate parameters.
- Sanitize parameters.
- Add permission callbacks.
- Return proper `WP_REST_Response` / `WP_Error`.

Never expose private data through an unprotected endpoint.

---

# B54. CUSTOM ADMIN SETTINGS

> With ACF PRO (D110), site-wide settings (phone, email, address, social links, footer text) go on an **ACF Options Page** (local JSON, D111). Use the Settings API below only for a setting ACF can't hold.

If custom Theme Settings are needed:

Use the WordPress Settings API where appropriate.

Do not create insecure custom admin forms.

Use:

```text
register_setting()
add_settings_section()
add_settings_field()
```

Use capability checks and sanitization callbacks.

---

# B55. CHILD THEME

Do not create a child theme unless there is a specific reason.

The default target is a standalone custom theme.

---

# B56. PLUGIN VS THEME RESPONSIBILITY

Theme should handle:

```text
Presentation
Templates
Theme-specific settings
Frontend UI
```

Site functionality that must survive theme switching should ideally be a plugin.

However, for this build, do not unnecessarily create plugins unless the requirement warrants it.

For example:

```text
Partner CPT
```

can be implemented in a site plugin if long-term portability is important.

If implementing it in the theme, keep the code isolated in:

```text
inc/post-types.php
```

so it can later be moved easily.

---

# B57. DATABASE DESIGN

Do not create custom database tables unless necessary.

Prefer:

```text
Posts
Pages
Custom Post Types
Post Meta
Taxonomies
Options
```

Use custom tables only for genuinely relational/high-volume data where WordPress post/meta is inappropriate.

---

# B58. QUERY OPTIMIZATION

Avoid:

```text
query inside query
query inside loop
unnecessary get_posts()
repeated WP_Query
```

Use efficient queries.

For repeated cards, query once and loop.

Always reset post data after custom loops.

---

# B59. CACHING COMPATIBILITY

Do not write code that prevents normal WordPress caching.

Avoid unnecessary dynamic queries.

Do not depend on PHP session state unless absolutely necessary.

---

# B60. INTERNATIONALIZATION

Use translation-ready strings for theme UI text.

Example:

```php
__('Read More', 'site-theme');
```

Text domain:

```text
site-theme
```

Use one consistent text domain.

---

# B61. WORDPRESS CODING STANDARDS

Follow WordPress coding conventions:

- Proper indentation
- Meaningful function names
- Prefix custom functions/classes
- Avoid generic function names
- Avoid naming collisions
- Escape output
- Sanitize input

Example prefix:

```text
site_theme_
```

Avoid generic names like:

```php
function setup()
function helper()
function options()
```

---

# B62. FUNCTION / CLASS NAMING

Use project-specific prefixes.

Example:

```php
site_theme_setup()
site_theme_enqueue_assets()
site_theme_register_partner_cpt()
site_theme_register_menus()
```

Avoid collisions with plugins or WordPress core.

---

# B63. FILE NAMING

Use predictable names:

```text
single-partner.php
archive-partner.php
partner-card.php
blog-card.php
theme-settings.php
post-types.php
```

Do not create files with random names.

---

# B64. DEVELOPMENT ORDER

Follow this order:

```text
0. Ask: Page builder — ACF PRO Flexible Content / Gutenberg (ACF Blocks) / Both /
        Elementor Pro / other (E1)
   Ask: Gravity Forms, Contact Form 7 or (Elementor) Elementor Pro Form widget (D109)
   Ask: WooCommerce in this build? Cart/Checkout Block or Classic? (D113)
1. Collect inputs (A1)
2. Export the Figma design into Design/ (A2), then A3–A4
3. Normalise tokens (A5) → TOKENS.md approval gate (A6) — do not build before approval
4. Build the theme front-end system: vendor.css, vendor.js, sliders.js, animations.js (A7)
5. Build the style guide page (A8); follow the front-end rules (A9)
6. Inspect the complete Figma design (Design/pages, Design/components, CONTENT.md, ANIMATIONS.md)
7. Create site inventory
8. Identify templates
9. Identify reusable components
10. Identify dynamic content
11. Identify Partner architecture
12. Identify Blog architecture
13. Inspect Figma Header frames (every breakpoint)
14. Inspect Figma Footer frames (every breakpoint)
15. Plan WordPress theme (architecture report, B65)
16. Create theme structure
17. Create theme setup
18. Enqueue CSS/JS (A7.6 order)
19. Build Header
20. Build Footer
21. Register Menus
22. Build reusable sections + page builder (E1), then build Homepage
23. Build standard Pages (same sections, E1)
24. Create Partner CPT
25. Create Partner fields
26. Create Partner archive
27. Create Partner single
28. Build Blog
29. Build forms with the chosen plugin (D109)
30. Integrate animations (Design/ANIMATIONS.md → data-anim, A9.8)
31. Test responsive behavior (compare with Figma frames at 1600 / 900 / 375)
32. Test URLs
33. Test SEO
34. Test PHP
35. Test JavaScript
36. Performance review
37. Build one-click setup importer (D96–D102), seed content from Design/CONTENT.md
38. Verify against Figma (F1), then Final QA (B70 / F2)
```

Do not skip the inspection stage.

---

# B65. BEFORE CODING: ARCHITECTURE REPORT

Before making major code changes, create an internal implementation plan containing:

```text
Page inventory
Template mapping
Header structure
Footer structure
Menu locations
Partner CPT structure
Partner fields
Blog structure
Theme settings
Asset mapping
JS dependencies
CSS dependencies
Page builder choice (E1)
Forms plugin choice (D109)
WooCommerce choices (D113)
Potential risks
Migration/redirect requirements (only if the client has an old live site, B30)
```

Do not ask unnecessary questions if the answer can be determined by inspecting the Figma design and `Design/` export.

Only ask the user when a business requirement genuinely cannot be inferred from the supplied design.

Exception: the page-builder choice (E1) and the forms plugin (Gravity Forms / Contact Form 7 / Elementor Pro Form widget, D109)
are **always** asked first and recorded in this report.
If the site sells products, the WooCommerce questions (D113) are asked and recorded here too.
All of them are asked one question per message (A1).

---

# B66. DO NOT MAKE ASSUMPTIONS

If something is unclear:

First search the design export and the theme.

For example:

```text
Search where a class is used.
Search where an image is used (Design/images/, Design/svg/).
Search where a JS function is called.
Search which Figma frames use a component.
Search Design/CONTENT.md for the exact copy.
```

Do not guess.

---

# B67. HEADER / FOOTER SPECIAL RULE

This is mandatory:

```text
FIRST:
Inspect the Figma Header frames (1600 / 900 / 375) + header component + tokens + interactions + assets.

THEN:
Build header.php.

FIRST:
Inspect the Figma Footer frames (1600 / 900 / 375) + footer component + tokens + interactions + assets.

THEN:
Build footer.php.
```

The WordPress version must be based on the actual Figma design.

---

# B68. BUILD RULE

Do not simply paste static markup into a PHP file.

A proper WordPress build requires:

```text
Figma Design (Design/ + TOKENS.md)
↓
Part A Front-end System (tokens, BEM sections, data-anim, data-slider)
↓
WordPress Template
↓
Dynamic Content
↓
WordPress APIs
↓
Secure PHP
```

---

# B69. DO NOT DRIFT FROM THE FIGMA DESIGN

Before building or modifying any component:

1. Identify its Figma frame(s) and component.
2. Identify its CSS (tokens + section file).
3. Identify its JS (data-anim, data-slider, interactions).
4. Identify its assets.
5. Identify pages using it.
6. Build it.
7. Test it against the Figma frames.

---

# B70. FINAL QA CHECKLIST

> **Elementor Pro chosen in E1:** the Elementor list is **E5.33**; F2 runs every list.

Before declaring the project complete:

## Theme

- [ ] Theme activates without errors.
- [ ] No PHP fatal errors.
- [ ] No PHP warnings/notices.
- [ ] No deprecated errors caused by theme code.
- [ ] Theme files are organized.

## Header

- [ ] Header matches the Figma header frames.
- [ ] Logo works.
- [ ] Desktop menu works.
- [ ] Mobile menu works.
- [ ] Dropdowns work.
- [ ] CTA works.
- [ ] Sticky behavior works.
- [ ] Header animations work.

## Footer

- [ ] Footer matches the Figma footer frames.
- [ ] Footer menu works.
- [ ] Social links work.
- [ ] Contact details work.
- [ ] Copyright works.
- [ ] Responsive footer works.

## Pages

- [ ] Homepage works.
- [ ] Every standard page works.
- [ ] Images work.
- [ ] Internal links work.
- [ ] Every button, card, logo, menu and footer link is set — no `#` / empty links (D115).
- [ ] Every page compared with its Figma frames at 1600 / 900 / 375 (screenshots vs `Design/pages/`).

## Partners

- [ ] Partner CPT works.
- [ ] Partner admin works.
- [ ] Partner archive works.
- [ ] Partner single works.
- [ ] Partner fields work.
- [ ] Partner images work.
- [ ] Partner URLs work.

## Blog

- [ ] Blog archive works.
- [ ] Blog single works.
- [ ] Categories work.
- [ ] Tags work if required.
- [ ] Featured images work.
- [ ] Pagination works if required.

## Assets

- [ ] CSS loads.
- [ ] JS loads.
- [ ] Fonts load.
- [ ] Images load (sharp, Figma-exported, WebP; SVG for vectors).
- [ ] SVGs load.
- [ ] Vendor libraries load.

## JavaScript

- [ ] No console errors.
- [ ] Menu JS works.
- [ ] Sliders work.
- [ ] Animations work (match `Design/ANIMATIONS.md`).
- [ ] GSAP works if present.
- [ ] Three.js works if present.
- [ ] Forms work.
- [ ] AJAX works if present.

## Responsive

- [ ] Mobile tested.
- [ ] Tablet tested.
- [ ] Desktop tested.
- [ ] Compared with the Figma frames at 1600 / 900 / 375.
- [ ] Safari tested.
- [ ] Chrome tested.
- [ ] Firefox tested.
- [ ] Edge tested.

## SEO

- [ ] Titles set (and preserved if the client has an old live site).
- [ ] Heading hierarchy correct.
- [ ] Meta integration works.
- [ ] `<meta name="DeveloperAI" content="Developer Skill">` is in `<head>` on every page (D112).
- [ ] Favicon made from the logo icon is set as the WordPress Site Icon and shows on every page (D114).
- [ ] Canonicals work.
- [ ] Image ALT works.
- [ ] URLs are correct.
- [ ] Redirects work (if an old live site exists, B30).

## Security

- [ ] Output escaped.
- [ ] Input sanitized.
- [ ] Admin forms protected.
- [ ] Nonces implemented where required.
- [ ] Capability checks implemented.
- [ ] No credentials in source.
- [ ] No debug output.

## Performance

- [ ] No unnecessary duplicate assets.
- [ ] Images optimized.
- [ ] Libraries loaded only where needed.
- [ ] Queries reviewed.
- [ ] No obvious N+1 queries.

## Build lessons (D103–D107)

- [ ] Sections have appropriate section-level classes; no unnecessary inline styles (D103). Elementor: no class by habit — only where a native control could not do it (E5.18).
- [ ] Font sizes, colours, spacing and other repeated values use CSS variables (D103).
- [ ] Section spacing is consistent; container left/right spacing is set once globally (D103).
- [ ] Meaningful SVG icons have alternative text; decorative ones are `aria-hidden="true"` (D104).
- [ ] Every image has an SEO-friendly file name and proper alt text — none blank or default (D104).
- [ ] Images, avatars and SVGs are named after what they show — no Figma layer names (D104).
- [ ] Every image/icon is in the Media Library **once** — no `-1` / `-2` copies (D105).
- [ ] `404.php` and `search.php` match the design; search "no results" state works (D106).
- [ ] HTML Sitemap page exists, lists content automatically and is linked in the footer (D107).
- [ ] XML sitemap loads with correct URLs (D107).
- [ ] Page builder (E1) passes its own QA list: any section, any page, any order.
- [ ] Forms use the chosen plugin (Gravity Forms / Contact Form 7 / Elementor Pro Form widget) and pass the D109 QA list.
- [ ] Only ACF PRO is used, SCF is not installed (D110); all field groups are in `acf-json/` and synced (D111).
- [ ] WooCommerce sites: shop design, CSS, JS and responsive pass the D113 QA list.

---

# B71. FINAL DELIVERABLE

The final result must be a complete WordPress theme that can be installed in:

```text
/wp-content/themes/
```

and activated from:

```text
WordPress Admin
→ Appearance
→ Themes
```

The theme must be production-ready.

---

# B72. FINAL PRINCIPLE

Always prioritize:

```text
FIGMA DESIGN (Design/ + TOKENS.md)
        ↓
UNDERSTAND
        ↓
MATCH
        ↓
BUILD
        ↓
DYNAMIC WORDPRESS
        ↓
SECURE PHP
        ↓
TEST
        ↓
PRODUCTION
```

Never prioritize a faster build over matching the Figma design and its functionality.

Never redesign the site during the build.

Never guess when the answer can be found by inspecting the Figma design, the `Design/` export or `TOKENS.md`.

Never leave known PHP errors, query issues, broken links, missing assets, or JavaScript console errors unresolved.

**The Figma design (exported in `Design/`, with the approved `TOKENS.md`) is the source of truth for the frontend. WordPress is the CMS layer built around it.**

---

# C73. EXISTING WORDPRESS THEME — UPDATE / EXTEND MODE

## CRITICAL RULE

The project may already contain a WordPress theme.

If an existing WordPress theme is present, **DO NOT automatically create a new theme**.

First inspect the existing theme completely and determine whether the Figma design should be built as:

```text
Existing Theme
      +
Figma Design (Design/ + TOKENS.md)
      ↓
Safe Theme Update / Extension
```

The existing theme becomes the starting point unless there is a clear technical reason to replace it.

---

# C74. DETECT EXISTING THEME

Before creating theme files, check for:

```text
style.css
functions.php
header.php
footer.php
index.php
page.php
single.php
archive.php
front-page.php
theme.json
screenshot.png
wp-content/themes/
```

Also inspect:

```text
inc/
template-parts/
assets/
woocommerce/
acf/
```

If an existing theme is found, identify:

```text
Theme Name
Theme Version
Theme URI
Author
Template hierarchy
Existing functions
Existing hooks
Existing menus
Existing widgets
Existing custom post types
Existing taxonomies
Existing custom fields
Existing theme settings
Existing CSS
Existing JS
Existing plugins/dependencies
```

---

# C75. EXISTING THEME MUST BE BACKED UP

Before modifying an existing theme:

1. Inspect it.
2. Create a backup/copy.
3. Record the current structure.
4. Record important existing functionality.
5. Identify files that will be modified.
6. Avoid destructive changes.

Never delete existing theme functionality simply because it is not immediately understood.

---

# C76. DO NOT OVERWRITE EXISTING FUNCTIONALITY

Before changing any existing PHP file:

Search where its functions/hooks/classes are used.

Check:

```text
functions.php
inc/
template-parts/
hooks
filters
actions
shortcodes
REST endpoints
AJAX
custom post types
taxonomies
widgets
menus
theme settings
```

Do not remove existing code unless it is proven obsolete and safe to remove.

---

# C77. EXISTING THEME + FIGMA DESIGN STRATEGY

If the existing theme already provides:

```text
Header
Footer
Menus
Theme Settings
Page templates
Blog templates
```

reuse them where appropriate, and update them to match the Figma design.

Do not create duplicate systems.

Example:

If the theme already has:

```text
site-header.php
```

do not automatically create a second unrelated:

```text
header-new.php
```

Instead, update the existing implementation safely.

**Front-end system in an existing theme**

- The Figma design is still built with the token system and front-end rules of Part A (A7: `vendor.css`
  tokens, one CSS file per section under `assets/css/sections/`, BEM section classes, `data-anim`,
  `data-slider`).
- Add these files inside the existing theme (or its child theme, C86) at the paths Part A defines.
- If the theme already has a `:root` variable set, a global button style or a container class, map
  the approved `TOKENS.md` values into it instead of creating a second, competing set (C89).
- If the theme already loads jQuery, Slick, GSAP or ScrollTrigger, reuse that copy. Never load the
  same library twice (C89). Keep the enqueue order of A7.6.
- Do not redesign the Figma design to fit the old theme's look. The Figma design wins for every
  page, section and component that is in it. Old-theme screens that are not in the Figma design keep
  working as they are.

---

# C78. EXISTING HEADER UPDATE

If an existing `header.php` exists:

1. Read the existing `header.php`.
2. Read related CSS.
3. Read related JS.
4. Identify existing WordPress hooks.
5. Identify existing menu locations.
6. Identify existing logo/settings.
7. Compare it with the Figma header frame(s) at every breakpoint (`Design/pages/`, `Design/components/`).
8. Adapt the existing WordPress Header to match the Figma design.

Do NOT blindly replace the existing header.

Target:

```text
Existing WordPress Header
        +
Figma Header Design
        ↓
Updated Dynamic WordPress Header
```

Preserve working functionality.

---

# C79. EXISTING FOOTER UPDATE

If an existing `footer.php` exists:

1. Read the existing footer.
2. Read footer CSS.
3. Read footer JS.
4. Identify widgets/menu locations.
5. Identify existing theme settings.
6. Compare with the Figma footer frame(s) at every breakpoint.
7. Update the existing implementation safely.

Do not blindly replace the footer.

---

# C80. EXISTING MENU SYSTEM

If the theme already has registered menus:

```php
register_nav_menus()
```

inspect and reuse them.

Do not register duplicate menu locations such as:

```text
primary
main
header-menu
main-menu
primary-menu
```

all for the same purpose.

Consolidate only when safe.

Preserve existing menu behavior and custom walkers.

---

# C81. EXISTING THEME SETTINGS

If the theme already has:

```text
Customizer
Theme Options
Settings API
ACF Options Page
theme.json
```

reuse the existing settings architecture where practical.

Do not create duplicate settings such as:

```text
Existing Logo Setting
New Logo Setting
Another Logo Setting
```

Use the existing source of truth.

---

# C82. EXISTING CUSTOM POST TYPES

Before registering a new Partner CPT, check whether the existing theme/plugin already has:

```text
partner
partners
client
clients
case-study
```

or another equivalent CPT.

If an existing CPT already represents Partners, reuse it rather than registering a duplicate post type.

Do not create:

```text
partner
partners
partner_item
partner_profile
```

all for the same content.

---

# C83. EXISTING BLOG SYSTEM

Check whether the existing theme already modifies:

```text
Posts
Categories
Tags
Blog archive
Single post
```

Reuse the native WordPress Post system unless the existing project has a valid custom architecture.

Do not create a duplicate Blog CPT.

---

# C84. EXISTING PLUGINS

Before adding functionality, inspect installed/required plugins.

Check whether functionality is already provided by:

```text
ACF
Yoast SEO
Rank Math
WooCommerce
Contact Form 7
Gravity Forms
Elementor
WPForms
custom plugins
```

Do not recreate functionality already provided by an installed plugin.

Do not remove plugin dependencies without checking their usage.

---

# C85. EXISTING ACF

If ACF is already being used:

- Inspect existing field groups.
- Reuse existing fields.
- Extend existing field groups when appropriate.
- Do not duplicate fields unnecessarily.
- Preserve field names if existing templates depend on them.

If ACF is not installed and the project does not permit adding it, use secure native WordPress meta/settings.

(ACF PRO only — never install SCF, D110.)

---

# C86. EXISTING THEME CHILD THEME

If the current setup uses a child theme:

```text
parent-theme/
child-theme/
```

determine whether the build should be implemented in the child theme.

Do not modify a third-party parent theme unnecessarily.

Preferred approach:

```text
Parent Theme
      ↓
Existing Child Theme
      ↓
Figma Design → WordPress Changes
```

This protects parent-theme updates.

---

# C87. THIRD-PARTY THEME

If the existing theme is a commercial/third-party theme:

1. Identify it.
2. Do not modify vendor files unless unavoidable.
3. Prefer child-theme overrides.
4. Preserve vendor update compatibility.
5. Document any required overrides.

Never destroy third-party theme functionality without a clear reason.

---

# C88. EXISTING THEME FILE MODIFICATION RULE

Before modifying any existing file, classify it:

```text
SAFE TO MODIFY
NEEDS CARE
DO NOT MODIFY
```

Examples:

```text
Custom child theme files       → Safe with review
Custom theme files             → Safe with review
Third-party parent files       → Avoid
WordPress core files            → NEVER
Plugin core files               → NEVER
```

---

# C89. NO DUPLICATE ARCHITECTURE

The final project must have one clear source of truth.

Avoid duplicates such as:

```text
Two headers
Two footers
Two partner CPTs
Two blog systems
Two menu systems
Two logo settings
Two theme option systems
Two versions of the same JS library
Two copies of the same CSS
```

If duplicates already exist, inspect dependencies before consolidating.

---

# C90. SAFE UPDATE PRINCIPLE

When an existing theme is present:

```text
Inspect
   ↓
Understand
   ↓
Backup
   ↓
Compare with the Figma design
   ↓
Reuse existing architecture
   ↓
Update only what is required
   ↓
Test existing functionality
   ↓
Test new functionality
```

Never:

```text
Delete theme
↓
Start from zero
```

unless explicitly instructed or technically necessary.

---

# C91. EXISTING THEME REGRESSION TEST

After updating an existing theme, verify that all previous functionality still works.

Test:

```text
Existing pages
Existing posts
Existing menus
Existing forms
Existing widgets
Existing shortcodes
Existing CPTs
Existing taxonomies
Existing settings
Existing integrations
Existing scripts
Existing responsive behavior
```

The Figma build must not break unrelated existing functionality.

---

# C92. EXISTING THEME VERSION CONTROL

If the project contains Git:

First inspect:

```text
.git/
.gitignore
branch
status
recent changes
```

Do not overwrite or discard user changes.

Do not reset or force-delete existing work.

If Git is not available, create a safe backup before major modifications.

---

# C93. EXISTING THEME FINAL ARCHITECTURE

The final architecture should be:

```text
Existing WordPress Theme
          +
Figma Design (Design/ + TOKENS.md)
          ↓
        Merge
          ↓
Updated Custom Theme
          ↓
Dynamic WordPress CMS
```

The existing theme's valid functionality must be retained unless explicitly replaced.

---

# C94. DECISION RULE

Use this decision tree:

```text
Is an existing WordPress theme present?
            │
          YES
            ↓
   Inspect existing theme
            │
            ↓
Can existing architecture support the Figma design?
       │                 │
      YES                NO
       ↓                  ↓
Update/extend       Safely refactor only
existing theme      required architecture
       │                  │
       └────────┬─────────┘
                ↓
        Test existing features
                ↓
        Test new / updated pages against Figma (F1)
                ↓
          Final QA
```

If no existing WordPress theme is present:

```text
Create a new custom theme
```

---

# C95. FINAL INSTRUCTION FOR THE CODING AGENT

If an existing WordPress theme is supplied, your first responsibility is **not to create another theme**.

Your first responsibility is to:

1. Inspect the existing theme.
2. Understand its architecture.
3. Identify what can be reused.
4. Identify what must be updated.
5. Compare it with the complete Figma design (every frame in `Design/pages/`).
6. Safely build the Figma design into it.
7. Preserve existing WordPress functionality.
8. Avoid duplicate systems.
9. Test the complete site.
10. Deliver a production-ready updated theme.

**Never destroy an existing working theme architecture just to make the build easier.**

---

# D96. ONE-CLICK SETUP — TOOLS → {PROJECT} SETUP (MANDATORY)

Every theme must ship with a **one-click content importer**, so a fresh WordPress install becomes the complete website with one button and no manual page building or data entry.

```text
Install theme  →  ACF PRO active (never SCF, D110)  →  Sync ACF field groups (D111)  →  Tools → {Project} Setup  →  Import
        ↓
Pages + templates, front page, posts page, posts, CPT entries,
Media Library images, ACF field values, SEO, menus — all created
```

Reference implementation: the Agency Core theme (`ag-theme/inc/importer.php` + `inc/seed/*.json`). Follow the same pattern.

**Works for every page builder.** The importer must work for whichever builder was chosen in E1:
ACF Flexible Content rows, ACF Blocks in `post_content`, both, Elementor templates + page data, or
another builder. The importer screen, steps, rules and QA (D97–D102) are the same for all of them.
Only the format of each page's `sections` seed and the function that writes it change — E2–E6
define the builder-specific seed format.

---

# D97. IMPORTER FILE STRUCTURE

```text
theme-name/
├── inc/
│   ├── importer.php          ← admin screen, handler, WP-CLI command, import functions
│   ├── custom-fields.php     ← ACF JSON save/load paths + field helpers (D111)
│   └── seed/
│       ├── pages.json        ← pages: slug, title, template, status, SEO, optional content
│       ├── posts.json        ← blog posts: slug, title, date, excerpt, content, image, alt, SEO
│       ├── {cpt}.json        ← one file per CPT (partners.json, webinars.json …) incl. ACF field values
│       └── menus.json        ← menus: name, location, nested items
└── assets/images/            ← every image the seed references (WebP + SVG from Design/, imported into the Media Library)
```

- Load `inc/importer.php` from `functions.php` like every other `inc/` file.
- Use the theme prefix for every function, option, nonce, transient and meta key (`{prefix}_import_*`).

---

# D98. SEED DATA — FROM THE FIGMA EXPORT, NEVER RETYPED, NEVER INVENTED

- Build the `inc/seed/*.json` files from the Figma export: all text from **`Design/CONTENT.md`**
  (every text copy exported from Figma, A2), images from **`Design/images/`** and icons / logos from
  **`Design/svg/`**. Never retype, rewrite or invent content.
- Content must be identical to the Figma design: titles, copy, headings, button / link labels, dates,
  image file names (after the D104 rename) and alt text (D104).
- Each page entry: `slug`, `title`, `template` (path in `page-templates/` or empty for default), `status`, `seo_title`, `seo_description`, optional `content`, plus `builder` and `sections` (E1; format per builder in E2–E6).
- Sections are seeded in the same order as the Figma frame, top to bottom.
- Each post / CPT entry: `slug`, `title`, `date`, `excerpt`, `content`, `image`, `image_alt`, SEO, and **every ACF field value keyed by the field `name`** (repeaters as arrays of rows).
- Menus: `name`, `location`, `items` as `[ title, target, extra, children ]`, where target is `page:slug`, `{cpt}:slug`, `archive:{cpt}`, `blog`, or a URL.
- Images referenced by file name only; the files live in `assets/images/` (copied from `Design/images/`
  and `Design/svg/`, D104). File names and alt text follow D104, and each image appears in the
  seed's Media Library list once (D105).
- `pages.json` includes the **Sitemap** page (slug `sitemap`, template `page-templates/page-sitemap.php`, D107).
- If a text or image the seed needs is missing from `Design/CONTENT.md` / `Design/images/`, re-export
  it from Figma (A2 retry rule). If the design really does not have it, use the D98.1 dummy content
  and list it in the handover report. Never make it up.
- Never put credentials, private URLs, form API keys or personal data in seed files.

## D98.1 Dummy content & placeholder images

*Noted during real builds — use one consistent placeholder system across the whole site.*

Only where the Figma design itself has no real content or image for a slot (e.g. a grey "image here"
box, or more blog / CPT items than the design shows), use dummy content **with a proper placeholder
image**. Never leave an empty space, a broken image or a random stock photo. Never replace an image
that exists in the Figma design with a placeholder — if its export failed, retry the export (A2).

### Image placeholders

- Match the **aspect ratio, dimensions and styling** of the design's real image slot (same radius,
  same object-fit, same size at every breakpoint).
- Create one placeholder per ratio the design uses (e.g. `placeholder-16x9.webp`,
  `placeholder-4x3.webp`, `placeholder-1x1.webp`). Upload them once to the Media Library (D105) and
  reuse them everywhere. Don't make a new one per section.
- Lock the ratio with one reusable class in the global CSS (`vendor.css`, A7.2; for Elementor builds
  see E5) — e.g. `.img-placeholder` with `aspect-ratio` and `object-fit: cover`. Don't set heights
  section by section or widget by widget.
- Give placeholders alt text that names the intended content (e.g. `Team photo placeholder`) so
  they are easy to find and replace.

### Logo placeholders

- Light gray / neutral background (a CSS variable, e.g. `--placeholder-bg`).
- Logo centered **both vertically and horizontally** inside a fixed-ratio box (flex,
  `align-items: center; justify-content: center`).
- Every logo box is the same size, and the logo keeps its proportions (`object-fit: contain`,
  max width/height).
- One reusable class (e.g. `.logo-placeholder`) for every logo grid and logo slider on the site.

### Responsive and consistent

- Check placeholders on **Desktop, Tablet and Mobile**. The ratio and centering must hold, nothing
  stretches, and nothing overflows.
- Use the **same placeholder treatment on every page**. One look for image placeholders and one
  for logo placeholders.

---

# D99. ADMIN SCREEN — TOOLS → {PROJECT} SETUP

Register with `add_management_page()` under **Tools**, capability `manage_options`.

The screen must show:

1. A short description of what is imported.
2. A **status table**: for each content type, `count in seed / count on site`.
3. An **"Update existing content to match the seed"** checkbox (off by default), with a warning that edits made on the site to those items will be lost.
4. One **Import {Project} content** button.
5. A **live progress area** while the import runs: progress bar + log line per step (e.g. `Pages 4/12 — Home created, set as front page`).
6. After the run, a **report**: per content type `X created, Y updated, Z existing kept`, plus front page / posts page / menus status.
7. A short "after importing" note listing the manual steps that remain (site title, form shortcodes, etc.).

**Step-by-step processing — never one long request (MANDATORY)**

*Noted during a real build: a single-request import timed out ("first pass is still creating pages")
and the front page was left unset. One click must still do everything, but the work is split into
small steps so no request comes near the PHP / server timeout.*

- The button starts a JS runner. It calls `admin-ajax.php` (custom action) **once per step** and
  only sends the next request after the previous one has returned.
- Each request does **one small batch** and returns quickly: e.g. 1–5 pages, 1–5 posts / CPT
  entries, 1–3 images, one menu. Never import a whole content type, or all images, in one request.
- Step queue, in the D100 order: `pages` → `posts` → `{cpt}` (one queue per CPT) → `menus` →
  `finalize` (settings check, `flush_rewrite_rules()`, final report).
- The server returns JSON: `{ step, offset, done, total, log[], next }`. The runner updates the
  progress bar and log, then requests `next`.
- Progress and the running report live in a per-user option / transient (`{prefix}_import_state`),
  so a failed or closed tab can **resume** from the last finished step instead of starting over.
- A failed request is retried (max 3) and then shown in the log with a **Resume** button. One
  failed item never stops the rest of the import.
- Image processing (`wp_generate_attachment_metadata`) is the slowest part — keep image batches the
  smallest, or generate an item's images in its own step.

Handler rules (every AJAX step):

- `current_user_can( 'manage_options' )` + `check_ajax_referer()` nonce on **every** request.
- Sanitize `step` against a whitelist and `offset` with `absint()`.
- Return with `wp_send_json_success()` / `wp_send_json_error()`; never echo HTML.
- Escape every output on the screen (`esc_html`, `esc_url`, `esc_attr`).

Also register a WP-CLI command: `wp {prefix} import [--overwrite]`. It runs the **same step
functions** in a loop (CLI has no web timeout), printing one line per step.

---

# D100. IMPORT ORDER

```text
1. Pages           → create (Home first), set _wp_page_template, SEO;
                     set front page the moment Home exists, posts page the moment Blog exists
2. Posts           → create, category, date, featured image, SEO
3. CPT entries     → create, featured image, ACF fields (incl. repeaters + images), SEO
4. Menus           → build from menus.json, assign to theme locations
5. Finalize        → re-check front page / posts page / menu locations, flush_rewrite_rules()
```

Each line is split into small AJAX steps (D99). Pages must exist before menus, because menu items link to them by slug.

**Front page — set it as soon as the Home page exists**

- Put the Home page **first** in `pages.json` (and the Blog / posts page early), so they are created in the first batch.
- Right after Home is created (or found), set `show_on_front = page` + `page_on_front` in the **same step**. Do the same for `page_for_posts` when the Blog page is created.
- Never wait until all pages exist. If the import stops halfway, the front page must already be set.
- The `finalize` step only re-checks it (fills it if still empty) — it is a safety net, not the place where it is first set.

---

# D101. IMPORTER RULES

**Safe to run again (idempotent)**

- Find existing items by **slug + post type**, checking all statuses (`publish`, `private`, `draft`, `pending`, `future`) — `'any'` skips private items and creates duplicates.
- Default mode: existing items are **kept**, never overwritten. Only missing items are created.
- Overwrite mode (checkbox / `--overwrite`): existing items are updated to match the seed; menus are emptied and rebuilt; front page / posts page are reset.
- Front page, posts page, menu locations and other settings are only filled **where empty**, unless overwrite mode is on.

**Images**

- Copy each file from `assets/images/` into the Media Library **once** (`wp_upload_bits` + `wp_insert_attachment` + `wp_generate_attachment_metadata`).
- Tag the attachment with a source meta key (`_{prefix}_source` = file name) and reuse it on every later run.
- Set the alt text from the seed.

**ACF fields**

- Write values using the **same field definitions** as the `acf-json/` files (D111) — never a second copy of the field list.
- Store `{name}` = value and `_{name}` = field key, so ACF recognises the value.
- Repeaters: store the row count in `{name}` and each sub-field as `{name}_{i}_{sub}`.
- Image fields: convert the file name to an attachment ID through the image importer.

**SEO**

- Fill the theme's own SEO fields, and Yoast / Rank Math meta when either plugin is active.

**Robustness**

- Avoid timeouts by **small step batches (D99)**, not by one long request. `set_time_limit()` may be
  added per step as a safety margin, but it is not the fix (many hosts ignore it).
- `require_once wp-admin/includes/image.php`.
- `wp_slash()` data passed to `wp_insert_post` / `update_post_meta`.
- Check `is_wp_error()` on every insert; skip and report failures instead of stopping.
- Never delete content that is not in the seed.

---

# D102. IMPORTER QA

Before delivery, test on a **fresh** WordPress install and on staging:

- [ ] Fresh install → Import creates every page, post, CPT entry, image and menu; site matches the Figma frames (F1).
- [ ] Second run (default mode) → nothing duplicated; report shows everything "kept".
- [ ] Overwrite run → items updated, menus rebuilt, no duplicates.
- [ ] Every ACF field (incl. repeaters and images) shows its value in the edit screen.
- [ ] Every page shows its sections in the Figma order, in the format of the builder chosen in E1 (Flexible Content and/or ACF Blocks, Elementor data, or other builder — E2–E6).
- [ ] Every text in the seed matches `Design/CONTENT.md`; every image comes from `Design/images/` / `Design/svg/`.
- [ ] Front page, posts page and all menu locations are set.
- [ ] Import runs step by step via AJAX with a live progress log; no request times out, even on a slow host with all images.
- [ ] Front page is set right after the Home page is created (check it while the import is still on Pages).
- [ ] Closing the tab mid-import and clicking Import / Resume continues from the last step — no duplicates.
- [ ] Images appear once in the Media Library, with alt text.
- [ ] `wp {prefix} import` works from WP-CLI.
- [ ] No PHP warnings or notices during import.

Add the importer to the README **Install** steps:

```text
1. Copy theme to wp-content/themes/ and activate it.
2. Make sure ACF PRO is active (never install SCF, D110), plus the builder plugin chosen in E1 if any,
   then ACF → Field Groups → Sync (D111).
3. Tools → {Project} Setup → Import {Project} content.
4. Settings → Permalinks → Post name (D107.1).
5. Remaining manual steps (site title, form shortcodes …).
```

---

# D103. CSS & GLOBAL STRUCTURE

*Noted during real builds. B20 still applies: keep the Figma look exactly and do not rewrite CSS for
its own sake. These rules decide **how** the CSS is organised; they follow the Part A front-end
system (A7.2 `vendor.css`, A9.5–A9.7).*

**Where the CSS lives**
- Tokens (`:root` variables) and global components (container, buttons, links, headings, forms,
  section titles) live in `assets/css/vendor.css` (A7.2), built from the approved `TOKENS.md`.
- Each section has its own file `assets/css/sections/<section>.css` (A9.6.1): filename = the block
  class, one top-level selector + that section's media queries, enqueued after `vendor.css` in page
  order through `inc/enqueue.php` (A7.6).
- Never re-create a global component inside a section file. Reuse it.

**Section-level classes, wherever required**

> **Elementor Pro chosen in E1:** no section classes by habit — native Elementor controls first; add a class only when a native control cannot do it, with its rule in `custom.css` (E5.18).

- Every template section gets a section-level BEM block class with a matching `id` (A9.6.2), e.g.
  `about`, `partners`, with elements `about__grid`, `about__card`.
- Shared components get a reusable class (e.g. `card`, `logo-grid`).
- Name classes by role. Never name them after a page or a colour.

**No unnecessary inline styles**
- Styling goes in the theme CSS (`assets/css/`), managed through reusable classes. No `style=""` in
  PHP templates.
- Only exception: a **dynamic value from the admin** (e.g. an ACF background image or colour) may be
  passed as a CSS variable, nothing else: `style="--section-bg: url(<?php echo esc_url( $bg ); ?>)"`.
  The CSS rule itself still lives in the stylesheet.

**CSS variables for common values**
- Font sizes, colours, spacing, radius, shadows and other repeated values live in the one `:root`
  block at the top of `vendor.css` (A7.2), named as in `TOKENS.md`. Never invent a new token without
  adding it to `TOKENS.md` first.
- Section CSS uses `var(--…)`, not the same raw value repeated in many rules. No raw hex / px values
  that a token already covers.

**Consistent spacing**
- Section top/bottom spacing is one variable (`--sectionGap`, A9.5) applied the same way to every
  section as its `padding-block`.
- Container left/right spacing (`--gutter`) is set **once** globally on the `.container` class in
  `vendor.css`. Never re-declare it in each section.

**Clean, scalable, reusable**
- One comment banner per section or component (the banner format of A9.6.1).
- No duplicate rules, no dead CSS. Avoid `!important`.
- Responsive rules sit at the bottom of the section's own file, using only the seven breakpoints of
  A9.7 (A5.8), ordered largest → smallest. Only write a breakpoint the design actually changes at.

---

# D104. SVG & IMAGE ACCESSIBILITY + NAMING

*Noted during real builds — image names and accessibility attributes were left incomplete before.*

### Images come only from the Figma export — sharp, never blurry

- Every image, icon and logo comes from the Figma export: rasters from `Design/images/`, icons /
  logos / vector shapes from `Design/svg/` (A2). No stock photos, no AI images, no screenshots of
  the design used as images.
- Rasters are exported at **2x** and only ever **resized down** to the size actually used — never
  scaled up.
- Convert rasters (JPG / PNG / GIF) to **WebP at quality 85 or higher**. No visible loss versus the
  Figma export. Keep transparency where the original uses it.
- Icons, logos and vector shapes **stay SVG** (WebP is for raster images only).
- Check every image at 1600 / 900 / 375 (F1): if it looks soft or blurry, re-export from Figma at a
  higher scale. A blurry image is a bug.
- Copy the renamed files into the theme's `assets/images/`; content images then go to the Media
  Library once (D105).

```text
Design/images/ (2x) + Design/svg/
    ↓
Rename by what it shows (below)
    ↓
Raster → WebP q85+, resize down to the size used · SVG stays SVG
    ↓
theme assets/images/
    ↓
Media Library — already there? Reuse it (D105). Upload ONLY if not there yet
```

- Do not hardcode image URLs to content images; content images are selected from the Media Library
  (by attachment ID).

### SVG icons

- **Meaningful SVG** (it carries information or is the only content of a link/button):
  - Inline SVG in a template: `role="img"` + `aria-label="<?php esc_attr_e( 'Phone', 'textdomain' ); ?>"`
    (or a `<title>` inside it).
  - Icon-only link/button: put the `aria-label` on the link/button and `aria-hidden="true"`
    `focusable="false"` on the SVG.
  - SVG `<img>`: proper `alt`.
  - SVG uploaded through the Media Library: fill in its **Alt Text** there.
- **Decorative SVG** (the text next to it already says the same thing): `aria-hidden="true"`, and
  no alt text.
- Never leave an icon-only control without an accessible name.

### Images — every image gets a proper name and alt text

- **Rename every image, avatar and SVG by what it actually shows.** Open or view the file first.
  - Use lowercase, hyphens, 2–5 meaningful words.
  - Never keep Figma or export names (`Ellipse 3`, `Group 1000004521`, `Frame 12`, `Vector`,
    `IMG_1234`, `image (1)`).
  - Never guess from where the file sits in the design. Describe what is actually in it.
  - Rename **before** the file is referenced in the templates or the seed, and update every
    reference. WordPress keeps the uploaded file name in the URL, so the name must be right before
    upload.

| File type | Pattern | Example | Alt text |
|---|---|---|---|
| Content image | `<what-it-shows>.webp` | `doctor-consulting-patient.webp` | `Doctor consulting a patient at a clinic desk` |
| Hero image | `<page>-hero-<subject>.webp` | `about-hero-team-workshop.webp` | describe the scene |
| Blog / post image | `<post-topic>.webp` | `seo-audit-checklist.webp` | describe the image, not just the post title |
| Avatar — real person | `<first>-<last>-avatar.webp` | `priya-shah-avatar.webp` | `Priya Shah, Marketing Director at Acme` |
| Avatar — dummy | `<context>-avatar-<n>.webp` | `testimonial-avatar-01.webp` | `Testimonial author photo` (never invent a name) |
| Team photo | `<first>-<last>-<role>.webp` | `rahul-mehta-ceo.webp` | `Rahul Mehta, CEO` |
| Logo | `<brand>-logo.svg` | `acme-logo.svg` | `Acme` |
| Site logo | `<site>-logo.svg` | `brightpath-logo.svg` | `<Site name> home` when it links home |
| SVG icon | `icon-<what-it-shows>.svg` | `icon-phone.svg`, `icon-arrow-right.svg` | its meaning, or empty if decorative |
| Social icon | `icon-<network>.svg` | `icon-linkedin.svg` | `LinkedIn` (on the link: `Follow us on LinkedIn`) |
| Decorative background | `bg-<what-it-is>.svg/webp` | `bg-wave-pattern.svg` | `alt=""` |
| Placeholder | `placeholder-<ratio>.webp` | `placeholder-16x9.webp` | `<Intended content> placeholder` (D98.1) |

- **Alt text:** describe what is in the image and why it is there.
  - Keep it under ~125 characters. Don't start with "image of", and don't stuff keywords.
  - A purely decorative image gets `alt=""`. The `alt` attribute is never missing.
  - Same image in two places = same alt (it comes from the Media Library).
- **Where the alt comes from:**
  - Media Library images: use `wp_get_attachment_image()` / `the_post_thumbnail()` so the library's
    Alt Text is printed.
  - Set Alt Text and Title on every attachment (the importer sets it from the seed, D101).
  - Static theme images: write the alt in the template with `esc_attr()`.
- **If renaming is not possible** (the file is already uploaded and in use): do not upload a second
  copy just to change its name (D105). Correct its **Title** and **Alt Text** in the Media Library,
  and list the file in the handover report as "file name not SEO-friendly".
- Do not leave image names or accessibility attributes incomplete. A build with default file names
  or blank alt text is not done.

---

# D105. MEDIA LIBRARY — UPLOAD EVERY IMAGE / ICON ONLY ONCE

*Noted during real builds — the same images and icons were uploaded to the Media Library twice.*

**Rule: one file = one Media Library item.** The same image used in five places is uploaded once
and reused by its attachment ID.

**Decide each file's home — theme or Media Library, never both:**
- **Fixed UI files** that admins never change stay in the theme's `assets/images/` and are **not**
  uploaded to the Media Library. These are UI icons, arrows, decorative shapes and the SVG sprite.
- **Content images** that admins can change go into the Media Library (through the importer). These
  are page images, post/CPT images, ACF image fields, partner logos and avatars.

**Before every upload (manual, script, WP-CLI or importer):**
1. Look the file up first: by the source meta key (`_{prefix}_source`, D101), by file name or by
   title.
2. Found → reuse that attachment ID. Not found → upload it once with its D104 name, alt and title.
3. Re-running the importer or a build step must not upload anything that already exists.

**Signs of a duplicate:**
- The same name with a WordPress suffix: `acme-logo-1.webp`, `acme-logo-2.webp`.
- Two items with the same title or the same thumbnail.

**Clean-up:**
1. Find every post, page, ACF field and option that uses the copy.
2. Re-point each one to the original attachment.
3. Only then delete the copy.

Never delete an attachment that is still in use.

---

# D106. ADDITIONAL PAGES — 404 AND SEARCH

*Noted during real builds — these two pages were missed before. They are part of every build.*

### 404 Page — `404.php`

- Build it **as per the Figma design** (its 404 frame in `Design/pages/`): heading, message, button
  back to the homepage (`home_url( '/' )`), and the search form if the design has one.
- Use the normal `get_header()` / `get_footer()`, global classes, variables and buttons (`vendor.css`, A7.2).
- Test it by opening a URL that does not exist. The response must be a real 404 status.

### Search Page — `search.php` + `searchform.php`

- Build `search.php` **as per the Figma design**:
  - A heading with the search term (`get_search_query()`, escaped).
  - The results loop: title, excerpt/meta, and image if the design shows one.
  - Pagination (`the_posts_pagination()`).
  - A clear **"no results"** state, with the search form again.
- `searchform.php` holds the site's search form. It uses `role="search"`, a labelled input with
  `name="s"`, and `action="<?php echo esc_url( home_url( '/' ) ); ?>"`. The header/site search uses
  it via `get_search_form()`.
- Decide which post types search includes (Posts, Pages, Partners), using `pre_get_posts` on the
  main query only.
- Test it with a term that returns results, a term that returns nothing, and a term containing
  special characters.

If the Figma design has no 404 or Search frame, build a simple one from the global design system
(tokens + global components, style guide page A8), and say so in the handover report.

---

# D107. SITEMAP PAGE

*Noted during real builds — the sitemap page was missed before. It is part of every build.*

### HTML Sitemap page (for visitors) — `page-templates/page-sitemap.php`

- A page template named "Sitemap", assigned to a Page with slug `sitemap`. The importer creates it
  (D98).
- The list must be **dynamic**. Never hardcode links. New pages and posts appear automatically.
  - Pages: `wp_list_pages( [ 'title_li' => '', 'exclude' => … ] )`.
  - Posts and the Partner CPT: `WP_Query` / `get_posts()`, grouped under headings.
  - Categories, if the design uses them.
- Leave out Thank You, 404, private/draft and utility pages.
- Style it with the global section classes and variables (D103).
- Link it in the footer (usually next to Privacy / Terms) through the WordPress footer menu.

### XML Sitemap (for search engines)

- If Yoast or Rank Math is active, use **its** XML sitemap and do not add another one.
- If neither is active, WordPress core's `/wp-sitemap.xml` is enough. Don't add a sitemap plugin, and
  don't build a custom XML sitemap.
- Check that Pages, Posts and the Partner CPT are included, and that Thank You / utility pages are
  excluded. Use the `wp_sitemaps_*` filters, or the SEO plugin's settings.
- Open the sitemap URL after the build and confirm it loads with the correct (Post name) URLs.

---

# D107.1. PERMALINK STRUCTURE

Set the WordPress permalink structure to **Post name**:

```text
Settings → Permalinks → Post name   (/%postname%/)
```

Set this before building internal links, then flush permalinks (save the Permalinks screen once) after registering any Custom Post Types.

---

> **D108** — no section here: the page builder choice is in **E1**.

---

# D109. FORMS PLUGIN — GRAVITY FORMS, CONTACT FORM 7 OR ELEMENTOR PRO FORM

*Added after a real build: the forms plugin was guessed. The choice must come from the user.*

## Ask before building (MANDATORY)

Ask this right after the page-builder question (E1), at the very start of the project — as its own
question, one question per message (A1). Never
assume the answer, even if one plugin looks like the obvious choice.

```text
Which forms plugin should the site use?
  1. Gravity Forms
  2. Contact Form 7
  3. Elementor Pro Form widget   ← offer only when E1 = Elementor Pro
```

- Write the answer into the architecture report (B65) and use **only** that plugin for every form on
  the site. Never mix two form plugins.
- If the existing WordPress site already uses a forms plugin (C84), say so in the question, so the
  user can decide whether to keep it.
- If a form must post to a CRM / API / third-party service (B28), connect that integration through
  the chosen plugin (Gravity Forms add-on / webhook, or a CF7 integration). Ask the user for the
  target; never guess it.

## Rules for every option

- Every form in the Figma design (contact, newsletter, demo request, …) becomes one form in the
  chosen plugin, with the same fields, labels, placeholders, required fields, order and button text
  (text from `Design/CONTENT.md`).
- **The form must look like the Figma form.** Use the global form styles from `vendor.css` (A7.2) and
  the section's own CSS (D103). No default plugin look.
- Place the form through a field, not hardcoded in a template: a **Form** field in the section
  (E1–E6) or in Theme Settings holds the form ID / shortcode.
- Success message, error messages and redirect (e.g. Thank You page) match the Figma design. If the
  design does not show them, use the global design system and ask the user for the wording.
- Spam protection on every form (Gravity Forms honeypot / reCAPTCHA / Turnstile; CF7 reCAPTCHA /
  Turnstile / Akismet).
- Email notification goes to the admin email from Settings, never a hardcoded address.
- No API keys or credentials in the theme or seed files (D98).

## Gravity Forms

- Output with `gravity_form( $id, false, false, false, null, true )` or the `[gravityform]`
  shortcode (AJAX on).
- Turn off the default Gravity Forms theme CSS for these forms (form theme "None" / no-conflict
  styles) and style with the theme classes. Add the theme's form classes through the field's
  **Custom CSS Class** setting.

## Contact Form 7

- Output with `[contact-form-7 id="…"]`.
- Write the form template in CF7 with the theme's form wrapper and classes (BEM, D103), so the theme
  CSS applies directly.
- Load CF7 CSS/JS only on pages that have a form (`wpcf7_load_css` / `wpcf7_load_js` filters +
  enqueue where needed, B40).

## Elementor Pro Form widget (Elementor builds only)

- Build each form with the native **Form** widget; save it as a reusable template / Global Widget
  when it repeats (E5.8).
- Style it with the widget's own style controls and the Elementor globals (E5.3–E5.5) — no class CSS
  unless a native control cannot do it (E5.18).
- Actions After Submit: **Email** to the admin email (Site Settings), **Redirect** / message as in
  the design; add CRM / webhook actions only when the user gives the target.
- Spam: the widget's **Honeypot** field plus reCAPTCHA / Turnstile when available.
- Follow the Form-widget gotchas in E5.37.10.

## QA

- [ ] The forms plugin (Gravity Forms / Contact Form 7 / Elementor Pro Form widget) was asked and recorded before building.
- [ ] Only the chosen plugin is used; every Figma form exists in it.
- [ ] Every form looks like the Figma form on mobile, tablet and desktop.
- [ ] Submissions arrive by email; success / error messages and redirects work.
- [ ] Spam protection is on; no console errors on pages with forms.

---

# D110. ACF PRO ONLY, NEVER INSTALL SCF + SITE LOGIN (login.md)

*Added after a real build: the site already runs **ACF PRO**. Installing Secure Custom Fields (SCF)
next to it causes plugin conflicts and duplicate field systems.*

## ACF PRO only (STRICT)

- **Never install, activate or require Secure Custom Fields (SCF)** — not on the user's site, not on a
  local server or staging, not in the README. Not even "as a fallback".
- The theme is built for **ACF PRO**, which is already installed on the site. Check that it is active
  before building; if it is not active, stop and tell the user — do not install anything in its place.
- Never install any other plugin on the site without asking the user first.

## Site login — `login.md`

- **`login.md`** in the project folder holds the details of the **user's own WordPress site** (site
  URL, admin URL, login). It does **not** hold local server details.
- Use it to log in to the user's site admin. With that login you may: upload / update the theme,
  **activate (change) the theme**, run the importer (D96–D102), and sync ACF field groups (D111).
- Before changing the theme on the site, keep a backup of the current theme (C75).
- Local server details are not in `login.md`. If a local server is needed and its details are not
  known, ask the user — never use the `login.md` details for anything other than the user's site.
- Never print, copy or commit the credentials from `login.md` — not in code, seed files, logs,
  reports or chat replies. Add `login.md` to `.gitignore`.
- If `login.md` is missing or the login fails, stop and ask the user. Never guess passwords or
  create new admin users.

---

# D111. ACF FIELDS — LOCAL JSON + SYNC (MANDATORY)

*Added after a real build: field groups existed only in PHP, so they could not be edited or synced
in the admin. Every ACF field group must also be saved as a JSON file and synced.*

## Local JSON

- The theme has an **`acf-json/`** folder. Every field group is saved there as one JSON file
  (`group_{key}.json`) — Partner fields, page sections (E1–E4), Theme Settings, blocks, everything.
- Point ACF to that folder in `inc/custom-fields.php`:
  - `acf/settings/save_json` → `get_stylesheet_directory() . '/acf-json'`
  - `acf/settings/load_json` → add the same path.
- The `acf-json/` files are the **single source of truth** for fields. Do not also register the same
  groups in PHP with `acf_add_local_field_group()` — that creates a second copy that cannot be
  synced or edited in the admin.
- Field keys are fixed and unique (`field_{prefix}_{name}`, `group_{prefix}_{name}`) and never change
  after the first build — the importer and saved content depend on them.
- Section fields for E2–E4: one field group per section in `acf-json/`. The Flexible Content layout
  and the ACF Block field group reuse it through an ACF PRO **Clone** field (seamless), so each
  section's fields exist once.

## Sync

- After the field groups are created, **sync them into the database**: WP Admin → ACF → Field
  Groups → **Sync available** → Sync all (log in with `login.md`, D110). Then every group is
  visible and editable in the admin.
- After any change to a field group in the admin, ACF writes the change back to `acf-json/`
  automatically. Keep those updated JSON files in the theme.
- After the theme is updated on another site, run Sync again there.
- The importer (D101) reads field keys from the same `acf-json/` files — never a second list.

## QA

- [ ] Every field group has a JSON file in `acf-json/`; no field group exists only in PHP or only in
      the database.
- [ ] ACF → Field Groups shows **no "Sync available"** items after the sync.
- [ ] Editing a field group in the admin updates its JSON file in the theme.
- [ ] Only ACF PRO is installed — SCF is not installed on the site (D110).

---

# D112. DEVELOPERAI META TAG IN `<head>` — EVERY PAGE (MANDATORY)

*Default on every build.* Every front-end page of the site — pages, posts, archives, CPT pages, 404,
Search, Sitemap — outputs this hidden tag in `<head>`:

```html
<meta name="DeveloperAI" content="Developer Skill">
```

- Copy it exactly: `name="DeveloperAI"`, `content="Developer Skill"`. It is a hidden meta tag; it is
  never shown on the page.
- Add it once, through the `wp_head` hook, in the theme's `inc/template-functions.php` (or the child theme's `functions.php` in Update / Extend mode, C73):

```php
function {prefix}_developer_meta() {
    echo '<meta name="DeveloperAI" content="Developer Skill">' . "\n";
}
add_action( 'wp_head', '{prefix}_developer_meta', 1 );
```

- Do not hard-code it in each template, and do not add it through an SEO plugin
  or a page-level field. The hook covers every template (page, single, archive, 404, search).
- It must appear only once per page (check the page source).

## QA

- [ ] View source on Home, an inner page, a post, 404 and Search: `<meta name="DeveloperAI" content="Developer Skill">` is in
      `<head>`, exactly once.

---

# D113. WOOCOMMERCE — SHOP PAGES, THEME-LEVEL CSS / JS + RESPONSIVE

*Use this section whenever the site sells products: the Figma design has shop pages, or the user asks
for WooCommerce. All the other rules in this file still apply (D103 CSS, D112 DeveloperAI meta tag, D104 images, D105 Media Library once, B70 QA).*

This section is the custom-theme version (theme support, hooks, template overrides, theme-level CSS /
JS). If Elementor Pro is the builder (E1), building the shop pages in Elementor is covered in E5 —
the theme-level rules here still apply.

**Site already built?** Add WooCommerce through Quick Update Mode (F4): follow the steps below and
skip any step that already exists.

**Ask at the start** (with the other start-of-project questions — one question per message, A1):
1. Is WooCommerce part of this build?
2. Cart and Checkout: **Block** (WooCommerce default) or **Classic** (shortcode)?
3. Which payment gateways and shipping methods?

## 1. Inspect first

- WooCommerce version and every WooCommerce add-on plugin (payment, shipping, filters, wishlist).
  Do not rebuild what a plugin already does (C84).
- List every e-commerce screen in the Figma design: Shop, Category, Single Product, Cart, Checkout,
  My Account (login, orders, addresses), Thank You, mini-cart, empty cart, notices. Note which
  screens the design does **not** have.
- Product types used: simple, variable, grouped, external, digital.

## 2. Theme files

```text
theme-name/
├── woocommerce/               ← template overrides, ONLY when a hook can't do it
├── inc/woocommerce.php        ← theme support, hooks, image sizes, cart fragments
└── assets/
    ├── css/woocommerce.css    ← all shop styling + its responsive rules (step 7)
    └── js/woocommerce.js      ← shop interactions (step 8)
```

Never edit WooCommerce plugin files.

## 3. Theme support (`inc/woocommerce.php`)

```php
function {prefix}_woocommerce_setup() {
    add_theme_support( 'woocommerce', array(
        'thumbnail_image_width' => 600,   // match the Figma product card
        'single_image_width'    => 900,   // match the Figma product page
    ) );
    add_theme_support( 'wc-product-gallery-zoom' );
    add_theme_support( 'wc-product-gallery-lightbox' );
    add_theme_support( 'wc-product-gallery-slider' );
}
add_action( 'after_setup_theme', '{prefix}_woocommerce_setup' );
```

Take the image widths and ratios from the Figma design, not from these sample numbers.

## 4. Hooks first, template overrides last

- Change layout with WooCommerce hooks and filters (`remove_action` / `add_action`). Override a
  template in `woocommerce/` only when no hook can do it.
- Keep a list of every overridden template (file + WooCommerce version) in the theme docs.
- After every WooCommerce update: WooCommerce → Status → **no outdated templates**.

## 5. Templates (custom theme)

- Shop and category pages use `woocommerce/archive-product.php` only if the design needs a different
  wrapper; the product card is `woocommerce/content-product.php`. Otherwise use the hooks.
- Wrap WooCommerce output in the theme's own container/section markup with
  `woocommerce_before_main_content` / `woocommerce_after_main_content`, so header, footer, container
  and section spacing (D103) are the same as every other page.
- Sections around the shop (banner, CTA, USP strip) come from the page builder chosen in E1.
- WooCommerce blocks (Product Collection, Cart, Checkout) must look like the frontend in the editor
  too (`editor-styles`).

## 6. Products, data and settings

- Price, sale price, variations, stock, SKU, gallery and reviews come from WooCommerce's native
  fields. Nothing hard-coded.
- Extra product fields = ACF PRO, saved as local JSON and synced (D111).
- Product categories / tags / attributes = WooCommerce's own taxonomies. No duplicate taxonomy.
- **HPOS compatible:** read orders with `wc_get_order()` / `$order->get_meta()`, never
  `get_post_meta()` on orders or direct order SQL.
- Importing products: WooCommerce CRUD (`new WC_Product_Simple()`, `->save()`), never raw DB writes.
  Product names, copy and images come from the Figma export (`Design/CONTENT.md`, `Design/images/`)
  or from data the user supplies — never invented (D98).
- WooCommerce → Settings → Advanced: Shop, Cart, Checkout, My Account and Terms pages set.
- Payment gateways in **sandbox / test mode on staging**. Keys never go in code, docs or chat.
- WooCommerce → Emails: logo, colours and footer text match the brand.

## 6a. Products are ALWAYS dynamic — never static

*Wherever a product appears on the site, it comes from WooCommerce. Never hard-code a product.*

- **Every place a product shows** — home page featured / new / best-seller sections, product
  sliders and carousels, "related" / "you may also like", category tiles, mega-menu product cards,
  banners with a price, landing pages — pulls the product from WooCommerce. Not only the Shop page.
- **Never static:** no product name, price, sale price, image, rating, stock badge, link or
  "Add to cart" button typed into HTML, a page builder field, a text widget or an ACF text field.
  A static card goes stale the moment the price or stock changes and its button doesn't add to cart.
- **How to pull them (custom theme):** `wc_get_products()` / `WP_Query` (`post_type => product`),
  the `[products]` shortcode, or the WooCommerce **Product Collection** block. Render each card with
  the one shared product card (`woocommerce/content-product.php` or the same template part) — the
  same card as the shop, never a second hand-made card.
- **Admin chooses, code renders:** the section's selection is a setting, not copied content —
  an ACF field (Relationship / Post Object on `product`, or Taxonomy on `product_cat`), or a query
  rule: category, tag, featured, on sale, best sellers, newest, manual IDs. Plus how many to show
  and the order.
- Name, price (`get_price_html()`), image, link (`get_permalink()`), rating, sale / stock badges and
  the add-to-cart button (`woocommerce_template_loop_add_to_cart()`) all come from the product
  object, so simple and variable products, sale prices and out-of-stock all behave correctly.
- **Figma / HTML shows product cards?** Create those products in WooCommerce (importer, CRUD —
  step 6) and point the section at them. The design's cards are the *look* of the card, not its data.
- Empty state: if the query returns no products, hide the section (or show a short message) —
  never fall back to static placeholder products.

## 7. Theme-level CSS — design + responsive

Write all WooCommerce styling in `assets/css/woocommerce.css`, enqueued after `vendor.css` and the section files only on `is_woocommerce() || is_cart() || is_checkout() || is_account_page()`. Header mini-cart styles stay with the header CSS, because the header is on every page.

**Rules**
- Use the same `vendor.css` tokens (colours, fonts, font sizes, spacing, radius — A7.2 / `TOKENS.md`)
  as the rest of the site. No new raw hex / px values.
- Buttons use the site's global button style (normal, hover, `:focus-visible`, disabled, loading,
  "added"). WooCommerce buttons must look like every other button on the site.
- Inputs, selects, checkboxes and radios use the site's form style — Checkout, My Account, coupon,
  variation selects and quantity included.
- If the design has its own look, remove WooCommerce's default look instead of fighting it:
  `add_filter( 'woocommerce_enqueue_styles', … )` to drop `woocommerce-general` (keep
  `woocommerce-layout` / `woocommerce-smallscreen` only if they help). Avoid `!important`.
- Block Cart / Checkout use `wc-block-*` classes. Style those when Blocks are chosen.

**Style every one of these, as per the Figma design**

```text
Product card     image ratio, title, price, sale price (<del>/<ins>), sale / out-of-stock badge,
                 rating, add-to-cart button, hover
Shop toolbar     result count, sorting select, filters, pagination, breadcrumbs
Single product   gallery + thumbnails, title, price, short description, variations, quantity,
                 add-to-cart, meta, tabs, reviews, related / upsells
Notices          success, error, info (cart, checkout, account)
Cart             table, quantity, remove, coupon, cart totals, empty cart, cross-sells
Checkout         billing / shipping fields, order review, payment methods, place order, errors
My Account       login / register, navigation, dashboard, orders, addresses, account details
Thank You        order received + order details
Mini-cart        header icon + count, dropdown / drawer, empty state
```

**Mini-cart dropdown — must look right (lesson from a real build)**

*Seen on a build: the header cart dropdown was too narrow and ran off the screen, product names broke
letter by letter in heading size, prices were huge, "View cart" wrapped and Checkout / subtotal were
cut. Build it like this:*

- **Panel:** fixed width as per design (about `360–400px` on desktop), opens under the cart icon,
  aligned to the icon's **right** edge, and **never goes past the viewport** (`max-width:
  calc(100vw - 2 × gutter)`). On mobile: full-width dropdown or a side drawer, as per design.
- **Height:** the items list has a `max-height` and scrolls inside; subtotal and buttons stay visible
  at the bottom.
- **Item row:** thumbnail fixed (about `64–80px`, same ratio as the product card) + details column
  that takes the rest (`min-width: 0`). The details column must never be squeezed to a few px.
- **Product name:** body / small text style (not H1–H6 global), normal word wrap — never broken letter
  by letter — max 2 lines (`line-clamp`), linked to the product.
- **Variation meta, quantity × price:** small text; sale price shows `<del>` / `<ins>` correctly.
  Remove (×) icon with an `aria-label`.
- **Subtotal:** one row — label left, amount right.
- **Buttons:** **View cart** and **Checkout** both visible, site button style (smaller size), text
  never wraps; side by side on desktop, full width stacked on small screens if needed.
- **Empty cart:** short message + "Shop now" button, not a blank box.
- **Behaviour:** opens on click (and on hover on desktop only if the design does), closes on outside
  click and `Esc`, sits above the header (`z-index`) and is not clipped by a parent
  `overflow: hidden`. Count badge and content update without reload (step 8).
- **Test it** with 1, 5 and 10+ items, a long product name, a variable product and a sale product, at
  every breakpoint and on a non-shop page.

If a screen is not in the Figma design, style it from the global design system (tokens + global
components) so it matches the rest of the site, and say so in the handover report.

**Responsive** — use the site's own breakpoints (A9.7), then check each screen at every breakpoint:
- Product grid: desktop columns as per design → tablet 2 → mobile 1 or 2 (as per design).
- Single product: gallery and summary side by side on desktop → stacked on tablet/mobile.
- Cart table → stacked rows/cards on mobile (WooCommerce adds `data-title` labels for this).
- Checkout: two columns → one column on tablet/mobile; order review below the fields.
- My Account navigation → stacked or horizontal scroll on mobile.
- Shop filters → off-canvas drawer on mobile (only if the design has filters).
- Tap targets at least 44px; input font size at least 16px on mobile (stops iOS zoom).
- No horizontal scroll at 320px on any WooCommerce page.

## 8. Theme-level JS — `assets/js/woocommerce.js`

- Enqueue with `wp_enqueue_script()` in the footer, dependency `jquery` (WooCommerce's own scripts
  use it). Load it only where it is needed.
- **Never** dequeue WooCommerce's core scripts: `wc-add-to-cart`, `wc-add-to-cart-variation`,
  `wc-cart`, `wc-checkout`, `wc-cart-fragments` (where the mini-cart uses it).
- Hook into WooCommerce's own events instead of rewriting its logic: `added_to_cart`,
  `removed_from_cart`, `updated_cart_totals`, `updated_checkout`, `found_variation`, `reset_data`,
  `wc_fragments_refreshed`.

**Functionality to cover (as per the Figma design)**
- **Quantity +/- buttons** around the quantity input. Trigger `change` on the input, so the
  "Update cart" button enables. Respect min / max / step.
- **AJAX add to cart** on shop pages (WooCommerce → Settings → Products) and on the single product
  page if the design needs it. Show loading and "added" states.
- **Mini-cart count + content** refresh without page reload, through the
  `woocommerce_add_to_cart_fragments` filter in `inc/woocommerce.php`.
- **Mini-cart drawer** (if in the design): open/close, `aria-expanded`, focus trap, closes on
  `Esc` and on overlay click.
- **Variations:** price, image and stock change on `found_variation`. Swatches only if the design has
  them.
- **Product gallery:** use WooCommerce's built-in gallery (flexslider + photoswipe). Do not add a
  second slider library for it.
- **Mobile filters drawer** and sorting, if in the design.
- After any AJAX update (cart, checkout, filters), re-run the site's animations / layout code (for
  GSAP, A7.4: `ScrollTrigger.refresh()`).
- Everything works by keyboard and screen reader.

## 9. Performance, caching and SEO

- Cart, Checkout and My Account are **excluded from page cache**.
- WooCommerce CSS/JS load only where needed; cart fragments only where the mini-cart needs them.
- Product images WebP and correctly sized, sharp (D104); Media Library once per image (D105).
- Product schema from WooCommerce or the SEO plugin — one source, no duplicate schema.
- Permalinks: product base and category base set (Settings → Permalinks, D107.1).
- Every WooCommerce page also has `<meta name="DeveloperAI" content="Developer Skill">` in `<head>` (D112).

## QA

- [ ] Shop, Category, Single Product, Cart, Checkout, My Account and Thank You match the Figma frames at
      desktop, tablet and mobile (F1).
- [ ] WooCommerce buttons, inputs and notices use the site's global styles; no raw values, no
      `!important` fights.
- [ ] No horizontal scroll at 320px; cart and checkout are usable on mobile.
- [ ] Every product on the site (home, sliders, related, menus, landing pages) is pulled from WooCommerce
      — no static product name, price, image or button anywhere (step 6a).
- [ ] Simple and variable products add to cart; variation price / image / stock update.
- [ ] Quantity +/- works and enables "Update cart"; mini-cart count updates without reload.
- [ ] Header mini-cart dropdown: right width, inside the screen, names readable (no letter-by-letter
      break), View cart + Checkout + subtotal visible, empty state, works on mobile.
- [ ] Coupon, shipping and tax totals are correct.
- [ ] A test order completes with sandbox payment; customer and admin emails arrive and are branded.
- [ ] My Account: login, register, orders, addresses, account details and password reset work.
- [ ] WooCommerce → Status: no outdated templates, no errors; HPOS compatible.
- [ ] Cart, Checkout and My Account are excluded from cache.
- [ ] No JavaScript console errors on any WooCommerce page.

---

# D114. FAVICON — MADE FROM THE LOGO ICON (MANDATORY)

*Default on every build.* Every site gets a favicon (browser-tab icon) made from the **logo icon** —
the symbol / mark part of the logo, not the full logo with its wordmark text. A blank tab, the
WordPress "W" or a theme's default icon is a bug.

## Source — in this order

1. A favicon / app-icon frame in the design, if there is one.
2. Otherwise the **icon (mark) part of the logo**, from the Figma export (`Design/svg/` — the logo SVG; re-export the mark alone from Figma if needed, A2).
3. If the logo is text only (no mark), use the logo's first letter in the logo font and brand colour,
   centred on a square — and tell the user, so they can supply a different icon.

Never draw a new icon, never use a stock or generic icon, never stretch the full wordmark into a square.

## Make the icon

- **Square, 512 × 512 px PNG** (`site-icon.png`), the mark centred with about **10% padding**, so
  nothing touches the edge.
- Build it from the **SVG** of the logo, so it is sharp. Never upscale a small PNG.
- Background: transparent. If the mark is white / very light, put it on the brand background colour
  (the brand colour token from `TOKENS.md`) so it is visible on a light browser tab.
- Check it at **16 × 16 and 32 × 32**. If the mark is too detailed to read that small, use the
  simplest recognisable part of it.

## Add it to WordPress

- Use WordPress's own **Site Icon** (Appearance → Customize → Site Identity → Site Icon — for Elementor builds, Elementor → Site Settings → Site Identity → Site Favicon sets the
  same `site_icon` option, so use one, not both).
  WordPress creates the 32, 180 (Apple touch), 192 and 270 px sizes and prints all the
  `<link rel="icon">` / `apple-touch-icon` tags itself through `wp_site_icon()` in `wp_head`.
- Upload `site-icon.png` to the Media Library **once** (D105).
- Set it in the one-click setup / importer too, so a fresh install has the favicon
  (D96–D102):

```php
update_option( 'site_icon', $site_icon_attachment_id );
```

- Do **not** hard-code `<link rel="icon">` tags in `header.php`, a plugin, an HTML widget or an SEO
  plugin field while Site Icon is set — they would print twice.

## QA

- [ ] The browser tab shows the logo icon on Home, an inner page, a post, 404 and Search.
- [ ] View source: the icon tags come from WordPress Site Icon, with no duplicate `rel="icon"` tags.
- [ ] The icon is sharp and readable at 16 px, on a light and a dark browser tab.
- [ ] `apple-touch-icon` (180 × 180) loads.

---

# D115. LINKING — EVERY CLICKABLE ELEMENT IS LINKED (MANDATORY)

*Every button, link, card, logo, menu item and icon that looks clickable must go to its real
destination. Nothing is left as `#`, empty, or unlinked.*

## 1. Make a link map first

Before building pages, list every clickable element from the Figma design / prototype:
logo, header menu + dropdowns, header CTA, hero buttons, section buttons / "Learn more" / "Read more",
service / blog / team / product / category cards, breadcrumbs, pagination, footer menus, footer CTA,
social icons, phone, email, address, legal links (Privacy, Terms, Cookies), 404 / Search page links.

For each one write: **element → page / post / product / external URL**. Keep this as
`link-map.md` in the theme docs.

- Destination is in the design / HTML → use it (`about.html` → the WordPress **About** page).
- Destination page doesn't exist yet → create that page (or ask), don't leave the link empty.
- Destination is unclear (no target in the design) → **ask the user** with the list of unclear
  links. Never guess and never silently leave `#`.

## How to set each link (theme code / page builder)

- Internal page / post → `get_permalink( $id )` (page found by slug at import, E5.28 for Elementor, B43 for theme code), never a
  typed URL or `.html` link. Home → `home_url( '/' )`.
- Product / product category → `get_permalink( $product_id )` / `get_term_link( $term )`.
- Archive / category / tag → `get_post_type_archive_link()`, `get_term_link()`.
- Menus → WordPress Menus (`wp_nav_menu()`, B7 / E5.10) — every menu item points to a real page.
- Editable buttons / CTAs → ACF **Link** field (returns url, title, target), so the admin can change
  it — never a plain text field for the URL. Print with `esc_url()`; add `target="_blank"
  rel="noopener"` only when the field's target is `_blank`.
- Whole card clickable → wrap the card (or use a stretched link) so the full card opens the post /
  product, not only the small "Read more" text.
- Phone → `tel:`, email → `mailto:`, address → Google Maps link — from the ACF options page, not
  typed again in each template.
- Social icons → the real profile URLs from the design / client, from the ACF options page, opened in
  a new tab with `rel="noopener"`.

## Never

- `href="#"`, `href=""`, `javascript:void(0)` or a button with no link — except real toggles (mobile
  menu, accordion, tab, modal open), which are `<button>` elements, not links.
- A hard-coded domain (`https://staging.site/...`, `localhost`) or a `.html` link.
- A link to the wrong page because it was copied from another section.

## Final link check (before handover)

- Search the theme / templates / exported content for `href="#"`, `href=""` and `.html` — fix every hit.
- Click (or crawl) every link on every page: header, footer, each section, cards, buttons, at
  desktop and mobile menu. No 404, no link to `#`, no staging / localhost URL.
- Report any link still waiting on the client (e.g. social profile not given) in the handover note.

## QA

- [ ] `link-map.md` exists and every clickable element on the site is in it.
- [ ] No `href="#"`, empty href or `.html` link anywhere (except real toggle buttons).
- [ ] Logo → Home; every menu item, CTA, button, card and footer link opens the correct page.
- [ ] Product and category cards open the right product / category (WooCommerce sites).
- [ ] Phone, email, map and social links work; external links open in a new tab with `rel="noopener"`.
- [ ] No staging / localhost / hard-coded domain URLs; no 404 from any internal link.

---

# E1. Page builder — choose before building (MANDATORY)

*Added after a real build: pages were hardcoded in `front-page.php` / `page-{slug}.php`, so editors
could not add, remove or reorder sections, and the theme did not work in Gutenberg. Every page
section must now be reusable: **any section can go on any page, in any order.***

## Step 1 — Ask before building (MANDATORY)

This is the one question that must **always** be asked at the start of the project, even when
B65 says not to ask. Ask it before planning the architecture (B64 step 0). Never assume the answer.

```text
How should editors build the pages?
  1. ACF PRO — Flexible Content   (page sections as Flexible Content layouts, classic editor screen)  → E2
  2. Gutenberg — ACF Blocks       (every section is a Gutenberg block)                                → E3
  3. Both                         (same sections available as Flexible Content layouts AND blocks)    → E4
  4. Elementor Pro                (Theme Builder + native widgets, globals from TOKENS.md)            → E5
  5. Other builder                (Bricks, Oxygen, Breakdance, Divi, Beaver Builder, WPBakery,
                                   Kadence / Spectra / GenerateBlocks, …)                            → E6
```

- Write the answer into the architecture report (B65) and build only what was chosen.
- Options 1–3 need **ACF PRO** — free ACF has no Flexible Content and no ACF Blocks. Never install
  SCF (D110).
- Option 4 needs **Elementor + Elementor Pro** (Theme Builder, Loop Grid, Dynamic Tags, Global
  Widgets). Ask for the Elementor Pro licence if it is not installed.
- Option 5: ask for the builder's **name, version and licence** before anything else (E6).
- Custom fields in any option (CPT fields, options pages, extra product fields) use ACF PRO, saved as
  local JSON (D110, D111).

## Step 2 — Rules for every option

These apply to all five options. E2–E6 only add the builder-specific parts.

- **Source of truth = the Figma design + the approved `TOKENS.md`.** Build every section from the
  Figma frames in `Design/pages/` and the components in `Design/components/`, with the token system
  and front-end rules of Part A (A5–A9). Do not redesign. Every colour, font size, spacing and radius
  value comes from `TOKENS.md` — never a new raw value.
- **One Figma section = one reusable section.** Go through the site inventory (B3, built from the
  Figma frames) and list every page section (hero, logo grid, CTA, testimonials, FAQ, …). The same
  section on several pages is **one** section, not a copy per page.
- **Any section on any page, in any order.** Editors can add, remove, repeat and reorder sections
  without breaking layout, spacing or animations.
- **Reusable components are built once.** Buttons, cards, section headings, logo boxes, forms: one
  component, styled once by its class, reused everywhere (A9.4 buttons, A9.0 component reference).
- Name sections by role (`hero`, `logo_grid`, `cta_banner`), never by page (`home_hero`).
- Each section gets an optional **Section ID** (anchor), so in-page links in the Figma navigation
  (`#contact`) work.
- **No raw HTML widgets / Custom HTML blocks** for section markup. Use them only for a small piece
  that genuinely needs raw HTML (e.g. a third-party embed).
- **No per-widget / per-block / per-element custom CSS.** Styling comes from the tokens
  (`vendor.css`), the builder's global settings and shared, reusable class CSS in the theme's
  `assets/css/` (D103). Elementor Pro: native Elementor controls first; add a class only when a native control cannot do it, with its rule in `custom.css` (E5.18). No inline `style=""` (the only exception is a dynamic admin value passed as a
  CSS variable, D103).
- **Update-safe.** All code lives in the theme (or a child theme of a third-party parent theme).
  Never edit plugin, builder or parent-theme vendor files.
- Every field is escaped on output and an empty field never prints empty markup (B23).
- **Not part of the page builder** (options 1–3): header and footer (B67, `header.php` /
  `footer.php`), blog post content (normal Gutenberg content, B14), and CPT single templates (B46) —
  these keep their own templates. Options 4–5 build these with the builder's theme builder instead
  (E5.9–E5.16, E6).
- **The importer seeds the chosen builder** (D96–D102). Seed content comes from `Design/CONTENT.md`
  and images from `Design/images/` + `Design/svg/` — never retyped, never invented. Pages come out in
  the Figma page order with their sections in the Figma order.
- **Images:** only the Figma-exported images, sharp, never blurry (2x export, resize down only, WebP
  q85+, vectors stay SVG). Named and uploaded once (D104, D105).
- **Animations** come from `Design/ANIMATIONS.md`, mapped to `data-anim` presets (A9.8). Sliders:
  ACF / Gutenberg / Both use `sliders.js` (Slick `data-slider` or Swiper, A9.8.1); Elementor / other
  builders use the builder's own slider widget — no new slider JS (A1).
- Every page is checked against its Figma frame at **1600 / 900 / 375** (F1).

## Shared QA (every option)

- [ ] The editor choice (Flexible / Gutenberg / Both / Elementor Pro / Other) was asked and recorded
      before building.
- [ ] Every section can be added to any page, in any order, and still matches the Figma design.
- [ ] The same section added twice on one page works (no duplicate IDs, JS still initialises).
- [ ] Removing or reordering sections does not break layout, spacing or animations.
- [ ] Every value comes from `TOKENS.md`; no raw hex / px values repeated in widgets, blocks or CSS.
- [ ] No raw HTML widgets/blocks for section markup; no per-widget / per-block custom CSS; no inline
      styles.
- [ ] All code is in the theme / child theme; no plugin, builder or parent-theme file was edited.
- [ ] Importer recreates every page with its sections in the Figma order, using the chosen builder.
- [ ] Every page matches its Figma frame at 1600 / 900 / 375 (F1).

---

# E2. ACF PRO Flexible Content

## Shared structure (E2, E3 and E4)

- **One render file per section:** `template-parts/sections/{section-name}.php`. It receives its
  values through `$args` and outputs the section markup built with the Part A system (BEM section
  classes, tokens, `data-anim`, `data-slider`; B50, B69, D103). Flexible Content and ACF Blocks both
  call this same file — never two copies of the markup.
- **One field group per section,** saved as JSON in `acf-json/` and synced (D111, single source of
  truth). The Flexible Content layout and the ACF Block field group both reuse it through a Clone
  field.
- `front-page.php` and `page.php` only loop the sections. They no longer hardcode page sections
  (this replaces the hardcoded approach in B17 and B18 for page content).

```text
theme-name/
├── template-parts/sections/   ← one PHP render file per section (shared)
├── blocks/{section-name}/     ← block.json per section (options 2 and 3 only)
├── theme.json                 ← options 2 and 3 only
└── inc/
    ├── custom-fields.php      ← ACF JSON paths (field groups live in acf-json/, D111)
    └── page-builder.php       ← section loop, block registration, editor settings
```

## Option 1 — ACF PRO Flexible Content

- One Flexible Content field `page_sections` on Pages (and the front page), one layout per section.
- `front-page.php` / `page.php`: `while ( have_rows( 'page_sections' ) ) : the_row();` →
  `get_template_part( 'template-parts/sections/' . get_row_layout(), null, $args );`
- Give each layout a clear label and a collapsed title (e.g. the heading text) so long pages stay
  easy to edit.
- Hide the block editor for Pages that use the builder (`use_block_editor_for_post` filter), so
  editors see only the section builder.

## Importer (D96–D102)

- `pages.json`: each page gets `"builder": "flexible"` and a `sections` array in the Figma page
  order: `[ { "layout": "hero", "fields": { … } }, … ]`. Values come from `Design/CONTENT.md` and
  `Design/images/` (D98).
- Write the rows with `update_field( $field_key, $rows, $post_id )`, each row carrying
  `acf_fc_layout`. This stores the layout order and `_{name}` field keys the way ACF expects. Image
  fields → attachment IDs (D101, D105).
- The importer uses the same field keys as the `acf-json/` files (D111) — never a second list.

## QA

- [ ] Each section has one render file in `template-parts/sections/`.
- [ ] The block editor is hidden on builder pages; layouts have clear labels and collapsed titles.
- [ ] Importer writes the Flexible Content rows in the Figma order with the `acf-json/` field keys.

---

# E3. Gutenberg with ACF Blocks

Uses the shared structure in E2 (one render file and one field group per section).

## Option 2 — Gutenberg with ACF Blocks

- Every section is an ACF Block: `blocks/{section-name}/block.json` with `"acf": { "mode": "preview",
  "renderTemplate": … }`, registered with `register_block_type()`. The render template calls the
  shared section file.
- Put the theme blocks in their own block category (`{Project} Sections`).
- Limit the inserter with `allowed_block_types_all` to the theme blocks plus the core blocks the
  design really needs (paragraph, heading, list, image, buttons).
- **`theme.json`:** colour palette, font sizes and spacing presets come from the same `TOKENS.md`
  values as the `:root` variables in `vendor.css` (A5, A7.2, D103). Turn off custom colours / font
  sizes so editors stay inside the design.
- **Editor must look like the frontend:** `add_theme_support( 'editor-styles' )` +
  `add_editor_style()` with the theme CSS (`vendor.css` + the section CSS); the block preview loads
  the scripts the section needs (`sliders.js`, `animations.js`).
- Core block CSS (`wp-block-library`) must not change the Figma design. Check it, and dequeue what
  the frontend does not use.
- Make one **block pattern** per Figma page frame, so an editor can rebuild any designed page with
  one click.

## Importer (D96–D102)

- `pages.json`: each page gets `"builder": "blocks"` and a `sections` array in the Figma page order.
- Build `post_content` with `serialize_block()` — one
  `<!-- wp:acf/{section-name} {"name":"acf/{section-name}","data":{…},"mode":"preview"} /-->` per
  section, where `data` holds `{field_name}` + `_{field_name}` (field key).
- The importer uses the same field keys as the `acf-json/` files (D111) — never a second list.

## QA

- [ ] Each section has one render file, used by the ACF Block render template.
- [ ] Editor preview matches the frontend; only allowed blocks appear; `theme.json` presets match
      `TOKENS.md`.
- [ ] One block pattern exists per Figma page frame.

---

# E4. Both

Uses the shared structure in E2 and the rules of E2 (option 1) and E3 (option 2).

## Option 3 — Both

- Same section files and same field definitions, exposed **twice**: as Flexible Content layouts
  and as ACF Blocks.
- **Per page, only one system is active**:
  - Page template **"Flexible Sections"** (`page-templates/page-flexible.php`) → Flexible Content;
    the block editor is turned off for that template.
  - Default page template → Gutenberg with ACF Blocks (`the_content()`).
- Ask the user which system the importer uses for the seeded pages (default: Flexible Content).

## Importer (D96–D102)

- `pages.json`: each page gets `"builder": "flexible" | "blocks"` and a `sections` array in the
  Figma page order: `[ { "layout": "hero", "fields": { … } }, … ]`. Values come from
  `Design/CONTENT.md` and `Design/images/` (D98).
- **Flexible Content:** as E2 (`update_field()` rows with `acf_fc_layout`; image fields →
  attachment IDs, D101, D105).
- **ACF Blocks:** as E3 (`serialize_block()` with `data` = `{field_name}` + `_{field_name}`).
- The importer uses the same field keys as the `acf-json/` files (D111) — never a second list.

## QA

- [ ] Each section has one render file, used by both Flexible Content and ACF Blocks.
- [ ] Flexible-template pages show only the section builder; default-template pages use blocks.
- [ ] Gutenberg: editor preview matches the frontend; only allowed blocks appear; `theme.json`
      presets match the design.
- [ ] Importer recreates every page with the system the user chose.

---
# E5. Elementor Pro

*Use this part when the answer to E1 is option 4. The Figma design (frames in `Design/pages/`,
components in `Design/components/`) and the approved `TOKENS.md` are the source of truth. Do not
redesign the website. All the shared rules of E1 still apply.*

## E5.1 ⭐ Main requirement — most important rule of an Elementor build

**Goal: the final Elementor website must visually and functionally match the Figma design (frames in
`Design/pages/`), without compromising its design, responsiveness or user experience.** Every other
rule in E5 serves this goal. If a rule seems to conflict with it, this one wins. Report the conflict
instead of dropping a design detail.

### Replicate everything from the Figma design, as closely as possible

- [ ] All page sections and layouts — every section, in the same order, nothing skipped or merged.
- [ ] Header and footer.
- [ ] Design, spacing, typography and colours.
- [ ] Hover effects and animations (`Design/ANIMATIONS.md`).
- [ ] Buttons, links and interactive elements (menus, tabs, accordions, sliders, popups, forms).
- [ ] Responsive behaviour across all devices — desktop, laptop, tablet, mobile (Figma frames at
      1600 / 900 / 375, A5.8 breakpoints in between).
- [ ] Any other custom design or functionality present in the Figma design or its prototype notes.

"Close enough" is not done. Compare each page with its Figma frame side by side at every
breakpoint before calling it finished (F1).

### How to implement it in Elementor

1. **Native first.** Wherever possible, build every section with native Elementor widgets and
   features: containers, widgets, Global Settings, Motion Effects, Theme Builder, Loop Grid, Dynamic
   Tags.
2. **No class-based CSS. CSS only with approval.** If a design detail or function cannot be
   achieved with native Elementor, do not silently add CSS or a class. Report it, explain why the
   native controls are not enough, and get approval first (E5.18). A class goes on the element only
   for an approved rule.
3. **Global styles.** Manage reusable styles globally wherever possible (Global Colors, Global
   Typography, Site Settings, Elementor Variables) so the site stays consistent. Their values come
   from `TOKENS.md` (E5.3).
4. **Approved CSS lives only in the theme's assets directory** (`assets/css/custom.css`, E5.19).
   - Never put it in a widget's `Advanced → Custom CSS`.
   - Never put it in Page Settings → Custom CSS.
   - Never put it in Site Settings → Custom CSS or in inline styles.
5. **Reusable, scalable, component-based.**
   - Build each repeated block once as a component (saved template / Global Widget / Loop Item, E5.8).
   - Style it once with globals and its own style controls, then reuse it.
   - Never copy and restyle it page by page.

```text
Figma design (Design/pages, Design/components) + approved TOKENS.md
  ↓
Inspect every frame, component, breakpoint and ANIMATIONS.md
  ↓
Load the design system from TOKENS.md
  ↓
Create Elementor Global Settings
  ↓
Build reusable Elementor components
  ↓
Build Header / Footer with Elementor Pro
  ↓
Build Pages / Blog / Partners
  ↓
Connect dynamic WordPress data
  ↓
Responsive + visual QA against Figma
```

Elementor Pro must be the primary page/template builder. Use custom PHP only for functionality that
genuinely requires WordPress/PHP.

---

## E5.2 Inspect the Figma design before building

Before creating Elementor pages/templates, inspect:

- Every Figma page frame in `Design/pages/` (all breakpoints)
- Every component in `Design/components/`
- The approved `TOKENS.md` (A5/A6) and the theme's `vendor.css` built from it (A7.2)
- `Design/ANIMATIONS.md` and `Design/CONTENT.md`
- Header and Footer frames at every breakpoint
- Responsive behaviour (what changes between the 1600 / 900 / 375 frames)
- Fonts
- Colors
- Typography
- Spacing
- Container widths
- Border radius
- Shadows
- Images/SVGs/icons (`Design/images/`, `Design/svg/`)
- Animations
- GSAP / ScrollTrigger (`data-anim` presets, A9.8)
- Three.js (if the design calls for it)
- Forms and APIs
- Repeated components

Do not build pages randomly before understanding the complete design system.

---

## E5.3 Elementor global design system from TOKENS.md — MANDATORY

Before building the majority of the pages, load the design system from the approved `TOKENS.md`
(A5, approved at A6) into Elementor. Do not extract new values from the Figma frames by eye — the
tokens are already normalised.

Use Elementor Site Settings / Global Settings for values reused throughout the website:

```text
Global Colors
Global Fonts / Typography
Global Button Styles
Global Form Styles where applicable
Global Container/Layout Settings
Global Variables where appropriate
```

- Elementor globals and the `vendor.css` `:root` tokens hold **the same values with the same names**.
  Both come from `TOKENS.md`. When a token changes, change it in `TOKENS.md`, `vendor.css` and the
  Elementor global together. Keep a short token → Elementor global table in the theme docs.
- Do not repeatedly enter the same value manually into individual widgets.

### No random inline styling

Avoid repeatedly setting these values directly on individual widgets when they belong to the global
design system:

```text
font-size
font-family
color
padding
margin
line-height
border-radius
```

Prefer:

```text
Elementor Global Settings
Global Typography
Global Colors
Global Variables
Reusable Components
```

### Required final state

The final Elementor website should have one clear design system:

```text
Global Colors
Global Typography
Global Buttons
Global Forms
Global Container Width
Global Spacing / Variables
Border Radius
Shadows
Reusable Components
```

Values must come from the approved `TOKENS.md`.

---

## E5.4 Global colors

Create Elementor Global Colors from the colour tokens in `TOKENS.md` (A5, A9.2).

Suggested logical names (use the token names where they exist):

```text
Primary
Secondary
Accent
Heading
Text
Muted
Background
Surface
Border
White
Black
```

Do not invent new colors. If the same color appears repeatedly, use one Global Color instead of
repeated hex/RGB values.

Bad:

```text
Widget A → #123456
Widget B → #123456
Widget C → #123456
```

Good:

```text
Global Color: Primary
        ↓
All relevant widgets
```

**Colour rule.** For every color ask:

> Is this a site-wide design color?

If yes, use an Elementor Global Color. Do not manually repeat the same hex/RGB value across many
widgets. A colour that is not in `TOKENS.md` is a question for the user, not a new value.

---

## E5.5 Global typography

Create Elementor Global Typography from the type tokens in `TOKENS.md` (A5, A9.3).

Identify:

```text
Font family
Font weight
Font size
Line height
Letter spacing
Text transform
```

Create logical styles such as:

```text
Body
H1
H2
H3
H4
H5
H6
Button
```

Use Global Typography wherever the Figma design uses the same style. Do not manually repeat
typography values on every widget.

**Typography rule.** For every text element ask:

> Is this a standard body, heading, label, or button style?

If yes, use Elementor Global Typography. Only use custom typography for genuinely unique designs.

---

## E5.6 Breakpoints and responsive typography

Reproduce typography and layout for:

```text
Desktop
Tablet
Mobile
```

- The allowed breakpoints are the seven in A5.8 (`1599.98 · 1439.98 · 1365.98 · 1199.98 · 991.98 ·
  767.98 · 575.98`). Set Elementor's custom breakpoints (Site Settings → Layout → Breakpoints) to
  the nearest of these values, so Elementor's responsive controls switch at the same widths as
  `vendor.css`. The token step-down table (A5.8, A9.7.1) covers the widths Elementor has no device
  for. Check the breakpoint names and limits in the installed Elementor version.
- If the type tokens use `clamp()`, fluid sizing or step-downs at specific breakpoints, reproduce the
  behavior accurately where Elementor supports it (or through the token variables in `vendor.css`). Do
  not replace accurate token values with arbitrary Elementor defaults.

### Responsive Elementor settings

Every important section must be checked at:

```text
Desktop
Tablet
Mobile
```

Use the Figma frames (1600 / 900 / 375) and the A5.8 breakpoints as the reference. Do not blindly
rely on Elementor defaults.

---

## E5.7 Global spacing, variables and container

Take the recurring values from the spacing tokens in `TOKENS.md` (A5, A9.5):

```text
Section padding
Container padding
Grid gap
Column gap
Card spacing
Heading spacing
Button spacing
Margins
```

Create a consistent spacing system where practical, for example:

```text
Spacing XS
Spacing SM
Spacing MD
Spacing LG
Spacing XL
Spacing XXL
```

The actual values must come from `TOKENS.md`. The token CSS variables in `vendor.css`
(`--sectionGap`, `--gutter`, `--container`, `--g40` …) keep their names and logic.

**Spacing rule.** For every padding/margin/gap ask:

> Is this part of the global design system or unique to this component?

If global, use a Global Spacing/Variable. If unique, use a component-specific value. Do not create
unnecessary one-off values.

### Container / layout system

Inspect the Figma frames and the layout tokens:

```text
Maximum width
Side padding
Columns
Grid
Gaps
Section widths
```

Configure Elementor global container/layout settings accordingly (from `--container` and `--gutter`).
Do not randomly choose a container width.

### Global container — set ONCE in Site Settings, never per container (MANDATORY)

Set the container system globally, in **Site Settings → Layout**, before building any page:

| Site Settings → Layout | Value comes from |
|---|---|
| **Content Width** — **every breakpoint** | `--container` (e.g. `1200px`), then each breakpoint's stepped-down value |
| **Container Padding** — desktop / tablet / mobile | `--gutter` — set for every breakpoint |
| **Gaps** (Widgets / Elements gap) | the global grid / column gap |
| **Breakpoints** | the project breakpoints (E5.6) |

**The container is responsive.** `--container` (and `--gutter`) change at the breakpoints, so the
global Content Width and Container Padding are set on **each device** in Site Settings → Layout
(switch the responsive icon: Widescreen, Desktop, Laptop, Tablet Extra, Tablet, Mobile Extra,
Mobile) with the values from the `--container` / `--gutter` step-down table in `TOKENS.md` (A5.8). Example: 1200 → 1150 → 1105 → 1010 → 910 → 960 fixed → 100%.
Never set only the desktop value and leave the smaller screens on Elementor's default.

Then on every page and template:

- **Outer section container = Boxed** with the width **left empty / Default**, so it inherits the
  global Content Width. Never type a pixel width (`1180px`, `1240px`) or a side padding on each
  container.
- **Full-bleed sections** (background to the screen edge) = outer container **Full Width**, with
  one **inner Boxed container on Default** that holds the content — so the content still lines up
  with every other section.
- **Inner / nested containers = Full Width (100%)** of their parent. Their gap and padding use
  Global Spacing values (above), not one-off numbers.
- Section top / bottom padding = the global section-spacing value; only the left / right padding
  comes from the global Container Padding.
- Need a narrower block (text column, form)? Give that **inner** element a max-width from the design,
  keep the outer container on the global width.
- Changing the site width later = change **one** value in Site Settings → Layout, and every page
  follows. If a page still looks different, a container has a hard-coded width — remove it.
- Never set the container width in `custom.css` or widget Custom CSS to fight the global setting.

QA:
- [ ] Site Settings → Layout has Content Width + Container Padding set on every breakpoint, and Gaps, set from
      `TOKENS.md`.
- [ ] No container has its own pixel width or side padding; content edges line up on every section
      at desktop, tablet and mobile.

---

## E5.8 Reusable Elementor components

Identify repeated Figma components (`Design/components/`) and make them reusable.

Examples:

```text
Section Heading
Primary Button
Secondary Button
Blog Card
Partner Card
CTA Banner
Testimonial
Feature Card
```

Use Elementor reusable templates, Global Widgets, Loop Items, or other appropriate Elementor Pro
mechanisms. Do not duplicate identical structures across many pages unnecessarily.

**Create reusable components and templates wherever possible.** Any section, block, or widget group
that appears more than once (or is likely to be reused later) must be saved as a reusable template /
Global Widget / Loop Item, so that a single edit updates every instance. This maintains consistency
and simplifies future updates. Any saved section template must work on any page, in any order (E1).

---

## E5.9 Header — Elementor Pro Theme Builder

The Header must use:

```text
Elementor Pro → Theme Builder → Header
```

**FIRST inspect the Figma header frame(s) at every breakpoint, its components, assets, menu
structure and prototype behaviour. THEN build it in Elementor.**

Inspect:

- Exact structure
- Logo
- Desktop menu
- Mobile menu
- Dropdowns / mega menu
- CTA
- Search
- Icons
- Sticky behavior
- Scroll states
- Animations
- Breakpoints

Do not create a generic Elementor header.

---

## E5.10 Header menu

Use the WordPress menu/navigation system with Elementor Pro (B7). Do not hard-code navigation links
into text widgets.

Preserve, as designed in Figma:

- Menu hierarchy
- Dropdowns
- Active state
- Hover state
- Mobile navigation
- Responsive behavior

If an existing menu requires custom PHP/Walker logic, inspect it before changing it.

---

## E5.11 Footer — Elementor Pro Theme Builder

The Footer must use:

```text
Elementor Pro → Theme Builder → Footer
```

**FIRST inspect the Figma footer frame(s) at every breakpoint, its components and assets. THEN build
it in Elementor.**

Preserve:

```text
Footer layout
Logo
Description
Navigation
Contact details
Social links
CTA
Legal links
Copyright
```

Do not create a generic footer.

---

## E5.12 Header / footer site-wide settings

Site-wide values must have one source of truth.

Examples:

```text
Logo
Header CTA
Footer Logo
Footer Contact
Social Links
Copyright
```

Do not duplicate these values on every page.

---

## E5.13 Existing WordPress theme + Elementor Pro

If an existing WordPress theme is present (see also Part C):

1. Inspect it first.
2. Check whether Elementor Pro is installed.
3. Inspect existing Elementor templates and Theme Builder templates.
4. Inspect Global Colors and Global Fonts.
5. Inspect Custom CSS/Code.
6. Inspect CPTs, ACF and plugins.
7. Reuse the existing architecture where practical.
8. Do not automatically replace the existing theme.

If a third-party parent theme is present, avoid modifying vendor files unnecessarily and prefer a
child-theme/customization approach where appropriate.

If a new theme (or child theme) has to be created, follow E5.19 — it must include the Part A assets
(`vendor.css`, `vendor.js`, `animations.js`) and `custom.css` — no `sliders.js` (E5.20).

---

## E5.14 Existing Elementor site

If Elementor is already used, inspect:

```text
Saved Templates
Theme Builder
Global Colors
Global Fonts
Site Settings
Global Widgets
Loop Templates
Custom CSS
Custom Code
```

Reuse working structures rather than rebuilding them unnecessarily.

---

## E5.15 Partners — Elementor Pro + CPT

If Partner pages exist, use a dedicated Partner Custom Post Type (B11–B13) and Elementor Pro Theme
Builder templates:

```text
Partner CPT
   ↓
Elementor Pro Single Template
+
Elementor Pro Archive Template
```

Use dynamic Partner data. Do not hard-code individual Partner information into Elementor templates.

Use ACF PRO fields (D110, D111) with Elementor Dynamic Tags where appropriate.

---

## E5.16 Blog — Elementor Pro

Use native WordPress Posts for Blog (B14). Do not create a duplicate Blog CPT unless explicitly
required.

Use Elementor Pro Theme Builder for:

```text
Blog Archive
Single Blog Post
Category Archive if required
```

Use dynamic Elementor widgets/Loop Grid where they match the Figma design.

---

## E5.17 Figma → Elementor mapping

Do not put an entire page or section into one Elementor HTML widget.

Map each Figma section/component to native Elementor structures: Figma auto-layout frames →
containers (flex/grid), text layers → Heading / Text Editor, buttons → Button (global button
style), repeated cards → Loop Item / Global Widget, the whole section → a saved section template.

Example:

```text
Figma section frame "Intro"
  auto-layout container
    heading
    text
    button

Elementor
Container
 ├── Heading      (Global Typography H2)
 ├── Text Editor  (Global Typography Body)
 └── Button       (Global button style)
```

Use the HTML widget only for small pieces that genuinely require raw HTML.

---

## E5.18 No custom CSS / no class-based CSS — MANDATORY

**Do not add Custom CSS in any widget.** The widget-level `Advanced → Custom CSS` field must stay
empty on every widget, container and template.

**Do not write class-based CSS.** Do not create custom CSS classes (via `Advanced → CSS Classes` or
otherwise) and then write CSS rules targeting them — not in widgets, page settings, Site Settings
Custom CSS, or theme stylesheets.

All styling must be done through native Elementor controls, in this order:

```text
Elementor Global Settings (Colors, Typography, Buttons, Forms, Layout) — values from TOKENS.md
    ↓
Global Variables (Elementor Variables, from the TOKENS.md tokens)
    ↓
Elementor Widget / Container Style Controls
```

If a design detail genuinely cannot be reproduced with native Elementor controls, do not silently
add CSS. Report it, explain why the controls are insufficient, and get approval before using any
CSS. Approved CSS goes **only** in the theme's `assets/css/custom.css` (E5.19), using the
`vendor.css` tokens — never in a widget.

**Exception for approved CSS:** when a rule in `custom.css` has been approved, a class on the
Elementor element may be added **only for that rule** (e.g. a curved-panel mask, a full-bleed
carousel). Keep a list of these classes; never add "section-name" classes by habit (E5.37.1
rule 5). Before writing CSS, check whether a native control or a native widget can do it — on Buell
the client rejected custom CSS for card hover, logo marquee and waves and asked for native hover,
the Elementor carousel and native shape dividers instead.

Avoid `!important`.

---

## E5.19 Theme assets — vendor.js, custom.css and the Part A files

Elementor builds use the same theme and front-end system as every other option (Part A, A7), plus
`custom.css`:

```text
theme-name/
└── assets/
    ├── js/
    │   ├── vendor.js      ← small guarded UI inits (A7.3)
    │   └── animations.js  ← GSAP + ScrollTrigger, data-anim (A7.4)
    └── css/
        ├── vendor.css     ← :root tokens ONLY (from TOKENS.md) — no components
        └── custom.css     ← the ONLY place for approved Elementor custom CSS
```

### Scripts (vendor.js, sliders.js, animations.js)

- Third-party libraries (GSAP, ScrollTrigger) are enqueued in the A7.6 order:
  GSAP → ScrollTrigger → `animations.js`. Never hard-coded `<script>` tags. No `sliders.js`, Slick or
  Swiper of our own — sliders are Elementor's own carousel widgets (E5.20).
- Do not load the same library twice (check whether Elementor or a plugin already loads it — e.g.
  Elementor ships its own jQuery and Swiper).
- Enqueue with `wp_enqueue_script()` in the footer, with correct dependencies and version numbers.
- Load only on the pages that need it where practical.
- `vendor.js` may also hold small, approved front-end fixes that Elementor cannot do (e.g. adding a
  missing `aria-label`, a menu-chevron toggle). Fix things in the template/PHP first; use JS only
  when the markup comes from Elementor/Pro and cannot be changed. Comment each fix with what it fixes.

### vendor.css (Elementor: tokens only)

- Holds **only the `:root` tokens** from `TOKENS.md` (colours, fonts, spacing, `--transition` …) so
  `custom.css` and `animations.js` can use `var(--…)`.
- **No global components** (no `.container`, buttons, headings, forms) — those are set in Elementor
  Site Settings and the Global Design System (E5.3–E5.7).

### custom.css

- The single home for any CSS that E5.18 approved. Never put CSS in widget
  `Advanced → Custom CSS`.
- Do not use it to replace things Elementor Global Settings or widget controls can handle.
- Comment each rule with the section/component it belongs to.
- Enqueue `vendor.css` and `custom.css` with `wp_enqueue_style()` after Elementor's styles, so their
  rules apply in the right order.
- Avoid `!important`.

Register everything through `inc/enqueue.php` (or `functions.php` for a minimal child theme) using
`get_stylesheet_directory_uri()` / `get_template_directory_uri()`. Never hardcode paths or domains.

---

## E5.20 Animations, GSAP and sliders with Elementor

Animations come from `Design/ANIMATIONS.md`, mapped to the `data-anim` presets (A9.8). Inspect the
motion spec first.

- Use Elementor Motion Effects only when they reproduce the specified behavior accurately. Otherwise
  use the theme's `animations.js` (A7.4).
- Put `data-anim`, `data-anim-delay`, `data-anim-stagger` … on the element through
  `Advanced → Attributes` (Elementor Pro). Never add GSAP code in an HTML widget or Custom Code.
- Never use Elementor Entrance Animations and `data-anim` on the same element (one animation per
  element, A9.8 rule 3).
- Sliders: use Elementor's own default carousel widgets (Image Carousel, Loop Carousel, Media
  Carousel …), styled to match the Figma design. Never add a new slider JS (no Slick, no Swiper of
  our own) (A1). Follow the slider + animation rules in A9.8 rule 10.

If GSAP or Three.js is present:

- Preserve required selectors and data attributes.
- Avoid duplicate library versions.
- Load scripts only where required.
- Avoid duplicate initialization.
- Ensure Elementor Editor mode does not initialize frontend animations twice.
- Test desktop and mobile.
- Test resizing.

---

## E5.21 Elementor editor compatibility

Custom JavaScript must not break Elementor Editor.

Check for:

```text
Duplicate event listeners
Duplicate animations
Duplicate sliders
Duplicate Three.js canvases
```

If required, prevent frontend-only initialization while editing in Elementor.

---

## E5.22 Elementor performance

Do not create excessive nested containers.

Avoid unnecessary:

```text
Container → Container → Container → Container
```

Use the simplest Elementor structure that reproduces the design.

Do not load every custom library on every page if it is only needed on specific pages.

---

## E5.23 Elementor template naming

Use clear names:

```text
[Global] Header
[Global] Footer
[Page] Home
[Page] About
[Page] Contact
[Partner] Archive
[Partner] Single
[Blog] Archive
[Blog] Single
[Component] Blog Card
[Component] Partner Card
[Component] Section Heading
```

Use one consistent naming convention. Reusable page sections use `[Section] <Role>` (e.g.
`[Section] CTA Banner`).

---

## E5.24 Elementor display conditions

Use Theme Builder conditions correctly.

Typical:

```text
Header → Entire Site
Footer → Entire Site
Partner Single → Partner
Partner Archive → Partner Archive
Blog Single → Posts
Blog Archive → Posts Archive
```

Avoid overlapping templates that produce duplicate headers, footers, or content.

---

## E5.25 No design drift

Compare the Elementor version against the Figma frames page-by-page (F1).

Check:

```text
Layout
Position
Width
Height
Typography
Color
Spacing
Images
Buttons
Borders
Radius
Shadows
Animations
Responsive behavior
```

Do not accept “close enough” when the Figma values can be reproduced accurately.

---

## E5.26 Images in Elementor — WebP, accessibility, Media Library once

The generic rules are in D104 (SVG & image accessibility + naming) and D105 (Media Library once).
Images are only the Figma exports from `Design/images/` + `Design/svg/` (2x export, resize down only,
WebP q85+, vectors stay SVG). Elementor-specific differences:

```text
Figma-exported images (Design/images, Design/svg)
    ↓
Convert raster images to WebP (q85+), resize down only to the size actually used
    ↓
Rename by what it shows (D104)
    ↓
Check the Media Library — already there? Reuse it (D105)
    ↓
Upload to the WordPress Media Library ONLY if it is not there yet
    ↓
Use in Elementor widgets from the Media Library
```

- Do not hardcode image URLs in widgets; always select images from the Media Library. Fixed UI icons
  that live in the theme (D105) are used through the theme templates/classes, not pasted URLs.
- Keep sensible quality — no visible degradation versus the Figma frame. Never blurry.
- Preserve transparency where the design uses it.
- Alt text and Title are filled **in the Media Library**, so every widget that uses the image
  inherits them. If a different context needs a different alt, override it in that widget only.
- SVG uploaded through the Media Library: fill in its **Alt Text** there. Icon widgets: meaningful
  icon-only links get an `aria-label` on the link (`Advanced → Attributes`); decorative icons are
  hidden from screen readers (D104).
- Also covered for Elementor (from the Elementor doc table): Blog / post image
  `<post-topic>.webp` (e.g. `seo-audit-checklist.webp`, alt describes the image, not just the post
  title); Team photo `<first>-<last>-<role>.webp` (e.g. `rahul-mehta-ceo.webp`, alt `Rahul Mehta,
  CEO`); Site logo `<site>-logo.svg` (alt `<Site name> home` when it links home); Social icon
  `icon-<network>.svg` (alt `LinkedIn`, on the link: `Follow us on LinkedIn`); Placeholder
  `placeholder-<ratio>.webp` (alt `<Intended content> placeholder`, E5.27).
- **If renaming is not possible** (the file is already uploaded and in use):
  - Do not upload a second copy just to change its name (D105).
  - Correct its **Title** and **Alt Text** in the Media Library.
  - List the file in the handover report as "file name not SEO-friendly".
- **Uploading by script / WP-CLI / REST API:** before the run, list existing attachments
  (`wp post list --post_type=attachment --fields=ID,post_name,guid`) and keep a **file name →
  attachment ID map**. Upload only the files missing from that map.
- When importing an Elementor template or kit that contains images, check whether those images are
  already in the library before importing them.
- Duplicate clean-up: find every **widget/page** that uses the copy, re-point it to the original
  item, only then delete the copy (D105). Check the Media Library once at the end of the build:
  every image and icon appears exactly once.

---

## E5.27 Dummy content & placeholder images

*Noted during real builds — use one consistent placeholder system across the whole site.*

This is the D98.1 placeholder system applied to Elementor — the rules below add the Elementor
class and widget details.

Real copy comes from `Design/CONTENT.md` and real images from `Design/images/`. Only where the Figma
design has no content for a slot (e.g. extra blog posts, CMS items beyond the designed ones), use
dummy content **with a proper placeholder image**. Never leave an empty space, a broken image or a
random stock photo.

### Image placeholders

- Match the **aspect ratio, dimensions and styling** of the design's real image slot (same radius,
  same object-fit, same size at every breakpoint).
- Create one placeholder per ratio the design uses (e.g. `placeholder-16x9.webp`,
  `placeholder-4x3.webp`, `placeholder-1x1.webp`). Upload them once to the Media Library and reuse
  them everywhere. Don't make a new one per section.
- Lock the ratio with native controls (Image widget height + Object Fit: Cover, set per
  breakpoint). Save the placeholder setup as one reusable component. CSS only after E5.18 approval.
- Give placeholders alt text that names the intended content (e.g. `Team photo placeholder`) so
  they are easy to find and replace.

### Logo placeholders

- Light gray / neutral background (a Global Color, e.g. `Placeholder BG`).
- Logo centered **both vertically and horizontally** inside a fixed-ratio box (flex,
  `align-items: center; justify-content: center`).
- Every logo box is the same size, and the logo keeps its proportions (`object-fit: contain`,
  max width/height).
- One saved component / Loop Item for every logo grid and logo slider on the site (no class-based CSS, E5.18).

### Responsive and consistent

- Check placeholders on **Desktop, Tablet and Mobile**. The ratio and centering must hold, nothing
  stretches, and nothing overflows.
- Use the **same placeholder treatment on every page**. One look for image placeholders and one
  for logo placeholders.

---

## E5.28 Internal links — Elementor Dynamic Tags + permalinks

All internal page links must be configured correctly and must not be hardcoded (B43, B44).

For every internal link in Elementor (buttons, headings, images, icons, icon lists, etc.) use:

```text
Link → Dynamic Tags → Internal URL → Type → Content
    ↓
Select the appropriate Page or Post
```

Rules:

- Do not type or paste internal URLs manually (no `https://domain.com/about/`).
- Every link in the Figma design / prototype maps to the matching WordPress page/post.
- This keeps links working when slugs or domains change.
- Navigation menus use WordPress Menus (E5.10, E5.11), not hardcoded links.
- Verify every internal link after the build — no 404s.
- Every clickable element must be linked (link map, no `#` / empty links) — see D115.

**How to set each link (Elementor):**

- Internal page / post → **Link → Dynamic Tags → Internal URL → Content** (above). Never a typed URL.
- Product / product category → the product's own link from the **Loop Item** / WooCommerce widgets
  (Product Title, Product Image, Add to Cart) or Internal URL → the product / category (E5.31).
- Archive / category / tag → Dynamic Tags → Archive URL / Internal URL → Taxonomy.
- Menus → WordPress Menus in the Nav Menu widget (E5.10) — every menu item points to a real page.
- Whole card clickable → set the link on the Loop Item / container (Elementor container link), not
  only on the small "Read more" text.
- Phone → `tel:`, email → `mailto:`, address → Google Maps link — taken from Site Settings / ACF
  options, not typed again per widget.
- Social icons → the real profile URLs from the design / client, opened in a new tab with
  `rel="noopener"`.

**Permalinks** are set to **Post name** by the importer (D100 step 4):

```text
Settings → Permalinks → Post name   (/%postname%/)
```

Set this before building internal links, then flush permalinks (save the Permalinks screen once)
after registering any Custom Post Types.

---

## E5.29 404, Search and Sitemap — built with Theme Builder

The generic rules (content, testing, XML sitemap) are in D106 and D107. In an Elementor build these
pages are Elementor templates/pages instead of PHP templates:

### 404 page

- Build it in **Theme Builder** as a 404 page template, matching the design: heading, message,
  button back to the homepage, and the search field if the design has one.
- Use the global header and footer, global typography, colours and buttons. Link the home button
  with Dynamic Tags (E5.28).
- Test it by opening a URL that does not exist.

### Search Results page

- Build the **Search Results** page in **Theme Builder** as an Archive template with the display
  condition **Search Results**, matching the design.
- Include the search term heading, the results loop (title, excerpt/meta, image if designed),
  pagination, and a clear **"no results"** state.
- Make sure the header/site search form submits to it.
- Test it with a term that returns results and with one that returns nothing.

If the design has no 404 or Search layout, build a simple one from the global design system, and
say so in the handover report.

### HTML Sitemap page

- A normal WordPress Page called **Sitemap** (slug `sitemap`), built with Elementor and the global
  header, footer, typography and colours. The importer creates it (D98).
- Use the Elementor Pro **Sitemap** widget so the list is **dynamic**: Pages, Posts, Partners (CPT),
  and Categories if the design uses them. Never type the links by hand.
- Leave out Thank You, 404, private/draft and utility pages. Link it from the footer through the
  WordPress footer menu.
- Match the design if it has a sitemap layout. If it doesn't, keep it simple, using a section title
  and grouped lists from the global design system.
- XML sitemap: as D107.

---

## E5.30 DeveloperAI meta tag — Elementor differences

Follow D112 (the `wp_head` hook in the theme's `inc/template-functions.php`, or the child theme's
`functions.php`). In an Elementor build also:

- Do not add the tag through a Custom HTML widget, Elementor Custom Code or a page setting.
- The hook covers every template, including **Elementor Canvas / Full Width** and every Theme
  Builder template (404, Search, archives, singles). Check the source of a Canvas page too.

---

## E5.31 WooCommerce with Elementor Pro

Follow D113 for everything (questions, inspect, theme support, hooks first, products and data,
responsive list, `woocommerce.js`, performance, QA). These are the Elementor-specific differences.

### Theme files (replaces the D113 file tree)

```text
theme-name/
├── woocommerce/               ← template overrides, ONLY when hooks + Elementor can't do it
├── inc/woocommerce.php        ← theme support, hooks, image sizes, cart fragments
└── assets/
    ├── css/custom.css         ← WooCommerce block inside it (the only custom CSS file, E5.19)
    └── js/woocommerce.js      ← shop interactions (D113 step 8)
```

Never edit WooCommerce plugin files.

### Build with Elementor Pro (native first, E5.1) — replaces D113 step 5

- **Theme Builder:** a **Single Product** template and a **Product Archive** template, with display
  conditions (E5.24). Product cards are one **Loop Item** template, reused on the shop, category,
  related and home sections (E5.8).
- **Native WooCommerce widgets first:** Products, Product Title, Product Price, Product Images,
  Add to Cart, Product Meta, Product Data Tabs, Upsells / Related, Breadcrumb, Menu Cart, Cart,
  Checkout, My Account, Purchase Summary.
- **Site Settings → WooCommerce:** set the Cart, Checkout, My Account, Terms and Purchase Summary
  pages there.
- **Styling order:** Elementor Global Colors / Typography / Buttons / Forms → the widgets' own style
  controls → only where those can't match the Figma design, report it and get approval, then CSS in
  `custom.css` (E5.18). Never widget Custom CSS.
- Header mini-cart = the **Menu Cart** widget, inside the Theme Builder header (E5.9).
  Style it per the D113 step 7 "Mini-cart dropdown" rules with the widget's own controls first
  (container width, title / price typography = Global body text, subtotal, View cart / Checkout
  buttons); only what they can't match goes in `custom.css` (E5.18).

### Products always dynamic in Elementor (replaces the "how to pull them" line of D113 step 6a)

- Every product on the site is pulled from WooCommerce with the **Products** widget, or **Loop Grid /
  Loop Carousel** with source = **Products** and the product **Loop Item** — never a card built from
  Heading + Image + Text + Button widgets, never a price typed into a Text Editor. Sliders = Loop
  Carousel, not a static Image Carousel / Slides widget.
- The widget's Query picks the products (category, tag, featured, on sale, best sellers, newest,
  manual selection, count, order). Inside the Loop Item use dynamic tags (Product Title, Product
  Price, Featured Image, Product Rating, Add to Cart, Sale / Stock badge).
- Empty state: set the widget's "Nothing found" message or hide the section — never fall back to
  static placeholder products.

### Theme-level CSS (replaces where D113 step 7 puts the CSS)

Write all WooCommerce styling in `assets/css/custom.css`, under one
`/* ===== WooCommerce ===== */` banner, after the native Elementor styling has been used, and only
for CSS that was approved first (E5.18).
All D113 step 7 rules and the "style every one of these" + responsive lists still apply.

### Extra QA

- [ ] Single Product and Product Archive are Theme Builder templates with correct conditions; the
      product card is one Loop Item.
- [ ] Every product section (home, sliders, related, landing pages) is a Products / Loop Grid /
      Loop Carousel widget with a Products query — no static product cards.
- [ ] Native WooCommerce widgets used; no widget Custom CSS; WooCommerce CSS is in the `custom.css`
      WooCommerce block.

---

## E5.32 Importer seeds Elementor

The one-click importer (D96–D102) also seeds an Elementor build. Only the page-content step differs:

- Build the Theme Builder templates (header, footer, singles, archives, 404, search), the section
  templates and the Loop Items first, then export them as Elementor JSON
  (Templates → Saved Templates → Export) into the importer's data folder.
- The importer imports those templates once (skip any that already exist, matched by title), and
  creates each page in the Figma page order with its sections in the Figma order.
- Page text and images come from `Design/CONTENT.md` and the Media Library attachment IDs (D98,
  D101, D105) — never retyped, never the export's own image URLs.
- Set the display conditions, Site Settings (globals from `TOKENS.md`, breakpoints, WooCommerce pages)
  and permalinks as part of the import, then regenerate Elementor's CSS
  (Elementor → Tools → Regenerate CSS & Data).
- Use only documented Elementor import methods for the installed version (its template import, Kit
  import or WP-CLI commands). If unsure how a version stores or imports data, check Elementor's
  official docs or ask — never guess meta keys or formats.
- Re-running the importer must not create duplicate pages, templates or media (D101).

---

## E5.33 Elementor final QA

The shared QA in E1 and the final QA in F2 also apply.

### ⭐ Main Requirement (E5.1) — check this first

- [ ] Every page compared side by side with its Figma frame at 1600 / 900 / 375 (and the A5.8
      breakpoints in between).
- [ ] All sections and layouts present, in the same order. Header and footer match.
- [ ] Design, spacing, typography and colours match.
- [ ] Hover effects and animations match `Design/ANIMATIONS.md`.
- [ ] Buttons, links and every interactive element work as designed.
- [ ] Any other custom design or functionality from the Figma design is reproduced.
- [ ] Native Elementor used everywhere. No class-based CSS; any CSS was approved first (E5.18).
- [ ] Approved CSS is only in the theme's `assets/css/custom.css`. None is in widget, page or Site
      Settings Custom CSS.
- [ ] Repeated blocks are components (template / Global Widget / Loop Item), not page-by-page copies.
- [ ] Every page has `<meta name="DeveloperAI" content="Developer Skill">` in `<head>` (D112, E5.30).

### Global design

- [ ] Global colors created from `TOKENS.md` and actually used.
- [ ] Global typography created from `TOKENS.md` and actually used.
- [ ] Global spacing/variables established.
- [ ] Container width matches the `--container` token.
- [ ] Container width + padding set once in Site Settings → Layout; no per-container widths (E5.7).
- [ ] Elementor breakpoints set to the A5.8 values; responsive typography matches.
- [ ] Repeated components are reusable.
- [ ] Favicon made from the logo icon is set as the Site Icon and shows on every page (D114).

### Header

- [ ] Figma header frames inspected at every breakpoint.
- [ ] Elementor Pro Header created.
- [ ] Logo is dynamic.
- [ ] Menu is dynamic.
- [ ] Desktop menu matches.
- [ ] Mobile menu matches.
- [ ] Dropdowns work.
- [ ] CTA works.
- [ ] Sticky behavior works.

### Footer

- [ ] Figma footer frames inspected at every breakpoint.
- [ ] Elementor Pro Footer created.
- [ ] Footer menus are dynamic.
- [ ] Social links work.
- [ ] Contact information works.
- [ ] Responsive Footer matches.

### Pages

- [ ] All Figma pages built.
- [ ] No unnecessary giant HTML widgets.
- [ ] Figma design preserved.
- [ ] Dynamic content connected.

### Partners

- [ ] Partner CPT works.
- [ ] Partner fields work.
- [ ] Single Partner Elementor template works.
- [ ] Partner Archive works.
- [ ] Dynamic data displays correctly.

### Blog

- [ ] Native WordPress Posts used.
- [ ] Blog Archive works.
- [ ] Single Post works.
- [ ] Categories work where required.
- [ ] Dynamic post data works.

### Elementor

- [ ] No conflicting Theme Builder templates.
- [ ] No duplicate Header/Footer.
- [ ] No unnecessary nested containers.
- [ ] Global settings are actually used.
- [ ] Elementor Editor works.
- [ ] Frontend works.
- [ ] No widget contains Custom CSS (`Advanced → Custom CSS` empty everywhere).
- [ ] No class-based CSS was written; classes exist only for approved `custom.css` rules (E5.18).
- [ ] Font sizes, colours and spacing use Elementor globals / Variables (from the tokens), not repeated raw
      values.
- [ ] Section spacing is consistent; container left/right spacing is set once globally.
- [ ] Repeated sections/blocks saved as reusable templates / Global Widgets.
- [ ] Theme assets exist and are enqueued in the A7.6 order: `vendor.css`, `custom.css`,
      `vendor.js`, `animations.js` (no `sliders.js` — sliders are Elementor carousel widgets).

### Images, links & permalinks

- [ ] Meaningful SVG icons have alternative text; decorative ones are hidden from screen readers
      (D104, E5.26).
- [ ] Every image has an SEO-friendly file name and proper alt text — none left blank or default
      (D104).
- [ ] Images, avatars and SVG icons renamed from what they actually show (`icon-phone.svg`,
      `priya-shah-avatar.webp`) — no Figma layer names left (D104).
- [ ] Only Figma-exported images used; all sharp, none blurry.
- [ ] Dummy content (only where Figma has none) uses the standard placeholders with correct ratio,
      size and styling at every breakpoint (E5.27).
- [ ] Logo placeholders: light gray background, logo centered both ways (E5.27).
- [ ] 404 page and Search Results page built and checked against the design (E5.29, D106).
- [ ] HTML Sitemap page exists, lists everything automatically and is linked in the footer; XML
      sitemap works (E5.29, D107).
- [ ] Every image/icon is in the Media Library **once** — no `-1` / `-2` copies, no duplicate
      uploads (D105).
- [ ] WooCommerce sites: shop design, CSS, JS and responsive pass the D113 QA list and E5.31 extra QA.
- [ ] All website raster images converted to and served as WebP.
- [ ] Images optimized (compressed, correctly sized).
- [ ] All internal links use Dynamic Tags → Internal URL → Type: Content.
- [ ] No hardcoded internal URLs; no broken internal links.
- [ ] Permalink structure set to **Post name**.

### JavaScript

- [ ] No console errors.
- [ ] GSAP works if present (`data-anim` presets as in `Design/ANIMATIONS.md`).
- [ ] Three.js works if present.
- [ ] Sliders work.
- [ ] Mobile interactions work.
- [ ] No duplicate initialization (frontend or editor).

### Responsive

- [ ] Desktop matches.
- [ ] Tablet matches.
- [ ] Mobile matches.


### Pre-delivery sign-off — tick every box TWICE (summary of E5.36 + E5.37)

Do not tell the user "done" until all are ticked after the **second** pass.

#### Setup & rules
- [ ] Worked on staging only; no credentials in any file/reply; unique theme name.
- [ ] CSS per E5.18 (no HTML widget / inline style for layout; classes only for approved rules).
- [ ] Zero static px for spacing (Elementor controls AND custom.css); section spacing editable via Elementor Variables.
- [ ] Global colours/fonts used; real client fonts uploaded; ™ ® 4 & % glyphs compared with design.
- [ ] Repeating content = CPT + Loop Grid/Carousel; every loop image has a placeholder fallback (grey + site logo).

#### Design match (PDF zoomed, per section)
- [ ] Border-radius, shapes, curves, icon sizes, alignment, left/centre text as in PDF.
- [ ] Card/section padding equal on 4 sides (phone 20px); top and bottom section space equal.
- [ ] Gaps 40/30/20 everywhere; icons/logos smaller on tablet/mobile; no image crop on mobile.
- [ ] Buttons: text vertically centred, not full width on phones; optical centring in menu/tabs/inputs/page numbers.

#### Header / menu / footer
- [ ] Top bar centred; device rules for location/phone/tagline; logo links home.
- [ ] Mega menu: starts at header bottom, not cut, hover image works, no logo placeholder; active states right.
- [ ] Mobile menu: custom burger, scroll lock, dividers, chevron toggle; 700–800 px OK.
- [ ] Footer: hovers, email not cut, ® centred, copyright centred + last on mobile, social icons smaller.

#### Interactions
- [ ] Sliders: arrows per design, disabled = not-allowed, gaps right, hover not cut, zoom on product images.
- [ ] Logo tiles fully clickable → detail page; marquee speed; entrance animations play.
- [ ] Forms: validation, checkbox above button; accordions/tabs/pagination work.

#### Content & pages
- [ ] Every post type tested with temporary data in every view; empty fields hide cleanly; few/many entries OK.
- [ ] All designed pages built (incl. detail templates and archives); slugs semantic (/faq/).
- [ ] Breadcrumbs dynamic and correct (no "About / About").
- [ ] Dummy content and links waiting on the client listed (links: D115).

#### Technical QA
- [ ] 1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320 crawled (F2): no console errors, failed requests, broken images, missing alt, overflow, clipping.
- [ ] axe: one `<main>`, no duplicate landmarks, unique `<nav>` labels, icon-only names, contrast measured and reported.
- [ ] Unique title + description ≤160 on every page incl. archives/search/404; noindex/canonical noted for launch.
- [ ] Elementor cache cleared; theme version bumped; site returns 200 after upload.
- [ ] `PROJECT-SUMMARY.md` and `DAILY-LOG.md` updated; final report by QA bug type (fixed / clean / needs decision).

---

## E5.34 Definition of done and final goal (Elementor)

The Elementor build is complete only when:

```text
Figma Design (Design/pages)
        ≈
Elementor Pro Frontend
```

while:

```text
Figma Content (Design/CONTENT.md)
    ↓
WordPress / Elementor Dynamic Content
```

and:

```text
Repeated Design Values (TOKENS.md)
    ↓
Elementor Global Settings / Variables
```

and:

```text
Repeated Components
    ↓
Reusable Elementor Templates / Widgets
```

and:

```text
Internal Links       → Elementor Dynamic Tags (Internal URL → Content)
Images               → Optimized WebP (Figma exports, sharp)
Permalink Structure  → Post name
Styling              → Native Elementor controls only (no Custom CSS, no class-based CSS; approved CSS only in custom.css)
<head> meta          → <meta name="DeveloperAI" content="Developer Skill"> on every page (D112)
Favicon              → WordPress Site Icon made from the logo icon (D114)
WooCommerce (if any) → Theme Builder + native Woo widgets, CSS in custom.css, woocommerce.js (E5.31)
Images & SVGs        → accessible, SEO-friendly names and alt text (D104), Media Library once (D105)
Placeholders         → consistent placeholders (D98.1); 404 + Search pages built (D106, E5.29)
Links                → every clickable element linked, no `#` (D115, E5.28)
Content              → manageable by the client in WordPress / Elementor
```

Normal content changes must be possible through WordPress/Elementor without touching code.

**Final goal:** deliver a complete, responsive, fully functional WordPress website built with
**Elementor Pro** that faithfully reproduces the Figma design, while following WordPress best
practices and ensuring:

```text
Proper Global Styling
Reusable Components / Templates
Correct Internal Linking (Dynamic Tags)
Optimized WebP Images
Post name Permalinks
Manageable Content
No Custom CSS in widgets / No class-based CSS
Accessible SVGs and images, SEO-friendly image names
Consistent placeholders, working 404 and Search pages
```

---

## E5.35 Elementor development order

Always follow:

```text
INSPECT THE FIGMA DESIGN
   ↓
UNDERSTAND
   ↓
LOAD THE DESIGN SYSTEM FROM TOKENS.md
   ↓
CREATE GLOBAL COLORS
   ↓
CREATE GLOBAL TYPOGRAPHY
   ↓
CREATE GLOBAL SPACING / VARIABLES / BREAKPOINTS
   ↓
BUILD REUSABLE COMPONENTS
   ↓
BUILD HEADER / FOOTER
   ↓
BUILD PAGES
   ↓
BUILD PARTNER CPT TEMPLATES
   ↓
BUILD BLOG TEMPLATES
   ↓
CONNECT DYNAMIC DATA
   ↓
CONVERT IMAGES TO WEBP / SET INTERNAL LINKS / SET PERMALINKS
   ↓
MATCH THE FIGMA DESIGN
   ↓
TEST
   ↓
FINAL QA
```

**Do not build the entire Elementor website first and try to standardize the design later. The
global design system must be in Elementor before large-scale page construction.**

---

## E5.36 LESSONS LEARNED — ISSUES QA ALREADY FOUND (DO NOT REPEAT)

These came from the Buell Recreation build. On any new Elementor project, **check each one yourself and fix it before anyone else sees the site.** If you hit a new kind of issue, fix it and add it here.

### E5.36.1 Landmarks & structure
- Every page needs exactly **one `<main>`** (role="main" on the page content container). Check pages, single, archive, 404 and search templates.
- The Theme Builder Header/Footer *location* already renders `<header>` / `<footer>`. The top container inside the template must be `html_tag = div`, never `header` / `footer`, otherwise the page gets duplicate `banner` / `contentinfo` landmarks.
- Loop Grid / Carousel containers must not carry `role="list"` unless every child is `role="listitem"`. Remove the stray role. Same for `.elementor-posts-container` on search/archive.
- One `<h1>` per page, no skipped heading levels (a card `<h3>` after the `<h1>` with no `<h2>` is a bug — fix the tag, keep the visual size through the style controls).

### E5.36.2 Accessibility names
- Every icon-only link/button gets an `aria-label`: social icons (name from the network), slider arrows, off-canvas open/close, card arrows (use the card heading).
- When a page has more than one `<nav>`, each needs a **unique** `aria-label` (Main menu, Footer – Markets, Footer – Company…).
- **Colour contrast is a QA bug.** Test every text/background pair with axe: white on a light brand blue/green usually fails (below 4.5:1, large text 3:1). Do NOT silently change the brand colour — list the failing pairs (ratio + where) in the handover so the client picks a darker token. Never hide the fix in custom CSS.

### E5.36.3 Text & symbols
- Superscripts (®, ™) inside headings, banners and breadcrumbs must render as a real `<sup>`. Breadcrumb/label code that uses `esc_html()` prints `<sup>` as raw text — use `wp_kses( $label, [ 'sup' => [] ] )`.
- A ® styled as a knocked-out disc can show a blank circle if the glyph colour equals the disc colour. Set the glyph ink from the surrounding text luminance and look at it on both dark and light backgrounds.

### E5.36.4 Breadcrumbs & menus
- Breadcrumb must not repeat the current page ("About / About"): skip a menu ancestor whose object id equals the current page.
- An item in several categories shows only the first in the breadcrumb; decide on purpose (primary category) and note it.
- **Mega-menu hover image:** never fall back to a logo/placeholder. Fallback chain = featured image → gallery → product image → category banner → nothing. When there is no image, restore the default panel image.
- New CPT entries do **not** appear in a static WP menu. Say so, or build the menu column from a query.

### E5.36.5 Page hierarchy
- A sub-page of a section (e.g. Designers under Markets) must be a real child page (parent id + menu_order), use the same template/builder script as its siblings, and appear in the section's card list.
- The parent landing page's card list shows **children only** — never the parent itself.
- Give the parent its own landing content (banner + cards + contact panel) rather than leaving it empty.

### E5.36.6 Dynamic content (CPT / ACF / Loop)
- **Verify data flow for every post type:** create a temporary entry, fill every field, open every view where it should appear (single, archive, home carousel, related carousel, category page), then delete the temporary entry. Report what is manual (menus, relations) vs automatic.
- Check empty states: an empty ACF field must hide its block, not show an empty box or "Lorem".
- A "related / explore other…" carousel must query the **right source**. Example: brand page "Explore Other Product Categories" must loop **Product Categories** (taxonomy terms), not the brand gallery.
- Never run `WP_Query` / `get_children` / `get_pages` inside an `elementor/query/*` hook — infinite recursion → HTTP 500. Alter the passed query object (`$query->set(...)`), or use `$wpdb` / post meta.
- Don't post `acf` props to core REST endpoints that don't register them; post core fields only.

### E5.36.7 SEO
- Every page: unique `<title>` and meta description (≤160 characters, count them). Include the archive, search and 404 templates.
- Never copy one page's SEO text to another (Markets once carried Designers' copy). When a page is split or renamed, rewrite its entry.
- Staging is `noindex` → no canonical tag. Add "turn on search-engine visibility + check canonicals" to the launch checklist.

### E5.36.8 Links & content
- Links: D115. **List every link still waiting on the client and every placeholder/lorem block in the handover** so QA does not log them as bugs.
- Read the copy: wrong excerpts pasted from another brand/page and test data (e.g. a "Visit Website" pointing to a search engine) are content bugs. Fix or list.

### E5.36.9 Cache & tooling
- Hosts like WP Engine cache query-string URLs. After a rebuild clear the Elementor cache (`DELETE /elementor/v1/cache`) and use a harmless cache-buster (not `?s=` — that triggers WordPress search).
- `curl` may get 403 from host bot protection; verify links in a real browser run.

### E5.36.10 SELF-QA — MANDATORY, TWO PASSES
Before telling the user "done", run QA yourself **twice** (pass 2 after fixing pass 1):
1. Crawl every page, CPT single/archive, taxonomy page, search and 404 at **1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320** (F2).
2. Per page record: console errors, failed requests, broken images, missing alt, horizontal overflow, clipped text, heading order, landmarks, axe violations (Playwright + axe-core), title/description, `#` links (D115) / placeholder content, form validation.
3. Take screenshots and **look** at the header, mega menu, banner, cards and footer on desktop and mobile; measure spacing instead of guessing.
4. Fix everything that is code. Anything that needs the client (colour, copy, URLs) goes in a short "needs decision" list.
5. Final report grouped by QA bug type (Accessibility, SEO, Content, Links, Responsive, Functional) — what was fixed, what is clean, what is open.

### E5.36.11 DOCUMENTATION HABIT
- Keep `PROJECT-SUMMARY.md` (what exists, how to rebuild, content model, open items) and `DAILY-LOG.md` (newest first, what changed today) updated every working day so teammates can pick the project up cold.


---

## E5.37 BUG LIBRARY — EVERYTHING QA / THE CLIENT FOUND ON THE BUELL PROJECT (50+ ITEMS)

**How to use this section:** it is a pre-flight checklist. Before you show ANY page, go through every line below and test it yourself at **1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320** (F2). If an item is not applicable, skip it. If you meet a new kind of bug, fix it and add one line here. Sections E5.36 and E5.37 together are the "do not repeat" list.

Legend: **[R]** = responsive-specific.

**Numbers below are Buell values (examples).** On a new project keep the rule, take the number from the new design. Example: "gaps 40/30/20" means *decide one gap scale from the design and use it on every slider and grid at every breakpoint*.

### E5.37.1 Working rules the client set (apply from day one)
1. Work **directly on the staging site** through the REST API / Application Password. Never build locally. Never put credentials, tokens or private URLs in any file, reply, commit or chat.
2. Create a **new, uniquely named theme/child theme** for the project (never reuse another project's theme name).
3. Match the design **pixel-for-pixel from the PDF/Figma** — zoom in per section and measure (border-radius, shapes, spacing, icon size, text alignment). "Close enough" is a bug. Photos and hero text can be client-changed later, so build them as editable.
4. CSS and styling: E5.18 — no HTML widget / raw markup / inline styles for layout.
5. **No classes or IDs in Elementor unless truly required** (e.g. a class that custom.css needs). Do not add "section-name" classes by habit.
6. **No static px for spacing** — margins, paddings, gaps use CSS variables (custom unit `var(--…)`), also inside `custom.css`. Section spacing must be editable in the admin (Elementor Variables, e.g. `section-space`, `section-top-space`, `section-bottom-space`, each with laptop/tablet/mobile values).
7. Links: D115. **Never link to the live/old site.** Dummy content is OK only where real data is missing; list it in the handover.
8. Build repeating content (projects, testimonials, team, brands, markets, categories, promotions, FAQ, areas) as **CPT + Loop Grid / Loop Carousel**, not copied static cards, so new entries show up automatically.
9. Give the client one final **list of: pages built, checks done, what is missing, what needs their input**.

### E5.37.2 Global header / top bar
- Top bar items (phone, location, tagline) must be **vertically centred** — measure it; demo/custom fonts often sit low in the line box (see E5.37.6).
- Top-bar content per device: keep the tagline on all devices; on small screens collapse phone/location to **icon-only** (text hidden) or move them into the menu. Never leave a half-shown mix. [R]
- Right-side top-bar icons, search icon and logo shape must match the design; the whole logo click area works (links: D115).
- Shape under the logo / curved header edge must match the PDF exactly at every width. [R]
- Header sticky behaviour and z-index must not hide content or cut dropdowns.
- Header "Products" mega panel starts **at the bottom edge of the header** (not overlapping it), is a floating card with the design's radius, and is **never cut off** (check desktop 1366–1920 with the panel open).

### E5.37.3 Menus (desktop)
- Submenu items carry the icon from the design; submenu panel layout (columns, photos) matches the design.
- Hover on a top item = the design's hover style (pill/underline). **Active state:** when a sub-page is current, its top-level item is also active. Home must not look active because of an `#anchor` or query-string menu link — test on the bare URL, not just with `?shot=`.
- Mega-menu image changes on hover of each product/brand link (fallback chain: E5.36.4).

### E5.37.4 Mobile / off-canvas menu [R]
- Hamburger uses a **custom uploaded SVG icon** (not the default Elementor icon) in the design's shape; check its size at 375/576/768.
- When the menu opens: **page scroll is locked**, only the menu scrolls.
- Menu rows: proper `li` borders/dividers and padding, clear tap targets, design look. Sub-menu chevron is a pure open/close toggle (does not navigate).
- Everything keeps working at 700–800 px where tablets/landscape phones show the menu: banner arrows must not be touched by the menu or overlap content.
- Social icons in the menu/footer are smaller on mobile.

### E5.37.5 Footer
- Copyright + privacy links: on small screens **centred and ordered last**; on a full-width bar the legal menu and copyright are centred.
- Phone and email links have **hover states**; the email never gets cut off (test around 1208 px — use `minmax(max-content, …)` column).
- ® / ™ symbols sit centred in their box; compare with the PDF zoomed in.
- Social icons smaller on mobile; footer wave/curve matches the design.
- Last section before the footer tucks under the wave by exactly the design's wave height (35–41 px); bottom padding matches the PDF. A section whose bottom space looks different from its top is a bug — view it, don't trust the numbers.

### E5.37.6 Typography & fonts
- Use the **client's real fonts** uploaded as Elementor Custom Fonts / local WOFF2 (remove placeholder fonts such as Sour Gummy when told). Keep exact weights; declare `@font-face` per real weight (a 100–900 range face can make 700 fall back to Arial bold).
- Demo/trial font files have watermarked glyphs (e.g. digit 4, `& - ' % ! / ™ ®`). Use `unicode-range` so only real glyphs come from the font and symbols come from a fallback or a small subset font. Compare ™ and ® with the design — client reported them looking different.
- Optical centring: trial fonts ride low in the line box → text looks low in buttons, header menu, tabs, page numbers, inputs. Fix with em-based offsets, then **measure** cap-height vs the box (not ink rows near rounded corners). Re-measure after the fix.
- Form fields need a font with correct ascent/descent metrics or Chrome clips placeholder descenders.
- Line-height: check multi-line card titles and big headings (a line-height bug was reported on link-card titles); h2 demo-font lift only when no intro follows.
- Text that is left-aligned in the PDF must stay **left** at all widths (do not centre it on mobile unless the PDF does).
- Title → content gap is **30 px (token)** on all pages and `section-title-wrap` bottom margin 30; use the token.
- Text/logos in the hero or a brand wordmark that gets cut by the wave must scale and keep the cut as designed; supply it as SVG and make it scale down on mobile. [R]

### E5.37.7 Spacing, sizing and responsive rules [R]
- Container side padding is global (gutter variable) — never added per section. Same padding on **all four sides** of cards and sections on phones (**20 px, never 15**), as a variable.
- Section top and bottom space must look equal when viewed; check every band, panel and CTA.
- Card padding variable (`--card-space`) = equal on 4 sides: 40 → 30 (≤1365) → 25 (≤991) → 20 (≤767).
- **Slider/loop gaps:** 40 desktop → 30 → 20, same on every page. Set the carousel gap (`image_spacing_custom`) at **every breakpoint explicitly** — an inherited value showed 16 px in the editor and a `var()` gap became 0, so carousel gaps stay in px.
- Icons and logos must be **smaller on tablet/mobile** (client repeatedly found them too big). Badge/icon glyph is centred with flex, not padding; check every icon badge.
- Brand logos in loop cards: width 100% + a max-width that steps down per breakpoint.
- Responsive typography steps per the design; at phone width a big number/heading must not be tiny (404 text) or huge.
- Buttons never stretch to 100 % width on phones; button text is vertically centred.
- Photos do **not crop** on mobile unless the PDF crops (About "Our Story" photo bug): use auto height ≤991. Inclusive/background photos keep behaving as background at all widths.
- A section with icon + title + text centred on mobile (e.g. "Why Communities") centres **all three**; its icons may be larger only for that section.
- Fixed breakpoints: do not leave a value for 991 and 575 inconsistent (client set 767 = 36 px, 991 and below = 40 px). Ask which value applies at each breakpoint or copy the design.
- Forms: checkbox row is placed **above** the Send button on all widths; phone field with country prefix looks like the design.
- Border radius at tablet/mobile must not look broken (curved panels use a 9-slice mask from the PDF path, with the same corner/bulge size); check every rounded card at 375.
- Marquee/logo band speed: ~4 s per logo (a slow marquee was a bug); paused on hover, reduced for `prefers-reduced-motion`.

### E5.37.8 Sliders, carousels & loops
- Arrows are the design's arrow (circle/square, chevron, hover and pressed colour). **Disabled arrow = `cursor: not-allowed`** + disabled colour.
- Carousel maths: slide width = (widget width − offset − gaps)/n; peeking cards use fractional `slides_to_show` at every breakpoint. Always set `slides_to_show` for desktop, laptop, tablet, mobile explicitly (Elementor's default tablet 2 showed two hero slides).
- Home hero = exactly the number of slides the client approved (2), banner slider on other pages uses dummy data if there is none.
- Do not let a slider "break" by trying to fix one slide: a fix for one slide must not rearrange the rest — test the whole row.
- Cards in the slider must not get cut on hover (hover transform/scale needs overflow space). Add zoom-on-hover for **product/gallery images** inside an overflow-hidden wrapper, using the global `--transition` variable.
- Loop Grid pagination (page numbers + prev/next) is styled like the slider arrows and is centred; search results pagination too.
- Every loop image (featured or ACF) has an Elementor dynamic-tag **fallback** = the E5.27 placeholder (Buell: neutral grey, correct ratio, **site logo centred**), same on every page and breakpoint.
- Brand/partner logo tiles are **clickable on the whole bordered box**, and go to the detail page. Logos in marquees link to the brand detail page too.
- Nested Carousel: the `offset` (peeking next card) only works with **infinite loop on**; the NEXT arrow's horizontal offset sign is the opposite of the PREV arrow's — check both arrows' positions at every breakpoint.
- A carousel that must be wider than its container (bleeding to the screen edge) needs `max-width: none` on the widget, otherwise a width over 100 % is ignored.
- Side arrows on wide screens (1366–1920) are placed with `calc()`/`clamp()` on `100vw` so they never sit on top of the cards or off-screen. Check at 1366, 1440, 1600 and 1920.
- After changing a slider, test the **whole row on every breakpoint** (slides shown, gap, peek, arrows, dots, swipe on mobile, autoplay pause) — not only the slide you changed.

### E5.37.9 Content, data & dynamic pages
- Every post type gets dummy data when empty so no section looks empty (area tiles, slider 2, team, projects). Mark dummy data clearly (e.g. "Sample") and list it.
- Every detail template must be built when the design exists (e.g. Areas We Serve detail page from the PDF). A button that leads nowhere is a bug.
- Dynamic pages must not break the design when data is **more or less** than the design shows (long titles, 1 item, 12 items). Test with few and with many entries.
- If the admin adds content (e.g. brand detail fields) and it does not show in the view, the template is missing a dynamic field — check each ACF field against the template.
- Slugs must be semantic (the FAQ page must be `/faq/`).
- Breadcrumbs are **dynamic** (page parent chain / menu / post type), not static text; correct on every page type. Static crumb code left in builders will crash rebuilds — remove it.
- Search and 404 pages: E5.29 — plus a search empty state + pagination, and the large "404" text scales on mobile. [R]
- Animations: `fade-up` and similar entrance animations must actually play on the sections the design has them (client reported a section not animating). Test scroll animations in a real browser.

### E5.37.10 Elementor technical gotchas (save hours)
- Container desktop width is printed inside `@media(min-width:576px)` at the end of the post CSS, so tablet-extra widths never apply → restate widths ≤1199 explicitly for every container (inheritance) via the generator/`custom_css`.
- Grid/flex containers: set columns at **every** breakpoint (Elementor defaults tablet 2 / mobile 1); containers wrap by default — set `nowrap` for rows.
- Elementor per-element CSS beats `body .elementor .x` rules; use `.elementor-element.x` or set the control in the generator.
- An undefined CSS var inside `calc()` silently breaks a whole width/card row — verify every var exists on `:root`.
- Zero spacing must be `0px` when a var-converted control is involved (a unitless 0 invalidates the whole margin). Form-widget column/row gap must stay px.
- Overlay default opacity 0.5 halves the scrim — set the overlay opacity explicitly.
- Linked image `<a>` is inline-block → fixed-height cover photos shrink; make it `display:block` for image-link wrappers.
- Form field custom classes go on the input, not the group; select has no `.elementor-field` class.
- Icon widget size key is `size`; get real control names from the widget config (`get_widgets_config`) and validate generated JSON before saving.
- Mega-menu / off-canvas: Pro off-canvas widget for mobile, Pro mega-menu widget for desktop panels; mobile nav submenus forced inline in custom CSS (approved CSS, E5.18).
- Custom `?area=`-style query vars can collide with a CPT query var; strip/handle them in a `request` filter on the archive.
- Theme upload: after every upload check the site returns 200; bump the theme version to flush rewrites/caches. No PHP linter may exist — review PHP by eye.
- Run a **pixel-diff A/B** when you bulk-convert (e.g. px → var tokens) to prove nothing moved.
- **Check you are on the right site before any write.** A shared dev install once had another client's theme active — confirm site name/theme via REST first (kit, Theme Builder and theme activation are site-wide).
- REST quirks on managed hosts: if `/wp-json/` redirects, use `/?rest_route=/…`; theme install/upload through REST can fail on WP Engine (`copy_failed_pclzip`) → upload the theme zip in wp-admin; SVG/WOFF uploads need a mime filter (admins only).
- ACF fields appear in REST only when the field group has "Show in REST" on; Yoast fields need registering for REST before a script can write them.
- Google Maps widget needs ~5 s to render before a screenshot; lazy content needs a scroll before QA screenshots.
- Git Bash on Windows rewrites arguments like `/about/` into file paths — set `MSYS_NO_PATHCONV=1` when passing URL paths to scripts.

### E5.37.11 QA protocol (client expectation: "zero bugs")
1. Compare **every page against the PDF/Figma, zoomed in**, not only overflow/console checks. Technical-only QA already failed once (the client found 10 visual bugs after a "clean" technical report).
2. Measure: padding on all four sides, icon centring in badges, image crops, gaps per breakpoint, text overflow, vertical centring in buttons, equal top/bottom section space.
3. Click-test: menus (desktop + mobile), accordions, tabs, sliders and arrows, forms (validation), logo links, brand links, pagination, hover states.
4. Devices: 1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320 (F2) — plus 700–800 for the menu/banner arrow case.
5. Run the checks twice (after fixes) and write the result per QA bug type.
6. Before saying "done": updated `PROJECT-SUMMARY.md` + `DAILY-LOG.md`, listed dummy content, links waiting on the client, client decisions, and the theme version.


---

## E5.38 PRE-DELIVERY SIGN-OFF

→ Merged into **E5.33 Elementor final QA** (Pre-delivery sign-off — tick every box twice). See E5.33.

---

## E5.39 BUILD IT RIGHT THE FIRST TIME — ICONS, IMAGES, MENU, DESIGN

E5.36–E5.38 find bugs **after** building. This section prevents the most frequent ones **while** building. Follow it in this order on every new project.

### E5.39.1 Design numbers come from the source, not from eyes
1. If a PDF/Figma exists, extract exact values first: text, positions, box sizes, colours, radii and vector paths (e.g. PyMuPDF `get_text("dict")`, `get_drawings()`, `get_images()`; Figma variables/context). Save them (`pdftext` / `pdfboxes` style JSON).
2. Build from those numbers (padding, gaps, radius, icon size, font size per breakpoint).
3. After building, compare **live DOM boxes/text vs the extracted PDF boxes** with a script, then zoom-compare screenshots. Fix every difference above ~2 px.
4. Curved edges, waves and bulged panels: take the path from the PDF and build a reusable mask (9-slice: fixed corner size + bulge), do not approximate with a plain border-radius.

### E5.39.2 Icons — no icon issues
- Take **every icon from the design/HTML itself** (extract SVG from the PDF/HTML). Never substitute Font Awesome/Elementor default icons unless the design uses them.
- Enable SVG upload (theme, admins only), upload each SVG once (D105), store the ids in a map (e.g. `svgmedia.json`) and reuse via the native Icon widget.
- Icons must use the global colour (currentColor / Elementor global), keep cut-out parts (detail paths) in the secondary colour, and keep their own fills only when the icon is an illustration (flag, logo).
- Size by variable per breakpoint (smaller on tablet/mobile); centre in the badge with flex — verify centre point by measurement.
- Hamburger, search, arrows, social: all custom SVG from the design (accessible names: E5.36.2).
- Check every icon: correct glyph, colour, size, centring, hover colour, at 1920 and 375.

### E5.39.3 Images — no image issues
- WebP, names, alt text and Media Library: E5.26, D104, D105. Resize to ≤2× the largest display size; SVG stays SVG (logos, icons, wordmarks, watermark).
- Use PDF-extracted crops/positions: hero, split-intro and card photos get `aspect-ratio` + `object-fit: cover` + focal position as in the design; photos that must not crop on mobile use auto height ≤991.
- Wide/background photos keep behaving as background at every width; overlays have explicit opacity.
- Logo tiles: contain, centred, whitespace-trimmed so logos look the same optical size (dynamic-image fallback: E5.37.8).
- Check: no stretched, blurry, cropped-wrong, missing or oversized (heavy) image at any breakpoint; `loading="lazy"` below the fold, priority on the LCP image.

### E5.39.4 Menu — no menu issues
1. Menus are real WordPress menus (E5.10); header, footer and mobile reuse them.
2. Desktop: Pro Mega Menu widget for panels, native submenu for simple dropdowns. Build the panel from the design: columns, icons, photos, floating card, radius, start at the header bottom.
3. States: hover, focus-visible, active (also when a child page is current), open. Test on the **bare URL**, no query string.
4. Hover image and CPT columns: E5.36.4. Mobile: E5.37.4 (Pro Off-Canvas, works from 1199 px down).
5. Keyboard: Tab order, Esc closes, `aria-expanded`.

### E5.39.5 Design system first, pages second
Global Colours → Global Fonts (real client fonts) → Elementor Variables for spacing (section, card, gap, gutter, per breakpoint) → saved components (heading, button, card, loop items, CTA) → Header/Footer → pages → CPT templates. Building pages before the variables exist is what creates static-px and inconsistent-gap bugs.

### E5.39.6 Honest limit
These sections lower the bug count a lot, but they do not guarantee zero. Every new project has its own design details, so the two-pass self-QA (E5.36.10, E5.33 pre-delivery sign-off) and the zoomed PDF compare stay mandatory. Any new kind of bug → fix it, then add one line to E5.36/E5.37.

---

# E6. Any other builder

*Use this part when the answer to E1 is option 5 (Bricks, Oxygen, Breakdance, Divi, Beaver Builder,
WPBakery, Kadence / Spectra / GenerateBlocks, or any other builder). These are **general rules**,
written to work with any builder. The builder's own official documentation decides how each rule is
done in that tool. All the shared rules of E1 still apply.*

## E6.1 Ask first (MANDATORY)

Ask, and record the answers in the architecture report (B65):

1. The builder's **name** and the exact **version** to use (and its Pro / add-on version, if any).
2. The **licence**: who owns it, and whether it is active on this site. Never use a nulled copy.
3. Whether a theme / theme builder from the same vendor is part of it (e.g. a builder that is also
   the theme).
4. The link to the builder's **official documentation**, if you are not sure where it is.

## E6.2 Official docs only — never invent

- Use only the builder's official documentation (and its official developer / API docs) for
  element names, settings, global styles, theme-builder templates, dynamic data, hooks, filters and
  import/export.
- **Never invent APIs, hooks, element names, setting names or data formats.** If the docs don't say
  how to do something, ask the user — don't guess.
- When a rule in this part cannot be done in that builder, say so in the architecture report and
  propose the closest update-safe alternative. The Figma match (E6.9) still wins.

## E6.3 Global design system from TOKENS.md

- Load the approved `TOKENS.md` (A5/A6) into the builder's **global colours, global fonts /
  typography, global spacing / variables, button and form styles and container width** — whatever
  the builder calls them.
- Builder globals and the `vendor.css` `:root` tokens hold the same values with the same names.
  Keep a token → builder global table in the theme docs.
- Set the builder's responsive breakpoints to the A5.8 values where the builder allows it.
- Never type the same raw value into many elements. A value not in `TOKENS.md` is a question for the
  user.

## E6.4 Reusable components and sections

- Build every repeated Figma component (`Design/components/`) once, as the builder's global /
  reusable element, component, template or partial.
- Every page section is a reusable template that works on any page, in any order (E1).
- Name templates by role, with one convention (e.g. `[Global] Header`, `[Section] CTA Banner`,
  `[Component] Blog Card`).

## E6.5 Header, footer, archives and singles

- Header and footer through the builder's **theme builder** (with display conditions) if it has
  one; otherwise through the theme files (`header.php` / `footer.php`, B67).
- Inspect the Figma header/footer frames at every breakpoint first. Menus use WordPress Menus (B7).
- Blog, CPT singles/archives, 404, Search and Sitemap pages are built with the builder's templates if
  it supports them, else with theme templates (B14, B46, D106, D107).
- Avoid overlapping templates that produce duplicate headers, footers or content.

## E6.6 Markup and CSS

- Map each Figma section/component to the builder's native structure elements (section / container
  / div / grid + heading, text, button, image …).
- **No HTML / code elements for section markup.** Use them only for a small piece that genuinely
  needs raw HTML.
- **No per-element custom CSS** and no page-level custom CSS. Where native controls can't match the
  design, give the element a reusable class and write the CSS once in the theme (`assets/css/`,
  shared class CSS using the `vendor.css` tokens, D103). No inline styles.
- Keep the nesting as shallow as the design allows.

## E6.7 Theme, scripts and update safety

- Code lives in the theme or a **child theme** (of the builder's theme, if it is one). Never edit
  builder, plugin or parent-theme files.
- The Part A assets (`vendor.css`, `vendor.js`, `animations.js`) are enqueued in the
  A7.6 order through `inc/enqueue.php`. Sliders use the builder's own default slider widget — no
  `sliders.js` and no new slider JS (A1). Check what the builder already loads (jQuery, sliders,
  animation libraries) and never load a library twice.
- Animations use `data-anim` attributes (A9.8), added through the builder's custom-attributes
  setting, if it has one. Use the builder's own motion features only when they match
  `Design/ANIMATIONS.md` exactly — never both on one element.
- Custom JS must not break or double-initialise inside the builder's editor.

## E6.8 Dynamic data and the importer

- Use the builder's dynamic data feature with WordPress fields, ACF PRO fields (D110, D111) and
  CPTs, where the builder supports it. Never hard-code CPT or site-wide values.
- Internal links use the builder's dynamic link to the page/post, where available — never typed
  URLs. Permalinks are **Post name** (D107.1).
- Images come from the Media Library, uploaded once (D104, D105).
- The importer (D96–D102) seeds the builder's templates and pages, using only the builder's
  documented import method (template import, export file or CLI). If none is documented, ask the
  user. Seed content comes from `Design/CONTENT.md` and `Design/images/`. Re-running creates no
  duplicates.
- The DeveloperAI meta tag comes from the theme hook (D112), never a builder code element.

## E6.9 Match the Figma design

- The site must visually and functionally match the Figma frames in `Design/pages/` — layout,
  spacing, typography, colours, images, hover, animations, interactions — at **1600 / 900 / 375**
  and the A5.8 breakpoints in between (F1). "Close enough" is not done.

## E6.10 QA

- [ ] Builder name, version and licence asked and recorded; official docs link known.
- [ ] Nothing in the build relies on an invented API, hook, element or setting.
- [ ] Builder globals (colours, fonts, spacing, buttons, forms, container) created from `TOKENS.md`
      and actually used; breakpoints match A5.8 where possible.
- [ ] Repeated components and sections are global/reusable, built once; any section works on any
      page, in any order.
- [ ] Header and footer built through the theme builder or theme files; menus are WordPress Menus;
      no duplicate header/footer.
- [ ] Blog, CPT, 404, Search and Sitemap pages built and dynamic.
- [ ] No HTML/code elements for section markup; no per-element or page custom CSS; no inline styles;
      shared class CSS lives in the theme.
- [ ] Child theme / theme files only; no builder, plugin or parent-theme file edited.
- [ ] No library loaded twice; animations and sliders work on the frontend and do not
      double-initialise in the editor.
- [ ] Dynamic data connected (ACF PRO / CPT fields); internal links dynamic; Post name permalinks.
- [ ] Images: Figma exports only, sharp, WebP, renamed, alt text set, in the Media Library once.
- [ ] Importer seeds the builder's templates and pages in the Figma order, with no duplicates.
- [ ] DeveloperAI meta tag once per page (D112).
- [ ] Every page matches its Figma frame at 1600 / 900 / 375; no console errors.

---

---

# F1. Verify against the Figma design

Every page and every section is checked against its Figma frame before it is called done.

1. Build **one real section** first as a proving run — ideally the header or hero, since it
   exercises type, colour, buttons, icons and responsive behaviour at once. Fix the drift before
   building the rest.
2. Screenshot every page headless at **1600 / 900 / 375** and compare it side by side with the
   matching frame in `Design/pages/` (desktop / tablet / mobile).
   > Headless Chrome enforces a ~500px minimum window, so a 375px screenshot comes out **cropped,
   > not narrow**. To check true mobile, load the page in a 375px-wide `<iframe>` inside a wrapper
   > page and screenshot that instead.
3. Check, per section: spacing, font sizes, line heights, colours, alignment, radius, shadows,
   image crops, icon sizes, hover / focus states, and the animation from `Design/ANIMATIONS.md`.
4. Zoom in on every image in the screenshot. If any image looks soft or blurry, export it again from
   Figma at a higher size (A2, A9.1 rule 16).
5. Check `Design/EXPORT-LOG.md` — every node must say `ok`. Nothing from the design is missing on the
   site.
6. Check in the builder too: open each page in the editor chosen in E1 and confirm an editor can
   change text, images, links and section order without code.
7. Fix the drift, then report: what was detected, what was built, what still needs the user's
   input (usually real copy, real images and real URLs).

---

# F2. Final QA

Run every list below that applies to this build. A build is not done while any item is open.

- [ ] Part A — the section delivery checklist (A9.9) for every section, and the design guardrails (A10).
- [ ] Part B — the final QA checklist (B70), including the build-lessons list.
- [ ] Part C — the existing-theme regression test (C91), if this build extended an existing theme.
- [ ] Part D — importer QA (D102), and the QA list of every D section used: D104–D107.1, D109–D115.
- [ ] Part E — the shared builder QA (E1) and the QA list of the chosen builder (E2 / E3 / E4 /
      E5.33 / E6.10).
- [ ] F1 — every page compared with its Figma frame at 1600 / 900 / 375; every image sharp; every
      Figma animation built; `EXPORT-LOG.md` all `ok`.
- [ ] `<meta name="DeveloperAI" content="Developer Skill">` is in `<head>` on every page (D112).
- [ ] Favicon made from the logo icon is set as the WordPress Site Icon and shows on every page (D114).
- [ ] Every clickable element is linked per `link-map.md` — no `#` / empty links (D115).
- [ ] `login.md` was never printed, committed or copied into the theme (D110).

## Final round — one last full check of the whole site (MANDATORY)

*Every QA list above has passed? Now do **one more complete round over the whole site** before
saying "done". It catches what the per-section lists miss.*

1. **Make the checklist file** `FINAL-QA.md` in the project root: one row per page / template
   (every page, single / archive templates, 404, Search, Sitemap, WooCommerce pages) × the checks below. Tick a box only after actually checking it, and note how
   (screenshot compare, crawl, test submit, test order).
2. **Design match — 100% to the Figma / PDF:** every page and every section side by side with its
   Figma frame / PDF page, zoomed in: section order, layout, spacing, sizes, fonts, weights, line
   heights, colours, radius, shadows, icons, images (sharp, right crop), hover / active states and
   animations. Measure the numbers, don't judge by eye — any difference over ~2 px gets fixed.
3. **Responsive:** every page at 1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 (A5.8) and the Figma frames 1600 / 900 / 375, plus 320 px: no horizontal scroll, no overlap, no
   clipped or squeezed text, menus / dropdowns / mini-cart stay inside the screen, tap targets at
   least 44 px, images not wrongly cropped.
4. **Functionality:** header menu, dropdowns, mobile menu, sticky header, search, sliders, tabs,
   accordions, modals, video, pagination, filters, animations; forms (validation, submit, the email
   arrives); WooCommerce (if any): add to cart, mini-cart, cart, coupon, checkout test order, emails, My Account. Everything works by keyboard. No console errors, PHP warnings or failed requests.
5. **Content:** exact text from the design (`Design/CONTENT.md`) — no lorem ipsum, no made-up copy;
   spelling, ™ / ®, phone, email and address correct. Placeholders only where they are listed for
   the client.
6. **Linking:** every clickable element opens the right page (D115); no `#`, empty, `.html`,
   staging or localhost links; no 404; external links open in a new tab.
7. **Images & icons:** all from the design export, sharp, WebP, alt text set, Media Library once (D105).
8. **SEO & `<head>`:** unique title and meta description per page, one `<h1>`, DeveloperAI meta tag
   and favicon on every page (D112, D114).
9. **Accessibility:** landmarks, heading order, alt text, form labels, visible focus, colour contrast
   measured.
10. **Performance:** pages load fast; no unused heavy assets; images correctly sized.
11. **Browsers:** Chrome, Safari, Firefox and Edge, plus one real phone (iOS Safari) where possible.
12. **Admin / editability:** every text, image, button and link is editable in WordPress / the
    builder chosen in E1; dynamic content (posts, CPTs, products, menus, settings) comes from the
    CMS — nothing hard-coded.

**Rules**
- Any fail → fix it → re-check that item **and** every page that shares that part.
- Never tick a box that was not actually checked. Anything that could not be checked is written
  down as "not checked + why".
- Only when every row in `FINAL-QA.md` is ticked, report: what was checked, what the final round
  fixed, and what is still waiting on the client.

---

# F3. Definition of done

The build is complete only when:

```text
Figma design (Design/pages, Design/components)
        ≈
WordPress front end (built with the builder chosen in E1)
```

while:

```text
Figma colours, fonts, sizes, spacing, radii   → approved TOKENS.md → vendor.css tokens (+ builder globals)
Figma text copy                               → Design/CONTENT.md → importer seed → editable in WordPress
Figma images / icons                          → Design/images + Design/svg → sharp WebP / SVG, Media Library once
Figma animations                              → Design/ANIMATIONS.md → data-anim presets
Repeated components                           → reusable sections / blocks / templates, built once
Static content                                → dynamic WordPress content (pages, CPTs, menus, options)
Internal links                                → WordPress permalinks / dynamic links, never hard-coded domains
<head> meta                                   → <meta name="DeveloperAI" content="Developer Skill"> on every page (D112)
Favicon                                       → WordPress Site Icon made from the logo icon (D114)
WooCommerce (if any)                          → D113 (+ E5.31 for Elementor)
```

Normal content changes must be possible from WP Admin / the chosen builder without editing code.
For Elementor builds, E5.34 adds its own definition of done on top of this one.

---

# F4. Quick update mode — site already built, go straight to the point

*When the site has already been built from this file and the user asks for one change ("add
WooCommerce", "fix the cart on mobile", "change the footer", "add a section"), do **not** run the
full build again. Go straight to that point.*

## Which mode?

```text
No theme / site yet                          → FULL BUILD (this whole file, from A1)
Site already built + one change is asked for → QUICK UPDATE (this section)
```

Signs the site is already built: the theme exists in `wp-content/themes/`, pages exist in WP Admin,
`TOKENS.md` and `Design/` exist, the project has a `CLAUDE.md` / theme docs from the earlier build.

## Quick update — steps

1. **Understand the ask.** Restate the one change in one line. Do not widen the scope.
2. **Read only the matching sections** of this file (tables below). Skip the rest — no full site
   inventory, no full architecture report, no re-reading every page, no new Figma export of the
   whole file.
3. **Do not ask again** what the earlier build already decided (page builder E1, forms plugin D109,
   ACF setup D110/D111, Elementor globals, header/footer templates, `custom.css` structure). Read it
   from the theme / project docs. Ask only if this change needs a **new** decision (e.g. WooCommerce
   Block or Classic, if WooCommerce was never set up).
4. **If the change comes from a new or changed Figma frame:** export only that frame and its images /
   SVGs into `Design/` (A2 rules: retry, never skip, sharp images), and add any new animation to
   `Design/ANIMATIONS.md`. A new colour / size / spacing value goes into `TOKENS.md` and `vendor.css`
   first — ask the user to confirm it.
5. **Inspect only what the change touches:** the files / templates / widgets for that feature and
   the pages that use them.
6. **Back up only those files** before editing (git commit, or a dated copy) — the light version of
   C75. Log in with `login.md` if WP Admin is needed; never expose it.
7. **Make the change** following that section's rules. Reuse the existing tokens, classes,
   components and file structure — no parallel structure, no duplicate CSS/JS.
8. **Check only what it affects:** that section's own QA list + the pages that use the changed part,
   at 1600 / 900 / 375 against the Figma frame. Confirm nothing else broke there.
9. **Report briefly:** what changed, which files, what was checked, anything still open.

## Topic → section index (all builders)

| Change asked for | Read only |
|---|---|
| New / changed Figma design | A2, A5, A6 (only for new values), F1 |
| Header / menu | B6, B7, B44, B67, C78, C80 |
| Footer | B8, C79 |
| CSS / design / spacing / colours | A9, B20, D103 |
| JavaScript / sliders / animations | A7.4, A7.5, A9.8, B21, B22, B51 |
| Responsive | A5.8, A9.7, B27, D103 |
| New page or section | B16, B17, E1 + the chosen builder (E2–E6) |
| Forms | B28, D109 |
| SEO / meta | B29, D112 |
| Favicon / site icon | D114 |
| Links / buttons / cards not linked | D115, B43 |
| Images / SVG / Media Library | A2, B26, D104, D105 |
| 404 / Search / Sitemap | D106, D107 |
| Permalinks | D107.1 |
| ACF fields | D110, D111 |
| Setup / importer | D96–D102 |
| Partners / Blog | B11–B15, B45–B48 |
| WooCommerce (add or change) | D113 |

## Extra index — Elementor builds

| Change asked for | Read only |
|---|---|
| Any visual change | E5.1 ⭐ Main Requirement first, then the row below |
| Reusing existing templates / globals | E5.14 |
| Global colours / typography / spacing | E5.3–E5.7 |
| Header / menu | E5.9, E5.10, E5.12 |
| Footer | E5.11, E5.12 |
| CSS / design (`custom.css`) | E5.18, E5.19 |
| Reusable components / templates | E5.8, E5.23, E5.24 |
| Responsive | E5.6 |
| Animations / GSAP / JS | E5.20, E5.19 |
| Forms | B28, D109 |
| SEO / meta | B29, D112, E5.30 |
| Favicon / site icon | D114 |
| Images / SVG / placeholders / Media Library | E5.26, E5.27, D104, D105 |
| Internal links / permalinks | E5.28, D107.1, D115 |
| Button / card / menu links not set | D115 |
| 404 / Search / Sitemap | E5.29 |
| Partners / Blog | E5.15, E5.16 |
| WooCommerce (add or change) | D113, E5.31 |
| Known bugs / lessons / final sign-off | E5.36–E5.39 |

Other builders: read E6 plus the matching row of the first table.

Always-on rules still apply in quick mode: no inline styles / widget CSS, tokens only, Figma images
only and sharp, images once in the Media Library, the DeveloperAI meta tag, never expose login
details.

## Example — add WooCommerce to an already-built site

1. Read only D113 (and E5.31 for Elementor).
2. Ask only the 3 WooCommerce start questions in D113 — one at a time (A1).
3. Go through D113 step by step and **skip any step that already exists** (e.g. theme support
   already added, pages already set).
4. Reuse the site's existing tokens, buttons, forms, header and breakpoints for the shop CSS/JS.
5. Run the D113 QA list and check the header (mini-cart) on a few non-shop pages.

## QA

- [ ] Only the asked change was made; nothing outside its scope changed.
- [ ] That section's own QA list passes (and, for Elementor, the E5.1 Main Requirement still holds).
- [ ] Pages using the changed part look and work the same as before, apart from the change.
