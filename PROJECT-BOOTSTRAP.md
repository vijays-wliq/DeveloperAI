# PROJECT BOOTSTRAP — Figma → Design System → CLAUDE.md

**What this file is:** a one-time setup instruction set. Drop it into an **empty project folder**
next to a Figma design, tell Claude to run it, and it produces a complete, token-driven front-end
starter — `vendor.css`, `global.html`, `CLAUDE.md`, folder structure — with every colour, font,
font-size, spacing value, radius and breakpoint **auto-detected from the Figma file**.

After it runs, `CLAUDE.md` becomes the project's law and this file is no longer needed.

**How to use it:**

```
Read PROJECT-BOOTSTRAP.md and run it.
Figma: <paste figma.com URL, or point at a folder of design screenshots>
```

---

## Contents

| Phase | What happens | Output |
|---|---|---|
| 0 | Gather inputs, confirm design source | — |
| 1 | Export the design into `Design/` (pages, components, images, SVG, animations) and extract raw design data | `../Design/`, in-memory |
| 2 | Normalise raw values into a token set | `TOKENS.md` |
| 3 | **Approval gate** — user confirms the token set | — |
| 4 | Scaffold folders + `vendor.css` | `assets/…`, `vendor.css` |
| 5 | Build `global.html` style guide | `global.html` |
| 6 | Generate the project's `CLAUDE.md` | `CLAUDE.md` |
| 7 | Verify, then hand over | screenshot + summary |

**Do not skip Phase 3.** Everything downstream is built on those numbers; a wrong type scale
caught at Phase 3 costs a minute, caught at page 6 costs a day.

---

## PHASE 0 — Inputs

Establish these before doing anything else.

1. **Design source.** One of:
   - A `figma.com/design/…` or `figma.com/file/…` URL → use the Figma MCP tools.
   - A folder of exported screenshots / PNGs → read them with the image reader.
   - The design exported as **PDF** (all pages, usually in `../Design/`) → read it with the PDF
     reader, max 20 pages per call; treat each PDF page as one artboard.
   - Both → prefer Figma for numbers, screenshots / PDF for layout intent.

2. **Is the Figma MCP server connected — and does the link actually work?** Check the available
   tool list for `get_variable_defs`, `get_design_context`, `get_metadata`, `get_screenshot`, then
   make one test call (`get_metadata`) on the link.
   - **Connected and the test call succeeds** → Phase 1A (exact values, no guessing).
   - **Not connected** → Phase 1B (measure from screenshots / PDF pages, then confirm every number
     with the user at Phase 3). Say plainly that values are measured, not read.
   - **Connected but the link fails** (no access, wrong or missing node-id, rate limit, empty
     response) → **fall back to the PDF / screenshots in `../Design/`** and go to Phase 1B. Tell the
     user in one line: *"The Figma link did not work (<reason>), so I am using the Design/ PDF — values
     are measured, not read."* If `Design/` is empty too, stop and ask for the PDF export. Never guess
     a design you cannot see.

3. **Project name and page list.** Ask: *"What is the project called, and which pages are in
   scope?"* Needed for `<title>`, the `global.html` page index and `CLAUDE.md`.

4. **Animation library.** Default to **GSAP 3.12.5 + ScrollTrigger**, loaded from pinned cdnjs URLs
   and driven by `data-anim` attributes through `assets/js/animations.js` (exact URLs, attribute API
   and preset list are fixed in §8 of the generated `CLAUDE.md` — use them verbatim). If the user
   names another library, use it **only** when they also supply its exact CDN URLs, attribute API
   and preset list (or its official docs); never invent a URL, attribute or preset name. If the user
   wants none, drop §8 from the generated `CLAUDE.md`, and remove the script tags and
   `animations.js` from the templates.

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

**Sliders are not a question — they are fixed.** Every slider, carousel or logo strip in the design
is built with **Slick 1.8.1** (needs **jQuery 3.7.1**), driven by `data-slider` attributes through
`assets/js/sliders.js` (§4.5). Never use Swiper, Owl, Splide or hand-written slider JS.

---

## PHASE 1A — Extract from Figma (MCP connected)

### 1A.0 Export the whole design into `../Design/` (Figma connected)

When the Figma link works, export the design into `../Design/` **before** extracting tokens (use
`get_metadata` first to get the frame tree). This folder is the local copy of the design for the
whole build:

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
   closest `data-anim` preset from §8. If no preset fits, ask. Never drop a Figma animation without
   telling the user.
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

   `Design/CONTENT.md` is the **only** source for the text in the HTML pages. Never retype copy from
   a screenshot by eye, never shorten it, never invent copy. If a text is missing or unreadable in
   Figma, write `MISSING — ask` in `CONTENT.md` and list it for the user.

### Images in the site — Figma export only, always sharp

Every image in the site comes **only** from `Design/images/` (raster) and `Design/svg/` (icons,
logos, vector art). No stock photos, no AI images, no crops of the page PNGs. **Images must never
look blurry:**

- Export from Figma at **2x** the size the image is shown on screen.
- When converting to `.webp`, resize **down only** to 2x the largest display size. Never upscale.
- WebP quality **85 or higher** for photos. Use lossless for images with text or sharp edges.
- Icons, logos and line art stay **SVG**. Never convert them to PNG or WebP.
- Set `width` / `height` on `<img>` from the design, and use `object-fit` so it is never stretched.
- After each page, zoom the screenshot on every image. If any image looks soft, export it again at a
  higher size.

### 1A.1 Extract the design values

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

## PHASE 1B — Extract from screenshots or PDF (no MCP, or the Figma link failed)

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

Round everything per Phase 2 and **flag the whole set as "measured — please confirm"** in
`TOKENS.md`.

In this mode, still fill `../Design/` as far as the source allows: page PNGs in `Design/pages/`,
every text read from the PDF / screenshots into `Design/CONTENT.md` (same structure as 1A.0 step 5),
and every animation the user describes into `Design/ANIMATIONS.md`. Images and icons that cannot be
taken from the PDF as clean files are listed for the user — ask for the original files. Never crop
them out of a page screenshot.

---

## PHASE 2 — Normalise raw data into tokens

This is the phase that matters. Raw Figma data is messy — 41 slightly different greys, font sizes
of 17.6px. Normalising turns that into a scale a human can hold in their head.

### 2.1 Global normalisation rules

- **Round every px to a whole number.** `17.6px → 18px`.
- **Snap to the nearest 5** for anything ≥ 20px (spacing, large type). `33px → 35px`, `62px → 60px`.
  Below 20px **never snap** — keep whole px (`4, 8, 10, 12, 14, 16, 18`). This rule applies
  everywhere, including the responsive step-down in §2.8.
- **Merge only fractional drift.** Values that round to the same whole px are one token
  (`15.8 → 16`, `16.2 → 16`). **Never merge two distinct whole-px values** — `12 / 14 / 16` type and
  `8 / 10` spacing stay separate tokens. Snap-to-5 above 20px is the only other merge allowed.
- **Drop one-offs.** A value used **once** in the whole design is not a token; it is a mistake or a
  detail. Fold it into the nearest neighbour. Exception: the single largest display size, which is
  usually a deliberate hero number.
- **Colours:** merge two hexes whose channels are all within 8 of each other. Keep the more frequent.

### 2.2 Fonts

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
- Build the Google Fonts `<link>` from the families and the weights actually present. Do not
  request weights the design never uses.

### 2.3 Type scale — three tiers

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

### 2.4 Line heights

```
--bodyLh:  most frequent line-height on paragraph nodes, as a unitless ratio, rounded to 1 dp
--lhTight: most frequent line-height on heading nodes, rounded to 2 dp
```
Typical: `1.4` and `1.15`. If a design shows `1.0` on headings, use `1.05` — exactly 1 clips
descenders in most faces.

### 2.5 Colours

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
- Every hex appears **exactly once** in `:root` and nowhere else in the codebase.

### 2.6 Spacing

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

### 2.7 Geometry & motion

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

### 2.8 Breakpoints — always these seven, never others

Desktop-first, `max-width`, always `.98`:

```
1599.98 · 1439.98 · 1365.98 · 1199.98 · 991.98 · 767.98 · 575.98
```

This is fixed regardless of what the Figma file contains. Figma artboards tell you *what changes*,
not *where the breakpoint sits*.

Derive the **token step-down table** — the single most valuable thing this bootstrap produces,
because it makes every future layout responsive for free:

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

Round every result with the §2.1 rule: **results ≥ 20px snap to the nearest 5, results below 20px
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

## PHASE 3 — Approval gate

Write **`TOKENS.md`** and stop. It must contain:

1. A one-paragraph summary: *N colours, N font families, N type sizes, N spacing steps detected.*
2. Every token as a table: **token name · value · where it came from** (node name / frequency count).
3. A **"Needs your confirmation"** list — every value that was measured rather than read, every
   near-duplicate you merged, every one-off you dropped.
4. The semantic mapping (`h1 = --f65`, `p = --f18`, …).
5. The step-down table with real numbers filled in.

Then ask: *"Confirm these tokens and I'll scaffold the project."* Do not write `vendor.css` before
the user answers. If they correct a value, update `TOKENS.md` and re-confirm only that value.

---

## PHASE 4 — Scaffold

### 4.1 Folder structure

```
<project>/
├── index.html
├── global.html                     living style guide + page index
├── CLAUDE.md                       generated in Phase 6
├── TOKENS.md                       generated in Phase 3
└── assets/
    ├── css/
    │   ├── vendor.css              GLOBAL ONLY — tokens, reset, typography, buttons, breakpoints
    │   ├── global/                 shared components that are not sections (pages-nav, forms…)
    │   └── sections/               ONE FILE PER SECTION
    ├── js/
    │   ├── vendor.js               nav toggle, accordions — all init guarded
    │   ├── sliders.js              data-slider → Slick 1.8.1 (§4.5) — the ONLY slider JS
    │   ├── animations.js           data-anim → GSAP + ScrollTrigger (§4.4) — the ONLY animation JS
    │   └── global/                 optional per-feature helpers
    ├── images/                     WebP only (SVG for vector art) — optimised, sized to use
    │   └── favicon/                favicon.ico, favicon.svg, apple-touch-icon.png, site-icon.png (rule 19)
    └── fonts/                      only for self-hosted faces
```

Multi-page sites: once a page needs 3+ of its own sections, give it a folder —
`assets/css/<page>/<section>.css` — instead of crowding `sections/`.

### 4.2 `assets/css/vendor.css`

Write it in exactly this order. Everything in `{{ }}` comes from `TOKENS.md`; everything else is
boilerplate that is identical in every project.

```css
/* ===================================================================
   TOKENS
   =================================================================== */
:root {
    /* COLORS */
    {{ --brand and neutral colours, one per line, hex only, in the Phase 2.5 order }}
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
closed the moment Phase 4 ends. New global token or global component → `vendor.css` **and** a demo
in `global.html`. Anything else → the section's own file.

### 4.3 `assets/js/vendor.js`

Keep it small and **guard every init** so one missing element never breaks the page:

```js
document.addEventListener('DOMContentLoaded', () => {
    const nav = document.getElementById('primary-nav');
    if (nav) { /* toggle, backdrop, Esc-to-close, focus trap */ }

    const accordions = document.querySelectorAll('[data-accordion]');
    if (accordions.length) { /* … */ }
});
```

No scroll animation code — that lives only in `animations.js` (§4.4). No slider code — that lives
only in `sliders.js` (§4.5). Every init must also work when the same section appears twice on one
page — select all matches, never assume one.

### 4.4 `assets/js/animations.js` (default: GSAP + ScrollTrigger)

Copy this file verbatim. It turns `data-anim*` attributes into GSAP animations, so no section ever
needs its own animation JS. Content is fully visible without JS and with `prefers-reduced-motion`.

```js
/* Scroll animations — GSAP 3.12.5 + ScrollTrigger, attribute-driven (CLAUDE.md §8). */
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

Load order at the end of `<body>` on every page (all `defer`, so `DOMContentLoaded` runs after them):

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js" defer></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js" defer></script>
<!-- slider pages only (§4.5) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.7.1/jquery.min.js" defer></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.8.1/slick.min.js" defer></script>
<!-- /slider pages only -->
<script src="assets/js/vendor.js" defer></script>
<script src="assets/js/sliders.js" defer></script>
<script src="assets/js/animations.js" defer></script>
```

`sliders.js` must load **before** `animations.js`: Slick builds the slider layout first, then
ScrollTrigger measures the page.

Never change the version in these URLs without updating §8 of the generated `CLAUDE.md`.

### 4.5 `assets/js/sliders.js` (Slick 1.8.1 + jQuery 3.7.1)

Copy this file verbatim. It turns `data-slider*` attributes into Slick sliders, so no section ever
needs its own slider JS. It does nothing on a page with no `[data-slider]`.

**Only on pages that have a slider**, add the Slick core CSS in `<head>`, **before** `vendor.css` so
the project's slider rules win. Add the jQuery + Slick `<script>` tags shown in §4.4 too. Do **not**
load `slick-theme.css`. It ships the Slick icon font, which breaks the no-icon-fonts rule.

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.8.1/slick.min.css">
<link rel="stylesheet" href="assets/css/vendor.css">
```

```js
/* Sliders — Slick 1.8.1 + jQuery 3.7.1, attribute-driven (CLAUDE.md §8.1). */
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
**and** to the preset list in §8.1. Never pass Slick options inline from a section.

---

## PHASE 5 — `global.html`

The living style guide. It is the first file a teammate opens and the first file Claude opens
before building any section. Build it with:

1. **`.pages-nav`** — a table of every page in the build (`#` · Page Name · Link), styled by
   `assets/css/global/pages-nav.css`. Seed it with the pages from Phase 0.
2. **Display headings** — one row per display token, sized inline (`global.html` is the *only* file
   where inline `style` is allowed, and only for this).
3. **`h1`–`h6` + `p`**, rendered as bare tags so the global CSS is what you see.
4. **Heading helpers** — `.heading-accent`, `.section-title`, `.section-title-wrap` in all three
   forms (with desc, without desc, with eyebrow).
5. **Every button variant** and both arrow-link colours.
6. **The page shell** — skip links → `header#header` → `main#main` → `footer#footer` → the
   scripts from §4.4, shown as copyable markup. The skip links are always exactly:
   ```html
   <a class="skip-link" href="#main">Skip to content</a>
   <a class="skip-link" href="#footer">Skip to footer</a>
   ```
7. **Sliders** — only if the design has one: a working demo of each slider preset the design uses
   (`cards`, `logos`, `single`), with the arrow buttons and dots, built with the §8.1 markup.
   `global.html` then also loads the Slick CSS and the jQuery + Slick scripts (§4.5).

Rule that keeps it alive: **a new component is not done until it is also in `global.html`.**

Text in `global.html` is sample text for components only. Real page copy always comes from
`Design/CONTENT.md` (§1 rule 20).

---

## PHASE 6 — Generate the project's `CLAUDE.md`

Write the file below into the project root, with every `{{ }}` replaced by the real values from
`TOKENS.md`. This generated file — not this bootstrap — is what governs the project from here on.

Keep the numbered-rule structure intact. It is what makes the rules quotable in review
("rule 6", "§3.1") instead of vague.

````markdown
# {{ Project Name }} — Global Base Rules

**This is the single source of truth for how every section of this project is built.**
When a Figma section / design is given, build it **exactly** as designed — but always using the
tokens, headings, buttons, spacing, colors and breakpoints defined below. Never invent a new
value if a token already exists.

```
{{ folder tree from Phase 4.1 }}
```

`vendor.css` is closed to section code. Every new section gets its **own** file in
`assets/css/sections/` — see §6.1.

---

## 0. Component Reference — `global.html`

`global.html` is the **living style guide**. Every ready-made typography and button component is
already built there, using the exact classes and tokens from `vendor.css`.

**Workflow for every new section:**
1. **Open `global.html` first.** Find the heading / paragraph / button that matches the design.
2. **Copy that exact markup**, then add the section's own BEM class alongside it.
3. Only if nothing matches, build something new — and **add it to `global.html` too**.

### 0.1 Page index — every new page gets a row
The first section of `global.html` is `.pages-nav`, a table of every page in the build.
**Creating a page and adding its row happen in the same commit.** Keep `--num` sequential and
zero-padded; Page Name is the human name from the design; Link text is the filename.

{{ list the components that exist in this project's global.html }}

---

## 1. Non-Negotiable Rules

1. **Design accuracy first.** Match the design pixel-for-pixel — layout, order, spacing, sizes,
   colors, states. Do not "improve" or re-arrange it.
2. **Check `global.html` before writing anything.** Reuse what is there; add anything new to it.
3. **Use standard heading tags.** One `<h1>` per page, never skip levels. If the visual size ≠ the
   semantic level, keep the correct tag and override the size with a `--f*` token.
4. **Paragraphs use `<p>`.** Global `p` = `{{ --f<p> }}` / `{{ --font-body }}`.
5. **Every value comes from a token.** No raw hex, no raw px font-sizes in section CSS.
6. **Every section gets its own parent class**, and every child element gets its own class.
7. **Write section CSS with native CSS nesting** under that one parent class (§6).
8. **Responsive only through the 7 fixed breakpoints** (§7). No custom breakpoints.
9. **No inline `style=""`** in production markup (`global.html` excepted).
10. **All motion is {{ animation library }}, attribute-driven** — no custom scroll JS, no entrance
    keyframes in section CSS (§8).
11. **Accessibility is part of "done"**: landmarks, `alt` on every image, `aria-label` on icon-only
    controls, visible focus, keyboard-usable interactions.
12. **Every new section gets its own CSS file** at `assets/css/sections/<section>.css`, linked after
    `vendor.css`.
13. **No icon fonts — every icon is an inline SVG** with `fill="currentColor"`.
14. **Section titles are always the `.section-title-wrap` block** — bare `<h2>`, no class, no `id`,
    and never any section CSS for the title (§3.1).
15. **Every new page gets a row in the `global.html` page index** (§0.1).
16. **All raster images are WebP and optimised.** Convert every JPG/PNG/GIF to `.webp`, compress
    it with no visible quality loss, keep transparency where the source has it, and size it to the
    largest size it is actually displayed at (2× for retina). Store it in `assets/images/`.
    Logos, icons and illustrations that are vector art stay SVG. Never ship the original JPG/PNG.
    **Images come from the Figma export only** — every image in the HTML is the exported file from
    `Design/images/` (icons and logos inlined from `Design/svg/`). No stock photos, no AI images, no
    crops of the page PNG. **Images must never look blurry:**
    - Export from Figma at **2x** the size the image is shown on screen.
    - When converting to `.webp`, resize **down only** to 2x the largest display size. Never upscale.
    - WebP quality **85 or higher** for photos. Use lossless for images with text or sharp edges.
    - Icons, logos and line art stay **SVG**. Never convert them to PNG or WebP.
    - Set `width` / `height` on `<img>` from the design, and use `object-fit` so it is never stretched.
    - After each page, zoom the screenshot on every image. If any image looks soft, export it again
      at a higher size.
17. **Every slider is Slick 1.8.1, attribute-driven** (§8.1). Use `data-slider` presets only. No
    Swiper, Owl or Splide, no hand-written slider JS, no `$(…).slick()` call outside `sliders.js`.
18. **Every page has the DeveloperAI meta tag in `<head>`, by default.** Add this hidden tag to every
    `.html` page (`global.html` too), right after `<meta name="viewport" …>`. Copy it exactly — do
    not change the name or the content, and never show it on the page:
    ```html
    <meta name="DeveloperAI" content="Developer Skill">
    ```
19. **Every page has a favicon made from the logo icon.** Use the icon (mark) part of the logo — not
    the full logo with its wordmark text. Source, in this order: a favicon / app-icon frame in the
    design → the logo mark, exported from Figma as SVG → if the logo is text only, its first letter
    in the logo font and brand colour on a square (tell the user). Never draw a new icon or use a
    generic one.
    - Square, mark centred with about 10% padding. Built from the SVG, so it is sharp — never upscale
      a small PNG. Transparent background; if the mark is white / very light, put it on the brand
      background token colour.
    - Check it at 16 × 16 and 32 × 32. If it is too detailed to read, use the simplest part of the mark.
    - Files in `assets/images/favicon/`: `favicon.ico` (32 × 32), `favicon.svg`,
      `apple-touch-icon.png` (180 × 180) and `site-icon.png` (512 × 512 — kept for WordPress Site
      Icon later).
    - Add these to every `.html` page (`global.html` too), right after the DeveloperAI meta tag:
    ```html
    <link rel="icon" href="assets/images/favicon/favicon.ico" sizes="32x32">
    <link rel="icon" href="assets/images/favicon/favicon.svg" type="image/svg+xml">
    <link rel="apple-touch-icon" href="assets/images/favicon/apple-touch-icon.png">
    ```
20. **Every text comes from `Design/CONTENT.md`** (Phase 1A.0 step 5) — exact words, case and
    punctuation. Never retype copy from a screenshot, never write "Lorem ipsum" or made-up copy. A
    text marked `MISSING — ask` is asked, not invented.

---

## 2. Colors (from `:root`)

{{ colour tables: Brand / Neutrals & Surfaces / Gradients / Role aliases,
   each row = Token · Value · Use }}

**Rule:** never write a hex in section CSS. If a design colour genuinely does not exist yet, add it
to `:root` first, then use it.

---

## 3. Typography

### Font families
{{ table: Token · Family · Used by }}

### Type scale tokens
Fixed: {{ list }}
Fluid (`clamp`, auto-scales): {{ table: Token · Range }}
Display (stepped down per breakpoint, see §7): {{ list }}

### Line heights
`--bodyLh` = {{ }} (body) · `--lhTight` = {{ }} (all headings)

### Global heading defaults (already in `vendor.css` — do not duplicate)
{{ table: Tag · Size · Family · Weight }}

### 3.1 Section titles — always `.section-title-wrap`

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
- Drop `.section-title-desc` entirely when there is no copy.
- **Never write CSS for a section title.** The centring, gap, bottom margin, eyebrow style, desc
  measure and the accent-coloured `<span>` are all already handled.
- Because the `h2` has no `id`, give the `<section>` an **`aria-label`**, not `aria-labelledby`.
- Headings *inside* a component (card title, panel heading) keep their own BEM class — this rule is
  about section-level headings only.

---

## 4. Buttons & Links (global — reuse, never re-create)

{{ .btn base spec, variant table with default + hover, mobile behaviour }}
{{ .btn-link spec }}

### 4.1 Icons — inline SVG only
```html
<svg class="<section>__icon" width="20" height="20" viewBox="0 0 20 20" fill="none"
     xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <path d="…" fill="currentColor" />
</svg>
```
`fill="currentColor"` (or `stroke="currentColor"` for outlined icons) so the icon tracks the
parent's `color`. Explicit `width`/`height` + matching `viewBox`. Decorative → `aria-hidden="true"`;
icon-only control → `aria-label`.

**The `.btn-link` arrow is the exception** — it is global, on the `::after`. A section writes no
arrow markup and no arrow CSS. Never `content: none` it.

---

## 5. Spacing & Layout

### Container
{{ --container / --gutter table }}

```html
<section class="about" id="about">
  <div class="container">…</div>
</section>
```

### Vertical rhythm
`--sectionGap` ({{ }}) is **always** the section's `padding-block`.
⚠️ `vendor.css` also sets `main section { margin-bottom: var(--sectionGap) }`, so the real gap
between two sections is **padding + {{ }} + padding**. Account for it before adding more space.

### Spacing scale
{{ --sp* list }}
Spacing between blocks is set by the **parent** (`gap`) or a single-direction `margin-bottom` on the
child. Never mix `margin-top` + `margin-bottom` on siblings.

### Radius / Shadow / Z-index / Motion
{{ token lists }}

---

## 6. Naming & CSS Authoring

### 6.1 One CSS file per section
`assets/css/sections/<section>.css` — filename = the block class, lowercase, hyphenated.
One file = one section = one top-level selector + that section's media queries. Nothing else.
Link it after `vendor.css`, in the order the sections appear on the page.
Every section file starts with:
```css
/* =================================================================== ABOUT section css =================================================================== */
```

### 6.2 Class naming — BEM, per section
```
.<section>                block (the section parent)
.<section>__<element>     element
.<section>--<modifier>    block modifier
.<section>__<el>--<mod>   element modifier
```
The `<section>` element carries the block class **and** a matching `id`. Every child that is
positioned, sized or coloured gets a class — nothing is styled by bare tag name inside a section
(except `p` inside a repeating card, where a class would be pure noise).

### 6.3 HTML skeleton every section follows
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
Semantic tags throughout. Every image is an optimised `.webp` (§1 rule 16) and gets `alt` +
`width`/`height`; below-fold images get `loading="lazy"`.

### 6.4 CSS — one nested block per section
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

### 6.5 Property order inside a rule
`position/inset` → `display/flex/grid` → `width/height` → `margin/padding` →
`background/border/radius/shadow` → `font/color/text` → `transition/transform/animation`

---

## 7. Breakpoints (the only ones allowed)

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

### 7.1 What the global breakpoints already do
{{ the filled-in step-down table }}

Any layout using `.container`, `--gutter`, `--sectionGap`, `--g40` or the type tokens responds
automatically — **do not re-declare those values in a section.**

### 7.2 Where section responsive CSS goes
At the **bottom of that section's own file**, under a comment banner, ordered largest → smallest.
Only write a breakpoint the design actually changes at. Empty queries are not written.

---

## 8. Animation — GSAP 3.12.5 + ScrollTrigger (attribute-driven)

**Setup** — already in every page shell, in this order, all `defer`:
`gsap.min.js` → `ScrollTrigger.min.js` (both `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/`)
→ (slider pages only: jQuery 3.7.1 → Slick 1.8.1, §8.1) → `assets/js/vendor.js` →
`assets/js/sliders.js` → `assets/js/animations.js`.

**Attribute API** — the only animation interface:

| Attribute | On | Effect |
|---|---|---|
| `data-anim="<preset>"` | any element | Animates the element in when it scrolls into view |
| `data-anim-delay="200"` | with `data-anim` | Delay in ms (keep < 400) |
| `data-anim-duration="700"` | with any `data-anim*` | Duration in ms (400–900) |
| `data-anim-stagger="<preset>"` | a parent | Animates each direct child in turn |
| `data-anim-stagger-gap="120"` | with `data-anim-stagger` | Gap between children in ms |
| `data-anim-counter="579"` | a number element | Counts up to the value; put the same final number in the HTML text |

**Valid presets — no others exist:** `fade-up`, `fade-down`, `fade-left`, `fade-right`, `fade-in`,
`zoom-in`. A new preset is added to `PRESETS` in `animations.js` **and** to this list, never inline.

Every animation in `Design/ANIMATIONS.md` is mapped to one of these presets (Phase 1A.0 step 4).
Figma hover and pressed variants are built as CSS `:hover` / `:focus-visible` / `:active` states
with `--transition`, not as `data-anim`.

### Animation rules
1. **Attributes only.** No custom scroll JS, no `IntersectionObserver`, no entrance keyframes in a
   section file. Hover/focus micro-transitions still use `--transition`.
2. Animation attributes live in the **HTML**, never in section CSS.
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

### 8.1 Sliders — Slick 1.8.1 (attribute-driven)

**Setup — slider pages only:** `slick.min.css` in `<head>` before `vendor.css`, and
`jquery.min.js` (3.7.1) → `slick.min.js` (1.8.1) before `vendor.js`. All from
`https://cdnjs.cloudflare.com/ajax/libs/`. Then `assets/js/sliders.js`, before `animations.js`.
Never load `slick-theme.css`.

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

**Markup:**

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
7. Autoplay must pause on hover/focus (default). It is switched off for `prefers-reduced-motion`.

---

## 9. Section Delivery Checklist

- [ ] Design matches the reference at every breakpoint
- [ ] `global.html` was checked first and its components reused
- [ ] Any genuinely new component was added to `global.html`
- [ ] A new page has its row in the `.pages-nav` table (§0.1)
- [ ] Section has its **own file** `assets/css/sections/<section>.css`, linked after `vendor.css`
- [ ] `vendor.css` untouched
- [ ] Section has its own block class **and** matching `id`; every child has a BEM class
- [ ] Correct semantic heading order; one `h1` per page
- [ ] Section title is `.section-title-wrap` — bare `h2`, no class, no `id`, no title CSS (§3.1)
- [ ] Section uses `aria-label` (not `aria-labelledby`)
- [ ] Zero raw hex, zero raw px font-sizes, zero magic numbers — tokens only
- [ ] `padding-block: var(--sectionGap)` on the section
- [ ] CSS is one nested block with `&`, max 3 levels, comment banner at the top
- [ ] Responsive queries at the bottom of the same file, largest → smallest
- [ ] Every raster image is an optimised `.webp` — no JPG/PNG/GIF shipped
- [ ] Every image comes from the Figma export (`Design/images/`, `Design/svg/`) and is sharp, not blurry
- [ ] Every text comes from `Design/CONTENT.md` — nothing retyped or invented (rule 20)
- [ ] Every Figma animation for this section in `Design/ANIMATIONS.md` is built
- [ ] Images have `alt` + `width`/`height`; below-fold images `loading="lazy"`
- [ ] Every icon is an inline `<svg>` with `currentColor` — no icon fonts
- [ ] Hover **and** `:focus-visible` states work; keyboard reachable
- [ ] Motion via `data-anim*` attributes only, valid presets only; GSAP, ScrollTrigger and
      `animations.js` linked
- [ ] Any slider uses `data-slider` with a valid preset (§8.1). Slick CSS, jQuery, Slick and
      `sliders.js` are linked on that page. Arrows are SVG buttons. It works by swipe and keyboard
      at every breakpoint.
- [ ] No inline `style` attributes
- [ ] No horizontal scroll at 320px
- [ ] The page `<head>` has `<meta name="DeveloperAI" content="Developer Skill">` (rule 18)
- [ ] The page `<head>` has the favicon links, and the tab shows the logo icon (rule 19)

---

## 10. Final Round — whole site, before handover

*Every QA list above has passed? Now do **one more complete round over the whole site** before
saying "done". It catches what the per-section lists miss.*

1. **Make the checklist file** `FINAL-QA.md` in the project root: one row per page / template
   (every page incl. 404 if built) × the checks below. Tick a box only after actually checking it, and note how
   (screenshot compare, crawl, test submit).
2. **Design match — 100% to the Figma / PDF:** every page and every section side by side with its
   Figma frame / PDF page, zoomed in: section order, layout, spacing, sizes, fonts, weights, line
   heights, colours, radius, shadows, icons, images (sharp, right crop), hover / active states and
   animations. Measure the numbers, don't judge by eye — any difference over ~2 px gets fixed.
3. **Responsive:** every page at the 5 device sizes **1920** (desktop) / **1440** (laptop) / **1024** (iPad landscape) / **768** (iPad portrait) / **390** (iPhone, inside a 390 px `<iframe>`) — not every CSS breakpoint: no horizontal scroll, no overlap, no
   clipped or squeezed text, menus / dropdowns / mini-cart stay inside the screen, tap targets at
   least 44 px, images not wrongly cropped.
4. **Functionality:** header menu, dropdowns, mobile menu, sticky header, search, sliders, tabs,
   accordions, modals, video, pagination, filters, animations; forms (validation, submit, the email
   arrives). Everything works by keyboard. No console errors or failed requests.
5. **Content:** exact text from the design (`Design/CONTENT.md` / Figma / PDF) — no lorem ipsum, no made-up copy;
   spelling, ™ / ®, phone, email and address correct. Placeholders only where they are listed for
   the client.
6. **Linking:** every clickable element opens the right page (`.pages-nav` + every button, card and menu link); no `#`, empty,
   staging or localhost links; no 404 (every `.html` link opens an existing page); external links open in a new tab.
7. **Images & icons:** all from the design export, sharp, WebP, alt text set.
8. **SEO & `<head>`:** unique title and meta description per page, one `<h1>`, DeveloperAI meta tag
   and favicon on every page (rules 18, 19).
9. **Accessibility:** landmarks, heading order, alt text, form labels, visible focus, colour contrast
   measured.
10. **Performance:** pages load fast; no unused heavy assets; images correctly sized.
11. **Browsers:** Chrome, Safari, Firefox and Edge, plus one real phone (iOS Safari) where possible.

**Rules**
- Any fail → fix it → re-check that item **and** every page that shares that part.
- Never tick a box that was not actually checked. Anything that could not be checked is written
  down as "not checked + why".
- Only when every row in `FINAL-QA.md` is ticked, report: what was checked, what the final round
  fixed, and what is still waiting on the client.
````

---

## PHASE 7 — Verify and hand over

1. Build **one real section** from the design as a proving run — ideally the header or hero, since
   it exercises type, colour, buttons, icons and responsive behaviour at once.
2. Screenshot it headless at **1600 / 900 / 375** and compare against the Figma frame.
   > Headless Chrome enforces a ~500px minimum window, so a 375px screenshot comes out **cropped,
   > not narrow**. To check true mobile, load the page in a 375px-wide `<iframe>` inside a wrapper
   > page and screenshot that instead.
3. Fix the drift before building the rest.
4. As each page is built, screenshot it headless at **1600 / 900 / 375** and compare it side by side
   with the matching frame in `Design/pages/` (desktop / tablet / mobile). Check, per section:
   spacing, font sizes, line heights, colours, alignment, radius, shadows, image crops, icon sizes,
   hover / focus states, and the animation from `Design/ANIMATIONS.md`.
5. Zoom in on every image in the screenshot. If any image looks soft or blurry, export it again from
   Figma at a higher size.
6. Check `Design/EXPORT-LOG.md` — every node must say `ok`. Nothing from the design is missing on the
   site.
7. Report: what was detected, what was generated, what still needs the user's input (usually real
   copy, real images and real URLs).
8. When the **whole site** is built, run the **§10 Final Round** of the generated `CLAUDE.md` —
   `FINAL-QA.md`, design match 100% to Figma / PDF, responsive, functionality, content and links —
   before handing over.

---

## Guardrails — things that go wrong if you skip them

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
| Inventing a CDN URL, animation attribute or preset | Use only the pinned URLs and presets in §4.4 / §8. Ask for docs before adding a library. |
| Reaching for Swiper or writing a slider by hand | Every slider is Slick 1.8.1 through `data-slider` (§4.5 / §8.1). |
| Slider shows stacked slides or nothing at all | jQuery + Slick not linked on that page, or loaded after `sliders.js`. Check the §4.4 order. |
| `data-anim-stagger` on a slider track | Slick clones and re-wraps the slides. Animate the `[data-slider-wrap]` as one block. |
| Mobile text shrinking below 14px | Respect the §2.8 minimums after every step-down. |
| Building page 2 before `global.html` exists | The style guide is what stops page 2 from re-inventing page 1. |
| Skipping the Phase 3 approval gate | Every later file is built on those numbers. Confirm once, cheaply. |
| Dropping a Figma JPG/PNG export straight into `assets/images/` | Convert it to `.webp`, compress it and resize it first — resize down only, to 2x display size, quality 85+, so it never goes blurry. Vector art stays SVG. |
| Figma export timed out, so the image was skipped or replaced with a placeholder | Retry 3 times, then export in smaller parts. Every node in `EXPORT-LOG.md` must say `ok` (Phase 1A.0). |
| Blurry image on the page | Export at 2x, resize down only, WebP quality 85+, vectors stay SVG (§1 rule 16). |
| Retyping copy from a screenshot, or writing "Lorem ipsum" / made-up copy | Use the exact text from `Design/CONTENT.md` (Phase 1A.0 step 5, §1 rule 20). |
| Figma prototype animation ignored | Write it in `Design/ANIMATIONS.md` and map it to a §8 preset (Phase 1A.0). |
| A blank browser tab, or a favicon made from the full wordmark | Favicon = the logo icon (mark), square, sharp, linked on every page (§1 rule 19). |
| A page without the DeveloperAI meta tag | Every page `<head>` gets `<meta name="DeveloperAI" content="Developer Skill">` by default (§1 rule 18). |
