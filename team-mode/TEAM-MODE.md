# TEAM MODE — PM playbook (Figma → HTML + WordPress with an agent team)

> **Who reads this:** the **main Claude Code session**. In team mode the main session is the **PM**.
> It talks to the user, owns the gates and the task list, and calls the four agents in
> `.claude/agents/`: `designer`, `html-dev`, `wp-dev`, `qa`.
>
> **Why the PM is not an agent file:** a subagent cannot start other subagents. Only the main
> session can, so the main session plays PM.
>
> **Skill:** depends on the project type (step 0). For WordPress builds it is the Master skill
> `claude-master-wordpress-build/CLAUDE-MASTER-WORDPRESS-BUILD.md`. Team mode keeps every Master
> rule, with one exception in type 2: **an HTML developer builds every section as HTML first**
> (see "HTML + WordPress together" below), so Master's "no static HTML site first" rule does not
> apply there.

**How to start:**

```
Read TEAM-MODE.md and run the project in team mode.
Figma: <figma.com URL>
Login: login.md          (WordPress projects only)
```

---

## 0. Project type — the PM calls ONLY the agents this type needs

All four agents can stay installed. An agent does nothing until the PM calls it. **The first
question the PM asks is the project type**, then it uses only that row's agents — in every step,
including role input and the bug loop:

| # | Project type | Skill the agents follow | Agents the PM calls | Not called |
|---|---|---|---|---|
| 1 | **HTML only** (Figma → static HTML site) | `PROJECT-BOOTSTRAP.md` | `designer`, `html-dev`, `qa` | `wp-dev` |
| 2 | **Figma → WordPress, HTML + WP together** (default for WordPress) | Master skill | `designer`, `html-dev`, `wp-dev`, `qa` | — |
| 3 | **Figma → WordPress, no HTML pages** | Master skill | `designer`, `html-dev`, `wp-dev`, `qa` | — |
| 4 | **Existing WordPress theme** | `wordpress_theme_developer_brief.md` or Master Part C | `html-dev`, `wp-dev`, `qa` (+ `designer` if there is a Figma design) | — |

- Tell every agent the project type and its skill file in every call.
- Write the type in `SCOPE.md`. If the type changes later, ask the user before calling a new agent.
- Gates stay the same for every type: scope + tasks, `TOKENS.md` (types 1–3, or the brief's plan
  approval in type 4), final review.
- **Type 1 (HTML only):** there is no theme. `html-dev` builds in `HTML/` exactly as
  `PROJECT-BOOTSTRAP.md` says (`global.html`, `HTML/CLAUDE.md`, `assets/`), and `qa` routes every
  bug to `html-dev`. Skip the pipeline in step 6 — `html-dev` builds, `qa` tests.
- **Type 3 (no HTML pages):** same split as type 2, but there is no `html-preview/`. `html-dev`
  writes the front end straight into the theme — section markup in
  `template-parts/sections/<section>.php` (static, from `CONTENT.md`) + CSS / JS in `assets/` —
  and `wp-dev` then replaces the static content with ACF fields / the builder. Elementor:
  `html-dev` owns `vendor.css` tokens, `animations.js` and any approved `custom.css`; `wp-dev`
  builds the widgets. `qa` compares WordPress with Figma only.
- **Type 4 (existing theme):** `html-dev` owns the front-end work on the existing theme — CSS / JS
  in the child theme, matching the design on the theme's own classes and grid. `wp-dev` owns
  widgets, PHP, settings and content. Follow the brief's backup, plan-approval and update-safety
  rules.

---

## Project folder

```
<Project-Name>/
├── .claude/agents/                 designer.md  html-dev.md  wp-dev.md  qa.md
├── TEAM-MODE.md                    this playbook (PM)
├── claude-master-wordpress-build/  the Master skill
├── login.md                        WordPress login — only wp-dev uses it; never print or commit
├── Design/  TOKENS.md              designer (Master A2–A6)
├── SCOPE.md                        PM — scope + every role's input
├── TASKS.md                        PM — final task list, one row per section per owner
├── QA-CHECKLIST.md                 qa — checklist per developer
├── BUGS.md                         qa — every bug, owner, status
├── FINAL-QA.md                     qa — final round (Master F2)
└── <wordpress>/wp-content/themes/<project-slug>/
    ├── html-preview/               html-dev — one static HTML page per Figma page
    └── assets/css · assets/js      SHARED by html-preview and the WordPress theme
```

---

## The flow

```text
1  PM: scope and agenda        → ask the start questions (Master A1), one at a time
2  Role inputs (parallel)      → designer · html-dev · wp-dev · qa each add input to SCOPE.md
3  PM: final scope and tasks   → TASKS.md                                   ⛔ GATE 1 — user approves
4  qa: checklist per developer → QA-CHECKLIST.md
5  designer: design spec       → Design/ + TOKENS.md                         ⛔ GATE 2 — user approves
6  html-dev + wp-dev           → section by section, in parallel (pipeline below)
7  qa: test and verify         → BUGS.md; bugs go back to their owner until all are verified
8  PM: final review            → Master F3; issues go back to step 6        ⛔ GATE 3 — user approves
9  Approved: delivered         → final report
```

### 1. Scope and agenda (PM)

- First question: the **project type** (step 0). Then ask the start questions of that type's skill
  **one per message** (Master **A1** for WordPress, `PROJECT-BOOTSTRAP.md` Phase 0 for HTML only),
  and show one summary to confirm. For WordPress:
  project + theme slug, pages, Figma, animation, page builder (E1), forms (D109), WooCommerce,
  new or existing theme, site login.
- Write the confirmed answers to `SCOPE.md`.

### 2. Role inputs (agents, in parallel)

Call **this project type's agents** (step 0) **in one message** so they run at the same time.
Each one reads `SCOPE.md`, the Figma file and its own part of the Master skill, and appends a
section to `SCOPE.md`:

- **Risks** (e.g. "this slider will not match the builder's carousel")
- **Questions** for the user
- **Estimate** of its sections / tasks

The agents only add input in this step. They build nothing.

### 3. Final scope and tasks (PM) — ⛔ GATE 1

- Merge the inputs. Ask the user every open question, one at a time.
- Write `TASKS.md`: one row per **section × owner**, in build order (Master B64 / E5.35):

```markdown
| ID | Page | Section | Owner | Depends on | Status |
|---|---|---|---|---|---|
| T-001 | Global | Header | html-dev | TOKENS.md | todo |
| T-002 | Global | Header | wp-dev | T-001 | todo |
| T-003 | Home | Hero | html-dev | TOKENS.md | todo |
| T-004 | Home | Hero | wp-dev | T-003 | todo |
```

  Status: `todo` → `doing` → `done` → `qa-ok`.
- Write the architecture report (Master **B65**) into `SCOPE.md`.
- **Stop. Show the user `TASKS.md` + the report. Build nothing until they approve.**
- If the user gives an **Orbit project**, offer to mirror the tasks there. Ask first; never
  create Orbit tasks without approval.

### 4. Checklist per developer (qa)

`qa` writes `QA-CHECKLIST.md` with two lists: **html-dev** and **wp-dev**, taken from the Master
skill (see `qa.md`).

### 5. Design spec (designer) — ⛔ GATE 2

`designer` runs Master **A2–A6**: Figma export into `Design/` (pages, components, images, SVG,
`CONTENT.md`, `ANIMATIONS.md`, `EXPORT-LOG.md`) and `TOKENS.md`.
**Stop. Show the user `TOKENS.md`. Nothing is built until they approve.**

### 6. HTML + WordPress together (pipeline)

The two developers work **at the same time, one section apart**:

```text
html-dev: header (proving) → sections 2–5 → sections 6–9 → …
                ↓ qa ok          ↓ qa ok (batch)
wp-dev:         header        →  sections 2–5 → …
```

- `html-dev` builds section N (HTML + CSS + JS), marks its task `done`.
- `qa` checks it against the Figma frame. If it passes → task `qa-ok`.
- **QA in batches:** only the **proving section** (header, or hero) gets its own QA run. After it
  is `qa-ok`, the PM calls `qa` once per **batch of 3–4 `done` sections, or once a whole page is
  done** — not once per section. `html-dev` keeps building the next batch while `qa` tests this one.
- `wp-dev` takes **only `qa-ok`** HTML sections and turns them into the WordPress section (PHP +
  ACF fields, or the chosen builder), while `html-dev` builds section N+1.
- Call `html-dev` and `wp-dev` in the same message when both have work, so they run in parallel.
- First run: build **one proving section** (header or hero) through the whole chain before the
  rest (Master F1 step 1).

**One CSS, never two copies.** `html-dev` writes CSS and JS **directly in the theme's
`assets/css/` and `assets/js/`** (Master A7). The pages in `html-preview/` load those same files,
and the theme enqueues them (A7.6). A fix in the HTML is automatically a fix in WordPress.

**Elementor builds:** Elementor sections are built with native widgets, not the HTML markup.
`html-preview/` is then the approved visual reference that `wp-dev` and `qa` compare against.
`vendor.css` holds tokens only (E5.19).

### 7. Test and verify (qa) — the bug loop

- `qa` tests every `done` task and writes every bug to `BUGS.md`:

```markdown
| ID | Page | Section | Width | What is wrong | Expected (Figma) | Owner | Status |
|---|---|---|---|---|---|---|---|
| B-001 | Home | Hero | 375 | Title wraps to 4 lines | 2 lines, 32px | html-dev | open |
```

  Status: `open` → `fixed` (developer) → `verified` (qa) — or back to `open`.
- **Owner rule:**
  - design, spacing, fonts, responsive, animation, slider look → `html-dev`
  - content not editable, ACF, menus, forms, importer, PHP warnings, WP-only breakage → `wp-dev`
  - right in `html-preview/` but wrong in WordPress → `wp-dev`
- **Test by template:** `qa` gives the full test once per template and shared section; pages that
  reuse a `qa-ok` template get a light check (see `qa.md`). Site-wide checks run only in the
  final round.
- **QA model:** call `qa` with `model: sonnet` for script-only runs, role input, the checklist and
  verify runs; call it with `model: opus` for test runs with the design compare.
- PM sends each developer **only its own open bugs**, then calls `qa` again to verify.
- **Batch the verify:** wait until the developers have fixed all bugs of the current round, then
  call `qa` **once** for all `fixed` bugs — not one verify run per fix.
- **Rolling QA (QA and fixes in parallel):** after the first QA round, do not wait for all bugs to
  be fixed before testing more. While the developers fix the bugs of group A, `qa` tests group B;
  `qa`'s next run tests group C **and** verifies group A's `fixed` bugs in the same run. Rules:
  1. Bugs in shared sections (header, footer, templates) are fixed first, in round 1. `qa` does not
     test pages whose shared section is still being fixed — test them after the fix.
  2. `qa` works on a new group, the developers on an old one — never on the same page at the same time.
  3. Verify stays batched — inside the next group's test run, never one run per fix.
  4. In `BUGS.md`, `qa` only adds rows and sets `verified` / `open`; developers only set `fixed`.
  A one-page site has no next group to test — there `qa` simply waits for the fixes.
- **Loop limit: 3 rounds.** A bug still open after 3 rounds goes to the user with what was tried.

### 8. Final review (PM) — ⛔ GATE 3

- When every bug is `verified`, `qa` runs the final round into `FINAL-QA.md` (Master F2).
  Keep it light: the script re-runs on every page, but the design compare is done once per
  template (see `qa.md`) — not a second full QA.
- PM checks Master **F3 Definition of done**. Any issue → back to step 6 with an owner.
- **Stop. Show the user the summary and `FINAL-QA.md`. Deliver only after approval.**

### 9. Delivered

Final report: pages built, checks done (with how), bugs fixed, what still waits on the client
(real copy, images, URLs), and the open items list. Remind the user to revoke the Application
Password.

---

## PM rules

- The PM never writes theme code itself. It plans, asks, routes and reviews.
- Every agent call gets: the task IDs, the files to read, the files it may write, and what "done"
  means. Agents only know what the call tells them.
- Never pass `login.md` contents to an agent. Tell `wp-dev` to read it itself.
- Do not claim anything is tested unless `qa` actually tested it.
- **Small change after delivery?** Do not run team mode. Use **Master F4 Quick update mode**.
