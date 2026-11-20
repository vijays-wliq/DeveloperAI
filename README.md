# Developer AI

Instruction docs that Claude Code follows to build websites — from a Figma design to HTML, and from
HTML to a WordPress theme (custom PHP / ACF, or Elementor). Lessons from real builds are added to
these docs as new rules, so every next build starts from them.

## Instruction docs

| File | Use it for |
|---|---|
| [`PROJECT-BOOTSTRAP.md`](PROJECT-BOOTSTRAP.md) | **Figma → HTML.** One-time setup for an empty project folder. Builds a token-driven front-end starter (`vendor.css`, `global.html`, `CLAUDE.md`, folder structure) from the Figma file. |
| [`CLAUDE-WORDPRESS-CONVERSION-EXPERT.md`](CLAUDE-WORDPRESS-CONVERSION-EXPERT.md) | **HTML → custom WordPress theme** (PHP + ACF PRO). Includes the page builder choice — ACF Flexible Content / Gutenberg (ACF Blocks) / Both (§108), forms plugin choice (§109), ACF PRO only (§110) and ACF local JSON + sync (§111). |
| [`CLAUDE-ELEMENTOR-WORDPRESS-CONVERSION-EXPERT.md`](CLAUDE-ELEMENTOR-WORDPRESS-CONVERSION-EXPERT.md) | **HTML → WordPress with Elementor.** Main rule (§96): the Elementor site must match the HTML site visually and functionally. |
| [`wordpress_theme_developer_brief.md`](wordpress_theme_developer_brief.md) | Finishing a site on an **existing WordPress theme** to the client's requirements. |

## Other files

- `Skill-PDFs/` — PDF versions of the four skills: HTML Design, WordPress ACF, WordPress Elementor,
  WordPress Theme-Based.
- `HTML-Build-Time-Report.html` — build-time report for the HTML build with `PROJECT-BOOTSTRAP.md`.
- `All-Skills-Build-Time-Report.html` — build-time report for all four skills with Claude Code.

## How to use

1. Copy the right instruction doc into the project folder (next to the Figma link or the HTML site).
2. Tell Claude Code to follow it.
3. For WordPress builds, answer the questions asked at the start: page builder (§108) and forms
   plugin (§109).
4. Put the site login in `login.md` in the project folder (§110). **Never commit `login.md`.**
